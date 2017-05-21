---
title: Opinion Mining
author: Flowerowl
layout: post
permalink: /4036.html
views:
  - 667
categories:
  - note
tags:
  - note
---

最近要实现用户评论总结的功能，也可以说是观点提取， 效果类似于点评的"大家认为"。

![doc](http://lazynight.me/wp-content/uploads/2017/05/dianping.png)

花了两周调研、一周编码。

### Steps:

1. 抓取评论, 划分为训练集、测试集。

2. 使用训练集训练word2vec模型，保存词向量。

3. 针对测试集提取候选标签，并向量化。候选标签的构造依赖于StanfordCoreNlp的词性依赖功能，候选标签由三元组<主题词，ADVs，修饰词>构成。

![doc](http://lazynight.me/wp-content/uploads/2017/05/word_dep.jpeg)

4. 针对候选标签向量进行聚类。

5. 选出每个类簇中距离中心点最近的标签作为summary。

### Code:

<https://github.com/Flowerowl/opinion_mining>

### References：

1. Mining Opinion Features in Customer Reviews - Minqing Hu and Bing Liu

2. What Drives Consumer Choices? Mining Aspects and Opinions on Large Scale Review Data using Distributed Representation of Words

3. Building a Sentiment Summarizer for Local Service Reviews

4. Sentiment Analysis and Opinion Mining - Bing Liu

5. 用户评论中的标签抽取以及排序 - 李丕绩
