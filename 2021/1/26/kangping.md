# How machine learning powers Facebook’s News Feed ranking algorithm

* Author: Akos Lada, Meihong Wang, Tak Yan
* Link: https://engineering.fb.com/2021/01/26/ml-applications/news-feed-ranking/

Facebook 每天要为二十多亿人推荐相关性内容，这是一套庞大、复杂的 Ranking 系统。本文概略性的介绍了这个 Ranking 系统的整体架构与核心算法。


# Overview
Facebook needs to rank News Feed for each user. They try to model "meaningful interaction" and quality content by using multitask learning on neural networks, embeddings, and offline learning systems.

# Math setup
Ranking variable is defined as $Y_{ijtk}$ where $i$ is the id of the content generator. $j$ is the id of the ranking-targeted user. $t$ is time and $k$ is the type of the interaction. (likes/comment shares etc). And $Y_{ijtk}$ needs to be aggregate to a single value $V_{ijt}$, which is then used for ranking. Facebook uses a large size of feature $x_{ijtc}$ to predict $Y_{ijtk}$. The target $Y_{ijtk}$ is estimatede via surveying the users. And the aggreation of $Y_{ijtk}$ to $V_{ijt}$ is also estimated via survey data.

# System design
On average, FB needs to run ranking for 1000 posts per day for more than 2 billion users. All of these need to be done in real time with latency on the order of minutes if not seconds. All these was done in a feed aggregator. 

The way the aggregator works is by the following steps

1. Query inventory. Collects all available posts since last user login. Add on top, previous unseen posts, and posts with heavy actions among friends.
2. Features for are calculated in parallel on multiple machines. It is then inferenced using a pre-learned embedding neural net to get score of $Y_{ijt}$
3. Calculate $V_{ijt}$. FB uses a multiple pass approach. Pass 0 is lightweight, it trims eligible posts to about 500. Pass 1 is main scoring pass, and full rank is produced using the socre. Pass 2 is a contextual pass, which sounds like a rule base selecting process, and it is to guanrantee content diversity.
4. $V_{ijt} = W^T Y_{ijt}$ i.e. this is a linear step. 





