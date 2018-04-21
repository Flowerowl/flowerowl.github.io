---
title: ChiMerge discretization algorithm
author: Flowerowl
layout: post
permalink: /4047.html
views:
  - 667
categories:
  - algorithm
tags:
  - algorithm
---

## 卡方分箱

#### 数据分箱优点：

*  离散特征的增加和减少都很容易，易于模型的快速迭代；
*  稀疏向量内积乘法运算速度快，计算结果方便存储，容易扩展；
*  离散化后的特征对异常数据有很强的鲁棒性：比如一个特征是年龄>30是1，否则0。如果特征没有离散化，一个异常数据“年龄300岁”会给模型造成很大的干扰；
*  逻辑回归属于广义线性模型，表达能力受限；
*  单变量离散化为N个后，每个变量有单独的权重，相当于为模型引入了非线性，能够提升模型表达能力，加大拟合；
*  离散化后可以进行特征交叉，由M+N个变量变为M*N个变量，进一步引入非线性，提升表达能力；
*  特征离散化后，模型会更稳定，比如如果对用户年龄离散化，20-30作为一个区间，不会因为一个用户年龄长了一岁就变成一个完全不同的人。当然处于区间相邻处的样本会刚好相反，所以怎么划分区间是门学问；
*  特征离散化以后，起到了简化了逻辑回归模型的作用，降低了模型过拟合的风险。可以将缺失作为独立的一类带入模型。将所有变量变换到相似的尺度上。

#### 无监督分箱

* 等频分箱

* 等距分箱

* 自定义分箱

#### 有监督分箱

* 卡方分箱

卡方分箱是自底向上的基于合并的数据离散化方法。它依赖于卡方检验:具有最小卡方值的相邻区间合并在一起,直到满足确定的停止准则。

分箱之后进行评估，使用woe-iv衡量变量的预测能力。

```python
from collections import Counter

import numpy as np
import pandas as pd
np.seterr(divide='ignore', invalid='ignore')

def chi_merge(data, attr, label, max_intervals):
    """卡方分箱
    """
    distinct_vals = sorted(set(data[attr]))
    labels = sorted(set(data[label]))
    empty_count = {label: 0 for label in labels}
    intervals = [[val, val] for val in distinct_vals]

    while len(intervals) > max_intervals:
        chis = []
        for i in range(len(intervals)-1):
            obs0 = data[data[attr].between(intervals[i][0], intervals[i][1])]
            obs1 = data[data[attr].between(intervals[i+1][0], intervals[i+1][1])]
            total = len(obs0) + len(obs1)
            count_0 = np.array([v for i, v in {**empty_count, **Counter(obs0[label])}.items()])
            count_1 = np.array([v for i, v in {**empty_count, **Counter(obs1[label])}.items()])
            count_total = count_0 + count_1
            expected_0 = count_total * sum(count_0) / total
            expected_1 = count_total * sum(count_1) / total
            chi = (count_0 - expected_0)**2 / expected_0 + (count_1 - expected_1)**2 / expected_1
            chi = np.nan_to_num(chi)
            chis.append(sum(chi))

        min_chi_index = np.argmin(chis)
        tmp = intervals[min_chi_index] + intervals[min_chi_index+1]
        del intervals[min_chi_index: min_chi_index+2]
        intervals.insert(min_chi_index, [min(tmp), max(tmp)])

    return intervals
```

