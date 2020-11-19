# Reading note PART-1

When taking about the distributed system, most of us will have an ambivalence attitude that would rather hate it but also love it. Though it has tens of advantages to build a distributed system, problems orginate in the non-negligible delay of message transmission and the unpredicatable order of events as well. 

People must realize these two trouble-makers when design a program over a distributed system. In the distributed system, the traditional concept of "happened before" is merely a false appearance and you cannot simply guarantee anything based on this assumption, as it may describe the ordering partially. The paper [Time, Clocks, and the Ordering of Events in a Distributed System](https://www.microsoft.com/en-us/research/uploads/prod/2016/12/Time-Clocks-and-the-Ordering-of-Events-in-a-Distributed-System.pdf) presents a well known and widely referenced mathematical view of events ordering that could help people gain more insight over the distributed system.

The paper tried to find a way to prove a system of clocks correct based on the order oin which events occur but not on the physical time. Here are mathematic assumptions, definitions and deductions introduced in the paper. 

#### **Assumptions:**

1. Thes system is composed of a set of processes. Each process consists of a sequence of events.
2. Sending or Receiving a message is an event in a process.
3. Both logical and physical time is increasing monotonically.
4. 

#### **Definitions:** 

→ implies an irreflexive partial ordering relationship ("Happend Before") on the set of all events in the system, while ⇒ implies totoal ordering in the whole system.

1. **a** → **b** if
   1. **a** and **b** are events in the same process, and **a** comes before **b**	*OR*
   2. **a** causally affects **b**. Eq. **a** is the sending of a message by one process and **b** is the receipt of the same message by another process    *OR*
   3. **a** → **c** and **c** → **b**

2. Two distinct events a and b are said to be concurrent if **a** ↛ **b** and **b** ↛ **a**.

3. For any events **a**, **a** ↛ **a**.
4. Clock Condition: C(a) < C(b) if **a** → **b** for any events **a** and **b**, where C(x) states the logical clock time when x is happened.

#### **Deductions:**

1. A process cannot know what another process did at a physical time until a pair of happened-before events occurs between them.
2. Each process increments its logical clock between any two successive events.
3. If event **a** is the sending of a message m by process P<sub>i</sub>, then the message m contains a timestamp T<sub>m</sub>= C<sub>i</sub>(a). Upon receiving a message m, process P<sub>j</sub> sets C<sub>j</sub> greater than or equal to its present value and greater than T<sub>m</sub>.
4. If **a** is a event in P<sub>i</sub> and **b** is a event in P<sub>j</sub>, then **a** ⇒ **b** IF and ONLY IF 
   1. C<sub>i</sub>(a) < C<sub>j</sub>(b)	*OR*
   2. C<sub>i</sub>(a) = C<sub>j</sub>(b)	*OR*
   3. P<sub>i</sub> < P<sub>j</sub> (happened-before relationship between processes)
5. Total ordering relationship is not unique that different choices of clocks yield different relations ⇒.

#### **Algorithm:**

(Assumtion1: for any two P<sub>i</sub> and P<sub>j</sub>, one always receives messages in the same order as sent by another; Assumtion2: all messages are received; Assumtion3: a process can send messages directly to every other process)

Request broadcasting algorithm could ensure the request and release of resources following the total ordering by 5 rules: 

1. To request the resource, process P<sub>i</sub> sends the message **[T<sub>m</sub>:P<sub>i</sub>] Request** to every other process, and puts that message on its request queue.
2. When process P<sub>j</sub> receives the message **[T<sub>m</sub>:P<sub>i</sub>] Request**, it places it on its request queue and sends a **[T<sub>n</sub>:P<sub>j</sub>] ACK** message back to P<sub>i</sub>.
3. To release the resource, process P<sub>i</sub> removes **[T<sub>m</sub>:P<sub>i</sub>] Request** message from its request queue and sends a **[T<sub>o</sub>:P<sub>i</sub>] Release** to every other process.
4. When process P<sub>j</sub> receives a **[T<sub>o</sub>:P<sub>i</sub>] Release** message, it removes **[T<sub>m</sub>:P<sub>i</sub>] Request **message from its request queue.
5. Process P<sub>i</sub> is granted the resource when the following two conditions are satisfied:
   1. There is a **[T<sub>m</sub>:P<sub>i</sub>] Request** messagein its request queue which is ordered before any other request in its queue by the total order relation ⇒.  *AND*
   2. P<sub>i</sub> has received ACK messages from every other process with timpstamp greater than T<sub>m</sub>.

This is a distributed algorithm that does not need centralized management, and each process independently follows these rules and do local checks. However, the failure of one process would influence the synchronization fo all processes as this algorithm requires a joint participation of all processes and other process cannot figure out the difference between failure and the time gap between events.



#### To be continue

T_T 我看不太懂那些数学证明，我觉得我懂了，但我写不出来，所以我还没有很懂
