# [How Facebook is bringing QUIC to billions](https://engineering.fb.com/2020/10/21/networking-traffic/how-facebook-is-bringing-quic-to-billions/)

- Facebook 目前的情况
    - 75% 的流量已经切换到 （QUIC & HTTP/3），用户体验明显提高
    - 明显优化点
        - request errors
        - tail latency
        - response header size
        - 其他
- 什么是 QUIC 和 HTTP/3
    - QUIC 将会代替 TCP 协议
    - 历史
        - QUIC 最早是在谷歌内部开发的 gQUIC
        - 后面于 2015年 转给 IETF（Internet Engineering Task Force），由 IETF 再次开发和扩展形成的一个新的协议
    - QUIC and HTTP/3 优于 TCP 是有非常强大的丢失追回机制，避免像 tcp 一样因为丢包阻塞而导致整个连接变慢
    - 由于现在网络的复杂中间件如防火墙等对数据包编码格式有一定的要求导致 tcp 网络协议很难升级，QUIC 通过完全加密避免了这些问题，使协议有极好的扩展性，并且也很好的保证了未来的发展
    - QUIC 也支持使用 QLOG 进行流量监测和可视化传输
    - [QUIC 其他资料](https://docs.google.com/document/d/1gY9-YNDNAB1eip-RTPbqphgySwSNSDHLq9D5Bty4FSU/edit)
- 以经验为导向进行试验性接入
    - [mvfst](https://github.com/facebookincubator/mvfst)  a client and server implementation of IETF QUIC protocol in C++ by Facebook
    - 先把 QUIC 部署在 facebook 的网络上，包括内网和公共网络
    - 在排除一些问题和 bug 后，开始计划将流量均匀分布并保证 zero-downtime
    - 确定没有问题后在逐步将 QUIC 整体铺开
- facebook app 接入 QUIC
    - facebook app 作为首选接入 QUIC 的 app 可以以有限的方式安全地让程序进行更新，一开始接入是只在 dynamic GraphQL requests 中使用 QUIC（无静态资源内容）
        - 6 percent reduction in request errors
        - 20 percent tail latency reduction
        - 5 percent reduction in response header size relative to HTTP/2
    - 发现一个问题在动态请求切换成 QUIC 的时候发现静态请求文件（TCP）请求报错却在增加
        - 问题是因为使用启发式算法策略去控制 TCP 请求，但是在 QUIC 并不适用，阈值不准确导致请求一次发送太多，导致系统变慢
    - 静态资源请求接入 QUIC
        - 需要解决 mvfst 的 CPU 性能问题和 拥堵控制算法的效率（动态请求内容比较少，时间也比较短不会占用 CPU 太多消耗也不太需要拥堵控制）
        - 开发性能测试工具监测 CPU 消耗和拥堵算法对网络资源的利用效率
        - 性能优化
            - 优化 UDP 数据包，使其能够更加平滑的进行传输
            - 使用 GSO 同时发送多个 UDP 包
            - 优化数据结构和算法处理未确认的 QUIC 数据
    - facebook 全面 接入 QUIC 前，对处理视频请求进行了一个试验
        - MTBR 提升 22%
        - video request error 降低 8%
        - video 的卡顿降低 20%
- 同 facebook app 我们也在 Instagram 中部署了 QUIC，使得 Instagram 性能也有了很大的提升，未来我们也会对 facebook 旗下的其他应用进行 QUIC 升级
- IETF 将在 2021 年完成对 QUIC 协议的意见收集，相信在不久的将来像 QUIC 这样的技术将会成为互联网应用的关键
            

    
