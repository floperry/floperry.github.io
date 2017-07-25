
---
title: Titanic之特征选择  
tags: 
  - 机器学习
  - Python
---


在上一节[Titanic之数据分析](http://floperry.github.io/2017/06/21/Titanic%E4%B9%8B%E6%95%B0%E6%8D%AE%E5%88%86%E6%9E%90/)中，简单的对样本数据的类型及分布进行了分析。在本节中，我们将根据上一节的分析，选择出与后续预测结果相关性较强的特征。  
<!--more-->

### 前期工作

导入数据分析所需要的包以及训练和测试样本数据


```python
# data analysis and wrangling
import pandas as pd
import numpy as np
import random as rnd

# visualization
import seaborn as sns
import matplotlib.pyplot as plt
%matplotlib inline
```


```python
train_df = pd.read_csv('train.csv')
test_df = pd.read_csv('test.csv')
combine = [train_df, test_df]
```

### 特征旋转分析

首先，对类别型（Sex）、序数型（Pclass）以及离散型（SibSp，Parch）等特征与结果Survived之间的相关性进行分析。   

**Sex & Survived**


```python
train_df[['Sex', 'Survived']].groupby(['Sex'], as_index=False).mean().sort_values(by='Survived', ascending=False)
```

<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Sex</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>female</td>
      <td>0.742038</td>
    </tr>
    <tr>
      <th>1</th>
      <td>male</td>
      <td>0.188908</td>
    </tr>
  </tbody>
</table>
</div>

**Pclass & Survived**


```python
train_df[['Pclass', 'Survived']].groupby(['Pclass'], as_index=False).mean().sort_values(by='Survived', ascending=False)
```

<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Pclass</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0.629630</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>0.472826</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>0.242363</td>
    </tr>
  </tbody>
</table>
</div>

**SibSp & Survived**


```python
train_df[['SibSp', 'Survived']].groupby(['SibSp'], as_index=False).mean().sort_values(by='Survived', ascending=False)
```

<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>SibSp</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.535885</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.464286</td>
    </tr>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.345395</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.250000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>0.166667</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>8</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>



**Parch & Survived**


```python
train_df[['Parch', 'Survived']].groupby(['Parch'], as_index=False).mean().sort_values(by='Survived', ascending=False)
```

<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Parch</th>
      <th>Survived</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>0.600000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.550847</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.343658</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>0.200000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>

简单分析一下以上四个特征：

- **Sex** 很明显，取值为Female的人群有极大的生存比例（74.20%），说明该特征是一个相关性较强的特征
- ** Pclass** 很明显，取值为1的人群有更高的生存比例（62.96%），说明该特征是一个相关性较强的特征。
- **SibSp和Parch** 这两个特征的某一个值对Survived而言并没有较强的相关性，可以考虑通过这两个特征来创造出一个新的特征。  

### 特征可视化分析

另一方面，我们可以通过可视化特征来对数值型特征进行相关性分析。比如，可以分析其直方图：  


```python
g = sns.FacetGrid(train_df, col='Survived', size=2.0, aspect=1.5)
g.map(plt.hist, 'Age', bins=20)
```

![png](\images\Titanic2\output_17_1.png)


不难看出，船上的乘客年龄大多分布在15-40这个范围内，其中，年龄在15-30之间的乘客存活率较低，相反，5岁以下以及75岁以上的乘客存活率较高，说明年龄与存活率之间有较强的相关性。考虑保留年龄作为我们需要的一个特征，同时后面将对年龄做分段处理。  

接下来，我们考虑多个特征变量与存活率之间的相关性，比如Pclass和Age：  


```python
grid = sns.FacetGrid(train_df, col='Survived', row='Pclass', size=2.0, aspect=1.5)
grid.map(plt.hist, 'Age', bins=20)
```

![png](\images\Titanic2\output_20_1.png)


简单分析一下以上直方图，Pclass=3的乘客最多，而且其中的大多数人都没有存活下来。相对的，Pclass=1的乘客大多数人都存活下来。在Pclass=2的人群中，年龄较小的乘客都存活下来。整体而言，Pclass随着Age分布而变化，且与Survived有较强的相关性，考虑将其保留作为一个特征。   

考虑Embarked、Pclass、Sex与Survived之间的相关性：  


```python
grid = sns.FacetGrid(train_df, col='Embarked', size=2.0, aspect=1.5)
grid.map(sns.pointplot, 'Pclass', 'Survived', 'Sex', palette='deep')
grid.add_legend()
```

![png](\images\Titanic2\output_23_1.png)


除了Embarked=C，在Embarked=S和Embarked=Q时，Sex=female的人群有明显较高的存活率。另外，随着Embarked的不同，不同Pclass的人群的存活率也不相同。因此，考虑保留Sex和Embarked这两个特征。  

考虑Embarked、Sex、Fare与Survived之间的相关性：  


```python
grid = sns.FacetGrid(train_df, row='Embarked', col='Survived', size=2.0, aspect=1.5)
grid.map(sns.barplot, 'Sex', 'Fare', ci=None)
```

![png](\images\Titanic2\output_26_1.png)


很直观的可以看出，Fare值越高，Survived的比重越大，考虑保留Fare特征，并在后面对其进行分段化。  

### 结束语   

到目前为止，我们通过分析各个特征变量与目标变量之间的相关性，选择出了与目标变量之间具有较强相关性的特征变量，如Sex、Age、SipSb、Parch、Pclass、Embarked、Fare等，后续我们将对这些变量进行进一步的处理，来完成我们的预测任务；而对于余下的特征变量，如PassengerId、Ticket、Cabin等，由于其或有较高的重复率，或有较多的缺失值，或具有较强的唯一性，导致其余目标变量之间的相关性较低，我们在后续的处理中可能考虑将其丢弃。在下一节中，我们将对数据集进行数据清洗。
