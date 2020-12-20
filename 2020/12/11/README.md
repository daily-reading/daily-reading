# Modern storage is plenty fast. It is the APIs that are bad.

* Author: Glauber Costa
* Link: https://itnext.io/modern-storage-is-plenty-fast-it-is-the-apis-that-are-bad-6a68319fbc1a

现代 NVMe 设备改变了 I/O 操作的行为，让随机读的成本不再昂贵。作者基于 Intel Optane 设备设计了两个新的读文件 API，并且得到了不错的性能提升。本文讨论了如何设计 API 以适应更快的新型读写设备。
