---
layout: page
mathjax: true
permalink: /2018/projects/p11/midterm/
---

## 项目进展报告

### 数据获取及预处理

1. 采用了阿里云天池比赛提供的数据集，包括了20000用户的完整行为数据以及百万级的商品信息，数据包含两个部分。 第一部分是用户在商品全集上的移动端行为数据（D），第二个部分是商品子集（P）。

```
len is 23291027

user_id           int64
item_id           int64
behavior_type     int64
user_geohash     object
item_category     int64
time             object
dtype: object

            user_id       item_id  behavior_type  item_category
count  2.329103e+07  2.329103e+07   2.329103e+07   2.329103e+07
mean   7.006868e+07  2.023214e+08   1.106268e+00   6.835397e+03
std    4.569072e+07  1.167440e+08   4.599087e-01   3.812873e+03
min    4.920000e+02  3.700000e+01   1.000000e+00   2.000000e+00
25%    3.019541e+07  1.014417e+08   1.000000e+00   3.690000e+03
50%    5.626942e+07  2.022430e+08   1.000000e+00   6.054000e+03
75%    1.166482e+08  3.035325e+08   1.000000e+00   1.027100e+04
max    1.424430e+08  4.045625e+08   4.000000e+00   1.408000e+04

missing count of geohash: 15911010
missing count of time: 0
```

```
len is 620918

item_id           int64
item_geohash     object
item_category     int64
dtype: object

            item_id  item_category
count  6.209180e+05  620918.000000
mean   2.004351e+08    6970.213167
std    1.191648e+08    3479.627372
min    9.580000e+02       2.000000
25%    9.357641e+07    4245.000000
50%    2.053761e+08    6890.000000
75%    3.054015e+08   10120.000000
max    4.045624e+08   14071.000000

missing count of geohash: 417508

```
其中缺失较多的是记录了位置的geohash，但考虑到地点标记可能有用，不剔除这些数据。


2. 对数据中的的购买做统计分析

![](https://github.com/oneoyz/DM_User-Purchase-Prediction-Based-on-Mobile-Recommendation-Algorithm/blob/master/output/count_day.png)
![](https://github.com/oneoyz/DM_User-Purchase-Prediction-Based-on-Mobile-Recommendation-Algorithm/blob/master/output/count_day_of_P.png)

如图，在12.11和12.12操作次数远高于平常日，其中属于商品子集P的操作次数中，12.12的值也非常高，因此将这两天的数据剔除。

### 数据分析与可视化

上面两张图可以反映除了双12期间的操作记录猛增，购买总体都是比较稳定的。为了挖掘不同操作的具体反映，选取了12.17和12.18两日48小时的操作进行呈现。
![](https://github.com/oneoyz/DM_User-Purchase-Prediction-Based-on-Mobile-Recommendation-Algorithm/blob/master/output/count_hour_17-18.png)

0-3分别对应浏览、收藏、加购物车、购买。浏览的操作次数要远远多于其他，这符合常理，同时也反映出一些符合实际的规律，比如操作的高峰在较为空闲的晚上，凌晨的时候操作次数最少。

![](https://github.com/oneoyz/DM_User-Purchase-Prediction-Based-on-Mobile-Recommendation-Algorithm/blob/master/output/count_hour_17-18_4.png)

单独看购买的操作，10:00-15:00和20:00-22:00是两个操作高峰时段。

![](https://github.com/oneoyz/DM_User-Purchase-Prediction-Based-on-Mobile-Recommendation-Algorithm/blob/master/output/user_count.PNG)

上图是选取了部分的用户展示，CTR是购买转换率：浏览/购买。可以看到用户10001082的CTR为0.019，且没有收藏和加购物车的行为，说明该用户行为决断。用户108461135操作次数非常多，但购买率不怎么高。

### 模型选取

1.分为两个思路，进行比较。第一个思路是按照规则筛选数据，根据12.18的数据对12.19的购买情况进行预测，如果12.18加入购物车当天没有购买，判断12.19其会购买该商品。初步F1值为7%左右，效果不理想。

2.第二个思路是构建特征，放入xgboost构造决策树，最后输出<用户-商品>对的购买判断。理想的特征分为三部分：用户特征、商品特征和用户-商品特征。目前只完成了部分特征的提取，没有完全构建好训练集和测试集。用户特征包含：用户活跃天数，用户操作统计（浏览、收藏、购买多少次），用户活跃的时间段。商品特征包含：商品的操作统计（被浏览、收藏、购买多少次）。用户-商品特征：该对出现的次数。

3.数据打标签，某一天作为验证或测试集，当天根据用户操作进行打标<用户-商品-种类-标签>，1为购买，0为未购买。

4.数据负采样，根据打标签划分之前的数据，分为已购买的对和未购买的对，比例为1:0.9。

### 挖掘实验的结果

完成了数据可视化，进行了数据清洗。可以见用户操作的次数整理，集中在12.11和12.12的数据特别多，因此后续训练先去除这两天的数据，以免影响模型。大多数购买发生在10点和21点，浏览白天持平晚上剧增，这可以作为一个规则加入筛选，比如当天加入购物车的时间为晚上到凌晨的，第二天购买的可能性会增加。


### 存在的问题

特征选取有一定难度，需要多想规则，或者用神经网络提取特征。

### 下一步工作

完善特征信息，将特征合为一个特征表，构建提升树模型。
