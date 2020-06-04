---
layout:     post			    # 使用的布局（不需要改）
title:      tensorflow2学习/和pytorch对比 				# 标题 
subtitle:   始于2020 06~ #副标题
date:       2020-06-01				# 时间
author:     Tao				# 作者
header-img: img/post-bg-swift.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - tensorflow
---

## 相关网址
[pytorch](https://pytorch.org/docs/stable/index.html)

[tensorflow2](https://www.tensorflow.org/api_docs/python)

## 总体介绍

## 基础
### Tensor操作
#### Tensor定义
```python
# pytorch
# 大写的是一个用法，只有tensor不同
torch.Tensor(2, 4) # create a float32 [2, 4] shape Tensor
torch.Tensor([2, 4]) # create a float32 [2] shape Tensor
torch.tensor(2) # a int64 number of shape []
torch.tensor([1, 2, 3, 4]) # create a int64 [4] shape Tensor
torch.LongTensor(2, 4) # int64 shape: [2, 3]
torch.LongTensor([2, 4]) # int64 shape: [2]
# other type
torch.IntTensor # int32
torch.ShortTensor # int16
torch.ByteTensor # unsigned int8
torch.CharTensor # int8
torch.DoubleTensor # float64
torch.FloatTensor # float32 default Tensor
torch.HalfTensor # float16
torch.BoolTensor # bool True if value != 0

# tensorflow2
tf.constant(value=[], shape=(), dtype=type)
# type includes tf.int8/16/32/64 tf.float16/32/64
# shape can reshape value

# range
torch.arange(2, 10, dtype=torch.int16)
tf.range(2, 10, dtype=tf.int32)
```

### Layer定义

### 常用函数

## 简单示例

## 数据读取保存相关
