---
title: Extended Euclidean algorithm 
tags: IS_Algorithm
---

# 扩展欧几里得算法
* 记录一下，从欧几里得算法（又称辗转相除法）扩展求大整数的逆元

## 原始欧几里得算法
* 求解最大公约数算法
```python
def gcd(a,b):
    if b == 0:
        return a
    else
        return gcd(b,a%b)
```

## 扩展欧几里得算法-（a,b）=1 有 ax+by=gcd(a,b)=1
* 通过计算欧几里得的过程如图所示的一个线性过程,得出x,x为a逆元(modb 假设a < b)

* 或者表示为求方程ax+by=1的解，

  ![image-20201010190838413](http://Lajizz.github.io/assets/image/2020-10-10.JPG)
```python
def Ext_gcd(a,b):
    if  b == 0:
        return 1,0,a
    x,y,q = Ext_gcd(a,b)
    x,y = y,(x-(a//b)*y)
    return x,y,q
```

## python设置递归次数
```python
import sys
sys.setrecursionlimit(1500) # set the maximum depth as 1500
```