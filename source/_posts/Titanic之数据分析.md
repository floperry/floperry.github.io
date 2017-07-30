
---
title: Titanic之数据分析
tags: [机器学习, Python]
---

{% qnimg Titanic1/kaggle.png %}
<br>
本文来源于数据挖掘竞赛Kaggle上的经典入门项目[Titanic: Machine Learning from Disaster](https://www.kaggle.com/c/titanic)。个人感觉对机器学习非常有帮助，谨以此文献给自己以及众多的Machine learning Beginners。
<!-- more -->

---

### 项目描述

在本项目中，我们需要分析符合哪些特征的乘客更容易在Titanic灾难中存活下来，换句话说，我们需要利用机器学习的方法来对Titanic中乘客的存活与否进行预测。乘客的主要特征如下：

| 特征        | 描述     |
| ----        | ----     |
| PassengerId | &emsp;乘客编号 |
| Name        | &emsp;姓名     |
| Sex         | &emsp;性别     |
| Age         | &emsp;年龄     |
| SibSp       | &emsp;兄弟姐妹及配偶总人数 |
| Parch       | &emsp;父母及子女总人数     |
| Pclass      | &emsp;船票种类 |
| Ticket      | &emsp;船票编号 |
| Fare        | &emsp;票价     |
| Cabin       | &emsp;客舱     |
| Embarked    | &emsp;出发港   |

在问题的描述中，有以下三点需要我们重点关注一下：

1、在2224名乘客中，有1502人在这次灾难中丧生，换句话说，只有32%的人存活下来。  
2、乘客丧生的原因之一是由于没有足够的救生艇。  
3、排除运气好的因素，妇女、儿童以及上流社会的人在这次灾难中生存下来的几率更大。

在这篇文章中，我们的主要任务是对给定的数据进行分析，为下一步的特征选择做准备。

### 数据类型

首先，导入常用的数据处理包


```python
# data analysis and wrangling
import pandas as pd
import numpy as np
```

然后，使用pandas中的read_csv()方法读入训练数据和测试数据,顺便可以查看一下数据的表现形式。可以看到，在我们的训练集中，变量数据类型可以分为三大类，即分类数据、数值数据以及混合数据。其中，分类数据又包括类别型（Survived、Sex和Embarked）与序数型（Pclass），数值数据包括连续型（Age、Fare）与离散型（SibSp、Parch），混合数据是指同时含有数字和字母的变量，如此处的Ticket和Cabin。


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

更直观的，我们可以查看数据中每个特征的详细信息。很明显，训练集包含891个样本，对于每个样本中的特征变量，float类型的有2个（Age、Fare），int类型的有5个（PassengerId、Survived、Pclass、SibSp、Parch），object类型的有5个（Name、Sex、Ticket、Cabin、Embarked）。测试集包含418个样本，相比于训练集，缺少了Survived特征变量（因为是需要我们预测的）。另一方面，在训练集中，Age、Cabin和Embarked三个变量中存在缺失值，而在测试集中，Age、Fare和Embarked三个变量中存在缺失值。


```python
# type of features
print(train_df.info())
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

### 数据分布

明确了数据集中各个特征变量的类型之后，我们可以查看各个特征变量的数据分布情况。比如对于数值型变量：


```python
print(train_df.describe())
```

           PassengerId    Survived      Pclass         Age       SibSp       Parch        Fare
    count    891.000000  891.000000  891.000000  714.000000  891.000000  891.000000  891.000000
    mean     446.000000    0.383838    2.308642   29.699118    0.523008    0.381594   32.204208
    std      257.353842    0.486592    0.836071   14.526497    1.102743    0.806057   49.693429
    min        1.000000    0.000000    1.000000    0.420000    0.000000    0.000000    0.000000
    25%      223.500000    0.000000    2.000000   20.125000    0.000000    0.000000    7.910400
    50%      446.000000    0.000000    3.000000   28.000000    0.000000    0.000000   14.454200
    75%      668.500000    1.000000    3.000000   38.000000    1.000000    0.000000   31.000000
    max      891.000000    1.000000    3.000000   80.000000    8.000000    6.000000  512.329200
     

进一步的，我们可以对感兴趣的特征变量进行重点分析，进而得出一些初步的结论，比如：
1、每个乘客的编号都是唯一的；
2、在本次灾难中存活下来的乘客约占总数的38%（通过 train_df['Survived'].describe(percentiles=[.61, .62]) 查看）；  
3、超过75%的乘客没有同父母或子女共同出行（通过 train_df['Parch'].describe(percentiles=[.75, .8]) 查看）；  
4、将近30%的乘客是和兄弟姐妹或配偶共同出行（通过 train_df['SibSp'].describe(percentiles=[.68, .69]) 查看）；  
5、票价的变化非常大，大约只有不到1%的人票价高达$512（通过 train_df['Fare'].describe(percentiles=[.99]) 查看）；  
6、只有不到1%的乘客的年龄在65-80之间（通过 train_df['Age'].describe(percentiles=[0.99]) 查看）；  

对于类别型变量：


```python
print(train_df.describe(include=['O']))
```

                        Name   Sex    Ticket    Cabin Embarked
    count                 891   891       891      204      889
    unique                891     2       681      147        3
    top      Pernot, Mr. Rene  male  CA. 2343  B96 B98        S
    freq                    1   577         7        4      644
    

可以看出：  
1、船上没有姓名相同的乘客；  
2、男性大概占总人数的65%（577/891）；  
3、船票有将近22%的重复率（1-681/891）；  
4、船舱有重复的，表明存在多人共用一个船舱的情况；  
5、在S出发港上船的乘客最多。  

---

### 结束语

到此为止，对于项目中给出的数据集，我们已经进行了初步的分析，包括每个特征变量的数据类型以及数据分布。在下一篇文章Titanic之特征选择中，我们将根据本文的分析，选择出对我们的最终的预测尽可能有帮助的特征。
