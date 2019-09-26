# python数据分析—分类分析

分类分析通过对已知类别训练集的计算和分析，从中发现类别规则并预测新数据的类别。

实现分类的算法，特别是在具体实现中，被称为分类器。术语“分类器”有时也指由分类算法实现的数学函数，其将输入数据映射到类别。

常用的分类算法包括朴素贝叶斯、逻辑回归、决策树、随机森林、支持向量机等。

主要用途是预测，例如信用评级、风险评价、欺诈预测等；

#### 防止分类模型的过拟合

过拟合是在做分类训练时模型由于过度学习了训练集的特征，使得训练集的准确率很高，但将模型应用到新的数据集时，准确率却很差

分类算法尤其是决策树容易出现过拟合，避免方法与使用更多的数据，降维，使用正则化方法，使用组合方法。

#### 使用关联算法做分类分析

在关联算法中，我们经常得到不同的关联规则，或称为频繁规则，这种模式用来做关联模式挖掘。也可分类分析，

从数据集中选择属性特征＋目标（可能是商品、内容、广告等），使用相关规则得到频繁项集，频繁项集的前、后项分别是属性（可以是单个或者多个）和目标。

#### 用分类分析来提炼规则、提取变量、处理缺失值

**1，分类分析用于提炼应用规则。**

​      预测是分类分析的主要应用方向，从分类算法中提炼特征规则，利用的是在构建算法过程中的分类规则。以决策树为例，决策树的分裂节点是表示局部最优值的显著特征值，每个节点下的特征变量以及对应的值的组合构成了规则。

**2，分类分析用于提取变量特征**

​      提取权重较高的的几个特征。这一应用是数据规约和数据降维的重要方式。

​      具体实现思路是：获取原始数据集并对数据做预处理，将预处理的数据集放到分类算法中进行训练，然后从算   法模型中提取特征权重信息。

**3，分类分析用于处理缺失值**

   方法之一是模型法——  基于已有的其他字段，将确实字段作为目标变量进行预测，补全。

如是分类变量，则采用分类模型补全。

## 类别划分——分类算法和聚类算法的比较

1，学习方式不同。聚类是非监督式学习算法，而分类是监督学习算法。

​       监督算法是有准确性的指示属性，用来告诉算法模型某种行为是对还是错。

2，对源数据集要求不同。聚类不要求源数据集由预先定义的标签，分类需要。

3，应用场景不一样。聚类多用于数据探索性分析，降维，压缩等探索性，分类算法多用于预测性分析。

4，解读结果不同 。聚类不同见解分类不同，分类解读一样。

5，模型评估指标不同。聚类没有明确的准确标准。分类有。

使用K均值方法得到的聚类结果已经显示了及时在没有标签的情况下做非监督学习，也能获得良好的划分类别的效果。K均值这种聚类算法的准确率高是因为能正确的划分群体类别



#### 提供客户需要的内容

分类：基于已有 的会员及其特定类别标签做分类模型

聚类：将新会员和现有的会员作为一个整体做聚类分析。

#### 分类分析算法的选取：

文本分类时用到最多的是朴素贝叶斯
训练集比较小，那么选择高偏差且低方差的分类算法效果逢高，如朴素贝叶斯、支持向量机、这些算法不容易过拟合。
训练集比较大，选取何种方法都不会显著影响准去度
省时好操作选着用支持向量机，不要使用神经网络
重视算法准确度，那么选择算法精度高的算法，例如支持向量机、随机森林。
想得到有关预测结果的概率信息，使用逻辑回归
需要清洗的决策规则，使用决策树

```
import numpy as np #导入numpy库
import pandas as pd 
from sklearn.model_selection import train_test_split  #数据分区库
from sklearn.tree import DecisionTreeClassifier, export_graphviz   #导入决策树库
from sklearn.metrics import accuracy_score, auc, confusion_matrix, f1_score, precision_score, recall_score, roc_curve # 导入评估指标模块
import prettytable # 导入表格库
import pydotplus   # 导入dot插件库
import matplotlib.pyplot as plt  #导入图像展示库
import seaborn as sns  
%matplotlib inline

###### 数据导入 准备
df = pd.read_csv('https://raw.githubusercontent.com/ffzs/dataset/master/glass.csv', usecols=['Na','Ca','Type'])
# 为了决策树图示简洁我们尽量减少分类，和特征值
dfs = df[df.Type < 3]
X = dfs[dfs.columns[:-1]].values   ## 获取特征值
y = dfs['Type'].values - 1    # 获取标签值
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=2018)   ## 将数据37分为测试集合训练集 
###### 训练分类模型
dt_model = DecisionTreeClassifier(random_state=2018)   # 决策树模型
dt_model.fit(X_train, y_train)   # 训练模型
pre_y = dt_model.predict(X_test) #使用测试集做模型效果检验
####输出模型概述

###### 混淆矩阵

confusion_m = confusion_matrix(y_test, pre_y)

df_confusion_m = pd.DataFrame(confusion_m, columns=['0', '1'], index=['0', '1'])

df_confusion_m.index.name = 'Real'
df_confusion_m.columns.name = 'Predict'

df_confusion_m
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181129134211585.png)

```
# 获取决策树的预测概率

y_score = dt_model.predict_proba(X_test)

# ROC

fpr, tpr, thresholds = roc_curve(y_test, y_score[:, [1]])

# AUC

auc_s = auc(fpr, tpr)

# 准确率

accuracy_s = accuracy_score(y_test, pre_y)

# 精准度

precision_s = precision_score(y_test, pre_y)

# 召回率

recall_s = recall_score(y_test, pre_y)

# F1得分

f1_s = f1_score(y_test, pre_y) 

# 评估数据制表

df_metrics = pd.DataFrame([[auc_s, accuracy_s, precision_s, recall_s, f1_s]], columns=['auc', 'accuracy', 'precision', 'recall', 'f1'], index=['结果'])

df_metrics

```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181129134243534.png)



```
#### 可视化ROC##### 

plt.figure(figsize=(8, 7))
plt.plot(fpr, tpr, label='ROC')  # 画出ROC曲线
plt.plot([0, 1], [0, 1], linestyle='--', color='k', label='random chance')  

# 画出随机状态下的准确率线

plt.title('ROC')  # 子网格标题
plt.xlabel('false positive rate')  # X轴标题
plt.ylabel('true positive rate')  # y轴标题
plt.legend(loc=0)
plt.savefig('x.png')

```



![在这里插入图片描述](https://img-blog.csdnimg.cn/20181129134314152.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

```
####保存决策树桂枝图为pdf####

# 决策树规则生成dot对象

dot_data = export_graphviz(dt_model, max_depth=5, feature_names=dfs.columns[:-1], filled=True, rounded=True)

# 通过pydotplus将决策树规则解析为图形

graph = pydotplus.graph_from_dot_data(dot_data)

# 将决策树规则保存为PDF文件

graph.write_pdf('tree.pdf')

# 保存为jpg图片

graph.write_jpg('xx.jpg')
————————————————
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181129134351484.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

#### 决策树节点信息：

rfm_score是分裂阈值，
gini 是在当前规则下的基尼指数，
nsamples是当前节点下的总样本量，
nvalue为正例和负例的样本数量

#### 混淆矩阵

![在这里插入图片描述](https://img-blog.csdnimg.cn/20181129134502117.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3RvbnlkejA1MjM=,size_16,color_FFFFFF,t_70)

##### 表示分类正确：

真正（True Positive, TP）：本来是正例，分类成正例。
假正（True Negative, TN）：本来是负例，分类成负例。

##### 表示分类错误：

假负（False Positive, FP）：本来是负例，分类成正例。
真负（False Negative, FN）：本来是正例，分类成负例。

#### 评估指标解释：

auc_s:AUC（Area Under Curve）, ROC曲线下的面积。ROC曲线一般位于y=x上方，因此AUC的取值范围一般在0.5和1之间。AUC越大，分类效果越好。
accuracy_s：准确率（Accuracy），分类模型的预测结果中将正例预测为正例、将负例预测为负例的比例，公式为：A = (TP + TN)/(TP + FN + FP + TN)，取值范围[0,1]，值越大说明分类结果越准确。
precision_s：精确度（Precision），分类模型的预测结果中将正例预测为正例的比例，公式为：P = TP/(TP+FP)，取值范围[0,1]，值越大说明分类结果越准确。
recall_s：召回率（Recall），分类模型的预测结果被正确预测为正例占总的正例的比例，公式为：R = TP/(TP+FN)，取值范围[0,1]，值越大说明分类结果越准确。
f1_s:F1得分（F-score），准确度和召回率的调和均值，公式为：F1 = 2 ＊ (P ＊R) / (P + R)，取值范围[0,1]，值越大说明分类结果越准确。

















