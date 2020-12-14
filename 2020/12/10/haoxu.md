# The End of Cross-Platform as We Know It

* Author: Pen Magnet
* Link: https://medium.com/swlh/the-end-of-cross-platform-as-we-know-it-dad658d96b8
* 翻译: https://mp.weixin.qq.com/s/4TxAX0y8xkXl1ZovFmXBkQ

Cross platform的pro无非就是一个code base，run everywhere，减少开发的成本。但同时也会带来很多的con，我的理解是附带的overhead，这些额外的expense在曾经可能并不是那么显著，但本文从各个角度，阐述了一些观点
1. IOS is dominating。所以提供的各种support也比Android好，所以....开发IOS的回报比android多？话说最新的MAC OS update是不是把app store的APP在MAC OS下也可以运行了？我的理解是，当一个platform处于dominating的状态时，为了追求cross platform，而放弃native app的优势，可能有点得不偿失。
2. cross platform是饮鸩止渴。
3. it sucks. 维护性太差，package对于API的包装也可能不能满足需求。额外附带的packages可以算是累赘？导致最后的binaries会比native app大很多。甚至有PERF的问题，并且这种问题很可能无法通过有效的优化而消除。
“Most cross-platform enthusiasts are college kids who created their first software in those libraries, and can’t forget their first love.”
LOL
4. cross platform毕竟是一个middle layer，任何底层的变化都可能会导致broken version，并且拥有平台的公司也不会对这些framework负责。并且对于这些framework已有的bug，无法及时修补的话，会影响到基于这些framework的app。

conclusion
我觉得还是要基于case by case。功能简单，并且不太可能会有后续大量代码量的小APP，用cross platform无可厚非。但enterprise app，还是用native的好，一方面是perf，另一方面是a11y和security都会有比较好的支持。