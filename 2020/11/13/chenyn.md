# [How Facebook is bringing QUIC to billions](https://engineering.fb.com/2020/10/21/networking-traffic/how-facebook-is-bringing-quic-to-billions/)

- 介绍了一下 QUIC 和 HTTP3 的背景
    - 这里简单了解了下什么是 TCP 行头阻塞（ HOL ）https://qastack.cn/software/325881/is-it-a-good-idea-to-multiplex-blocking-streams-into-a-tcp-connection
    - QUIC 实质是将 TCP 迁移到 UDP，避免了 TCP 多路复用的尴尬，用 TSL 1.3 来实现可恢复，用 QLOG 实现可视化追踪
    - Node 15 已经尝试试验性支持 QUIC，见 https://mp.weixin.qq.com/s/8-82IaXIR6qEdHVgGBDG8A
    - 补充一篇介绍 https://docs.google.com/document/d/1gY9-YNDNAB1eip-RTPbqphgySwSNSDHLq9D5Bty4FSU/edit
- Facebook 决定使用 QUIC 时，先用了大量内部流量来验证可行性，还搞了个负载均衡器来监测发布
    - 开发了一套叫 mvfst 的协议实现用于部署 QUIC，且得力于之前的 Proxygen、Zero、Fizz 可以平滑迁移（ 这东西没细讲 ）
- 在对 Facebook 进行 QUIC 迁移时，尽管动态请求指标有所改善，但静态请求质量下降
    - 现象层原因是，只 enabled QUIC for dynamic GraphQL requests，导致 improving one type of request may have had detrimental side effects for others
    - 之前的 TCP 时代，先加载静态文本流然后视情况加载动态媒体流，可以通过阈值来控制，但对 QUIC 不起作用了，因为一瞬间尝试发送过多的请求，最终导致加载时间反而更长
- 尝试规模化，并在静态请求中启用 QUIC
    - 静态数据流更容易消耗 CPU，也会对 congestion control (BBR) 阻塞控制的实现有影响
    - 先建立起监控以及上面提到的负载均衡器来调节
    - 一个有效的方法是 pace UDP packets，我理解是控制 UDP 包的大小，以实现 smoother data transmission
    - 另一个方向是 generic segmentation offload (GSO)，找了篇 https://doc.dpdk.org/guides/prog_guide/generic_segmentation_offload_lib.html 和 https://lwn.net/Articles/752956/ ，大致意思是说允许发送超额的片段数据，提高传输效率
- 全员 QUIC 阶段
    - 上面提到了劣网环境 QUIC 有优势，所以在视频缓冲上验证了这个事实。提到了一个指标叫 Mean time between rebuffering (MTBR) 平均重缓存时间
    - Android 等移动设备上，会针对 TCP 行为评估下载带宽，而对 QUIC 会高估，最终反而造成卡顿
    - 另一个问题是 flow control 限流，这些之前适用于 TCP 的参数对 QUIC 不好用
- 扩展到 Ins 及其它
    - 提到了 0-RTT，见 https://blog.cloudflare.com/introducing-0-rtt/
    - 随着 Chrome https://blog.chromium.org/2020/10/chrome-is-deploying-http3-and-ietf-quic.html 和 Apple https://developer.apple.com/videos/play/wwdc2020/10111/ 的支持，以及 IETF 会在 2021 提出对于 QUIC 的 RFC，作者相信更多公司会踏上这条路，并坚信 QUIC is essential to unlocking innovative internet applications

---

总结，感觉是一篇科普性文章，提到了很多概念然后下一层去看一下是什么东西，整体上理解了 QUIC 的意图以及 Facebook 怎样一步步迁移到 QUIC 的，中间也提出了很多问题可能没有解决或者解决方案没有细说，但从作者提供的一部分数据来看 QUIC 确实是未来的一种不错的选择

附 InfoQ 的翻译：https://www.infoq.cn/article/RQXyiLAFcL8KNfLckN2U
