# 提交说明
- 编号：NO.25@20210710
- 文章：[Patterns for Managing Source Code Branches](https://martinfowler.com/articles/branching-patterns.html)

# 读书笔记
## 总体说明
> Modern source-control systems provide powerful tools that make it easy to create branches in source code. But eventually these branches have to be merged back together, and many teams spend an inordinate amount of time coping with their tangled thicket of branches.

开篇的描述很形象，分支的创建很容易，合并也很容易，但是分支模型要能够保证迭代的正常推进，保证上线的顺畅，就不那么容易了。正如微服务一样，创建一个微服务容易，微服务之间的互相调用也很容易，但是保证微服务的拆分能够支撑业务的演进，保证线上的稳定性就是一个极有挑战的事情了。

> Like most software patterns, few of them are gold standards that all teams should follow. Software development workflow is very dependent on context, in particular the social structure of the team and the other practices that the team follows.

分支管理模式没有万能的，需要结合当前团队的情况，进行选择和裁剪，核心是：解决问题。

## Base Patterns
> In thinking about these patterns, I find it useful to develop two main categories. One group looks at integration, how multiple developers combine their work into a coherent whole. The other looks at the path to production, using branching to help manage the route from an integrated code base to a product running in production.

模式分为两个维度，一个是为了集成，一个是为了上线。

- Source Branching
    - Create a copy and record all changes to that copy.
- Mainline
    - A single, shared, branch that acts as the current state of the product
Healthy Branch
    - On each commit, perform automated checks, usually building and running tests, to ensure there are no defects on the branch
    - There is an immense value in keeping the mainline healthy. If the mainline is healthy then a developer can start a new piece of work by just pulling the current mainline and not be tangled up in defects that get in the way of their work. Too often we hear people spending days trying to fix, or work around, bugs in the code they pull before they can start with a new piece of work.
    
作者在本节又一次强调mainline分支的健康可用的重要性。联想到斑马之前master开发模式非常的痛苦，线上代码特别的不稳定，为了上线需要做各种骚操作，风险极大，在周一为了上线admin服务拉了一个大群，互相协调，有时候不得不做cherry-pick，甚至为了上线需要额外开发代码保证稳定性。

## Integration Patterns
### Mainline Integration
- Developers integrate their work by pulling from mainline, merging, and - if healthy - pushing back into mainline
- One of the biggest benefits of using a mainline is that it simplifies integration.
### Feature Branching
- Put all work for a feature on its own branch, integrate into mainline when the feature is complete.
### Integration Frequency
- Smaller integrations mean less work, since there's less code changes that might hold up conflicts. But more importantly than less work, it's also less risk.
- What a lot of people don't realize is that a source control system is a communication tool. It allows Scarlett to see what other people on the team are doing. With frequent integrations, not just is she alerted right away when there are conflicts, she's also more aware of what everyone is up to, and how the codebase is evolving. We're less like individuals hacking away independently and more like a team working together.
### Continuous Integration
- Developers do mainline integration as soon as they have a healthy commit they can share, usually less than a day's work
- The rule of thumb is that "everyone commits to the mainline every day", or more precisely: you should never have more than a day's work sitting unintegrated in your local repository.
- partial feature
    - Hiding a partially built feature by hooking up a Keystone Interface last is often an effective technique.
    - If there's no way to easily hide the partial feature, we can use feature flags.
### Feature Branching VS Continuous Integration
- The difference between feature branching and continuous integration isn't whether or not there's a feature branch, but when developers integrate with mainline.
- Continuous Integration is near-impossible fit for occasional contributors to open-source work, but is a realistic alternative for commercial work. Teams should not assume that what works for an open-source environment is automatically correct for their different context.
- teams who want to do Continuous Integration must develop a strong testing regimen so they can be confident that mainline remains healthy even with many integrations a day.
- Feature Branching also discourages developers from making changes that aren't seen as part of the feature being built, which undermines the ability of refactoring to steadily improve a code base.

总结：这两者的区别不在于是不是用feature branch，区别在于开发者在什么时机合并到代码至mainline，一旦合并意味着立刻可以发布。

### Pre-Integration Review
> while pre-integration reviews can be a valuable practice, it's by no means a necessary route to a healthy code base, particularly if you're looking to grow a well-balanced team that isn't overly dependent on its initial leader.

总结：作者认为集成前的代码review，但是并不是一个强制必须做的事情。如果而且靠review与做到持续发布几乎没有关联。

### Integration Friction
- 【Review会导致集成减速，甚至大家不愿意做快速的持续的集成。】
    - > One of the problems of Pre-Integration Reviews, is that it often makes it more of a hassle to integrate. This is an example of integration friction - activities that make integration take time or be an effort to do.
- 【想想之前的admin，不知道是怎么活下来的，每天都很痛苦，周一上线从上午开始协调人，直至晚上才能上线！】
    - > Manual process are a common source of friction here, particularly if it involves coordination with separate organizations.
- 【大实话，90%的人并没有经历过真正的持续集成】
    - > I suspect much of the argument about the merits of Feature Branching and Continuous Integration is muddied because people haven't experienced both of these worlds, and thus can't fully understand both points of view.
- 【是团队信任，也是团队实力的问题，至少我感知到code review并不是必须的】
    - > But if I'm on a team where I trust the judgment of my colleagues, I'm likely to be more comfortable with a post-commit review, or cutting out the reviews entirely and rely on communal refactoring to clean up any problems. My gain in this environment is removing the friction that pre-commit reviews introduce, thus encouraging a higher-frequency of integration.

### Personal Thoughts on Integration Patterns
> Overall I much prefer to work on a team that uses Continuous Integration. I recognize that context is key, and there are many circumstances where Continuous Integration isn't the best option - but my reaction is to do the work to change that context. I have this preference because I want to be in an environment where everyone can easily keep refactoring the codebase, improving its modularity, keeping it healthy - all to enable us to quickly respond to changing business needs.

如作者所说的持续集成在绝大多数团队难以达成，但是并不意味着我们需要接受现状，我们通过明确问题所在，找到在环境之下更好的方案，如果能够消除掉当前环境的限制，那最好，如果不能，效率更高一点，大家工作更开心一点，何乐而不为

## The path from mainline to production release
保持mainline的健康和稳定是最为关键的
- > The mainline is an active branch, with regular drops of new and modified code. Keeping it healthy is important so that when people start new work, they are starting off a stable base. If it's healthy enough, you can also release code directly from mainline into production.
- > This philosophy of keeping the mainline in an always-releasable state is the central tenet of Continuous Delivery. To do this, there must be the determination and skills present to maintain mainline as a Healthy Branch, usually with Deployment Pipelines to support the intensive testing required.

Branching Patterns
- Release Branch
    - A branch that only accepts commits accepted to stabilize a version of the product ready for release.
- Maturity Branch
    - A branch whose head marks the latest version of a level of maturity of the code base.
- Environment Branch
    - Configure a product to run in a new environment by applying a source code commit.
- Hotfix Branch
    - A branch to capture work to fix an urgent production defect.
- Release Train
    - Release on a set interval of time, like trains departing on a regular schedule. Developers choose which train to catch when they have completed their feature.
- Release-Ready Mainline
    - Keep mainline sufficiently healthy that the head of mainline can always be put directly into production
  
## Other Branching Patterns
- Experimental Branch
    - Collects together experimental work on a code base, that's not expected to be merged directly into the product.
- Future Branch
    - A single branch used for changes that are too invasive to be handled with other approaches.
- Collaboration Branch
    - A branch created for a developer to share work with other members of the team without formal integration.
- Team Integration Branch
    - The obvious driver for using a team integration branch is for codebases that are being actively developed by so many developers that it makes sense to split them into separate teams.
    - A more important driver for team integration branches is a difference in desired integration frequency
    
### Looking at some branching policies
- Git-flow
- GitHub Flow
- Trunk-Based Development

## Final Thoughts and Recommendations
* 【很多时候，我们只是用分支来隔离共享代码，对于工作效率毫无提升】As I said at the beginning of this long piece: branching is easy, merging is harder. Branching is a powerful technique, but it makes me think of goto statements, global variables, and locks for concurrency. Powerful, easy to use, but easier to over-use, too often they become traps for the unwary and inexperienced. Source code control systems can help to control branching by carefully tracking changes, but in the end they can only act as witnesses to the problems.
* 【分支要正确的被利用】I'm not someone who says branching is evil. There are everyday problems, such as multiple developers contributing to a single codebase, where the judicious use of branching is essential. But we should always be wary of it and remember Paracelsus's observation that the difference between a beneficial drug and a poison is dosage.
* 【技术一定要跟替代品作比较，作权衡才能决定怎么被使用】
    - So my first tip for branching is: whenever you're considering using a branch, figure out how you are going to merge. Any time you use any technique, you're trading off against alternatives. You can't make a sensible trade-off decision without understanding all the costs of a technique, and with branching the piper exacts her fee when you merge.
    - Hence the next guideline: make sure you understand the alternatives to branching, they are usually superior. Remember Bodart's Law, is there a way to solve your problem by improving your modularity? Can you improve your deployment pipeline? Is a tag enough? What changes to your process would make this branch unnecessary? It's quite likely that the branch is, in fact, the best route to go right now - but is a smell that's alerting you to a deeper problem that should be tackled over the next couple of months. Getting rid of the need for a branch is usually a Good Thing.
* 【想办法提高你的集成次数！】Aim to double your integration frequency. (There's obviously a limit here, but you won't be near it unless you're in the zone of Continuous Integration.) There will be barriers to integrating more often, but those barriers are often exactly the ones that need to be given an excessive dose of dynamite in order to improve your development process.
* 【如果merge有问题，找到问题所在，可能是架构问题，可能是别的什么问题，千万不要陷入Stockholm Syndrome！！！！！】Since merging is the hard part of branching, pay attention to what's making merging difficult. Sometimes it's a process issue, sometimes its a failing of the architecture. Whatever it is, don't give in to Stockholm Syndrome. Any merge problem, especially one that causes a crisis, is a signpost to improving a team's effectiveness. Remember that mistakes are only valuable when you learn from them.

# 总体感受
大家理解的分支其实差异很大，大家理解的CI差异更大，还是回归到研发流程当中，当前的痛点在什么地方，解决它。这篇文章确实是分支模型的集大成者，强烈推荐！