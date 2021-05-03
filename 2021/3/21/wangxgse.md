# 提交说明
- 编号：NO.18@20210502
- 文章：[Conservation of complexity](https://thesephist.com/posts/complexity-conservation/)
- 本文 [Conservation of complexity](https://thesephist.com/posts/complexity-conservation/) 是一篇读后感，原始文章是[complexity has to live somewhere
](https://ferd.ca/complexity-has-to-live-somewhere.html)，本次读书笔记的内容包括这两篇文章。

# 读书笔记
## complexity has to live somewhere
- 作者：Fred Hebert: [complexity has to live somewhere](https://ferd.ca/complexity-has-to-live-somewhere.html)
- 复杂性无处不在，或者说总要待在哪个地方，你是如何清除掉复杂性的，只能做一定的挪移（shift around）
    - > Complexity has to live somewhere. It's always a part of people solving problems, whether you realize it or not.
    - > Focusing on simplicity is fraught with peril because complexity can't be removed: it can just be shifted around. If you move it out of your code, where does it go?
-  复杂性无处不在，可能在代码中，可能在流程中，可能在文档中，可能在 '口口相传' 中
    - > Complexity has to live somewhere. If you are lucky, it lives in well-defined places. In code where you decided a bit of complexity should go, in documentation that supports the code, in training sessions for your engineers. You give it a place without trying to hide all of it. You create ways to manage it. You know where to go to meet it when you need it. If you're unlucky and you just tried to pretend complexity could be avoided altogether, it has no place to go in this world. But it still doesn't stop existing.
    - > With nowhere to go, it has to roam everywhere in your system, both in your code and in people's heads. And as people shift around and leave, our understanding of it erodes.
    - > Complexity has to live somewhere. If you embrace it, give it the place it deserves, design your system and organisation knowing it exists, and focus on adapting, it might just become a strength.
## Conservation of complexity
- 不仅仅可以隔离和管理好复杂性，还可以使得我们的软件更可能的'软' (easiest to change and adapt as the real world changes around us, and as our requirements change)
    - > Good engineering, then, isolates and marshals complexity inherited from the problem to the parts of a system where it’s easiest to manage. But more than simply the ease of managing complexity now, I think we should optimize for encoding problem complexity in places that are easiest to change and adapt as the real world changes around us, and as our requirements change.
- 抽象的目的并不仅仅为了复用，更是为了解决复杂性问题
    - > I think we should try to push the conserved complexity from our problems towards the interfaces between our software components, rather than try to contain and abstract it away into idealized reusable parts.
- 最好的软件不是simple，而是如实(accurately)反应了现实的复杂性
    - > Let’s not shy away from complex interfaces, if the problem we’re solving is itself complex. The best software interfaces aren’t as simple or elegant as possible. The best interfaces accurately reflect the complexity of the problem, while hiding away unnecessary detail.