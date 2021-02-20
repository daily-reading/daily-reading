" in the case something is wrong, we have a quick way to rollback or revert the changes with the dropdown in the top right corner."
这个功能我喜欢，quick rollback，在处理live site incidet时候特别好用。

我们内部是用的自己公司的vs online， 基本上文章里说到的功能基本上都有了，git, test, deployment, CI, CD, team management全部都整合在一起，和文中不一样的是我们不在slack里通知，直接发邮件，也许teams里也可以设置？。github可能由于其特殊性，不同stage的canary都放在一个environment，我们是直接设了3层stage，第一层自己组用，第二层全公司和内测用户用，第三层rest of the world，其中最早deploy的几个（时差，非工作时间deploy)设置成了canary。每周一个branch fork，标记version number，然后每周一层的往前推进。这样就确保最大幅度的减少bug。需要hotfix的时候，也是一层一层推进，需要在前一层上verify过后才可以推进到下一层，并且leadership专门有个组叫shiproom，管理deployment schedule和hotfix。这么做的话优点是有一套规范化流程，缺点就是如果一个hotfix要从dev到prod最快也需要三天，所以需要尽量减少bug的可能，并且合理设置feature switch。
我们这套model也是一边用一边改，改到现在感觉用的还不错。