# Firecracker: Lightweight Virtualization for Serverless Applications

* Author: Alexandru Agache, Marc Brooker, Andreea Florescu, Alexandra Iordache, Anthony Liguori, Rolf Neugebauer, Phil Piwonka,
and Diana-Maria Popa, Amazon Web Services
* Link: https://www.usenix.org/system/files/nsdi20-paper-agache.pdf
* Presentation: https://www.youtube.com/watch?v=cwruf1ERAKM&feature=emb_logo

传统的 Serverless 方案会使用容器技术作为运行时环境，容器技术开销小，但在安全性和性能隔离方面要弱于虚拟机。为了解决这一问题，AWS 开发了 Firecracker，通过虚拟机来运行 Serverless 负载，并将 AWS Lambda 迁移到了 Firecracker。本文介绍了 Firecracker 的设计以及 Lambda 的迁移过程。
