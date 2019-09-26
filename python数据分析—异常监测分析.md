# python数据分析—异常监测分析

何为异常检测
在数据挖掘中，异常检测（anomaly detection）是通过与大多数数据显着不同而引起怀疑的稀有项目，事件或观察的识别。通常情况下，异常项目会转化为某种问题，例如银行欺诈，结构缺陷，医疗问题或文本错误。异常也被称为异常值，新奇，噪声，偏差和异常。

数据异常可以转化为各种应用领域中的重要（且常常是关键的）可操作信息。 例如，计算机网络中的异常流量模式可能意味着被黑客窃取的计算机在将敏感数据发送到未经授权的目的地；异常的MRI图像可能表明存在恶性肿瘤；信用卡交易数据的异常可能表明信用卡或身份盗用；或来自航天器传感器的异常读数可能意味着航天器某些部件的故障。我们将先介绍异常（值）的概念。

##### 常见的异常检测方法：

基于统计：基于泊松分布、正态分布找到异常分布点
基于距离：K-means
基于密度：knn，LOF(local outlier factor),隔离森林
one-class SVM
隐马尔可夫模型（HMM）
异常值检测常用于异常订单识别、风险客户预警、黄牛识别、贷款风险识别、欺诈检测、技术入侵等针对个体的分析场景。

## 新奇检测和离群检测

异常数据根据原始数据集的不同可以分为离群点检测和新奇检测：

#### 离群点检测(Outlier Detection)

大多数情况我们定义的异常数据都属于离群点检测，对这些数据训练完之后再在新的数据集中寻找异常点

#### 新奇检测(Novelty Detection)

所谓新奇检测是识别新的或未知数据模式和规律的检测方法，这些规律和只是在已有机器学习系统的训练集中没有被发掘出来。新奇检测的前提是已知训练数据集是“纯净”的，未被真正的“噪音”数据或真实的“离群点”污染，然后针对这些数据训练完成之后再对新的数据做训练以寻找新奇数据的模式。

新奇检测主要应用于新的模式、主题、趋势的探索和识别，包括信号处理、计算机视觉、模式识别、智能机器人等技术方向，应用领域例如潜在疾病的探索、新物种的发现、新传播主题的获取等。

新奇检测和异常检测有关，一开始的新奇点往往都以一种离群的方式出现在数据中，这种离群方式一般会被认为是离群点，因此二者的检测和识别模式非常类似。但是，当经过一段时间之后，新奇数据一旦被证实为正常模式，例如将新的疾病识别为一种普通疾病，那么新奇模式将被合并到正常模式之中，就不再属于异常点的范畴。

处理高维数据异常检测
拓展现有的离群点检测模式
发现子空间中的离群点
对高维数据进行建模
在大多数场景下通过非监督式方法实现的异常检测的结果只是用来缩小排查范围，为业务的执行提供更加精准和高效的执行目标而已

```
from sklearn.svm import OneClassSVM
import numpy as np
import plotly.offline as py
import plotly.graph_objs as go
py.init_notebook_mode(connected=True)

# 导入数据

data = np.loadtxt('https://raw.githubusercontent.com/ffzs/dataset/master/outlier.txt', delimiter=' ')

#分配训练集和测试集
train_set = data[:900, :]
test_set = data[-100:, :]

#### 异常检测

# 创建异常检测模型

one_svm = OneClassSVM(nu=0.1, kernel='rbf', random_state=2018)

# 训练模型

one_svm.fit(train_set)

# 预测异常数据

pre_test_outliers = one_svm.predict(test_set)

### 异常结果统计###

# 合并测试检测结果

total_test_data = np.hstack((test_set, pre_test_outliers.reshape(-1,1)))

# 获取正常数据

normal_test_data = total_test_data[total_test_data[:, -1] == 1]

# 获取异常数据

outlier_test_data = total_test_data[total_test_data[:, -1] == -1]

# 输出异常数据结果

print('异常数据为：{}/{}'.format(len(outlier_test_data), len(total_test_data)))

# 异常数据为：9/100

# 可视化结果

py.iplot([go.Scatter3d(x=total_test_data[:,0], y=total_test_data[:,1], z=total_test_data[:,2], 
                       mode='markers',marker=dict(color=total_test_data[:, -1], size=5))])

```

![1569414795063](C:\Users\83759\AppData\Roaming\Typora\typora-user-images\1569414795063.png)



### 

### OneClassSVM

One-Class SVM用于异常检测，它的基本原理是在给定的一组样本中，检测数据集的边界以便于区分新的数据点是否属于该类。

它是基于密度检测方法的一种，属于无监督学习算法，拟合过程由于不存在数据标签列，因此只需要输入一个矩阵X。

属于SVM的一种，可用于高维数据的异常检测。

