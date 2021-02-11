PullRequest

* Author: Martin Fowler
* Link: https://martinfowler.com/bliki/PullRequest.html

Pull Request 实现了 Feature Branch 和集成前 Code Review 两种 Pattern。Feature Branch 对于开发人员来说非常方便，但对于团队来说，它限制了集成的频次，导致了复杂的 CL，所以即使使用功能分支（PR 模式），团队也应该高频地进行主干集成。鉴于 PR 模式是如此的流行，以至于大部分团队并没有认真思考过是否适合，Martin Fowler 认为这个模式可能被错误使用了。
