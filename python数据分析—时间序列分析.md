#### 一、日期和时间数据类型

#### 1、datetime数据类型

```
from datetime import datetime

now = datetime.now()
print now
print now.year, now.month, now.day12345
2018-01-29 12:42:51.378000
2018 1 2912
```

#### 2、字符串和datetime相互转换

```
stamp = datetime(2017, 1, 3)
print str(stamp)
2017-01-03 00:00:00

value = '2017-01-04'
print datetime.strptime(value, '%Y-%m-%d')
2017-01-04 00:00:0012345678
```

# 二、时间序列基础

最基本类型的时间序列类型以时间戳为索引的Series：

```
dates = [datetime(2017, 1, 2), datetime(2017, 1, 5), datetime(2017, 1, 7), datetime(2017, 1, 8),
         datetime(2017, 1, 10), datetime(2017, 1, 12)]
ts = pd.Series(np.random.randn(6), index=dates) 
print ts
print ts.index12345
2017-01-02    1.891046
2017-01-05   -0.580931
2017-01-07   -1.205874
2017-01-08    1.581906
2017-01-10   -1.345287
2017-01-12   -0.856211
dtype: float64

DatetimeIndex(['2017-01-02', '2017-01-05', '2017-01-07', '2017-01-08','2017-01-10', '2017-01-12'],dtype='datetime64[ns]', freq=None)
12345678910
```

#### 1、索引

```
stamp = ts.index[2]
print ts[stamp]
2.17794314871

print ts['20170107']
2.17794314871123456
```

#### 2.切片

```
print ts['1/2/2017':'1/8/2017']

2017-01-02    1.598877
2017-01-05   -1.510436
2017-01-07    0.231524
2017-01-08   -0.472447
dtype: float641234567
```

#### 3.日期的范围

pandas.date_range生成固定长度的DatetimeIndex，默认按天计算时间点。

```
index = pd.date_range('4/1/2017', '4/10/2017')
print index

DatetimeIndex(['2017-04-01', '2017-04-02', '2017-04-03', '2017-04-04',
               '2017-04-05', '2017-04-06', '2017-04-07', '2017-04-08',
               '2017-04-09', '2017-04-10', '2017-04-11', '2017-04-12',
               '2017-04-13', '2017-04-14', '2017-04-15', '2017-04-16',
               '2017-04-17', '2017-04-18', '2017-04-19', '2017-04-20'],
              dtype='datetime64[ns]', freq='D')123456789
```

##### 传入起始时间和periods

```
index = pd.date_range(end='4/1/2017', periods=10)
print index

DatetimeIndex(['2017-03-23', '2017-03-24', '2017-03-25', '2017-03-26',
               '2017-03-27', '2017-03-28', '2017-03-29', '2017-03-30',
               '2017-03-31', '2017-04-01'],
              dtype='datetime64[ns]', freq='D')1234567
```

##### 传入指定频率

```
index = pd.date_range('3/1/2017', '10/1/2017', freq='BM')
print index

DatetimeIndex(['2017-03-31', '2017-04-28', '2017-05-31', '2017-06-30',
               '2017-07-31', '2017-08-31', '2017-09-29'],
              dtype='datetime64[ns]', freq='BM')123456
```

### 4.重采样

```
rng = pd.date_range('1/1/2017', periods=100, freq='D')
ts = pd.Series(np.random.randn(len(rng)), index=rng)
print ts.resample('M', how='mean')

2017-01-31   -0.080247
2017-02-28   -0.119564
2017-03-31   -0.113525
2017-04-30   -0.131885
Freq: M, dtype: float64123456789
```

### 5.降采样

```
rng = pd.date_range('1/1/2017', periods=12, freq='T')
ts = pd.Series(np.arange(12), index=rng)
print ts

2017-01-01 00:00:00     0
2017-01-01 00:01:00     1
2017-01-01 00:02:00     2
2017-01-01 00:03:00     3
2017-01-01 00:04:00     4
2017-01-01 00:05:00     5
2017-01-01 00:06:00     6
2017-01-01 00:07:00     7
2017-01-01 00:08:00     8
2017-01-01 00:09:00     9
2017-01-01 00:10:00    10
2017-01-01 00:11:00    11

print ts.resample('5min', how='sum')

2017-01-01 00:00:00    10
2017-01-01 00:05:00    35
2017-01-01 00:10:00    21
Freq: 5T, dtype: int32
```

#时间序列分析：给定一个已被观测了的时间序列，预测该序列的未来值

## 时间序列

> 时间序列数据是一种非常重要的结构化数据形式，应用于：金融学、经济学、神经科学、物理学等多个领域。

- 很多时间序列是固定频率的，数据点是根据某种规律定期出现的
- 时间序列也可以是不定期的，没有固定的单位或者单位之间的偏移量

## 应用场景

- 时间戳`timestamp`，特定的时刻
- `pandas`通过`numpy`的`datatime64`数据类型以`纳秒`形式存储`时间戳`
- 固定时期`period`
- 时间间隔`interval`，由起始和结束的时间戳表示
- 时间或者时间过程

**最常见的时间序列是时间戳进行索引**

## 日期和时间数据集工具

- 标准库：`date、time`和`calendar`
- 模块包含：`datetime、time、calendar`

**应用最多的是datetime.datetime**


  

# 时间序列算法：

#### 1、平滑法

常用语趋势分析和预测，利用修匀技术，虚弱短期随机波动对序列的影响，使序列平滑化，根据平滑技术的不同，分为移动平均法和指数平滑法

#### 2、趋势拟合法

把时间作为自变量，相应的序列观察值作为因变量，建立回归模型，根据序列的特征，可具体分为线性拟合和曲线拟合

#### 3、组合模型

时间序列的变化主要受到长期趋势，季节变动，周期变动和不规则变动四个因素的影响，可以构架加法模型和乘法模型

#### 4、AR模型

以前P期的序列值为自变量，随机变量的取值为因变量建立线性回归模型

描述当前值与历史值之间的关系，用变量自身的历史时间数据对自身进行预测。
自回归模型必须满足平稳性的要求。
p阶自回归过程的公式定义为：
                                                

是当前值，是常数项，p是阶数，是自相关系数，是误差。
自回归模型的限制：

自回归模型是用自身的数据来进行预测。
必须具有平稳性。
必须具有自相关性，如果自相关系数小于0.5，则不宜采用。
自回归只适用于预测与自身前期相关的现象。

#### 5、MA模型

随机变量的取值与前期序列值无关，建立随机变量的取值与前q期随机扰动的线性回归模型

- 移动平均模型关注的是自回归模型中的误差项的累加。
- q阶自回归过程的公式定义：

​                                                 ![img](https://img-blog.csdnimg.cn/20190115224658844.png)

- 移动平均法能有效地消除预测中的随机波动。

#### 6、ARMA模型（自回归移动平均模型）

随机变量的取值不仅与前期序列值有关还与前期随机扰动有关

- 自回归与移动平均的结合。
- 公式定义：

​                                                  ![img](https://img-blog.csdnimg.cn/20190115224038637.png)

#### 7、ARIMA模型

许多非平稳序列差分后会显示出平稳序列的性质，称这个非平稳序列为差分平稳序列。

1.平稳性：平稳性就是要求经由样本时间序列所得到的拟合曲线在未来的一段时间内仍能顺着现有的形态“惯性”地延续下去。平稳性要求序列的均值和方差不发生明显变化。

 严平稳：严平稳表示的分布不随时间的改变而改变，如：正态分布，无论如何取值，期望都是0，方差都是1。
 弱平稳：期望与相关系数(依赖性)不变，未来某时刻的t的值就要依赖于它的过去信息，所以需要依赖性。
2.原理：将非平稳时间序列转化为平稳时间序列，然后将因变量仅对它的滞后值以及随机误差项的现值和滞后值进行回归所建立的模型

3.各物理量含义：

AR：自回归
p：自回归项
MA：移动平均
q：移动平均项数
d：时间序列平稳时的差分次数

##### ARIMA建模流程

1.得到平稳的时间序列(用差分法求出d)。

2.根据ACF和PACF的图像求出p和q，ACF决定q，PACF决定p。

3.ARIMA(p,d,q)

#### 8、ARCH模型

适用于具有异方差性并且异方差函数短期自相关

#### 9、GARCH模型及其衍生模型

能够反应实际序列的长期记忆性、信息的非对称性



## 时间序列的预处理

首先要对它进行纯随机性和平稳性的检验，根据检验结果将序列分为不同类型，采用不同方法分析。

对于**纯随机序列**，又称为白噪声序列，序列各项之间没有任何相关关系，序列在进行完全无序的随机波动，可以终止对该序列的分析。白噪声序列是没有信息可提取的平稳序列。

对于**平稳非白噪声序列**，均值和方差是常数，建立一个线性模型来拟合该序列的发展，最常用ARMA模型。

对于**非平稳序列**，由于均值和方差不稳定，一般是将其转化为平稳序列再进行研究。

**平稳性检验：**如果时间序列在某一常数附近波动且波动范围有限，即有常数均值和常数方差，并且延迟K期的序列变量的自协方差和自相关系数是相等，则称之为平稳序列

两种方法检验：

1、根据时序图和自相关图的特征做出判断的图检验。

2、构造检验统计量进行检验，常用单位根检验。

（1）**时序图检验**：平稳序列的时序图显示该序列值始终在一个常数附近随机波动，且波动范围有界。如果有明显的趋势性或者周期性，那么通常不是平稳序列。

（2）**自相关图检验**：平稳序列具有短期相关性，间隔越远的过去值对现时值影响越小，平稳序列自相关系数会比较快的衰减趋向于零，并在零附近随机波动，而非平稳序列的自相关系数衰减的速度较慢。

（3）**单位根检验**：检验序列中是否存在单位根，如果存在则是非平稳序列。

**纯随机性检验：**纯随机序列的样本自相关系数不会绝对为零，但是很接近与零，并在零附近随机波动。

也称为白噪声检验，一般构造检验统计量来检验序列的纯随机性，常用Q统计量和LB统计量，计算出P值，如果P值显著大于显著性水平，表示不能拒绝纯随机的原假设，可以停止对该序列的分析。

**平稳时间序列建模：**

1、计算ACF和PACF

2、模型定阶，根据自相关系数和骗子相关系数的性质，选择合适的模型。

**非平稳时间序列建模：**

1、检验序列的平稳性

2、对原始序列进行一阶差分，并进行平稳性和白噪声检验：（1）对一阶差分后的序列再次做平稳性判断（2）对一阶差分后的序列进行白噪声检验

##### 3、对一阶差分后的平稳非白噪声序列进行拟合ARMA模型，即模型定阶。

# ARIMA模型实现代码
```
import pandas as pd
discfile='E:/WTTfiles/自我学习/机器学习/python数据分析与挖掘实战/chapter5/demo/data/arima_data.xls'
forecastnum=5
data=pd.read_excel(discfile,index_col=u'日期')
#时序图
import matplotlib.pyplot as plt
plt.rcParams['font.sans-serif'] = ['SimHei']
plt.rcParams['axes.unicode_minus'] = False
data.plot()
plt.show()
#自相关图
from statsmodels.graphics.tsaplots import plot_acf
plot_acf(data).show()
#平稳性检测
from statsmodels.tsa.stattools import adfuller as ADF
print(u'原始序列的ADF检验结果为：',ADF(data[u'销量']))

# 返回值一次为adf,pvalue,usedlag,nobs,critical values,icbest,regresults,resstore

# 差分后的结果

D_data=data.diff().dropna()
D_data.columns=[u'销量差分']
D_data.plot()
plt.show()
plot_acf(D_data).show()

# 平稳性检测

from statsmodels.graphics.tsaplots import plot_pacf
plot_pacf(D_data).show()
print(u'差分序列的ADF检验结果为：',ADF(D_data[u'销量差分']))

# 白噪声检验

from statsmodels.stats.diagnostic import acorr_ljungbox
print(u'差分序列的白噪声检验结果为：',acorr_ljungbox(D_data,lags=1))

# 定阶：

data[u'销量']=data[u'销量'].astype(float)
from statsmodels.tsa.arima_model import ARIMA
pmax=int(len(D_data)/10)#一般阶数不会超过length/10
qmax=int(len(D_data)/10)
bic_matrix=[]#bic矩阵
for p in range(pmax+1):
    tmp=[]
    for q in range(qmax+1):
        try:#存在部分报错，用try跳过报错
            tmp.append(ARIMA(data,(p,1,q)).fit(disp=-1).bic)
        except:
            tmp.append(None)
    bic_matrix.append(tmp)

bic_matrix=pd.DataFrame(bic_matrix)#从中可以找出最小值
p,q=bic_matrix.stack().idxmin()#先用stack展平，然后用idxmin找出最小值位置
print(u'BIC最小的p值和q值为：%s、%s' %(p,q))
model=ARIMA(data,(p,1,q)).fit(disp=-1)#建立ARIMA（0，1，1）模型
print(model.summary2())#给出一份模型报告
print(model.forecast(5))#作为期5天的预测，返回预测结果，标准误差，置信区间
```

# 常用时间序列分析：

时间序列的常用算法包括移动平均（MA, Moving Average）、指数平滑（ES, Exponen-tial Smoothing）、差分自回归移动平均模型（ARIMA , Auto-regressive Integrated Moving Average Model）三大主要类别，每个类别又可以细分和延伸出多种算法。

当数据集规模较大、数据粒度丰富时，我们可以选择多种时间序列的预测模式。时间序列的数据粒度可分为秒、分、小时、天、周、月、季度、年等，不同的粒度都可以用来做时间序列预测。
假如有3650天（完整10年）以每分钟时间戳为粒度的数据，那么这份数据集会有5256 000（60分钟24小时365天*10年）条记录。基于该数据集预测今天的总数据，可以考虑的时间序列模式有三种：

整合模式：将历史数据按每天数据进行汇总，得到按天为粒度的共计3650条时间序列数据；然后应用所有数据集做时间序列分析，预测的时间序列项目为1（天）。
横向模式：将历史数据按每小时做汇总，得到按小时为粒度87600条时间序列数据；然后将要预测的1天划分为24小时（即24个预测点），这样会形成24个预测模型，每个模型只预测对应的小时点数据，预测的时间序列项目为1（小时）；最后将预测得到的24个小时点的数据求和得到今天的总数据。
纵向模式：这种模式的粒度更细，无需对历史数据做任何形式的汇总，这样有525600条时间序列数据。将要预测的1天划分为1440个分钟点（60分钟*24小时），这样会形成1440个预测模型，每个模型只预测对应的分钟点的数据，预测的时间序列项目为1（分钟）；最后将预测得到的1440个分钟点的数据求和得到今天的总数据。
这三种方式的训练集都是3650条，即按天为粒度的数据。横向模式和纵向模式由于做更细的粒度切分，因此需要更多的模型，这意味着需要更多的时间来做训练和预测。当第一种整合模式不能满足需求或者预测结果不佳时，可以尝试后两种模式。

## 平稳性

平稳性是做时间序列分析的前提条件，所谓平稳通俗理解就是数据没有随着时间呈现明显的趋势和规律，例如剧烈波动、递增、递减等，而是相对均匀且随机地分布在均值附近。在ARIMA模型中的I就是对数据做差分以实现数据的平稳，而ARIMA关键参数p、d、q中的d即时间序列成为平稳时所做的差分次数。
如何判断时间序列数据是否需要平稳性处理？一般有三种方法：

#### 观察法：

通过输出时间序列图发现数据是否平稳。本示例中的adf_val函数便含有该方法。
自相关和偏相关法：通过观察自相关和偏相关的系数分析数据是否平稳。
ADF检验：通过ADF检验得到的显著性水平分析数据是否平稳。

本示例中的adf_val函数便有含有该方法。
实现数据的平稳有以下几种方法：

#### 对数法：

对数处理可以减小数据的波动，使其线性规律更加明显。但需要注意的是对数处理只能针对时间序列的值大于0的数据集。

#### 差分法：

一般来说，非纯随机的时间序列经一阶差分或者二阶差分之后就会变得平稳（statsmodels的ARIMA模型自动支持数据差分，但最大为2阶差分）。
**平滑法：**根据平滑技术的不同，可分为移动平均法和指数平均法，这两个都是最基本的时间序列方法。
**分解法**：将时序数据分离成不同的成分，包括成长期趋势、季节趋势和随机成分等。

## 白噪声

白噪声（white noise）检验也称为随机性检验，用于检验时间序列的各项数值之间是否具有任何相关关系，白噪声分布是应用时间序列分析的前提。检测时间序列是否属于随机性分布，可通过图形法和Ljung Box法检验。

图形法：时间序列应该是围绕均值随机性上下分布的状态。
Ljung Box法：是对时间序列是否存在滞后相关的一种统计检验，判断序列总体的相关性或者说随机性是否存在。
白噪声检验通常和数据平稳性检验是协同进行数据检验的，这意味着如果平稳性检验通过，白噪声检验一般也会通过。

```
import pandas as pd  # pandas库
import numpy as np  # numpy库
from statsmodels.graphics.tsaplots import plot_acf, plot_pacf  # acf和pacf展示库
from statsmodels.tsa.stattools import adfuller  # adf检验库
from statsmodels.stats.diagnostic import acorr_ljungbox  # 随机性检验库
from statsmodels.tsa.arima_model import ARMA  # ARMA库
import matplotlib.pyplot as plt  # matplotlib图形展示库
import prettytable  # 导入表格库

# 由于pandas判断周期性时会出现warning，这里忽略提示

import warnings
warnings.filterwarnings('ignore')

#### 读取数据

# 创建解析列的功能对象

date_parse = lambda dates: pd.datetime.strptime(dates, '%m-%d-%Y') 

# 读取数据

df = pd.read_table('https://raw.githubusercontent.com/ffzs/dataset/master/time_series.txt', delimiter='\t', index_col='date', date_parser=date_parse)  

# 将列转换为float32类型

ts_data = df['number'].astype('float32') 
print ('data summary') 

# 打印输出时间序列数据概况

print (ts_data.describe()) 
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181206135016889.png)

```
# 多次用到的表格

def pre_table(table_name, table_rows):
    '''
    :param table_name: 表格名称，字符串列表
    :param table_rows: 表格内容，嵌套列表
    :return: 展示表格对象
    '''
    table = prettytable.PrettyTable()  # 创建表格实例
    table.field_names = table_name  # 定义表格列名
    for i in table_rows:  # 循环读多条数据
        table.add_row(i)  # 增加数据
    return table

# 稳定性（ADF）检验

def adf_val(ts, ts_title, acf_title, pacf_title):
    '''
    :param ts: 时间序列数据，Series类型
    :param ts_title: 时间序列图的标题名称，字符串
    :param acf_title: acf图的标题名称，字符串
    :param pacf_title: pacf图的标题名称，字符串
    :return: adf值、adf的p值、三种状态的检验值
    '''
    plt.figure()
    plt.plot(ts)  # 时间序列图
    plt.title(ts_title)  # 时间序列标题
    plt.show()
    plot_acf(ts, lags=20, title=acf_title).show()  # 自相关检测
    plot_pacf(ts, lags=20, title=pacf_title).show()  # 偏相关检测
    adf, pvalue, usedlag, nobs, critical_values, icbest = adfuller(ts)  # 稳定性（ADF）检验
    table_name = ['adf', 'pvalue', 'usedlag', 'nobs', 'critical_values', 'icbest']  # 表格列名列表
    table_rows = [[adf, pvalue, usedlag, nobs, critical_values, icbest]]  # 表格行数据，嵌套列表
    adf_table = pre_table(table_name, table_rows)  # 获得平稳性展示表格对象
    print ('stochastic score')  # 打印标题
    print (adf_table)  # 打印展示表格
    return adf, pvalue, critical_values,  # 返回adf值、adf的p值、三种状态的检验值

# 白噪声（随机性）检验

def acorr_val(ts):
    '''
    :param ts: 时间序列数据，Series类型
    :return: 白噪声检验的P值和展示数据表格对象
    '''
    lbvalue, pvalue = acorr_ljungbox(ts, lags=1)  # 白噪声检验结果
    table_name = ['lbvalue', 'pvalue']  # 表格列名列表
    table_rows = [[lbvalue, pvalue]]  # 表格行数据，嵌套列表
    acorr_ljungbox_table = pre_table(table_name, table_rows)  # 获得白噪声检验展示表格对象
    print ('stationarity score')  # 打印标题
    print (acorr_ljungbox_table)  # 打印展示表格
    return pvalue  # 返回白噪声检验的P值和展示数据表格对象

##### 原始数据检验

# 稳定性检验

adf, pvalue1, critical_values = adf_val(ts_data, 'raw time series', 'raw acf', 'raw pacf')  

# 白噪声检验

pvalue2 = acorr_val(ts_data)  
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20181206135536596.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181206135545335.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181206135553730.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

```
# 数据平稳处理

def get_best_log(ts, max_log=5, rule1=True, rule2=True):
    '''
    :param ts: 时间序列数据，Series类型
    :param max_log: 最大log处理的次数，int型
    :param rule1: rule1规则布尔值，布尔型
    :param rule2: rule2规则布尔值，布尔型
    :return: 达到平稳处理的最佳次数值和处理后的时间序列
    '''
    if rule1 and rule2:  # 如果两个规则同时满足
        return 0, ts  # 直接返回0和原始时间序列数据
    else:  # 只要有一个规则不满足
        for i in range(1, max_log):  # 循环做log处理
            ts = np.log(ts)  # log处理
            adf, pvalue1, usedlag, nobs, critical_values, icbest = adfuller(ts)  # 稳定性（ADF）检验
            lbvalue, pvalue2 = acorr_ljungbox(ts, lags=1)  # 白噪声（随机性）检验
            rule_1 = (adf < critical_values['1%'] and adf < critical_values['5%'] and adf < critical_values[
                '10%'] and pvalue1 < 0.01)  # 稳定性（ADF）检验规则
            rule_2 = (pvalue2 < 0.05)  # 白噪声（随机性）规则
            rule_3 = (i < 5)
            if rule_1 and rule_2 and rule_3:  # 如果同时满足条件
                print ('The best log n is: {0}'.format(i))  # 打印输出最佳次数
                return i, ts  # 返回最佳次数和处理后的时间序列

# 还原经过平稳处理的数据

def recover_log(ts, log_n):
    '''
    :param ts: 经过log方法平稳处理的时间序列，Series类型
    :param log_n: log方法处理的次数，int型
    :return: 还原后的时间序列
    '''
    for i in range(1, log_n + 1):  # 循环多次
        ts = np.exp(ts)  # log方法还原
    return ts  # 返回时间序列

#### 对时间序列做稳定性处理

# 稳定性检验规则

rule1 = (adf < critical_values['1%'] and adf < critical_values['5%'] and adf < critical_values[
    '10%'] and pvalue1 < 0.01)  

# 白噪声检验的规则

rule2 = (pvalue2[0,] < 0.05)  

# 使用log进行稳定性处理

log_n, ts_data = get_best_log(ts_data, max_log=5, rule1=rule1, rule2=rule2)

#### 稳定后数据进行检验

# 稳定性检验

adf, pvalue1, critical_values = adf_val(ts_data, 'final time series', 'final acf', 'final pacf')  

# 白噪声检验

pvalue2 = acorr_val(ts_data)  
————————————————
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20181206135737790.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

```
# arma最优模型训练

def arma_fit(ts):
    '''
    :param ts: 时间序列数据，Series类型
    :return: 最优状态下的p值、q值、arma模型对象、pdq数据框和展示参数表格对象
    '''
    max_count = int(len(ts) / 10)  # 最大循环次数最大定义为记录数的10%
    bic = float('inf')  # 初始值为正无穷
    tmp_score = []  # 临时p、q、aic、bic和hqic的值的列表
    print ('each p/q traning record') # 打印标题
    print('p  q           aic          bic         hqic')
    for tmp_p in range(max_count + 1):  # p循环max_count+1次
        for tmp_q in range(max_count + 1):  # q循环max_count+1次
            model = ARMA(ts, order=(tmp_p, tmp_q))  # 创建ARMA模型对象
            try:
                # ARMA模型训练 disp不打印收敛信息 method条件平方和似然度最大化
                results_ARMA = model.fit(disp=-1, method='css')  
            except:
                continue  # 遇到报错继续
            finally:
                tmp_aic = results_ARMA.aic  # 模型的获得aic
                tmp_bic = results_ARMA.bic  # 模型的获得bic
                tmp_hqic = results_ARMA.hqic  # 模型的获得hqic
                print('{:2d}|{:2d}|{:.8f}|{:.8f}|{:.8f}'.format(tmp_p, tmp_q, tmp_aic, tmp_bic, tmp_hqic))
                tmp_score.append([tmp_p, tmp_q, tmp_aic, tmp_bic, tmp_hqic])  # 追加每个模型的训练参数和结果
                if tmp_bic < bic:  # 如果模型bic小于最小值，那么获得最优模型ARMA的下列参数：
                    p = tmp_p  # 最优模型ARMA的p值
                    q = tmp_q  # 最优模型ARMA的q值
                    model_arma = results_ARMA  # 最优模型ARMA的模型对象
                    aic = tmp_bic  # 最优模型ARMA的aic
                    bic = tmp_bic  # 最优模型ARMA的bic
                    hqic = tmp_bic  # 最优模型ARMA的hqic
    pdq_metrix = np.array(tmp_score)  # 将嵌套列表转换为矩阵
    pdq_pd = pd.DataFrame(pdq_metrix, columns=['p', 'q', 'aic', 'bic', 'hqic'])  # 基于矩阵创建数据框
    table_name = ['p', 'q', 'aic', 'bic', 'hqic']  # 表格列名列表
    table_rows = [[p, q, aic, bic, hqic]]  # 表格行数据，嵌套列表
    parameter_table = pre_table(table_name, table_rows)  # 获得最佳ARMA模型结果展示表格对象

# print ('each p/q traning record')  # 打印标题

# print (pdq_pd)  # 打印输出每次ARMA拟合结果，包含p、d、q以及对应的AIC、BIC、HQIC

​```
print ('best p and q')  # 打印标题
print (parameter_table)  # 输出最佳ARMA模型结果展示表格对象
return model_arma  # 最优状态下的arma模型对象
​```

# 模型训练和效果评估

def train_test(model_arma, ts, log_n, rule1=True, rule2=True):
    '''
    :param model_arma: 最优ARMA模型对象
    :param ts: 时间序列数据，Series类型
    :param log_n: 平稳性处理的log的次数，int型
    :param rule1: rule1规则布尔值，布尔型
    :param rule2: rule2规则布尔值，布尔型
    :return: 还原后的时间序列
    '''
    train_predict = model_arma.predict()  # 得到训练集的预测时间序列
    if not (rule1 and rule2):  # 如果两个条件有任意一个不满足
        train_predict = recover_log(train_predict, log_n)  # 恢复平稳性处理前的真实时间序列值
        ts = recover_log(ts, log_n)  # 时间序列还原处理
    ts_data_new = ts[train_predict.index]  # 将原始时间序列数据的长度与预测的周期对齐
    RMSE = np.sqrt(np.sum((train_predict - ts_data_new) ** 2) / ts_data_new.size)  # 求RMSE
    # 对比训练集的预测和真实数据
    plt.figure()  # 创建画布
    train_predict.plot(label='predicted data', style='--')  # 以虚线展示预测数据
    ts_data_new.plot(label='raw data')  # 以实线展示原始数据
    plt.legend(loc='best')  # 设置图例位置
    plt.title('raw data and predicted data with RMSE of %.2f' % RMSE)  # 设置标题
    plt.show()  # 展示图像
    return ts  # 返回还原后的时间序列

# 训练最佳ARMA模型并输出相关参数和对象

model_arma = arma_fit(ts_data)

# 模型训练和效果评估

ts_data = train_test(model_arma, ts_data, log_n, rule1=rule1, rule2=rule2)
```


![img](https://img-blog.csdnimg.cn/20181206135932614.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

```
# 预测未来指定时间项的数据

def predict_data(model_arma, ts, log_n, start, end, rule1=True, rule2=True):
    '''
    :param model_arma: 最优ARMA模型对象
    :param ts: 时间序列数据，Series类型
    :param log_n: 平稳性处理的log的次数，int型
    :param start: 要预测数据的开始时间索引
    :param end: 要预测数据的结束时间索引
    :param rule1: rule1规则布尔值，布尔型
    :param rule2: rule2规则布尔值，布尔型
    :return: 无
    '''
    predict_ts = model_arma.predict(start=start, end=end)  # 预测未来指定时间项的数据
    print ('-----------predict data----------')  # 打印标题
    if not (rule1 and rule2):  # 如果两个条件有任意一个不满足
        predict_ts = recover_log(predict_ts, log_n)  # 还原数据
    print (predict_ts)  # 展示预测数据
    # 展示预测趋势
    plt.figure()  # 创建画布
    ts.plot(label='raw time series')  # 设置推向标签
    predict_ts.plot(label='predicted data', style='--')  # 以虚线展示预测数据
    plt.legend(loc='best')  # 设置图例位置
    plt.title('predicted time series')  # 设置标题
    plt.show()  # 展示图像
    

#### 模型预测应用

# 设置时间

start = '1991-07-28'  
end = '1991-08-02' 

# 预测

predict_data(model_arma, ts_data, log_n, start, end, rule1=rule1, rule2=rule2)  
```


![在这里插入图片描述](https://img-blog.csdnimg.cn/20181206140001259.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

## 补充：statsmodels中的ARMA



statsmodels常用的时间序列模型包括AR、ARMA和ARIMA，并都集中在statsmodels. tsa.arima_model下面。ARMA(p, q)是AR§和MA(q)模型的组合，关于p和q的选择，一种方法是观察自相关图ACF和偏相关图PACF，通过截尾、拖尾等信息分析应用的模型以及适应的p和q的阶数；另一种方法是通过循环的方法，遍历所有条件并通过AIC、BIC值自动确定阶数。
ARMA（以及statsmodels中的其他算法）跟sklearn中的算法类似，也都有方法和属性两种应用方式。常用的ARMA方法：

fit：使用卡尔曼滤波器的最大似然法拟合ARMA模型。
from_formula：基于给定的公式和数据框创建模型。
geterrors：基于给定的参数获取ARMA进程的错误。
hessian：基于给定的参数计算Hessian信息。
loglike：计算ARMA模型的对数似然。
loglike_css：条件求和方差似然函数。
predict：应用ARMA模型做预测。
score：得分函数。
