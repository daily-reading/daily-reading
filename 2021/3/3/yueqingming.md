基于 Python 的构建规则的大致流程：

1. Wait for data to be logged and populated in Hive
2. Write Presto/Hive queries until you find the spam pattern and evaluate for false positives. You’ll likely need to query many different tables in order to get the attributes you want
3. Translate Presto/Hive query results to Python to create a rule.
4. Enable the rule in “dry run” mode (a mode that only logs the action but doesn’t take it). This is necessary since you don’t want to risk bugs that might deactivate good users when translating to Python
5. Validate that the rule is catching the intended users/Pins/ips.

Guardian and the Improved Rule Creation Workflow：

相对于 Python 来说，分析过程和规则生成过程保持一致，减少开发周期。

离线向实时的转换

引擎和外部存储的连接

### 整体架构设计

![b35dd7a6da1d439c677caf2e9cf18a0a.png](evernotecid://7F01B206-A616-4EA4-AABF-0C5FA9B75355/appyinxiangcom/26825047/ENResource/p416)


