## It's time to say goodbye to the GPL
>  原文地址：<https://martin.kleppmann.com/2021/04/14/goodbye-gpl.html>

the defining feature of the GPL family of licenses is the concept of copyleft, which states (roughly) that if you take some GPL-licensed code and modify it or build upon it, you must also make your modifications/extensions (known as a “derivative work”) freely available under the same license.

GPL许可证家族的定义特征是版权的概念,即如果你需要一些GPL许可的代码和修改它的或是建立它,你必须让你的修改/扩展(称为“派生作品”)免费提供相同的许可证。这就导致了GPL版本的源代码不能被合并到封闭源代码软件中。

In the 2020s, the enemy of freedom in computing is cloud software (aka software as a service/SaaS, aka web apps) – i.e. software that runs primarily on the vendor’s servers, with all your data also stored on those servers. 

在21世纪20年代，计算自由的敌人是云软件(又名软件即服务/SaaS，又名网络应用程序)——即主要运行在供应商服务器上的软件，你的所有数据也存储在这些服务器上。

The 1990s problem of not being able to customise or extend software you use is aggravated further in cloud software. With closed-source software that runs on your own computer, at least someone could reverse-engineer the file format it uses to store its data, so that you could load it into alternative software (think pre-OOXML Microsoft Office file formats, or Photoshop files before the spec was published). With cloud software, not even that is possible, since the data is only stored in the cloud, not in files on your own computer.

上世纪90年代无法定制或扩展您使用的软件的问题在云软件中进一步恶化。对于在你自己的电脑上运行的闭源软件，至少有人可以对它用来存储数据的文件格式进行逆向工程，这样你就可以把它加载到其他软件中(想想ooxml之前的Microsoft Office文件格式，或者规范发布之前的Photoshop文件)。而在云软件中，这甚至是不可能的，因为数据只存储在云中，而不是存储在你自己电脑上的文件中。

Other problems with GPL-family licenses

You can force a company to make their source code of a GPL-derived software project available, but you cannot force them to be good citizens of the open source community (e.g. continuing to maintain the features they have added, fixing bugs, helping other contributors, providing good documentation, participating in project governance). What worth is source code that is just “thrown over the wall” without genuine engagement in the open source project? At best it’s worthless, and at worst it’s harmful because it shifts the burden of maintenance to other contributors of the project.

我们需要人们成为开源社区的优秀贡献者，这是通过建立正确的激励机制和受欢迎来实现的，而不是通过软件许可。

此外,gpl系列许可证的一个实际问题是它们与其他广泛使用的许可证不兼容

基于以上原因，作者认为坚持GPL和copyleft已经没有意义了。他希望开源项目采用一个宽松的许可(例如MIT, BSD, Apache 2.0)，然后把你的精力集中在真正能改变软件自由的事情上:消除云软件的垄断效应，发展可持续的商业模式，让开源软件蓬勃发展，推动将软件用户的利益置于供应商利益之上的监管。