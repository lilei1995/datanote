# python数据预处理 ：样本分布不均（过采样和欠采样）

何为样本分布不均：
样本分布不均衡就是指样本差异非常大，例如共1000条数据样本的数据集中，其中占有10条样本分类，其特征无论如何你和也无法实现完整特征值的覆盖，此时属于严重的样本分布不均衡。

为何要解决样本分布不均：
样本分部不均衡的数据集也是很常见的：比如恶意刷单、黄牛订单、信用卡欺诈、电力窃电、设备故障、大企业客户流失等。
样本不均衡将导致样本量少的分类所包含的特征过少，很难从中提取规律，即使得到分类模型，也容易产生过度依赖于有限的数量样本而导致过拟合问题，当模型应用到新的数据上时，模型的准确性和健壮性将会很差。

### 样本分布不均的解决方法：

#### 过采样 

通过增加分类中样本较少的类别的采样数量来实现平衡，最直接的方法是简单复制小样本数据，缺点是如果特征少，会导致过拟合的问题。经过改进的过抽样方法通过在少数类中加入随机噪声、干扰数据或通过一定规则产生新的合成样本。

#### 欠采样 

通过减少分类中多数类样本的数量来实现样本均衡，最直接的方法是随机去掉一些多数类样本来减小多数类的规模，缺点是会丢失多数类中的一些重要信息。

#### 设置权重 

对不同样本数量的类别赋予不同的权重（通常会设置为与样本量成反比）

#### 集成方法 

每次生成训练集时使用所有分类中的小样本量，同时从分类中的大样本量中随机抽取数据来与小样本量合并构成训练集，这样反复多次会得到很多训练集和训练模型。最后在应用时，使用组合方法（例如投票、加权投票等）产生分类预测结果。这种方法类似于随机森林。缺点是，比较吃计算资源，费时。



# PYthon 代码实现

```
# 生成不平衡分类数据集

from collections import Counter
from sklearn.datasets import make_classification
X, y = make_classification(n_samples=3000, n_features=2, n_informative=2,
                           n_redundant=0, n_repeated=0, n_classes=3,
                           n_clusters_per_class=1,
                           weights=[0.1, 0.05, 0.85],
                           class_sep=0.8, random_state=2018)
Counter(y)

# Counter({2: 2532, 1: 163, 0: 305})

# 使用RandomOverSampler从少数类的样本中进行随机采样来增加新的样本使各个分类均衡

from imblearn.over_sampling import RandomOverSampler

ros = RandomOverSampler(random_state=0)
X_resampled, y_resampled = ros.fit_sample(X, y)
sorted(Counter(y_resampled).items())

# [(0, 2532), (1, 2532), (2, 2532)]

# SMOTE: 对于少数类样本a, 随机选择一个最近邻的样本b, 然后从a与b的连线上随机选取一个点c作为新的少数类样本

from imblearn.over_sampling import SMOTE

X_resampled_smote, y_resampled_smote = SMOTE().fit_sample(X, y)

sorted(Counter(y_resampled_smote).items())

# [(0, 2532), (1, 2532), (2, 2532)]

# ADASYN: 关注的是在那些基于K最近邻分类器被错误分类的原始样本附近生成新的少数类样本

from imblearn.over_sampling import ADASYN

X_resampled_adasyn, y_resampled_adasyn = ADASYN().fit_sample(X, y)

sorted(Counter(y_resampled_adasyn).items())

# [(0, 2522), (1, 2520), (2, 2532)]

# RandomUnderSampler函数是一种快速并十分简单的方式来平衡各个类别的数据: 随机选取数据的子集.

from imblearn.under_sampling import RandomUnderSampler
rus = RandomUnderSampler(random_state=0)
X_resampled, y_resampled = rus.fit_sample(X, y)

sorted(Counter(y_resampled).items())

# [(0, 163), (1, 163), (2, 163)]

# 在之前的SMOTE方法中, 当由边界的样本与其他样本进行过采样差值时, 很容易生成一些噪音数据. 因此, 在过采样之后需要对样本进行清洗. 

# 这样TomekLink 与 EditedNearestNeighbours方法就能实现上述的要求.

from imblearn.combine import SMOTEENN
smote_enn = SMOTEENN(random_state=0)
X_resampled, y_resampled = smote_enn.fit_sample(X, y)

sorted(Counter(y_resampled).items())

# [(0, 2111), (1, 2099), (2, 1893)]

from imblearn.combine import SMOTETomek
smote_tomek = SMOTETomek(random_state=0)
X_resampled, y_resampled = smote_tomek.fit_sample(X, y)

sorted(Counter(y_resampled).items())

# [(0, 2412), (1, 2414), (2, 2396)]

# 使用SVM的权重调节处理不均衡样本 权重为balanced 意味着权重为各分类数据量的反比

from sklearn.svm import SVC  
svm_model = SVC(class_weight='balanced')
svm_model.fit(X, y)

# # EasyEnsemble 通过对原始的数据集进行随机下采样实现对数据集进行集成.

# EasyEnsemble 有两个很重要的参数: (i) n_subsets 控制的是子集的个数 and (ii) replacement 决定是有放回还是无放回的随机采样.

from imblearn.ensemble import EasyEnsemble
ee = EasyEnsemble(random_state=0, n_subsets=10)
X_resampled, y_resampled = ee.fit_sample(X, y)
sorted(Counter(y_resampled[0]).items())

# [(0, 163), (1, 163), (2, 163)]

# BalanceCascade(级联平衡)的方法通过使用分类器(estimator参数)来确保那些被错分类的样本在下一次进行子集选取的时候也能被采样到. 同样, n_max_subset 参数控制子集的个数, 以及可以通过设置bootstrap=True来使用bootstraping(自助法).

from imblearn.ensemble import BalanceCascade
from sklearn.linear_model import LogisticRegression
bc = BalanceCascade(random_state=0,
                    estimator=LogisticRegression(random_state=0),
                    n_max_subset=4)
X_resampled, y_resampled = bc.fit_sample(X, y)

sorted(Counter(y_resampled[0]).items())

# [(0, 163), (1, 163), (2, 163)]

# BalancedBaggingClassifier 允许在训练每个基学习器之前对每个子集进行重抽样. 简而言之, 该方法结合了EasyEnsemble采样器与分类器(如BaggingClassifier)的结果.

from sklearn.tree import DecisionTreeClassifier
from imblearn.ensemble import BalancedBaggingClassifier
bbc = BalancedBaggingClassifier(base_estimator=DecisionTreeClassifier(),
                                ratio='auto',
                                replacement=False,
                                random_state=0)
bbc.fit(X, y) 

```

