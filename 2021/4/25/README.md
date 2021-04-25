# Zanzibar: Google’s Consistent, Global Authorization System

* Author: Ruoming Pang, Ramon Caceres, Mike Burrows, Zhifeng Chen, Pratik Dave, Nathan Germer, Alexander Golynski, Kevin Graney, and Nina Kang, Google; Lea Kissner, Humu, Inc.; Jeffrey L. Korn, Google; Abhishek Parmar, Carbon, Inc.; Christopher D. Richards and Mengzhi Wang, Google
* Link: https://www.usenix.org/conference/atc19/presentation/pang

Zanzibar 是 Google 的统一认证系统，它试图解决「某个人是否可以访问某个资源」这一问题。本质上 Zanzibar 是一个 Tuple 存储系统。本文分享了 Google 是如何对「身份认证」进行建模，以及如何设计一个大型的 Tuple 存储系统以支持超大规模、全球一致性、低延迟的身份鉴权问题。
