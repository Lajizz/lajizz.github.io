---
title: Hamming Code
tags: 编码
---
# HammingCode 汉明码
## 首先明确一些基本信息
* 汉明码是一种前向纠错码
* 信息接收端通过添加冗余信息来进行前向纠错
* 设传输的信息是一个k位的比特串，添加的冗余信息是r位比特串，那么经过汉明编码所要传输的信息是k+r=n;
## 纠错手段
### 以纠错一位举例
* 此时必须要求r位纠错码能够正确指示信息某一位的出错的位置
* 假如说信息比特串：
        $X_1X_2X_3X_4\;=1010$
  采用 通过添加log2(4) +1 = 3位来进行纠错
  令 $X_5\;=\;X_1\oplus X_2\oplus X_3$  注：采用偶校验
  $X_6\;=\;X_1\oplus X_2\oplus X_4$ 
  $X_7\;=\;X_2\oplus X_3\oplus X_4$    
* 此时整个信息空间是
```
0000 000
0001 011
0010 101
0011 011
0100 111
0101 100
0110 010
0111 001
1000 110
1001 101
1010 011
1011 000
1100 001
1101 010
1110 100
1111 111
```
* 假如说1010 flip to 1000 那么传递的信息就是 1000 011 接收端经过计算发现不对，就会对其进行纠错（通过交集，找到错误）
## 纠错能力衡量-汉明距离
* 计算方法，两个同等长度的字符串不同的位数即为它们的汉明距离
* 在同一编码空间，所有汉明距离中最小的叫做最小汉明距离-d
* 衡量
  * 1.  当码组用于检测错误时，设可检测e个位的错误，则 d>=e+1 
  * 2.  若码组用于纠错，设可纠错t个位的错误，则 d>=2t+1
  * 3.  如果码组用于纠正t个错，检测e个错，则 d>=e+t+1

### 从信道编码看汉明码
* 汉明码属于线性分组码
* channal:生成矩阵为G k*n
 * $\overrightarrow Vrec=\overrightarrow{V_S}\ast\overrightarrow G$
 * $\overrightarrow G\;=\;\begin{bmatrix}1&0&0&0&1&1&0\\\0&1&0&0&1&1&1\\\0&0&1&0&1&0&1\\\0&0&0&1&0&1&1\end{bmatrix}$

### 汉明码的最大后验（MaxP)
* 设 0000-1111是16个概率相等的message, p(Ci) = 1/16 (非常重要)
* 采用最大似然法，通过得到的出错信息 C',找出与在C'发生的概率下最大概率的Ci
* $P(Ci,C')=P(C'\vert C_i)P(C_i)$
* $P(C')\;=\underset{}{\sum P(C'\vert C_i)P(C_i)\;}$
* $P(C_i\vert C')\;=\;\frac{P(Ci,C')}{P(C')}\;\;=\;\frac{P(C'\vert C_i)P(C_i)}{\sum P(C'\vert C_i)P(C_i)\;}=\frac{P(C'\vert C_i)}{\sum P(C'\vert C_i)}$
* 从上式可以看出 当Ci与C'最相近的时候，它的概率最大，我们译码就译成Ci,那这里最近很容易知道就是Ci与C的汉明距离最小
### 汉明码的纠错能力的理解
* 最小码距<=abs(d/2)
* $d=\underset{i\neq j}{min}\vert Ci\oplus Cj\vert$
### 汉明码的校验矩阵H
* H 大小 (n-k)*n 
* 要求$G\ast H^T=\overset\rightharpoonup0$
* 上述(7,4)汉明码的校验矩阵为
* $H=\begin{bmatrix}0&0&0&1&1&1&1\\\0&1&1&0&0&1&1\\\1&0&1&0&1&0&1\end{bmatrix}$ 
  * 可以看出H的每一列是一个二进制的数
* 最终要求检验出来的信息 $x\ast H^T = cG\ast H^T = =\overset\rightharpoonup0$
### 真实的译码
* $c'=c\oplus e$
* $c'H^T=(c\oplus e)H^T=eH^T$
* 在(7,4)汉明码 可以计算出$eH^T$是1*3的矩阵，共有8种输出，称为8种syndrome
* e叫做差错图样
* 每一个syndrome会有对应的很多个e组成它的陪集{ej1,ej2,...,ejr}按照概率从大到小的排列所以说当c'发生就认为就会按照ej1这种错进行纠错。

### 完备码：汉明码是一个完备码
* 完备码的条件：$\sum_{i=0}^r\begin{pmatrix}n\\\i\end{pmatrix}=2^{n-k}$
* 若$\sum_{i=0}^r\begin{pmatrix}n\\\i\end{pmatrix}>2^{n-k}$
  * 说明有两个位数不超过r位的错对应一个syndrome
* 若$\sum_{i=0}^r\begin{pmatrix}n\\\i\end{pmatrix}<2^{n-k}$
  * 有能力纠更多的错



 


