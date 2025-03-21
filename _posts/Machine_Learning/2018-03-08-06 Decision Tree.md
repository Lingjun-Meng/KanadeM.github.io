---
layout:     post   				    # 使用的布局（不需要改）
title:      Decision Tree   	# 标题 
subtitle:   Decision Tree #副标题
date:       2018-03-08 				# 时间
author:     Leonard Meng						# 作者
header-img: img/post-banner-ai.jpeg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true                       # 是否启用 MathJax
tags:								#标签
    - Machine Learning
    - Decision Tree
    - AI
---

# Chapter 0 DataSet

| Outlook  | Temperature | Humidity | Windy | Play |
| -------- | ----------- | -------- | ----- | ---- |
| sunny    | hot         | high     | FALSE | no   |
| sunny    | hot         | high     | TRUE  | no   |
| overcast | hot         | high     | FALSE | yes  |
| rainy    | mild        | high     | FALSE | yes  |
| rainy    | cool        | normal   | FALSE | yes  |
| rainy    | cool        | normal   | TRUE  | no   |
| overcast | cool        | normal   | TRUE  | yes  |
| sunny    | mild        | high     | FALSE | no   |
| sunny    | cool        | normal   | FALSE | yes  |
| rainy    | mild        | normal   | FALSE | yes  |
| sunny    | mild        | normal   | TRUE  | yes  |
| overcast | mild        | high     | TRUE  | yes  |
| overcast | hot         | normal   | FALSE | yes  |
| rainy    | mild        | high     | TRUE  | no   |

# Chapter 1 Decision Tree

## 1.1 From Decision Stump to Decision Tree

### 1.1.1 0-R Baseline

There are several commonly used baselines in machine learning. Among them, 0-R baseline is a simple statistics of the most labels in the label, and then all instances are predicted to be this label.

<img src="./static/img/image-20210705161222888.png" alt="image-20210705161222888" style="zoom:50%;" />  

Based on the statistical results of the label in the above training set, we can predict all instances as play=yes.

Total errors = $\frac{5}{14}$

### 1.1.2 1-R Baselince(Decision Stump)

1-R baseline is based on 0-R baseline, selecting an attribute for decision classification.

Using Outlook:

<img src="./static/img/image-20210705162935868.png" alt="image-20210705162935868" style="zoom:50%;" /> 

According to the above statistical results, when outlook=sunny, we predict play=no. When outlook=overcast or rainy, we predict play=no.

Total errors = $\frac{4}{14}$

### 1.1.3 Decision Tree

The decision tree is based on the 1-R baseline using a variety of attributes to build a decision tree.

<img src="./static/img/image-20210705164249303.png" alt="image-20210705164249303" style="zoom:50%;" />

When does the decision tree terminate when it is built?

1. All the leaf nodes are (largely) dominated by a single class (that is the leaf nodes are nearly pure).
2. All attributes have been used.

# Chapter 2 Decision tree generation algorithm(ID3)

We have already understood the use of decision trees. So how should a decision tree be optimized? In other words, how do we generate an optimal decision tree?

==**Optimal construction of a Decision Tree is NP hard (non-deterministic polynomial).**==

## 2.0 Criterion for Attribute Selection

We want to get the smallest tree (Occam’s Razor; generalisability). Prefer the shortest hypothesis that fits the data.
In favor: 

 - Fewer short hypotheses than long hypotheses
    - a short hyp. that fits the data unlikely to be a coincidence
    - a long hyp. that fits data might be a coincidence

## 2.1 Introduction

Basic method: recursive divide-and-conquer

FUNCTION ID3 (Root) 

​	IF all instances at root have same class

​		THEN stop

​	ELSE 

​		**Step 1** Select a new attribute to use in partitioning root node instances 

​		**Step 2** Create a branch for each attribute value and parti- tion up root node instances according to each value

​		**Step 3** Call ID3(LEAFi) for each leaf node $\text{LEAF}_i$

## 2.2 Predefined

### 2.2.1 Entropy

==**The expected (average) level of surprise or uncertainty.**==

- ==**Low probability event**==: if it happens, it’s big news! High surprise! ==**High information!**==, ==**unpredictable**==
- ==**High probability event**==: it was likely to happen anyway. Not very surprising. ==**Low information!**==, ==**predictable**==

**Definition**

A measure of ==**unpredictability**==
Level of unpredictability (surprise) for a single event $i:$ self-information
$$
\text { self-info }(i)=\frac{1}{P(i)}=-\log _{2} P(i)
$$
Given a probability distribution, the information (in bits) required to predict an event is the distribution''s entropy or information value
The entropy of a discrete random event $x$ with possible outcomes $x_{1}, . . x_{n}$ is:
$$
\begin{aligned}
H(x) &=\sum_{i=1}^{n} P\left(x_{i}\right) \text { self-info }\left(x_{i}\right) \\
&=-\sum_{i=1}^{n} P\left(x_{i}\right) \log _{2} P\left(x_{i}\right)
\end{aligned}
$$
where $0 \log _{2} 0={ }^{\text {def }} 0$

**Example 1**

Biased coin. 55 flips: $50 \mathrm{x}$ head, $5 \mathrm{x}$ tail:
$$
\begin{aligned}
H &=-\left[\frac{50}{55} \log _{2}\left(\frac{50}{55}\right)+\frac{5}{55} \log _{2}\left(\frac{5}{55}\right)\right] \\
& \approx 0.44 \mathrm{bits}
\end{aligned}
$$
Fair coin. 55 flips: $30 \mathrm{x}$ head, $25 \mathrm{x}$ tail:
$$
\begin{aligned}
H &=-\left[\frac{30}{55} \log _{2}\left(\frac{30}{55}\right)+\frac{25}{55} \log _{2}\left(\frac{25}{55}\right)\right] \\
& \approx 0.99 \mathrm{bits}
\end{aligned}
$$
The more uncertainty, the higher the entropy.

**Example 2**

In the context of Decision Trees, we are looking at the class distribution at a node:
$50 \mathrm{Y}$ instances, $5 \mathrm{~N}$ instances:
$$
\begin{aligned}
H &=-\left[\frac{50}{55} \log _{2}\left(\frac{50}{55}\right)+\frac{5}{55} \log _{2}\left(\frac{5}{55}\right)\right] \\
& \approx 0.44 \mathrm{bits}
\end{aligned}
$$

- $30 \mathrm{Y}$ instances, $25 \mathrm{~N}$ instances:

$$
\begin{aligned}
H &=-\left[\frac{30}{55} \log _{2}\left(\frac{30}{55}\right)+\frac{25}{55} \log _{2}\left(\frac{25}{55}\right)\right] \\
& \approx 0.99 \mathrm{bits}
\end{aligned}
$$

We want to classify with high certainty. ==**We want leaves with low entropy!**==

### 2.2.2 Mean Information

The weighted average of the entropy over the children after the split. 

We calculate the mean information for a tree stump with $m$ attribute values as:
$$
\operatorname{Mean} \operatorname{Info}\left(x_{1}, . ., x_{m}\right)=\sum_{i=1}^{m} P\left(x_{i}\right) H\left(x_{i}\right)
$$
where $H\left(x_{i}\right)$ is the entropy of the class distribution for the instances at node $x_{i}$
and $P\left(x_{i}\right)$ is the proportion of instances at sub-node $x_{i}$

### 2.2.3 Information Gain

Decision tree with low entropy: class is more predictable.

‘==**Reduction of entropy**== before and after the data is partitioned using the attribute A’.

Select the attribute that has ==**largest information gain**==: the most entropy (==**uncertainty**==) is reduced, class is ==**most predictable**==.

If the entropy ==**decreases**==, then we have a better tree (more predictable)

- the entropy before splitting the tree using the attribute’s values

- We determine which attribute $R_{A}$ (with values $x_{1}, \ldots x_{m}$ ) best partitions the instances at a given root node $R$ according to information gain (IG):
  $$
  \begin{aligned}
  I G\left(R_{A} \mid R\right) &=H(R)-\operatorname{mean}-\operatorname{info}\left(R_{A}\right) \\
  &=H(R)-\sum_{i=1}^{m} P\left(x_{i}\right) H\left(x_{i}\right)
  \end{aligned}
  $$

## 2.3 Example

### 2.3.1 Root Node: Label

$$
\begin{aligned}
\mathrm{H}(\mathrm{R}) &=-\left(\left(\frac{9}{14}\right) \log _{2}\left(\frac{9}{14}\right)+\left(\frac{5}{14}\right) \log _{2}\left(\frac{5}{14}\right)\right) \\
&=-(-.4098-0.5305)=0.940
\end{aligned}
$$

### 2.3.2 Entropy

Here I will calculate based on outlook.

<img src="./static/img/image-20210706204516120.png" alt="image-20210706204516120" style="zoom:50%;" />

$$
\begin{aligned}
\mathrm{H}(\text { rainy }) \quad &\left.=-\left(\left(\frac{3}{5}\right) \log _{2}\left(\frac{3}{5}\right)+\left(\frac{2}{5}\right) \log _{2}\left(\frac{2}{5}\right)\right)\right) \\
&=-(-0.4422-0.5288)=0.971 \\

\mathrm{H} \text { (overcast) } &\left.=-\left(\left(\frac{4}{4}\right) \log _{2}\left(\frac{4}{4}\right)+\left(\frac{0}{4}\right) \log _{2}\left(\frac{0}{4}\right)\right)\right) \\
&=0 \\
\mathrm{H}(\text { sunny }) &\left.=-\left(\left(\frac{2}{5}\right) \log _{2}\left(\frac{2}{5}\right)+\left(\frac{3}{5}\right) \log _{2}\left(\frac{3}{5}\right)\right)\right) \\
&=0.971

\end{aligned}
$$

### 2.3.4 Mean Info

$$
\begin{aligned}
&\text{Mean Info(Outlook)}\\
&=P(\text {rainy}) H(\text {rainy})+P(\text {overcast}) H(\text {overcast})+P(\text {sunny }) H(\text {sunny}) \\
&=5 / 14 * 0.971+0+5 / 14 * 0.971=0.693
\end{aligned}
$$

Mean Info(Temperature) = 0.911

Mean Info(Humidity) = 0.787

Mean Info(Windy) = 0.892

### 2.3.5 Information Gain


$$
\begin{aligned}
&I G\left(R_{A} \mid R\right) =H(R)-\operatorname{mean}-\operatorname{info}\left(R_{A}\right) \\
&=H(R)-\sum_{i=1}^{m} P\left(x_{i}\right) H\left(x_{i}\right) \\
&\color{red}{I G(\text { outlook} \mid R) =0.247} \\
&I G(\text { temperature } \mid R) =0.029 \\
&I G(\text { humidity } \mid R) =0.152 \\
&I G(w i n d y \mid R) =0.048
\end{aligned}
$$

Based on above calculation, we should choose ==**outlook**== as the node of next level.

## 2.4 Shortcomings of Information Gain

### 2.4.1 Shortcomings

Information gain tends to ==**prefer highly-branching attributes:**==

- A subset of instances is more likely to be homogeneous (pure) if there are only a few instances
- Attribute with many values will have fewer instances at each child node 

This ==**may result in overfitting / fragmentation**==

### 2.4.2 Example

<img src="./static/img/image-20210706210150215.png" alt="image-20210706210150215" style="zoom:50%;" />

==**Mean info = 0**==

## 2.5 Solution

### 2.5.1 Split info (SI)

Split info (SI) is the entropy of a given split (evenness of the distribution of instances to attribute values)
$$
\begin{aligned}
G R\left(R_{A} \mid R\right) &=\frac{I G\left(R_{A} \mid R\right)}{S I\left(R_{A} \mid R\right)}=\frac{I G\left(R_{A} \mid R\right)}{H\left(R_{A}\right)} \\
&=\frac{H(R)-\sum_{i=1}^{m} P\left(x_{i}\right) H\left(x_{i}\right)}{-\sum_{i=1}^{m} P\left(x_{i}\right) \log _{2} P\left(x_{i}\right)}
\end{aligned}
$$

$$
\begin{aligned}
&S l(\text { outlook } \mid R) \\
&=-\left(\left(\frac{5}{14}\right) \log _{2}\left(\frac{5}{14}\right)+\left(\frac{4}{14}\right) \log _{2}\left(\frac{4}{14}\right)+\left(\frac{5}{14}\right) \log _{2}\left(\frac{5}{14}\right)\right) \\
&=1.577
\end{aligned}
$$

### 2.5.2 Split info (SI)

Gain ratio (GR) reduces the bias for information gain towards highly-branching attributes by normalising relative to the split information
$$
\begin{array}{lrl}
&IG(\text { outlook } \mid R) &=0.247 \\
&SI(\text { outlook } \mid R) &=1.577 \\
&GR(\text { outlook } \mid R) &=0.156 \\
&IG(\text { humidity } \mid R) &=0.152 \\
&SI(\text { humidity } \mid R)  &=1.000 \\
&GR(\text { humidity } \mid R)  &=0.152 \\
&I G(\text { temperature } \mid R) & =0.029 \\
&S I(\text { temperature } \mid R) & =1.557 \\
&G R(\text { temperature } \mid R) & =0.019 \\
&I G(\text { windy } \mid R) & =0.048 \\
&S I(\text { windy } \mid R) & =0.985 \\
&G R(\text { windy } \mid R) & =0.049
\end{array}
$$
Based on the calculation, we should choose outlook

## 2.6 Stopping criteria

- We recurse until the instances at a node ==**are of the same class**==
- This is consistent with our usage of entropy: if all of the instances are of a single class, the ==**entropy of the distribution is 0**==
- Considering other attributes cannot “improve” an entropy of 0 — the Info Gain is 0 by definition
- The Info Gain/Gain Ratio allows us to choose the (seemingly) best attribute at a given node
- However, it is also an approximate indication of how much absolute improvement we expect from partitioning the data according to the values of a given attribute
- An Info Gain of 0 means that there is no improvement; a very small improvement is often unjustifiable
- Typical modification of ID3: choose best attribute only if IG/GR is greater than some threshold τ
- Other similar approaches use pruning — post-process the tree to remove undesirable branches (with few instances, or small IG/GR improvements)
- We might observe improvement through every layer of the tree
- We then run out of attributes, even though one or more leaves could be improved further
- Fall back to majority class label for instances at a leaf with a mixed distribution — unclear what to do with ties
- Possibly can be taken as evidence that the given attributes are insufficient for solving the problem

# Chapter 3 Discussion

## 3.1 Hypothesis Space Search in ID3

ID3 (and DT learning in general) is an instance of ==**combinatorial optimization**== 

- ID3 can be characterized as searching a space of hypotheses for one that fits the training examples.

- The hypothesis space searched by ID3 is the set of possible decision trees.
- ID3 performs a greedy simple-to-complex search through this hypothesis space (with no backtracking)
  - beginning with the empty tree
  - considering progressively more elaborate hypotheses in search of a decision tree that correctly classifies the training data

## 3.2 Pros / Cons of Decision Trees

### 3.2.1 Pros

- Highly regarded among basic supervised learnersFa
- st to train, even faster to classify
- Very transparent (probably the most interpretable of all classification algorithms!)

### 3.2.2 Cons

- Prone to Overfitting
  - Our goal is to reduce the entropy to 0
- Loss of information for continuous variables
- Complex calculation if there are many classes
- No guarantee to return the globally optimal decision
- Information gain: Bias for attributes with greater no. of values.
