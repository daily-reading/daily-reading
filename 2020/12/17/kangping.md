# Structured Concurrency

## 1. Motivation
TCP session implementation is simple in Erlang but complicated in C or most other languages. Most of this complexity is due to concurrently opened TCP connections/error handling/shutdown process. The author claims an improvement in programming model is needed to solve this problem. 

## 2. Solution
(The author thinks you have to spawn one thread per TCP conncetion.) However, in order to reduce the overhead in sharing state between threads, you will need some version of lightweight green threads, and a simple communication mechanism. (e.g UNIX processes/pipes, GO goroutines/channels, Erlang processes/mailboxes.) The author also argues that if you have green threads and a sane cancellation mechanism, you will not need state machines.

## 3. Model of structured concurrency
You can think of structured concurrency as a borrowed concept from structured programming, (which has relatively strict requirement on "scope"). When green thread B is launched after green thread A, green thread 
B has to terminate before green thread A. 
`

	void foo(void) {
		...
		go(bar())
		...
	}
	
`


where `bar()` is fully local to `foo()`. Behavially the only question is what happens when `foo()` returns but `bar()` is still running. We could block `foo()` or we could cancel `bar()`, or we could choose a mixture of the two. Block is trivial, but we have to handle "cancellation".

## 4. Handle cancellation
Cooperative scheduling of green threads requires code to be split into reasonably sized chunks, so we can wait until a chunk to finish running and cancel the thread. Because of this, we would only need to check cancellation when calling function can switch threads, i.e. blocking functions. 

To signal that parent function is cancelling the current green thread is simple. However, initiate the cancellation from the parent function is harder. 

## 5. Initiate cancellation
To do this properly, you would need a thread handle 
`
	coro handle = go(bar())
`
This would mean green threads need to be explictedly cancelled. (This is semanticaly different from GO.)
So the API becomes, 
`
	void foo(void){
		coro cr = go(bar())
		...
		gocancel(cr)
	}
`
However, we would want give process a grace period to shutdown when needed. This means that we need two type of cancellation. One is "cancel if you have nothing to do", the other is "cancel even if you are not finished". Fortunately, we have two type of communication between the owner thread and the owned. One if the channel "ch" used to send messages, the other is the cancellation mechanism. We can introduce channel closing function `chdone()` which causes all subsequent operation on the channel to return `EPIPE` error. (This maps directly to the first type of cancellation). The second type of cancellation is `gocancel(handle, grace_period)`, which will terminated the owned process after the given grace_period. The API therefore changes into 

`
	void foo(void){
		coro handle = go(bar(ch))
		...
		chdone(ch)
		gocancel(handle, now()+1000)
	}
`

When `main` cancels `foo()` with a grace period shorter than `foo()` gives `bar()`, we need to make sure `bar()` terminate before `foo()` is terminated by `main()`. Fortunately, `gocancel()` is a blocking function, and could return `ECANCELED`.

We could also define a connection struct and allocated the struct to the heap so that it could be managed more freely.

