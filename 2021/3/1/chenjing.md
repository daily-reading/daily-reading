## We Burnt $72K testing Firebase + Cloud Run and almost went Bankrupt
> 原文地址：<https://www.infoq.cn/article/LK8dbL21b82Px8hpmkxr>

这是一个创业小团队因为一次错误的上云决策，差点破产、最后幸存下来并从中汲取教训的故事。

错误的原因在于：
1. 在云上部署了存在缺陷的算法
2. 使用默认选项部署 Cloud Run
3. 在不完全了解的情况下使用 Firebase

当然，GCP说明文档也存在问题：
1. 自动将 Firebase 账户升级为付费账户
2. 所谓的计费“限额”根本不存在，预算管理至少延迟了一天
3. 由于我们的账户一直没有实际支付，所以 GCP应该先根据账单信息收取 100美元的费用，并在无法继续付款后停止服务。但实际情况并非如此，后来我弄清了原因，问题仍然跟用户这方无关。
4. 不只是 Billing 功能，就连 Firebase 仪表板也要超过 24 个小时才能正常更新

万幸经过与谷歌的交涉以后，作者苟过去了。