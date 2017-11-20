---
layout: post
title:  "wide and deep model"
date:   2016-11-20 16:44:40
categories: Algorithm
---

wide and deep model 是一种组合学习方法，依据feature的特性，将模型分为wide和deep部分。顾名思义，wide部分是强调模型广度，即使用大量的稀疏特征，来扩展模型。而deep部分，则关注深度，即对特征进行高度的抽样，来提取有用信息。最终通过损失函数将两个部分融合在一起。

下文将首先介绍基于keras和pandas，如何实现wide部分：

{% highlight python %}
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
Created on Mon Nov 20 13:37:20 2017

@author: zhangbo
"""

import pandas as pd

from sklearn.preprocessing import MinMaxScaler

import matplotlib.pyplot as plt

from keras.layers import Input, Dense
from keras.models import Model
from keras.optimizers import Adam

train = pd.read_csv('train.csv')
test = pd.read_csv('test.csv')

n_train = train.shape[0]
df = pd.concat([train, test], axis=0)

# add label
df['label'] = df['income_bracket'].apply(lambda x: '<=50K' in x).astype(int)

# 年龄分组:
age_groups = [0, 25, 65, 90]
age_labels = range(len(age_groups) - 1)
df['age_group'] = pd.cut(df['age'], bins=age_groups, labels=age_labels)

# columns for wide model
# 广度特征
wide_cols = ['age','hours_per_week','education', 'relationship', 'workclass',
             'occupation','native_country','gender']
#交叉特征
cross_cols = {'edu_occ': ['education', 'occupation'],
              'nat_occ': ['native_country', 'occupation']}
#合并交叉特征
wide_cols += list(cross_cols.keys())
#交叉特征合并，一般组合特征都是分类特征，即可以将二者合并为新分类特征
for k, v in cross_cols.items():
  df[k] = df[v].apply(lambda x: '-'.join(x), axis=1)

#提取wide数据
df_wide = df[['label'] + wide_cols]
#增加虚拟变量（encoder特征）
dummy_cols = [col for col in wide_cols if col in list(df.select_dtypes(['object']).columns)]

#虚拟变量
df_wide = pd.get_dummies(df_wide, columns=dummy_cols)

#构造数据集
train_X, train_y = df_wide.values[:n_train, 1:], df_wide.values[:n_train, :1]
test_X, test_y = df_wide.values[n_train:, 1:], df_wide.values[n_train:, :1]

#参数模型，需要对变量进行归一化
scaler = MinMaxScaler()
train_X = scaler.fit_transform(train_X)
test_X = scaler.fit_transform(test_X)

# model params
activation = 'sigmoid'
loss = 'binary_crossentropy'
metrics = ['accuracy']

# net params，lr可以看做是神经网络的一个特例，即隐藏节点只有一个，且激活函数是sigmoid函数
# 损失函数为二值交叉熵
hidden_units = 1

# optimizer paras
learning_rate = 0.1
batch_size = 128
epochs = 20

wide_input = Input(shape=(train_X.shape[1],), dtype='float32', name='wide_input')
w = Dense(hidden_units, activation=activation)(wide_input)
wide = Model(wide_input, w)
wide.compile(Adam(learning_rate), loss=loss, metrics=metrics)
history = wide.fit(train_X, train_y, epochs=epochs, batch_size=batch_size, 
                   validation_data=(test_X, test_y))

# 对比图
# 损失函数
plt.plot(history.history['loss'], label='train')
plt.plot(history.history['val_loss'], label='test')
plt.legend()
plt.show()

# 准确率
plt.plot(history.history['acc'], label='train')
plt.plot(history.history['val_acc'], label='test')
plt.legend()
plt.show()

# 测试集准确率
results = wide.evaluate(test_X, test_y)
print('测试集准确率: %.3f' % results[1])




{% endhighlight %}


参见:
 
1. [Keras][keras]
2. [MySQL connector][mysql connector]
3. [示例][aaa]
4. [hive to mysql 乱码][乱码]
5. [stack flow MySQL 编码][默认编码]


[keras]: https://keras-cn.readthedocs.io/en/latest/getting_started/functional_API/#functional
[git example]: https://github.com/jrzaurin/Wide-and-Deep-Keras/blob/master/wide_and_deep_keras.py 
[wide and deep]: https://www.tensorflow.org/tutorials/wide_and_deep



