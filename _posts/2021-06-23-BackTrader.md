---
title: BackTrader
author: Flowerowl
layout: post
permalink: /4057.html
views:
  - 667
categories:
  - note
tags:
  - note
---

# 1. 简介

Backtrader 是 2015 年开源的 Python 量化回测框架（支持实盘交易），功能丰富，操作方便灵活；

品种多：股票、期货、期权、外汇、数字货币；

周期全：Ticks 级、秒级、分钟级、日度、周度、月度、年度；

速度快：pandas 矢量运算、多策略并行运算；

组件多：内置 Ta-lib 技术指标库、PyFlio 分析模块、plot 绘图模块、参数优化等；

超灵活：即可以随意搭配组件，又支持扩展自己开发的功能，想怎么玩就怎么玩；

社区活跃、帮助文档齐全。

# 2. 主要模块
	1. Datafeeds 数据模块
		a. Csvdatabase
		b. Pandasdata
		c. Yahoofinancedata
		d. …
	2. Broker 经纪商模块
		a. cash初始资金
		b. Commission 手续费
		c. Slippage 滑点
		d. …
	3. Sizers 仓位模块
	4. Strategy
		a. Next 主策略函数
		b. Nofity_order 订单通知、打印交易信息
	5. Indicators 指标模块
		a. Sma
		b. Ema
		c. ta-lib指标
		d. …
	6. Orders 订单模块
		a. Buy
		b. Sell
		c. Cancel 取消
		d. Close 平仓
	7. Cerebro 
		a. Adddata
		b. Addsizer
		c. Addstrategy
		d. Addanalyzer
		e. Addobserver
		f. Plot
		g. Run
	8. Analyzers 策略分析模块
		a. Annualreturn 年化收益
		b. Sharperatio 夏普比率
		c. Drawdown 回撤
		d. Pyfolio 分析工具
		e. …
		
	9. Observers 观测器模块
		a. Broker 资金、市值曲线
		b. Trades 盈亏曲线
		c. Buysell 买卖点
		d. …

# 3. 执行流程

	1. 准备回测数据
	2. 编写策略
	3. 实例cerebro
	4. 设置参数：初始资金、交易税费、滑点、成交量限制等；
	5. 设置分析指标
	6. Run
	7. 回测结果

	pic1
	
	
# 4. 数据格式
**Lines 列**

Backtrader 将数据表格的列拆成了一个个 line 线对象，一列→一个指标→该指标的时间序列→一条线 line。

Backtrader 默认情况下要求导入的数据表格要包含 7 个字段：'datetime'、 'open'、 'high'、 'low'、 'close'、 'volume'、 'openinterest' ，这 7 个字段序列就对应了 7 条 line 。其实给列赋予“线”的概念也很好理解，回测过程中用到的时间序列行情数据可视化后就是一条条曲线：close 曲线、 open 曲线、high 曲线 ......

Backtrader 支持导入各式各样的数据：第三方网站加载数据（Yahoo、VisualChart、Sierra Chart、Interactive Brokers 盈透、OANDA、Quandl）、CSV 文件、Pandas DataFrame、InfluxDB、MT4CSV 等，其中最基础或最常见的就是导入 CSV 和导入 DataFrame了。


pic2

# 5. 滑点管理

在实际交易中，由于市场波动、网络延迟等原因，交易指令中指定的交易价格与实际成交价格会存在较大差别，出现滑点。为了让回测结果更真实，在交易前可以通过 brokers 设置滑点，滑点的类型有 2 种：百分比滑点和固定滑点。

注：在 Backtrader 中，如果同时设置了百分比滑点和固定滑点，前者的优先级高于后者，最终按百分比滑点的设置处理。

### 5.1 百分比滑点

假设设置了 n% 的滑点，如果指定的买入价为 x，那实际成交时的买入价会提高至 x * (1+ n%)；同理，若指定的卖出价为 x，那实际成交时的卖出价会降低至 x * (1- n%)，下面时将滑点设置为 0.01% 的例子：


### 5.2 固定滑点
假设设置了大小为 n 的固定滑点，如果指定的买入价为 x，那实际成交时的买入价会提高至 x + n ；同理，若指定的卖出价为 x，那实际成交时的卖出价会降低至 x - n，下面时将滑点固定为 0.001 的例子：
cerebro.broker.set\_slippage\_fixed(fixed= 0.001)


### 5.3 其他设置
除了用于设置滑点的 slip\_perc 和 slip\_fixed 参数外，broker 还提供了其他参数用于处理价格出现滑点后的极端情况：

* slip\_open：是否对开盘价做滑点处理,  是否使用下个bar的开盘价来计算滑点，该参数在 BackBroker 类中默认为 False，在 set\_slippage\_perc 和set\_slippage\_fixed 方法中默认为 True；
* slip\_match：是否将滑点处理后的新成交价与成交当天的价格区间 low ~ high 做匹配，如果为 True，则根据新成交价重新匹配调整价格区间，确保订单能被执行；如果为 False，则不会与价格区间做匹配，订单不会执行，但会在下一日尝试执行；默认取值为 True；
* slip\_out：如果新成交价高于最高价或低于最高价，是否以超出的价格成交，如果为 True，则允许以超出的价格成交；如果为 Fasle，实际成交价将被限制在价格区间内 low ~ high；默认取值为 False；
* slip\_limit：是否对限价单执行滑点，如果为 True，即使 slip\_match 为False，也会对价格做匹配，确保订单被执行；如果为 Fasle，则不做价格匹配；默认取值为 True。

### 5.4 示例

pic3

**无滑点**

pic4

**配置1.5％的滑点（默认market类型，t时刻下单，t+1时刻执行，使用open price，超出最高价则使用最高价，反之亦然）**

pic5

**配置1.5％的滑点，且允许使用超出滑点价格下单**

pic6

**配置1.5％的滑点，不进行slip_match（加滑点后的价格超出high/low后不执行，下一个交易日尝试重新执行）**

pic7

# 6. 交易税费管理
Backtrader 也提供了多种交易费设置方式，既可以简单的通过参数进行设置，也可以结合交易条件自定义费用函数。

根据交易品种的不同，Backtrader 将交易费用分为 股票 Stock-like 模式和期货 Futures-like 种模式；

根据计算方式的不同，Backtrader 将交易费用分为 PERC 百分比费用模式 和 FIXED 固定费用模式 ；

Stock-like 模式与 PERC 百分比费用模式对应，期货 Futures-like 与 FIXED 固定费用模式对应；

在设置交易费用时，最常涉及如下 3 个参数：

commission：手续费 / 佣金；

mult：乘数；

margin：保证金 / 保证金比率 。

双边征收：买入和卖出操作都要收取相同的交易费用 。

pic8

第 1 条规则：未设置 margin（即 margin 为 0 / None / False）→ commtype 会指向 COMM_PERC 百分比费用 → 底层的 \_stocklike 属性会设置为 True → 对应的是“股票百分比费用”。所以如果想为股票设置交易费用，就令 margin = 0 / None / False，或者令 stocklike=True；

第 2 条规则：为 margin 设置了取值 → commtype 会指向 COMM_FIXED 固定费用 → 底层的 \_stocklike 属性会设置为 False → 对应的是“期货固定费用”，因为只有期货才会涉及保证金。所以如果想为期货设置交易费用，就需要设置 margin，此外还需令 stocklike=True，margin 参数才会起作用 。


# 7. 成交量限制管理

默认情况下，Broker 在撮合成交订单时，不会将订单上的购买数量与成交当天 bar 的总成交量 volume 进行对比，即使购买数量超出了当天该标的的总成交量，也会按购买数量全部撮合成交，显然这种“无限的流动性”是不现实的，这种 “不考虑成交量，默认全部成交” 的交易模式，也会使得回测结果与真实结果产生较大偏差。如果想要修改这种默认模式，可以通过 Backtrader 中的 fillers 模块来限制实际成交量，fillers 会告诉 Broker 在各个成交时间点应该成交多少量，一共有 3 种形式：

**形式1：bt.broker.fillers.FixedSize(size)**

通过 FixedSize() 方法设置最大的固定成交量：size，该种模式下的成交量限制规则如下：
订单实际成交量的确定规则：取（size、订单执行那天的 volume 、订单中要求的成交数量）中的最小者；
订单执行那天，如果订单中要求的成交数量无法全部满足，则只成交部分数量。
设置方式如下：

```
# 通过 BackBroker() 类直接设置
cerebro = Cerebro()
filler = bt.broker.fillers.FixedSize(size=xxx)
newbroker = bt.broker.BrokerBack(filler=filler)
cerebro.broker = newbroker
# 通过 set_filler 方法设置
cerebro = Cerebro()
cerebro.broker.set_filler(bt.broker.fillers.FixedSize(size=xxx))
```

**形式2：bt.broker.fillers.FixedBarPerc(perc)**

通过 FixedBarPerc(perc) 将 订单执行当天 bar 的总成交量 volume 的 perc % 设置为最大的固定成交量，该模式的成交量限制规则如下：
订单实际成交量的确定规则：取 （volume * perc /100、订单中要求的成交数量） 的 最小者；
订单执行那天，如果订单中要求的成交数量无法全部满足，则只成交部分数量。
设置方式如下：

```
# 通过 BackBroker() 类直接设置
cerebro = Cerebro()
filler = bt.broker.fillers.FixedBarPerc(perc=xxx)
newbroker = bt.broker.BrokerBack(filler=filler)
cerebro.broker = newbroker
# 通过 set_filler 方法设置
cerebro = Cerebro()
cerebro.broker.set_filler(bt.broker.fillers.FixedBarPerc(perc=xxx))
# perc 以 % 为单位，取值范围为[0.0,100.0]
```

# 8. 订单类型

不同的订单类型，下单时需要配置的参数会存在区别，所以在介绍交易函数之前，先介绍 Broker 能够识别和处理的各种订单类型，这样更有利于理解交易函数中各个参数的用途。Backtrader 支持许多不同类型的交易订单，用以满足不同的交易需求，不同类型的订单的成交逻辑存在差异。

**Order.Market**

市价单，以当时市场价格成交的订单，不需要自己设定价格。市价单能被快速达成交易，防止踏空，尽快止损/止盈；
按下一个 Bar （即生成订单的那个交易日的下一个交易日）的开盘价来执行成交；
例：self.buy(exectype=bt.Order.Market) 。

**Order.Close**

和 Order.Market 类似，也是市价单，只是成交价格不一样；
按下一个 Bar 的收盘价来执行成交；
例：self.buy(exectype=bt.Order.Close) 。

**Order.Limit**

限价单，需要指定成交价格，只有达到指定价格（limit Price）或有更好价格时才会执行，即以指定价或低于指点价买入，以指点价或更高指定价卖出；
在订单生成后，会通过比较 limit Price 与之后 Bar 的 open\high\low\close 行情数据来判断订单是否成交。如果下一个 Bar 的 open 触及到指定价格 limit Price，就以 open 价成交，订单在这个 Bar 的开始阶段就被执行完成；如果下一个 Bar 的 open 未触及到指定价格 limit Price，但是 limit Price 位于这个 bar 的价格区间内 （即 low ~ high），就以 limit Price 成交；
例：self.buy(exectype=bt.Order.Limit, price=price, valid=valid) 。

**Order.Stop**

止损单，需要指定止损价格（Stop Price），一旦股价突破止损价格，将会以市价单的方式成交；
在订单生成后，也是通过比较 Stop Price 与之后 Bar 的 open\high\low\close 行情数据来判断订单是否成交。如果下一个 Bar 的 open 触及到指定价格 limit Price，就以 open 价成交；如果下一个 Bar 的 open 未触及到指定价格 Stop Price，但是 Stop Price 位于这个 bar 的价格区间内 （即 low ~ high），就以 Stop Price 成交；
例：self.buy(exectype=bt.Order.Stop, price=price, valid=valid) 。

**Order.StopLimit**

止损限价单，需要指定止损价格（Stop price）和限价（Limit Price），一旦股价达到设置的止损价格，将以限价单的方式下单；
在下一个 Bar，按 Order.Stop 的逻辑触发订单，然后以 Order.Limit 的逻辑执行订单；
例：self.buy(exectype=bt.Order.StopLimit, price=price, valid=valid, plimit=plimit)。

**Order.StopTrail**

跟踪止损订单，是一种止损价格会自动调整的止损单，调整范围通过设置止损价格和市场价格之间的差价来确定。差价即可以用金额 trailamount 表示，也可以用市价的百分比 trailpercent 表示；
如果是通过 buy 下达了买入指令，就会“卖出”一个跟踪止损单，在市场价格上升时，止损价格会随之上升；若股价触及止损价格时，会以市价单的形式执行订单；若市场价格下降或保持不变，止损价格会保持不变；
如果是通过 sell 下达卖出指令，就会“买入”一个跟踪止损单，在市场价格下降时，止损价格会随之下降；若股价触及止损价格时，会以市价单的形式执行订单；但是当市场价格上升时，止损价格会保持不变；
例：self.buy(exectype=bt.Order.StopTrail, price=xxx, trailamount=xxx)。

**Order.StopTrailLimit**

跟踪止损限价单，是一种止损价格会自动调整的止损限价单，订单中的限价 Limit Price 不会发生变动，止损价会发生变动，变动逻辑与上面介绍的跟踪止损订单一致；
例：self.buy(exectype=bt.Order.StopTrailLimit, plimit=xxx, trailamount=xxx) 。
虽然订单的类型多种多样，但考虑到国内交易所的现状，我们在回测中使用比较多的还是市价单和限价单。

# 9. 订单状态

Broker 在执行交易时，会根据执行流程给订单赋予不同的状态，不同阶段的订单状态可以通过Strategy 中定义 notify_order() 方法来捕获，从而进行自定义的处理，从下达交易指令到订单执行结束，订单可能会依次呈现如下状态：

• Order.Created：订单已被创建；

• Order.Submitted：订单已被传递给经纪商 Broker；

• Order.Accepted：订单已被经纪商接收；

• Order.Partial：订单已被部分成交；

• Order.Complete：订单已成交；

• Order.Rejected：订单已被经纪商拒绝；

• Order.Margin：执行该订单需要追加保证金，并且先前接受的订单已从系统中删除；

• Order.Cancelled (or Order.Canceled)：确认订单已经被撤销；

• Order.Expired：订单已到期，其已经从系统中删除 。

# 10. Analyzers
待续

