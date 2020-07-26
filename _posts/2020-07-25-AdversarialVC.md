---
title: Adversarial VC-dimension
author: Fan Yao
date: 2020-07-23 11:33:00 +0800
categories: [Blogging, CS]
tags: [Learning Theory]
math: true
---

## VC-dimension and Adversarial VC-dimension

This post aims to study the concept of adversarial VC-dimension in paper [PAC-learning in the presence of evasion adversaries](https://arxiv.org/pdf/1806.01471.pdf). First we introduce the basic notations used in the following discussion:

| Symbol   |      Usage      |
|:----------:|:-------------:|
| \\( \mathcal{X} \\) | Space of examples |
| \\( \mathcal{C}=\{-1,1\}\\) |    Set of classes   |
| \\( \mathcal{H}\subseteq (\mathcal{X} \rightarrow \mathcal{C}) \\) | Set of hypotheses (labelings of examples) |
|\\(R \subseteq \mathcal{X} \times \mathcal{X}\\)|Binary nearness relation, dipicting the ability of the adversary|

First we follow the notation in the paper and revisit the standard definition of VC-dimension:

**Definition 1.** The **Shattering Coefficient** \\( \sigma(\mathcal{H}, i) = \max_{y \in \mathcal{X}^i} \| \\{ (h(y_0), ..., h(y_{i-1})):h\in\mathcal{H} \\} \| \\)

**Definition 2.** \\( \mathcal{F} = \\{f \in (\mathcal{X} \times \mathcal{C} \rightarrow \\{0,1\\}) :f(y,c)=\mathbb{I}(h(y) \neq c), h\in \mathcal{H}\\} \\)
. The **Shattering Coefficient** in terms of the loss class \\( \sigma'(\mathcal{F}, i) = \max_{(y, c) \in \mathcal{X}^i \times \mathcal{C}^i} \| \\{(f(y_0, c_0), ..., f(y_{i-1}, c_{i-1})):f\in\mathcal{F}\\}\| \\)

It is easy to see that \\(\sigma(\mathcal{H}, i)=\sigma'(\mathcal{F}, i)\\) if \\(\mathcal{H}\\) is a binary hypotheses class. The standard VC-dimension can thus be given by

**Definition 3.** **VC-dimension** \\(VC(\mathcal{H})=\sup\{n\in\mathbb{N}:\sigma(\mathcal{H}, n)=2^n\} = \sup\{n\in\mathbb{N}:\sigma'(\mathcal{\lambda(H)}, n)=2^n\}\\), where \\(\lambda(h)= (y,c) \rightarrow \mathbb{I}(h(y) \neq c)\\).

To derive adversarial VC-dimension, we need to define the corrupted hypotheses class that takes into account the presence of the adversary. When considering a sample \\(x \in \mathcal{X}\\) as an adversary, we assume that \\(x\\) has the freedom to deviate its feature around a region \\(x\\), where the neighbor mapping \\(N\\) is given by the binary nearness relation \\(R\\). For a binary classifier \\(h\in \mathcal{H}\\), we define the following mapping where 

$$\kappa_R=x \rightarrow
\begin{equation}\label{eq:eg3}
    \begin{cases}
     -1,  \forall y \in N(x): h(y)=-1 \\
     1,  \forall y \in N(x): h(y)=1 \\
     \perp, \exists y_0,y_1 \in N(x): h(y_0)=-1, h(y_1)=1.
    \end{cases}
\end{equation}
$$

\\(\kappa_R(h)\\) gives the third prediction result \\(\perp\\) only when the classifier is not able to determine whether the manipulation of input has contaminated the result. The corrupted hypotheses class is given by \\(\tilde{\mathcal{H}}=\{\kappa_R(h):h\in \mathcal{H}\}\\).

**Definition 4.** **Adversarial VC-dimension** \\(AVC(\mathcal{H}, R)=\sup\{n\in\mathbb{N}:\sigma'(\lambda (\tilde{\mathcal{H}}), n)=2^n\}.\\)

## Reference

[^CzechMath]: [**Miroslav Fiedler and Vlastimil Ptak. On matrices with non-positive off-diagonal elements and positiveprincipal minors.Czechoslovak Mathematical Journal**](https://dml.cz/bitstream/handle/10338.dmlcz/100526/CzechMathJ_12-1962-3_6.pdf)


