
### 要解决的的主要的问题：
**解决分布式系统中实践A，B发生顺序的全系关系**

### 简化与分析
从偏序的Happens Before关系推到全序关系，归纳出的两操作规则：
* IR1. Each process Pi increments Ci between any two successive events.
* IR2. (a) If event a is the sending of a message m by process Pi, then the message m contains a timestamp Tm= Ci<a>. (b) Upon receiving a message m, process Pi sets Ci greater than or equal to its present value and greater than Tm.

简化为以下三个条件：
* (I) A process which has been granted the resource must release it before it can be granted to another process.
* (II) Different requests for the resource must be granted in the order in which they are made.
* (III) If every process which is granted the resource eventually releases it, then every request is eventually granted.

### 本论文提出的核心算法
提出一个使I，II，III满足的算法：
1. To request the resource, process Pi sends the message Tm:Pi requests resource to every other process, and puts that message on its request queue, where Tm is the timestamp of the message.
2. When process Pj receives the message Tm:P requests resource, it places it on its request queue and sends a (timestamped) acknowledgment message to Pi.
3. To release the resource, process Pi removes any Tm:Pi requests resource message from its request queue and sends a (timestamped) Pi releases resource message to every other process.
4. When process Pj receives a Pi releases resource message, it removes any Tm:Pi requests resource message from its request queue.
5. Process Pi is granted the resource when the following two conditions are satisfied:
    (i) There is a Tm:Pi requests resource message in its request queue which is ordered before any other request in its queue by the relation =>.
    (ii) Pi has received a message from every other process timestamped later than Tm.
纵观全文上述算法是本篇论文的核心价值。
算法本身非常优雅，规则简单（只有5条），属于分布式算法，不需要存在中心存储与中心进程。

论文后续着重讨论了我们的物理宇宙中利用上述算法实现利用相对论的偏序关系构建全序关系系统的物理始终的设计方式。这部分本身高于了算法设计的本身，不过深研究了。


