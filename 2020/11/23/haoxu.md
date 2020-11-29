# How LinkedIn scales compatibility testing

* Author: Nima Dini, Dan Sully
* Link: https://engineering.linkedin.com/blog/2020/compatibility-testing

keynotes:
"library producers and consumers can release software independently."
strategy: semmantic versioning. So consumers know when it might be a good time for upgrading. And producers need to make sure validating backwards-compatibility, aka compatibility testing (also the topic of this article).

Q: how teams in linkedin are divided? Are they divided by components/library ownership? Or feature ownership? Or scenario based?

How compatibility testing is done? and what are the challenges.
Nothing fancy, similiar to integration tests. The CI flow consists of linting and extended testing for library consumers (UT and E2E?). Each consumers' test suites needs to be executed before the library can be successfully published. This requires the consumers' test suites to have a good coverage. And a good performance since this flow can be triggered many times during the release cycle. And how to deal with flaky tests? who owns the tests and who is responsible for maintaining the tests? What if those tests are "blocking" a major QFE because they are failing intermmittently (I assume the on-call person will be notified)?.

Debuggability
1. good reporting and navigation to the error with detailed logs.
2. ready-repro environment.
I love the idea of #2. The most efficient way to locate a regression is live-debugging. But how to avoid wasting resources because of infra or external issues? It's not reasonable to always reserve a dev box for every single compatibility test failure.

Stability
To ensure stability of the tests
1. Set a reasonable failure threshold.
2. Ignore flaky tests.
3. required consumers.
The idea is similiar to categorizing tests into different groups, which is what most of the test teams are doing. #1 is risky to me since it might let a true positive go into production (even the chance is very low). I personally prefer using #3 for a sanity check and let test team or CI to run the whole test suites, which might take hours. Each product team needs to act quickly on test failures since it could be a potential regression. Especially when the code will be deployed directly into production quickly.

Perf bottlenecks
1. job scheduler. Dont let a long running job delays the entire batch. Just kick off a new job when a worker is available.

2. pre-test preparation. Instead of simulating real-world HTTP calls, do it in a one-box which has everything the consumer test needs, to save the time wasted on downloading and network traffic. 
Thoughts: using images which has most of the required libraries and tools to allocate the one-box for testing and better perf? But needs someone to maintain the image anyway.

3. timely enforcement of success criteria. When it meets the success criteria, skip the rest tests.
Q: I assume all the tests are running randomly. Otherwise, some tests might never be executed. And again, I feel this is risky since it might let a real regression go through. It's always a balance between perf and accuracy.

Future work
1. more intuitive and discoverable test reports.
2. auto detack flaky tests. LOL, my team has been doing a weekly test review and all the test owners who own flaky tests (# of failure count passes a threadhold in release cycle) will have P1 bugs assigned, which means, stop what you are working immediately and fix this f**king test, otherwise, it will appear on the weekly service review for the leadership team, and your manager will be embrassed.
3. consumer prioritization. More intellectual for ordering consumer tests for the least execution time.

Conclusion:
Thoughts
1. How to act on flaky tests efficiently. AFAIK, most devs dont want to "waste" time on tests which cannot bring them visibility. They(We) want to complete those shinning features which could be helpful for career growth and promotion. And the enforcement is more for management and office politics.
2. This article is more for ES team I guess, for devs, it is good to have some background about how the tests are hooked into the system, in case some tests are failing constantly and unfortuantely you are on-call :-(
3. What is the SDP to deal with test failures in this pipeline.
4. How to review this compatibility test flow and what are the success criterias?
5. stage this flow for multiple PPE environments in case on layer fails to catch an error?

A good compatibilty test pipeline and framework would be super helpful for service reliability.