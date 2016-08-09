TuShare
----
![](https://api.travis-ci.org/waditu/tushare.png?branch=master)
[![](https://pypip.in/v/tushare/badge.png)](https://pypi.python.org/pypi/tushare/0.1.5)

TuShare是实现对股票/期货等金融数据从**数据采集**、**清洗加工** 到 **数据存储**过程的工具，满足金融量化分析师和学习数据分析的人在数据获取方面的需求，它的特点是数据覆盖范围广，接口调用简单,响应快速。

![](http://tushare.waditu.com/_images/main_pic_min.png)

欢迎关注扫描TuShare的微信公众号“挖地兔”，更多资源和信息与您分享：

![](http://tushare.waditu.com/_images/8.jpg)


Dependencies
=========
python 2.x/3.x   

[pandas](http://pandas.pydata.org/ "pandas")


Installation
====

- 方式1：pip install tushare
- 方式2：python setup.py install
- 方式3：访问[https://pypi.python.org/pypi/tushare/](https://pypi.python.org/pypi/tushare/)下载安装


Upgrade
=======

	pip install tushare --upgrade

Quick Start
======
**Example 1.** 获取个股历史交易数据（包括均线数据）：

    import tushare as ts

	ts.get_hist_data('600848') #一次性获取全部数据

结果显示：

> 日期 ，开盘价， 最高价， 收盘价， 最低价， 成交量， 价格变动 ，涨跌幅，5日均价，10日均价，20日均价，5日均量，10日均量，20日均量，换手率

    			 open    high   close     low     volume    p_change  ma5 \
	date                                                                     
	2012-01-11   6.880   7.380   7.060   6.880   14129.96     2.62   7.060   
	2012-01-12   7.050   7.100   6.980   6.900    7895.19    -1.13   7.020   
	2012-01-13   6.950   7.000   6.700   6.690    6611.87    -4.01   6.913   
	2012-01-16   6.680   6.750   6.510   6.480    2941.63    -2.84   6.813   
	2012-01-17   6.660   6.880   6.860   6.460    8642.57     5.38   6.822   
	2012-01-18   7.000   7.300   6.890   6.880   13075.40     0.44   6.788   
	2012-01-19   6.690   6.950   6.890   6.680    6117.32     0.00   6.770   
	2012-01-20   6.870   7.080   7.010   6.870    6813.09     1.74   6.832 

				 ma10    ma20      v_ma5     v_ma10     v_ma20     turnover  
	date                                                                  
	2012-01-11   7.060   7.060   14129.96   14129.96   14129.96     0.48  
	2012-01-12   7.020   7.020   11012.58   11012.58   11012.58     0.27  
	2012-01-13   6.913   6.913    9545.67    9545.67    9545.67     0.23  
	2012-01-16   6.813   6.813    7894.66    7894.66    7894.66     0.10  
	2012-01-17   6.822   6.822    8044.24    8044.24    8044.24     0.30  
	2012-01-18   6.833   6.833    7833.33    8882.77    8882.77     0.45  
	2012-01-19   6.841   6.841    7477.76    8487.71    8487.71     0.21  
	2012-01-20   6.863   6.863    7518.00    8278.38    8278.38     0.23  

设定历史数据的时间：      
	
	ts.get_hist_data('600848',start='2015-01-05',end='2015-01-09')

				open    high   close     low    volume   p_change     ma5    ma10 \  
	date                                                                            
	2015-01-05  11.160  11.390  11.260  10.890  46383.57     1.26  11.156  11.212   
	2015-01-06  11.130  11.660  11.610  11.030  59199.93     3.11  11.182  11.155   
	2015-01-07  11.580  11.990  11.920  11.480  86681.38     2.67  11.366  11.251   
	2015-01-08  11.700  11.920  11.670  11.640  56845.71    -2.10  11.516  11.349   
	2015-01-09  11.680  11.710  11.230  11.190  44851.56    -3.77  11.538  11.363   
	 			ma20     v_ma5    v_ma10     v_ma20 	 turnover  
	date                                                        
	2015-01-05  11.198  58648.75  68429.87   97141.81     1.59  
	2015-01-06  11.382  54854.38  63401.05   98686.98     2.03  
	2015-01-07  11.543  55049.74  61628.07  103010.58     2.97  
	2015-01-08  11.647  57268.99  61376.00  105823.50     1.95  
	2015-01-09  11.682  58792.43  60665.93  107924.27     1.54  


**复权历史数据**
获取历史复权数据，分为前复权和后复权数据，接口提供股票上市以来所有历史数据，默认为前复权。如果不设定开始和结束日期，则返回近一年的复权数据，从性能上考虑，推荐设定开始日期和结束日期，而且最好不要超过一年以上，获取到数据后，请及时在本地存储。

	ts.get_h_data('002337') #前复权
	ts.get_h_data('002337',autype='hfq') #后复权
	ts.get_h_data('002337',autype=None) #不复权
	ts.get_h_data('002337',start='2015-01-01',end='2015-03-16') #两个日期之间的前复权数据


**Example 2.** 一次性获取最近一个日交易日所有股票的交易数据（结果显示速度取决于网速）
	

	ts.get_today_all()


结果显示：

> 代码，名称，涨跌幅，现价，开盘价，最高价，最低价，最日收盘价，成交量，换手率

		  code    name     changepercent  trade   open   high    low  settlement \  
	0     002738  中矿资源         10.023  19.32  19.32  19.32  19.32       17.56   
	1     300410  正业科技         10.022  25.03  25.03  25.03  25.03       22.75   
	2     002736  国信证券         10.013  16.37  16.37  16.37  16.37       14.88   
	3     300412  迦南科技         10.010  31.54  31.54  31.54  31.54       28.67   
	4     300411  金盾股份         10.007  29.68  29.68  29.68  29.68       26.98   
	5     603636  南威软件         10.006  38.15  38.15  38.15  38.15       34.68   
	6     002664  信质电机         10.004  30.68  29.00  30.68  28.30       27.89   
	7     300367  东方网力         10.004  86.76  78.00  86.76  77.87       78.87   
	8     601299  中国北车         10.000  11.44  11.44  11.44  11.29       10.40   
	9     601880   大连港         10.000   5.72   5.34   5.72   5.22        5.20   
	10    000856  冀东装备         10.000   8.91   8.18   8.91   8.18        8.10  
			volume  	 turnoverratio  
	0        375100        1.25033  
	1         85800        0.57200  
	2       1058925        0.08824  
	3         69400        0.51791  
	4        252220        1.26110  
	5       1374630        5.49852  
	6       6448748        9.32700  
	7       2025030        6.88669  
	8     433453523        4.28056  
	9     323469835        9.61735  
	10     25768152       19.51090  

**Example 3.** 获取历史分笔数据

    import tushare as ts

	df = ts.get_tick_data('600848',date='2014-01-09')
	df.head(10)

结果显示：
>成交时间、成交价格、价格变动，成交手、成交金额(元)，买卖类型

    Out[3]: 
     	 time  		price change  volume  amount  type
	0    15:00:00   6.05     --       8    4840   卖盘
	1    14:59:55   6.05     --      50   30250   卖盘
	2    14:59:35   6.05     --      20   12100   卖盘
	3    14:59:30   6.05  -0.01     165   99825   卖盘
	4    14:59:20   6.06   0.01       4    2424   买盘
	5    14:59:05   6.05  -0.01       2    1210   卖盘
	6    14:58:55   6.06     --       4    2424   买盘
	7    14:58:45   6.06     --       2    1212   买盘
	8    14:58:35   6.06   0.01       2    1212   买盘
	9    14:58:25   6.05  -0.01      20   12100   卖盘
	10   14:58:05   6.06     --       5    3030   买盘

**Example 4.** 获取实时交易数据(Realtime Quotes Data)

    df = ts.get_realtime_quotes('000581') #Single stock symbol
	df[['code','name','price','bid','ask','volume','amount','time']]

结果显示：
>名称、开盘价、昨价、现价、最高、最低、买入价、卖出价、成交量、成交金额...more in docs


	   code    name     price  bid    ask    volume   amount        time
	0  000581  威孚高科  31.15  31.14  31.15  8183020  253494991.16  11:30:36 
	  
请求多个股票方法（一次最好不要超过30个）：
    
	ts.get_realtime_quotes(['600848','000980','000981']) #symbols from a list
	ts.get_realtime_quotes(df['code'].tail(10)) #from a Series


更多文档
========
[http://tushare.org/](http://tushare.org/ "TuShare Docs")
 
Change Logs
-----------

0.4.9 2016/03/26
=============
- 新增申万行业分类get_industry_classified(standard='sw')
- 新增交易日历trade_cal()
- 修复bug

0.4.3 2015/12/24
============
- 新增电影票房数据
- 修复部分bug

0.4.1 2015/11/27
==============

- 新增sina大单数据
- 修改当日分笔bug
- 深市融资融券数据修复

0.3.9 2015/10/13
============

- 新增期权隐含波动率数据
- 修复指数成份及权重接口问题

0.3.8 2015/09/19
============

- 完成通联数据SDK v0.2.0开发
- 沪深300成份股和权重接口问题修复
- 其它bug的修复
- [通联数据API文档](http://tushare.org/datayes.html)发布


0.3.5 2015/07/27
==========

- 部分代码修正
- 新增通联数据SDK0.1版

0.3.4 2015/06/15
===========

- 新增‘龙虎榜’模块
	1. 每日龙虎榜列表
	1. 个股上榜统计
	1. 营业部上榜统计
	1. 龙虎榜机构席位追踪
	1. 龙虎榜机构席位成交明细

- 修改get\_h\_data数据类型为float
- 修改get_index接口遗漏的open列
- 合并GitHub上提交的bug修复


0.2.8 2015/04/28
============

- 新增大盘指数实时行情列表
- 新增大盘指数历史行情数据（全部）
- 新增终止上市公司列表（退市）
- 新增暂停上市公司列表
- 修正融资融券明细无日期的缺陷
- 修正get\_h\_data部分bug

0.2.6
========
- 新增沪市融资融券列表
- 新增沪市融资融券明细列表
- 新增深市融资融券列表
- 新增深市融资融券明细列表
- 修正复权数据数据源出现null造成异常问题（对大约300个股票有影响）

0.2.5 2015/04/16
===========
- 完成python2.x和python3.x兼容性支持
- 部分算法优化和代码重构
- 新增中证500成份股
- 新增当日分笔交易明细
- 修正分配预案（高送转）bug

0.2.3 2015/04/11
===========
- 新增“新浪股吧”消息和热度
- 新增新股上市数据
- 修正“基本面”模块中数据重复的问题
- 修正历史数据缺少一列column（数据来源问题）的bug

0.2.0 2015/03/17
=======

 - 新增历史复权数据接口
 - 新增即时滚动新闻、信息地雷数据
 - 新增沪深300指数成股份及动态权重、
 - 新增上证50指数成份股
 - 修改历史行情数据类型为float

0.1.9 2015/02/06
========
- 增加分类数据
- 增加数据存储示例

0.1.6 2015/01/27
========
- 增加了重点指数的历史和实时行情
- 更新docs

0.1.5 2015/01/26
=====

- 增加基本面数据接口
- 发布一版使用手册，开通[TuShare docs](http://tushare.waditu.com)网站

0.1.3 2015/01/13
===
- 增加实时交易数据的获取
- Done for crawling Realtime Quotes data

0.1.1 2015/01/11
===

- 增加tick数据的获取

0.1.0 2014/12/01
===

- 创建第一个版本
- 实现个股历史数据的获取