# Things I Learnt from a Senior Software Engineer
作者刚在bloomberg工作一年，从senior里学到了很多，决定每年记录下来
- Further, in my team culture it’s not frowned upon to “snoop behind” people writing code.  感觉这点还是挺难得的，很多人会觉得不舒服
- 有兴趣就可以随时凑上去看
## Writing code
### How to name stuff
- Meaningful naming
- dont creat god class
- 不太认同命名为x，y，z
### Legacy code and the next developer
- 文档以及代码注释的重要性
- 如何让自己的代码易读
> “The main value in software is not the code produced, but the knowledge accumulated by the people who produced it.” - Li
### Atomic commits
- 一次commit干一件事
### Becoming confident about deleting shit code
- Delete code you’ll never reach, and be cautious with code you don’t understand.
### Code Reviews
- Don’t approve code till I understand how it works.
## Design
### Designing with maintenance in mind
- First part is not deprecating old stuff, always adding more
- The second part is designing with an end goal in mind.
## When things go wrong
> For when things go wrong, and they will, the golden rule is minimizing client impact.
- fisrt, roll back
- 再去fix bug
- 找bug深度优先搜索效率太低，宽度优先搜索
- 先查找环境问题
- line by line检查代码是最后的排查方法
- 总结排查bug一个小时以上的经历


# Things I Learned to Become a Senior Software Engineer
## Learning what people around me are doing
> Since we’re not in a closed system, it makes sense to better understand the job of the product managers, the sales people, and the analysts. 
- 自己的工作只是其中一环，能还原出全貌更能明白自己在做些什么
## Learning good habits of mind
- 发生Y就能立马想到X
### Strategies for making day-to-day more effective
- Never leave a meeting without making the decision / having a next action
- Decide who is going to get it done. Things without an owner rarely get done. (什么事情没有一个负责人基本都会无疾而终)
- Document design decisions made during a project  （RFC就是一个很好的形式）
## Acquiring new tools for thought & mental models
- I look for new tools only when I get stuck on something, or when I find out my abstractions and design decisions aren’t working well. 工作和学习相结合，会更加有效率
- 读一些咨询博客，但是效果没有上面好
- 通过学习不同的语言
## Protect your slack
- 磨刀不误砍柴工
> When there is slack, you get a chance to experiment, learn, and think things through. This means you get enough time to get things done.
> When there is no slack, deadlines are tight, and all your focus goes into getting shit done. 慢悠悠的，老板不会打人吗
- 举了一个例子，发现一个bug，然后就去stackoverflow上找解决方法去尝试，最后其实并没有真正弄懂原因
## Ask Questions
> you can’t judge a question as dumb until you figure out the answer.
- How did you find out X?
## Force multipliers
- 提到了一个团队文化。团队会关注团队产出，而不是个人产出  
## On Ownership
> oh well, I tried. Someone will reply someday and then we can continue the conversation
- 这句话也太真实了
> : always follow up, and if you own a task, it’s your responsibility to move it forward. Don’t get stuck playing the role, actually get shit done: be it by delegating or doing it yourself.
- 很多事情都是提了一个很好的构思，然后没有推进了。
