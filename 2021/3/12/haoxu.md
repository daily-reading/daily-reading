Performance is a FEATURE!

what makes a page slow?
1. # of requests
note: solution? batch and reduce the size of the payload.
2. code
note: solution? lazy load, virtualization, inifinite scroll, zone, requestanimationframe, etc.
3. Images
note: solution? no idea, use svg instead?
4. networks
note: solution? switch to a different ISP. lol and use CDN
5. Third parties
note: solution? 3rd parties is a double bladed sword. It brings convenience but also increases the cost of mainteance. and increases bundle size significantly. Use whenever the pros >> cons.

How to measure?
1. Synthetic monitoring
Note: hmmm....doesnt seems very practical, a few issues I notice, 1. what if it is an edge case path? does it necessary worth monitoring? 2. real world end user is MUCH MUCH MORE complicated than automations. IMO, bot is only useful for realibility testing.

2. Real user monitoring
Note: telemetry!!!!

What do the different metrics mean?
answer: DEPENDS. Need to go through the references. What I usually use is the time for an end to end scenario. and then break down into small slots. So we can see which part to improve.

How to use percentiles
1. Performance is a distribution.
note: hmmm, good point, I probably need to update my perf monitoring report with a histogram.

2. 50:median, 75:most popular 95:if your app is super fast.

Note: 这篇主要是从PM的角度讲了一些web perf的fundmental，对于DEV我觉得也是有用的，至少可以，做了IMPROVEMENT之后，如何从各个角度来MONITOR你的优化是否真的起到了作用。同时可以做出很好的REPORT拿去LEADERSHIP来BB。