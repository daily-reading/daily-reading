原文：https://thesephist.com/posts/complexity-conservation/

这篇文件讲的是对复杂性的讨论，作者认为，系统复杂性总会存在于某个地方。复杂性不能通过良好的设计消除，相反，好的设计应该将复杂性隔离到系统的各个地方。

complexity needs to live somewhere – we cannot simply eliminate complexity in the problem domain with good engineering; instead, good engineering isolates complexity to the parts of a system where that complexity is easiest for the team to manage over time.

作者主张应该将问题中的复杂性推向软件组件之间的接口，而不是试图将其包含和抽象化为理想的可重用部分。好的接口不是越简单越优雅越好，而是可以准确反映问题的复杂性，同时隐藏不必要的细节。

The best software interfaces aren’t as simple or elegant as possible. The best interfaces accurately reflect the complexity of the problem, while hiding away unnecessary detail.
