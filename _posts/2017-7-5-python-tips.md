---
layout: post
title: Python Tips
date: 2017-7-5
categories: blog
tags: [技术总结]
description: Using python in Linux
---

## 两个维数相同的bool类型数组做与运算
np.nonzero(ok1 * ok2)[0]
其中ok1和ok2都是一个一维数组bool型的
如果加[0]会使得到的结果仍为一维数组

         > ok1*ok2
         array([False,  True, False, False], dtype=bool)
         > np.nonzero(ok1*ok2)[0]
         array([1])
         > np.nonzero(ok1*ok2)
         (array([1]),)


         > b1*b2
         array([False,  True,  True, False], dtype=bool)
         > np.nonzero(b1*b2)
         (array([1, 2]),)
         > np.nonzero(b1*b2)[0]
         array([1, 2])

## 文本读取

使用例如json从文本中读取到的数字得到的是文本， 需要自己加上int、float强制转换

## 两个数组合并/某个添加一列

在shape=（432,2）的数组上加上一列使用 np.hstack()

e.g. 
- pixel = np.zeros((439,2), dtype=)
- s = pixel.shape[:-1]+(1,)
- result = np.hstack((pixel,np.ones(s)))

在shape=（432,2）的数组上加上一列使用 np.colomn_stack(())

- pixel = np.zeros((439,2), dtype=int)
- s = np.ones(439, dtype=int)
- result = np.column_stack((pixel,s))

## 矢量求范数

bearing_b3 是一个n×3的矩阵
normbearing = np.linalg.norm(bearing_b3, axis=1)[:, np.newaxis]]

e.g.

         > bearing_b3
         array([[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9],
                [2, 5, 8]])

         > np.linalg.norm(bearing_b3, axis=1)[:, np.newaxis]
         array([[  3.74165739],
                [  8.77496439],
                [ 13.92838828],
                [  9.64365076]])

         > np.linalg.norm(bearing_b3, axis=1)
         array([  3.74165739,   8.77496439,  13.92838828,   9.64365076])

## cv2.Rodrigues

Python: cv2.Rodrigues(src[, dst[, jacobian]]) → dst, jacobian¶

R' = cv2.Rodrigues(R)[0]

Note that: [0]

