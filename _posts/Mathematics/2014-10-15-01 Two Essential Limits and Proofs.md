---
layout:     post   				    # 使用的布局（不需要改）
title:      Two Essential Limits and Proofs   	# 标题 
subtitle:   Two Essential Limits and Proofs #副标题
date:       2014-10-15				# 时间
author:     Leonard Meng						# 作者
header-img: img/post-banner-math.jpeg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true                       # 是否启用 MathJax
tags:								#标签
    - Mathmetics
    - Calculus
---
# 一、 $\lim_{x \to 0}\frac{\sin{x}}{x}=1$
Proof:
Obviously: $\sin{x} < x < \tan{x}$ 

$$ \therefore 1 < \frac{x}{\sin{x}} < \frac{1}{\cos{x}} $$

$$ \therefore \cos{x} < \frac{\sin{x}}{x} < 1 $$

$$ \because \lim_{x\to 0} \cos{x} = 1 $$ 

由夹逼准则得，$\lim_{x \to 0}\frac{\sin{x}}{x}=1$ 
# 二、$\lim_{x \to \infty}(1+\frac{1}{x})^x = e$ 

首先证明此极限存在，构造数列$x_n=(1+\frac{1}{n})^n$

 $$   \begin{aligned} x_{n} &=1+C_{n}^{1} \frac{1}{n}+C_{n}^{2} \frac{1}{n^{2}}+C_{n}^{3} \frac{1}{n^{3}}+\ldots+C_{n}^{n} \frac{1}{n^{n}} \\ &=1+n \cdot \frac{1}{n}+\frac{n(n-1)}{2 !} \cdot \frac{1}{n^{2}}+\frac{n(n-1)(n-2)}{3 !} \cdot \frac{1}{n^{3}}+\cdots+\frac{n(n-1)(n-2) \cdots 1}{n !} \cdot \frac{1}{n^{n}} \\ &=1+1+\frac{1}{2 !} \cdot\left(1-\frac{1}{n}\right)+\frac{1}{3 !} \cdot\left(1-\frac{1}{n}\right)\left(1-\frac{2}{n}\right)+\cdots+\frac{1}{n !} \cdot\left(1-\frac{1}{n}\right)\left(1-\frac{2}{n}\right) \\ \cdots(1-&\left.\frac{n-1}{n}\right) \\ &< 2+\frac{1}{2 !}+\frac{1}{3 !}+\cdots+\frac{1}{n !} \\ &< 2+\frac{1}{2}+\frac{1}{2^{2}}+\cdots++\frac{1}{2^{n-1}} \\ &= 3-\frac{1}{2^{n-1}}\\ &< 3 \end{aligned}  $$ 

对于$x_{n+1}$有 

$$ \begin{aligned} x_{n+1}=&\left(1+\frac{1}{n+1}\right)^{n+1} \\ =& 1+1+\frac{1}{2 !} \cdot\left(1-\frac{1}{n+1}\right)+\frac{1}{3 !} \cdot\left(1-\frac{1}{n+1}\right)\left(1-\frac{2}{n+1}\right)+\cdots+\\ & \frac{1}{n !} \cdot\left(1-\frac{1}{n+1}\right)\left(1-\frac{2}{n+1}\right) \cdots\left(1-\frac{n-1}{n+1}\right)+\\ & \frac{1}{(n+1) !} \cdot\left(1-\frac{1}{n+1}\right)\left(1-\frac{2}{n+1}\right) \cdots\left(1-\frac{n-1}{n+1}\right)\left(1-\frac{n}{n+1}\right) \\ >& x_{n} \end{aligned} $$ 
由单调有界数列必有极限可知，数列$x_{n}=\left(1+\frac{1}{n} \right )^{n}$的极限一定存在。
记此极限为$e$ 对于实数 x, 则总存在整数n,  使得$n \leqslant x \leqslant n+1$ 则有 
$$ \begin{gathered} \left(1+\frac{1}{n+1}\right)^{n}<\left(1+\frac{1}{x}\right)^{x}<\left(1+\frac{1}{n}\right)^{n+1} \\ \lim _{n \rightarrow \infty}\left(1+\frac{1}{n+1}\right)^{n}=\lim _{n \rightarrow \infty} \frac{\left(1+\frac{1}{n+1}\right)^{n+1}}{\left(1+\frac{1}{n+1}\right)}=\frac{\lim _{x \rightarrow \infty}\left(1+\frac{1}{n+1}\right)^{n+1}}{\lim _{x \rightarrow \infty}\left(1+\frac{1}{n+1}\right)} \\ \quad=\frac{e}{1+0}=e \\ \lim _{n \rightarrow \infty}\left(1+\frac{1}{n}\right)^{n+1}=\lim _{n \rightarrow \infty}\left(\left(1+\frac{1}{n}\right)^{n}\left(1+\frac{1}{n}\right)\right) \\ =\lim _{n \rightarrow \infty}\left(1+\frac{1}{n}\right)^{n} \lim _{n \rightarrow \infty}\left(1+\frac{1}{n}\right) \\ =e \cdot(1+0) \\ =e \end{gathered} $$    
根据两边夹定理，函数 $f(x)=\lim_{x\rightarrow\infty}\left ( 1+\tfrac{1}{x} \right )^{x}$的极限存在，为$e$ 