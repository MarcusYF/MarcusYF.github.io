---
title: Mirror Descent
author: Fan Yao
date: 2020-07-12 11:33:00 +0800
categories: [Blogging, CS]
tags: [Online Optimization]
math: true
---

## Follow the Leader

**Definition 1.** We shall denote by \\(  \mathcal{Z} \\) the class of all real square matrices whose off-diagonal elements are non-positive.

**Remark** : A continuous N-player game defined on compact strategy set is supermodular \\( \Longleftrightarrow  \\) For all strategy profile \\( x \\), \\( -J(x) \in \mathcal{Z} \\).

**Definition 2.** Denote \\(  \mathcal{K} \subset \mathcal{Z} \\) as \\(\mathcal{K}=\{A \in \mathcal{Z}| Re(\lambda)>0, \forall \lambda \in spec(A)\} \\), where the set of eigenvalues of \\(A\\) is denoted as \\(\text{spec}(A)\\). 

**Claim** : In a continuous supermodular game, \\( -J(x^*) \in \mathcal{K} \\) iff \\( x^ * \\) is a LASE. The argument is straightforward: let's examine how the configuration of \\(\text{spec}(J)\\) affects the limit behavior of GD dynamics. WLOG, let \\( x^ * =0\\), the GD dynamics with learning rate \\(\eta\\) writes

$$x

$$J(a)=
{\left[ \begin{array}{ccc}
-4  & 2  & 0\\
0  &  -5 &  1 \\
a &  0  &  -3
\end{array} 
\right ]},$$

and a 3-player supermodular game with each players' utility being

$$
\begin{equation}\label{eq:eg3}
    \begin{cases}
     u_1(x, y, z) = -2x^2+2xy, \\
     u_2(x, y, z) = -2.5y^2+yz, \\
     u_3(x, y, z) = -1.5z^2+azx.
    \end{cases}
\end{equation}
$$

## Reference

[^CzechMath]: [**Miroslav Fiedler and Vlastimil Ptak. On matrices with non-positive off-diagonal elements and positiveprincipal minors.Czechoslovak Mathematical Journal**](https://dml.cz/bitstream/handle/10338.dmlcz/100526/CzechMathJ_12-1962-3_6.pdf)


