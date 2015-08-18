---
layout: post
title:  "Python process iamges"
date:   2015-08-17 16:44:40
categories: Python
---

### 程序

{% highlight py3 %}

# -*- coding: utf-8 -*-
"""
Created on Mon Aug 17 16:46:10 2015

@author: zhangbo
"""

from PIL import Image
import numpy as np
import matplotlib.pyplot as plt
import time
from functools import reduce

i = Image.open('images/eight.png')
iar = np.array(i)
newIar = np.array(i)
print(iar)
iar.shape

plt.imshow(iar)
plt.show()

def threshold(imageArray, alpha):
    """
    图像处理函数，将彩色图像处理为黑白图像，
    原理是首先计算平均RGB颜色值，将其作为阈值，
    """    
    avgPixArray = []
    newArray = imageArray
    
    for eachRow in imageArray:
        for eachPix in eachRow:
            avgNum = reduce(lambda x,y: x+y, eachPix[:3]) / len(eachPix[:3])
            avgPixArray.append(avgNum)

    avgPix = reduce(lambda x, y: x+y, avgPixArray) / len(avgPixArray)
    avgPix = avgPix * alpha
    
    for eachRow in newArray:
        for eachPix in eachRow:
            if reduce(lambda x, y: x+y, eachPix[:3]) / len(eachPix[:3]) < avgPix:
                eachPix[0] = 255
                eachPix[1] = 255
                eachPix[2] = 255
                eachPix[3] = 255
            else:
                eachPix[0] = 0
                eachPix[1] = 0
                eachPix[2] = 0
                eachPix[3] = 255
    return newArray

newIar = np.array(i)
threshold(newIar, 0.1)
plt.imshow(newIar)

plt.imshow(iar)
plt.imshow(newIar)
plt.show()

{% endhighlight %}

参考文献：[Image Recognition using Machine Learning Techniques][ref]

[ref]: http://praful.org/img-1/



