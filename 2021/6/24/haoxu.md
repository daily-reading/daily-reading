Nothing fancy, just feature switch.

Notes
1. "Have a first responder person in each team that is focused on reviewing pull requests quickly to unblock the progress of their own team or other teams."
大多数人都不太愿意花时间在CODE REVIEW上， 一方面很花时间，一方面可能吃力不讨好。但CR也是一个可以提升VISIBILITY很有用的办法，最好让组里经验比较老的人来做这个FIRST RESPONDER。

2. SPIKE branch和new branch的办法，有个问题就是如果这个FEATURE和其他的COMPONENT有关，那么很可能会频繁的resolve conflicts。我的经验还是直接MERGE到MAIN里面，BEHIND FS。

3. Testing feature，disable flag in code是怎么做到的？FRONTEND的可以用QUERY PARAMETER来TOGGLE，BACKEND可以用manifest或者其他CONFIG FILE来控制。

4. Different shipping strategies。coverage和怎么flight这个feature。

conclusion:
feature flag的好处是可以快速的turn off feature，in case of regression。在ONCALL时特别有效，这样就可以增加产品的容错率。但同时也会带来一些side affect，比如增加了dependency，在feature flag hub出问题的时候，连带着prod也会一起出问题。然后产品的代码量会变大，因为要host不同情况下的代码。