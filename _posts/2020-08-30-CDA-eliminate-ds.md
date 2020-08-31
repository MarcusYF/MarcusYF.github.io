---
title: Dual Averaging and Elimination of Dominated Strategies
author: Fan Yao
date: 2020-08-30 11:33:00 +0800
categories: [Blogging, CS]
tags: [Game Theory, Optimization]
math: true
---


We study some no-regret learning dynamics and their convergence result in games. Most results come from Panayotis Mertikopoulos' book [ONLINE OPTIMIZATION AND LEARNING IN GAMES: THEORY AND APPLICATIONS](http://polaris.imag.fr/panayotis.mertikopoulos/files/HDR.pdf). 



## Continuous dual averaging (CDA)
Remenber one drawback of FTRL is the dependence on full information of \\(l_t\\). In fact, we can overcome this shortcoming by applying linear approximation for \\(l_t\\) at \\(X_t\\), and to replace \\(l_t\\) with the linear surrogate

$$\tilde{l}_t(x)=l_t(X_t)+\nabla l_t(X_t) \cdot (X_t-x)$$
 
which gives the *follow-the-linearized-leader* (FTLL) policy

$$X_{t+1}=\text{argmax}_{x\in\mathcal{X}}\Big\{\gamma \sum_{s=1}^tV_s \cdot x-h(x)\Big\}$$.

If written in recursive form, FTLL yields the *dual averaging* (DA) method:

$$
\begin{equation}
    \begin{cases}
     Y_{t+1}=Y_t+\gamma_t V_t, V_t=-\nabla l_t(X_t),\\
     X_{t+1}=Q(Y_{t+1}),\\
     Q(y)=\text{argmax}_{x\in\mathcal{X}}\big\{y\cdot x-h(x)\big\}.
    \end{cases}
\end{equation}
$$

DA is offen referred to as the *"lazy"* variant of OMD, because of the fact that the algorithm aggregates gradient steps *“lazily”*
(i.e., without transporting them to the state at which they were generated), and only projects to \\(\mathcal{X}\\) in order to generate a new gradient signal. Furthermore, a closer look at the update schemes of OMD and DA tells us they are equivalent under some additional assumptions on \\(h\\):

**Proposition** Consider the eager and lazy update rules

$$
\begin{equation}
    \begin{cases}
     x^{eager}=P_x(w),\\
     x^{lazy}=Q(y+w), x=Q(y).
    \end{cases}
\end{equation}
$$

If \\((y-\nabla h(x))\cdot (x'-x)=0\\) for all \\(x'\in\mathcal{X}\\), then \\(x^{eager}=x^{lazy}\\).


