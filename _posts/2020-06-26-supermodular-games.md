---
title: Gradient Dynamics in Supermodular Games
author: Fan Yao
date: 2020-06-26 11:33:00 +0800
categories: [Blogging, CS]
tags: [Game Theory]
math: true
---

## Supermodular Games (SG)

In game theory, supermodular game refers to a class of games which display strategic complementary behaviors. The idea of strategic complementarily is formulized by the notion of increasing differences: 

A function \\( u_i(s_ {i}, s_ {-i}) \\) exhibits ***increasing differences*** if for any \\(s_i' \geq s_i\\) and \\(s'_ {-i} \geq s_{-i}\\)

$$ u_i(s'_i, s'_{-i}) - u_i(s_i, s'_{-i}) \geq u_i(s'_i, s_{-i}) - u_i(s_i, s_{-i}).$$

A game in which each player is indexed by \\(N=\{1, 2, ..., n\}\\) with corresponding action set \\(S_i\\) and utility function \\(u_i: S_i \times S_{-i} \rightarrow \mathbb{R}\\) is a ***supermodular game*** if for all \\(i \in N\\), the following conditions are satisfied:

* \\( S_i \\) is a compact subset of \\( \mathbb{R} \\).
* \\( u_i \\) is upper semi-continuous in \\( s_i \\), continuous in \\(s_i\\).
* \\(u_i\\) has increasing differences in \\((s_i, s_{-i})\\).

Further, if \\(u_i\\) is \\(C^2\\), then the increasing differences condition is equivalent to

$$\frac{\partial^2u_i}{\partial s_i \partial s_j} \geq 0, \forall i \in N, j \neq i.$$

Supermudular game (SG) is well-behaved and possesses many good properties, such as

* Any supermudular game has pure strategy Nash equilibria (PNE).
* If a supermudular game has a unique PNE, the iterated dominance elimination and best response iteration converge to the unique PNE. 

## Gradient Dynamics (GD) in SG

However, even in SG with unique PNE, gradient decent dynamics (GD) does not necessarily converge to the PNE. We have the following example:

$$
\begin{align}
 & u_1(x, y) = \frac{x^3}{3}-x^2+xy \\
 & u_2(x, y) = \frac{y^3}{3}-y^2+xy 
\end{align}, 
$$

where \\( x, y \in [-1, 4] \\). It is a 2-player symmetric supermodular game and with a unique NE at (4, 4). However, we can verify that GD starting from certain points does not converge to the global unique NE, which is shown in the following figure:

![Desktop View]({{ "/assets/img/matlab/gd_and_br.png" | relative_url }})

, where the solid and dashed lines correspond to the gradient descent and best response dynamics, respectively. 
 
The reason why GD has difficulties in finding the global unique NE is similar to the reason why gradient descent optimization method cannot guarantee the global minimum when optimizing a nonconvex function: it simply gets trapped at local minima. In fact, the action profile (0, 0) in this example is just like a *local* Nash equilibrium: although it does not satisfy the definition of NE, both players have no incentives to deviate unilaterally in the local region around (0, 0).  

To study the convergence of GD in SG, we need to introduce new solution concepts besides NE. 

***

**Definition 1.** A strategy profile \\(z^* \in \mathbb{R}^d\\) is a locally asymptotically stable equilibrium (LASE) of the continuous-time gradient dynamics \\(\dot{z}=v(z)\\) if \\(v(z^* )=0\\) and \\(Re(\lambda)<0\\) for all \\(\lambda \in \text{spec}(J(z^*))\\).

**Remark** : LASE have the desirable property that they are locally exponentially attracting under the gradient flow.

**Definition 2.** A strategy profile \\(z^* \in \mathbb{R}^d\\) is a differential Nash equilibrium (DNE) if \\(v(z^* )=0\\) and \\(D^2_{x_i x_i}u(z^*) \succ 0\\) for all \\(i\\).

**Remark** : The key difference between local NE and DNE is that in DNE the second-order derivatives to the utilities with respect to each player's own strategy are required to be definite instead of semi-definite.

***

Having introduced the new solution concept LASE and DNE, Mazumdar et al.[^FindLNE] pointed out that in zero-sum game, all DNE are LASE, which implies that all DNE can be approached by some GD dynamics but GD dynamics does not  necessarily converge to DNE. To address this issue, they designed a gradient  based algorithm for zero-sum games in which the only LASE are the DNE. 

Actually, Mazumdar et al.[^GDinContiGame] also summarized the convergence result for GD on continuous games, aiming to answer two questions for different classes of games. They claimed that for general games we can conclude nothing, but for some specific type of games like zero-sum games and potential games, we are able to prove some nontrivial result. The result is presented in the following table. 

* Qestion 1: Are all (local) Nash equilibria also attractors of GD?
* Qestion 2: Are all attractors of GD also (local) Nash equilibria?

|Class of Games|general games|zero-sum games|potential games|
|:-:|:-:|:-:|:-:|
|Q1| No | Yes | No |
|Q2| No | No | Yes |

Interestingly, for continuous supermodular game, we are able to derive the same result as the case in potential games, that is, limit point of GD must be a local NE but there is local NE which cannot be the attractor of any GD. We will prove this result in the next post by leveraging the property of a special class of matrices.


## Reference

[^GDinContiGame]: [**Eric Mazumdar, Lillian J Ratliff, and S Shankar Sastry. On gradient-based learning in continuousgames**](https://arxiv.org/pdf/1804.05464.pdf)

[^FindLNE]: [**Eric V Mazumdar, Michael I Jordan, and S Shankar Sastry. On finding local nash equilibria (and onlylocal nash equilibria) in zero-sum games**](https://arxiv.org/pdf/1901.00838.pdf)

