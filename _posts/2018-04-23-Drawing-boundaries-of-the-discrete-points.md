---
title: Drawing boundaries of the discrete points
author: Flowerowl
layout: post
permalink: /4043.html
views:
  - 667
categories:
  - algorithm
tags:
  - algorithm
---

### Intro

针对POI或者AOI，给点位画边界。

### Steps

1. 聚类, 将相邻点位归类

2. 构造凸包

3. 默认边界点个数较少，边界不平滑，填充点使其平滑

4. 画边界

![boundary](http://lazynight.me/wp-content/uploads/2018/04/boundary.png)

![boundary2](http://lazynight.me/wp-content/uploads/2018/04/boundary2.png)

### Code 

<https://github.com/Flowerowl/boundary>

### Todo

边界衔接出现不平滑情况
