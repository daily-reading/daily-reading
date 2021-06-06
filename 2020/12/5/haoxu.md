"So in other words, out of five backup/replication techniques deployed none are working reliably or set up in the first place. We ended up restoring a six-hour-old backup."
这让我想起了很久以前我自己的PERSONAL PROJECT，明明做了BACKUP，但当真正要用到的时候才发现做的BACKUP设置有问题都没法用。
solution? 对于大的web service, regular disaster recovery drill is necessary。我们现在每周都会在我们的PPE上冒个烟，确保在极端情况下至少availability不是问题。

文中也说了
“Losing production data is unacceptable”。