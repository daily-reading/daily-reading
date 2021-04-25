这标题第一反应是best friend forever....难道是起着名字的人故意留的梗吗...

关于DATA FORMATE的问题其实可以通过统一前后端的INTERFACE来实现（但无法避免有些人偶尔会弄乱造成conflict）。但BFF这个PATTERN有几点倒是让我挺感兴趣。

首先是data query 和 data transform分开了，这样microservice只管抓和算raw data就行了，decouple了。
还有个就是我最近要做的optimization，对于一些前期赶进度没做优化的API，后期如果想做优化，只要新建一个新的BFF就行了，不用在原来的flow上面做大改。
还有就是那个关于device的，这样就不用给mobile做专门的API了，MOBILE也可以根据自己的需求减小PAYLOAD。

Q1， 有一个问题还是没解决，如何统一和同步前后台关于数据格式interface的一致性。这个我到现在也还没想到很好的解决方法，只能在前后端的INTERFACE上分别多写点注释。。。。。
Q2， 有点没看懂，“There is no need to have a separate BFF for iOS and another for Android.” 为啥上面的那个图的例子用的就是IOS BFF和android BFF....