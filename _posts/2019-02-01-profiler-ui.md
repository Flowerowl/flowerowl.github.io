---
title: Profiler UI
author: Flowerowl
layout: post
permalink: /4049.html
views:
  - 667
categories:
  - tensorflow
tags:
  - tensorflow
---

使用Profiler UI可视化分析Tensorflow耗时

https://github.com/tensorflow/profiler-ui

使用方法：

1. 在model中加入with context，生成profile文件：

```python
with tf.contrib.tfprof.ProfileContext(model_dir) as pctx:
     tf.estimator.train_and_evaluate(model, train_spec, eval_spec)
```

2. 使用profiler_ui在chrome中可视化profile文件

```
python profiler-ui/ui.py --profile_context_path=checkpoints/xxx/profile_100
```

![1](http://lazynight.me/wp-content/uploads/2019/02/profiler_ui.png)
