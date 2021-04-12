# 提交说明
- 编号：NO.13@20210326
- 文章：[modules-monoliths-and-microservices](https://tailscale.com/blog/modules-monoliths-and-microservices/)

# 读书笔记

## What is a microservice
从一个单体应用 到一堆（fleet of）微服务，这是两个极端。
let's abandon the assumption that a monolith and a fleet of microservices are the only two options. There's a wide and nuanced continuum from "one giant service that does everything" to "infinite tiny services that each do nearly nothing."

## Boxes and arrows
作者用box 和 arrow 来解释模块和微服务，而box画多大，怎么使用arrow做连接是一个难题。
> how big are the boxes? How much goes in each box? How do we decide when to split one big box into two smaller ones? What's the best way to connect the boxes? There are many approaches to all this. Nobody quite knows what's best. It's one of the hardest problems in software architecture.

作者觉得近十多年的演进，是很混乱的。
> This all evolved over decades. Fancy people call it "path dependence." I call it a mess. And let's be clear: most of the mess no longer provides much value.

## The quest for modularity
划分模块，有如下基本目标：
- Isolate each bit of code from the other bits.
- Re-connect those bits only where explicitly intended (through a well-defined interface).
- Guarantee that bits you change will still be compatible with the right other bits.
- Upgrade, downgrade, and scale some bits without having to upgrade all the other bits simultaneously.

其中第一项 isolation 是基础，完成这一样，其它目标就容易实现了，但是从软件发展史来说，隔离这件事情做的非常的差。

## Great! VMs for everything!
隔离很难搞，花费大量的时间和精力来做隔离，最近还是被找到漏洞。我们系统需要保证基础的安全性和隔离，但是不能为了隔离和安全做太多复杂的事情。
> avoid overcomplicating your systems without improving actual security. 

作者把模块或者服务的边界划分为两类：
- Trustworthy: Boundaries where the two modules mutually trust each other not to be malicious and therefore can use weak isolation.
- Untrustworthy: Boundaries where modules do not trust each other, so they must use strong isolation.

如果是可信环境，就选择相信。

## Module boundaries vs service boundaries
单体也好，微服务也好，只要能够划分清楚边界，在这个层面上单体和微服务没有区别。
作者不期望把安全性、隔离性来做为划分边界的依据：
> you almost never define module boundaries for security reasons.

模块的遵循Conway's law
> module boundaries typically follow Conway's law. 

## Where should I put my service boundaries?
当遇到以下情况时，应该要划分边界了
- Does your monolith take a long time to startup
- Do you need the right datastore schema version
- Are continuous integration tests frequently failing
- Do some parts scale differently from other parts
- Do expensive requests need to be run with less parallelism
- Do you have services with different quality/reliability targets

##conclusion
> Remember, ChromeOS is a monolith. iOS is a monolith. Your team is probably much smaller than either of those teams. You simply don't need to juggle a lot of microservices to get what you want. 
> Architect things the easy way until you're absolutely forced to do them the hard way. 

