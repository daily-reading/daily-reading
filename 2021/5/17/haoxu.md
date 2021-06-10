"
The default behavior of a polyrepo is isolation — that’s the whole point. 
The default behavior of a monorepo is shared responsibility and visibility — that’s the whole point."

我觉得还是case by case。Refactoring导致的regression可以通过增加test coverage和stage release environment来减少。对于一个大的repo，monorepo的弊端我觉得还是大于利益，code management的cost会大大增加，对于中小型的project，那肯定还是monorepo更有优势。

文中有一点挺有意思， you cannot avoid having the hard conversation。真是大实话。