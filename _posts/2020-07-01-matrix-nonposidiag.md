---
title: On matrices with non-positive off-diagonal elements
author: Fan Yao
date: 2020-07-01 11:33:00 +0800
categories: [Blogging, CS]
tags: [Game Theory]
math: true
---

## Matrices of class \\(\mathcal{Z}\\)

In the previous post, I introduced supermodular game and we know that when each player's utility \\(u_i\\) is \\(C^2\\), the definition of such game is essentially \\(\frac{\partial^2u_i}{\partial s_i \partial s_j} \geq 0, \forall i \in N, j \neq i \\). Note that the Jacobian of \\( u = (u_1, \cdots, u_N) \\) writes

$$
J={\left[ \begin{array}{cccc}
\frac{\partial^2u_1}{\partial s_1^2}  & \frac{\partial^2u_1}{\partial s_1 \partial s_2}  & \cdots & \frac{\partial^2u_1}{\partial s_1 \partial s_N}\\
\frac{\partial^2u_2}{\partial s_2 \partial s_1}  & \frac{\partial^2u_2}{\partial s_2^2}  & \cdots & \frac{\partial^2u_2}{\partial s_2 \partial s_N}\\
\vdots  & \vdots  & & \vdots\\
\frac{\partial^2u_N}{\partial s_N \partial s_1}  & \frac{\partial^2u_N}{\partial s_N \partial s_2}  & \cdots & \frac{\partial^2u_N}{\partial s_N^2}\\
\end{array} 
\right ]},
$$

which satisfies that all the off-diagonal elements are non-negative. It is also well-known in the literature of dynamical system that the stability of critical points of \\( u \\) is assotiated with the eigenvalues of \\( J \\). Therefore, we can analyze the behavior of GD at critical points by studying the spectural properties of \\( J \\).

***

**Definition 1.** We shall denote by \\(  \mathcal{Z} \\) the class of all real square matrices whose off-diagonal elements are non-positive.

**Remark** : A continuous N-player game defined on compact strategy set is supermodular \\( \Longleftrightarrow  \\) For all strategy profile \\( x \\), \\( -J(x) \in \mathcal{Z} \\).

**Definition 2.** Denote \\(  \mathcal{K} \subset \mathcal{Z} \\) as \\(\mathcal{K}=\\{A \in \mathcal{Z}\| Re(\lambda)>0, \forall \lambda \in spec(A)\\} \\), where the set of eigenvalues of \\(A\\) is denoted as \\(\text{spec}(A)\\). 

**Claim** : In a continuous supermodular game, \\( -J(x^*) \in \mathcal{K} \\) iff \\( x^ * \\) is a LASE. The argument is straightforward: let's examine how the configuration of \\(\text{spec}(J)\\) affects the limit behavior of GD dynamics. WLOG, let \\( x^ * =0\\), the GD dynamics with learning rate \\(\eta\\) writes

$$x^{(t+1)}=(I+\eta J)x^{(t)},$$

and therefore \\(x^{(T)}=(I+\eta J)^T x^{(0)}\\). When \\(\eta\\) is sufficiently small, we have 

1. \\(x^{(T)} \xrightarrow{} x^* \Longleftrightarrow \rho(I+\eta J) < 1 \Longleftrightarrow Re(\lambda)<0 \\) for all $\lambda \in \text{spec}(J)$ (The definition of LASE).

2. \\(x^{(T)}\\) diverges \\(\Longleftrightarrow \rho(I+\eta J) > 1 \Longleftrightarrow \exists \lambda \in \text{spec}(J), Re(\lambda)>0 \\).

3. when \\(\rho(I+\eta J) = 1\\), it could either be \\(rank(J)<N\\) or \\(J\\) has pure imaginary eigenvalues. If we rule out the degenerated case (\\(rank(J)<N\\)), this case corresponds to the limit cycle.  

In supermodular games, case 1 and 2 are both possible. Consider the following 3 by 3 matrix \\( J(a) \in -\mathcal{Z} \\) parameterized by \\(a \geq 0 \\):

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

This game has a local NE \\(x^* =(0,0,0)\\) with the Jacobian at \\( x^* \\) being \\( J(x^*)=J(a)\\), and direct calculation gives 

$$\text{spec}(J(29))=\{-0.0430, -5.9785 + 3.2777i, -5.9785 - 3.2777i\},$$ $$\text{spec}(J(31))=\{0.0421, -6.0210 + 3.3547i, -6.0210 - 3.3547i\},$$

which means \\( J(29) \\) and \\(J(31)\\) correspond to a LASE and a non-LASE, respectively. 

This example shows that a DNE is not necessarily LASE in supermodular games. However, for the other direction, we can prove the following theorem:

**Theorem** : In supermodular games, all LASE are DNE.

**Proof.** It suffices to show that if \\( J \in \mathcal{K}\\), the diagonal elements of \\(J\\) must be positive.

From to Theorem 4.3 in [1][^CzechMath], \\(J^{-1}\\) exists and \\(J^{-1} \geq 0\\). If \\(J_{ii}\leq 0\\) for some \\(i\\) , the \\(i\\)-th row of \\(J\\) must be non-positive. Therefore, the \\((i,i)\\)-th element in \\(J \cdot J^{-1}\\) is non-positive, which contradicts \\(J \cdot J^{-1}=I\\). 

## Reference

[^CzechMath]: [**Miroslav Fiedler and Vlastimil Ptak. On matrices with non-positive off-diagonal elements and positiveprincipal minors.Czechoslovak Mathematical Journal**](https://dml.cz/bitstream/handle/10338.dmlcz/100526/CzechMathJ_12-1962-3_6.pdf)


