
---
title: Titanic之数据分析
tags: [机器学习, Python]
---

本文来源于数据挖掘竞赛Kaggle上的经典入门项目[Titanic: Machine Learning from Disaster](https://www.kaggle.com/c/titanic)。个人感觉对机器学习非常有帮助，谨以此文献给自己以及众多的Machine Learners。
<!-- more -->

---

### 项目描述：

在本项目中，我们需要分析符合哪些特征的乘客更容易在Titanic灾难中存活下来，换句话说，我们需要利用机器学习的方法来对Titanic中乘客的存活与否进行预测。乘客的主要特征如下：

| 特征        | 描述     |
| ----        | :----:   |
| PassengerId | 乘客编号 |
| Name        | 姓名     |
| Sex         | 性别     |
| Age         | 年龄     |
| SibSp       | 兄弟姐妹及配偶总人数 |
| Parch       | 父母及子女总人数     |
| Pclass      | 船票种类（1-高级，2-中级，3-低级） |
| Ticket      | 船票编号 |
| Fare        | 票价     |
| Cabin       | 客舱     |
| Embarked    | 出发港   |

在这篇文章中，我们的主要任务是对给定的数据进行分析，选择出能够很好的反映出乘客存活与否的特征，为下一步的预测做准备，这一过程也常被称为**特征工程**。

首先，导入常用的数据处理包


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

然后，使用pandas中的read_csv()方法读入训练数据和测试数据,顺便可以查看一下数据的表现形式


```python
train_df = pd.read_csv('train.csv')
test_df = pd.read_csv('test.csv')
combine = [train_df, test_df]
print(train_df.head())
```

      PassengerId  Survived  Pclass  Name                                              Sex     Age   SibSp    
    0  1            0         3       Braund, Mr. Owen Harris                           male    22.0  1        
    1  2            1         1       Cumings, Mrs. John Bradley (Florence Briggs Th... female  38.0  1        
    2  3            1         3       Heikkinen, Miss. Laina                            female  26.0  0        
    3  4            1         1       Futrelle, Mrs. Jacques Heath (Lily May Peel)      female  35.0  1        
    4  5            0         3       Allen, Mr. William Henry                          male    35.0  0        
       
       Parch  Ticket            Fare     Cabin  Embarked
    0  0      A/5 21171         7.2500   NaN    S       
    1  0      PC 17599          71.2833  C85    C       
    2  0      STON/O2. 3101282  7.9250   NaN    S       
    3  0      113803            53.1000  C123   S       
    4  0      373450            8.0500   NaN    S      

更直观的，我们可以查看数据中每个特征的详细信息


```python
# type of features
print(train_df.info())
print('-'*40)
print(test_df.info())
```

    <class 'pandas.core.frame.DataFrame'>                  <class 'pandas.core.frame.DataFrame'> 
    RangeIndex: 891 entries, 0 to 890                      RangeIndex: 418 entries, 0 to 417      
    Data columns (total 12 columns):                       Data columns (total 11 columns):       
    PassengerId    891 non-null int64                      PassengerId    418 non-null int64      
    Survived       891 non-null int64                      Pclass         418 non-null int64      
    Pclass         891 non-null int64                      Name           418 non-null object     
    Name           891 non-null object                     Sex            418 non-null object     
    Sex            891 non-null object                     Age            332 non-null float64    
    Age            714 non-null float64                    SibSp          418 non-null int64      
    SibSp          891 non-null int64                      Parch          418 non-null int64      
    Parch          891 non-null int64                      Ticket         418 non-null object     
    Ticket         891 non-null object                     Fare           417 non-null float64    
    Fare           891 non-null float64                    Cabin          91 non-null object      
    Cabin          204 non-null object                     Embarked       418 non-null object     
    Embarked       889 non-null object                     dtypes: float64(2), int64(4), object(5)
    dtypes: float64(2), int64(5), object(5)                memory usage: 36.0+ KB                 
    memory usage: 83.6+ KB                                                                        


```python

```
