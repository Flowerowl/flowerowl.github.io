---
title: 模型可解释工具what if tool
author: Flowerowl
layout: post
permalink: /4054.html
views:
  - 667
categories:
  - note
tags:
  - note
---

很多时候面对深度学习模型的预测结果，无法直接分析相关性，纯黑盒。针对深度模型可解释的工具也越来越多，这是官方的一个，可以较好的指导我们理解模型运行的蛛丝马迹。

https://pair-code.github.io/what-if-tool/index.html#demos

工具强制要求使用tfrecord格式数据，且需要tf-serving做服务

目前tensorboard集成what-if-tool，右上角选择即可

搭建本地运行环境

wit + tf-serving，使用tf-serving docker镜像的方式运行，serving使用方式见readme： https://github.com/tensorflow/serving

![image](wp-content/uploads/2020/04/20.png)

![image](wp-content/uploads/2020/04/21.png)