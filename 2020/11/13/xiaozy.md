# How Facebook is bringing QUIC to billions

> 原文地址：https://engineering.fb.com/2020/10/21/networking-traffic/how-facebook-is-bringing-quic-to-billions/

## QUIC 的优势

1. QUIC + HTTP 3.0 是 TCP + HTTP 2.0 的替代品，前者性能更好，原因是 QUIC 做到了（在多路复用时）流之前完全互相独立，从而避免了 TCP `Head of Line Blocking` 问题
2. QUIC 采用了更先进的丢包恢复技术，从而在弱网络环境下表现的更稳定
3. TCP 升级困难，原因是防火墙架设了 TCP 包的数据格式，而 QUIC 通过对包加密避免了这一问题

## 落地路径

1. Facebook 内存 -> Facebook app API (没有静态资源) -> Facebook App 静态资源 -> Facebook APP 所有内容 -> Instagram
   1. 推进过程中遇到过一个问题，应用程序原来对 TCP 做的一些启发式的优化（根据实际的网络状况调整请求方式和数量）在 QUIC 中引起了副作用，导致 app feed 流加载延迟变大
   2. 开发了配套的观测工具
