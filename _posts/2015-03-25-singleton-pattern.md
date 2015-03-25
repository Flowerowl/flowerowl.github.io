---
title: Singleton Pattern
author: Flowerowl
layout: post
permalink: /4003.html
views:
  - 667
categories:
  - Design Pattern
tags:
  - Design Pattern
  - Singleton Pattern
---

### 3种类型：
     1. 创建型
        *灵活方式创建对象*
             - 抽象工厂
             - 建造者
             - 工厂方法
             - 原型
             - 单例
     2. 结构型
         *将一种对象改装为另一种对象，或将小对象合成大对象*
             - 适配器
             - 桥接
             - 组合
             - 修饰器
             - 外观
             - 享元
             - 代理
     3. 行为型
         *关注做事过程，算法及对象间的交互*
             - 责任链
             - 命令
             - 解释器
             - 迭代器
             - 中介者
             - 备忘录
             - 观察者
             - 状态
             - 策略
             - 模板方法
             - 访问者


### 单例模式 Singleton Pattern

**概念：**

        一个类有且仅有一个实例，并且自行实例化向整个系统提供。
        单例模式可以保证系统中一个类只有一个实例而且该实例易于外界访问，从而方便对实例个数的控制并节约系统资源。

**场景：**

        一个系统中可以存在多个打印任务，但是只能有一个正在工作的任务；一个系统只能有一个窗口管理器或文件系统；一个系统只能有一个计时工具或ID(序号)生成器。
            
**示例：**

创建一个函数，返回今天的货币汇率（大多数情况下，汇率数据只获取一次即可，无须每次使用都获取一遍）。

 1. 


最简单的方法：创建模块时，把全局状态放在私有变量中，并提供用于访问此变量的公开函数。

    
    URL = 'http://forex.hexun.com/rmbhl/#zkRate'
    
    def get(refresh=False):
        if refresh:
            get.rates = {}
        if get.rates:
            return get.rates
    
        with urllib.request.urlopen(URL) as file:
            for line in file:
                line = line.rstrip().decode('utf-8')
                value = ...
    
                get.rates[key] = value
        return get.rates
    
    get.rates = {}
    get()

2.

baseclass
类每一次实例化后产生的过程都是通过new来控制的，所以通过重载new方法，可以很简单的实现单例模式。

    class Singleton(object):
        instances = {}
    
        def __new__(cls, *args, **kwargs):
            if cls not in cls.instances:
                cls.instances[cls] = super(Singleton, cls).__new__(cls, *args, **kwargs)
            return cls.instances[cls]
    
    
    class jijiyu(Singleton):
        def __init__(self, name):
            self.name = name
    
        def __str__(self):
            return self.name
    
    
    yu = jijiyu('yu')
    zhe = jijiyu('zhe')
    
    print id(yu), id(zhe)
    print yu, zhe


---------------------------------
╭─fuyu@Lazynight  ~/z/pj
╰─$ python singleton.py
4404881104 4404881104
zhe zhe

3.

decorators

    def singleton(_class):
        instances = {}
    
        def get_instance(*args, **kwargs):
            if _class not in instances:
                instances[_class] = _class(*args, **kwargs)
            return instances[_class]
        return get_instance
    
    
    @singleton
    class jijiyu(object):
        def __init__(self, name):
            self.name = name
    
        def __str__(self):
            return self.name
    
    
    yu = jijiyu('yu')
    zhe = jijiyu('zhe')
    
    
    print id(yu), id(zhe)
    print yu, zhe

----------------------------------

    ╭─fuyu@Lazynight  ~/z/pj
    ╰─$ python singleton.py
    4480058256 4480058256
    yu yu

4.
    metaclass

    class Singleton(type):
        instances = {}
    
        def __call__(cls, *args, **kwargs):
            if cls not in cls.instances:
                cls.instances[cls] = super(Singleton, cls).__call__(*args, **kwargs)
            return cls.instances[cls]
    
    
    class jijiyu(object):
        def __init__(self, name):
            self.name = name
    
        def __str__(self):
            return self.name
    
        __metaclass__ = Singleton
    
    
    yu = jijiyu('yu')
    zhe = jijiyu('zhe')
    
    
    print id(yu), id(zhe)
    print yu, zhe
    
----------------------

    ╭─fuyu@Lazynight  ~/z/pj
    ╰─$ python singleton.py
    4374202832 4374202832
    yu yu
