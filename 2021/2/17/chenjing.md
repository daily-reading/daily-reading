## Why Is Naming Things Hard?
> 原文地址：<https://neilkakkar.com/why-is-naming-things-hard.html>

- 一个好的名字是一个信息块，它编码了关于事物功能的重要信息。
- 命名是对功能信息的有损压缩，良好命名的目标是最小化这种损失。

#### Generating best practices
1. Make functions do one thing 当一个函数执行多个任务时，它的名称需要编码更多的信息
2. Keep functions small 长函数就像做很多事情的函数一样，需要将很多信息编码到名称中
3. Avoid meaningless names 避免无意义命名
4. Avoid too abstract names 避免过于抽象的命名
5. Be consistent 

#### Disrupting best practices
有时需要打破上述的常规规则，比如你在写下一段不希望别人碰的神秘代码的时候，这时候不采用驼峰法而采用下划线，并配上注释是个不错的选择。

#### Precision with words
口语是不精确的，而代码必须精确，因为我们试图把一种精确的语言压缩成一种不精确的语言，这使得搜索正确的英语单词更加困难。
