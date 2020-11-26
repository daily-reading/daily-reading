# Micro Frontends Pattern Comparison

* Author: Florian Rappl
* Link: https://blog.bitsrc.io/microfrontend-pattern-comparison-c50a9d2e4172

***Build-Time Integration
pro:简单
con:obviously, perf impact，每次有改动后都需要把整个package重新build。
用packages.json把各个microservice捆绑在一起，适合简单的app。实际应用中不太会用到。而且对于shared library的管理也比较麻烦

***Server-Side Integration
pro:best perf
con:complexity
之前没看到过的一种模式。类似于SSR？对于dev非常不友好，可以预见debug和test会很痛苦。文中也提到了，适合heavy content sites。感觉也不实用，但提供了一种有意思的思路。

***Run-Time Integration via iframe
pro: easy, built-in support from all modern browsers, strong isolation.
con: who likes iframe?
在某些情形下iframe确实是不二的选择，但多数情况下，我个人更觉得iframe是一种hack。这种approach对于styling就很不友好，页面也不responsive。

***Run-Time Integration via script
pro: modern, beautiful, standard approach to implement frontent micro-service.
con: someone needs to IMPLEMENT it :-)
文中说的loyal followers大概就是我这种吧,lol。可以写一个component来做dom manipulation或者类似的操作(router load components，etc.)来实现对于特定microservice的mount 和 unmount.

***Conclusion
我对于micro service的理解是，每一个service都是可以作为一个standalong的service来独立运行，top level app container只是一个mount和unmount各个service的容器，同时也是shared library和shared UX components/styling存放的地方。但这又会牵扯到另一个问题，就是各个service的decoupling，过多的shared library就会破坏这种decoupling，比如不同的service对于同一个library要使用不同的version之类的问题。我个人是比较倾向于via script的做法，感觉是比较适合绝大多数site的选择，其他几种approach都是对于特定的情况比较适用。这篇文章没有谈及cross service communication该怎么做，可以通过custom events，centralized store (redux)，甚至是url (router parameters/data)来实现。总而言之，具体问题具体分析。