# 提交说明
- 编号：NO.19@20210504
- 文章：[Get better at programming by learning how things work](https://jvns.ca/blog/learn-how-things-work/)

# 读书笔记

## 摘抄


### NO.1
When I’m programming and I’m missing a key concept about how something works, it doesn’t always show up in an obvious way. What will happen is:
- I’ll have bugs in my programs because of an incorrect mental model
- I’ll struggle to fix those bugs quickly and I won’t be able to find the right questions to ask to diagnose them
- I feel really frustrated
- I think it’s actually an important skill just to be able to recognize that this is happening: I’ve slowly learned to recognize the feeling of “wait, I’m really confused, I think there’s something I don’t understand about how this system works, what is it?”

很精彩的一段描述，你必须’知道’ 你不知道的是什么，即问出正确的问题，很多时候，问题一直得不到解决，是因为你根本不知道是什么困住了你】

### NO.2
- Being a senior developer is less about knowing absolutely everything and more about quickly being able to recognize when you don’t know something and learn it. Speaking of being a senior developer…
- It can feel bad to realise that you really don’t understand how a system you’ve been using works when you have 10 years of experience (“ugh, shouldn’t I know this already? I’ve been using this for so long!“), but it’s normal! There’s a lot to know about computers and we are constantly inventing new things to know, so nobody can keep up with every single thing.

所谓所到老，学到老，遇到搞不清楚的事情，搞清楚就可以了，完全没有必要有任何的挫败感。而且越为资深，越要认识到你不可能穷尽一切知识了，你需要做的是：知道你不知道什么。

### NO.3
When I notice I’m confused, I like to approach it like this:
- Notice I’m confused about a topic (“hey, when I write await in my Javascript program, what is actually happening?“)
- Break down my confusion into specific factual questions, like “when there’s an await and it’s waiting, how does it decide which part of my code runs next? Where is that information stored?”
- Find out the answers to those questions (by writing a program, reading something on the internet, or asking someone)
- Test my understanding by writing a program (“hey, that’s why I was having that async bug! And I can fix it like this!“)
The last “test my understanding” step is really important. The whole point of understanding how computers work is to actually write code to make them do things!

虽然并无法一语中地的明确你是被什么问题给困住了，要尝试多问具体问题，并依次解决问题，最后，落地为代码是学习的最佳方式

### NO.4
- just learning a few facts can help a lot
- connect new facts to information you already know
- asking a person yes/no questions
    - When I ask very open-ended questions like “how does X work?”, I find that it often goes wrong in one of 2 ways:
      - The person starts telling me a bunch of things that I already knew
      - The person starts telling me a bunch of things that I don’t know, but which aren’t really what I was interested in understanding
    - asking yes/no questions isn’t always easy
      - It never quite feels good to learn that my mental model was totally wrong, even though it’s incredibly helpful information. Asking this kind of really specific question (even though it’s more effective!) puts you in a more vulnerable position than asking a broader question, because sometimes you have to reveal specific things that you were totally wrong about!
- asking the computer
    - asking the computer is a skill
- be aware of what you still don’t understand
    - especially as you get more senior, it’s important to be aware of what you don’t know!
    
## 总结
在我看来，这是一篇关于认知的文章，最近一年多，慢慢的认识并认识到【活到老，学到老】这句陈词滥调的含义：
- 首先这句话是一句自我激励
- 其次这也是不得不面对的现实，尤其是在当今这个时代，再尤其是软件这个行当
- 最后这个句话其实是跟保持好奇心本质上是一个意思