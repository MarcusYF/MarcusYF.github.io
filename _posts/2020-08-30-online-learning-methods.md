---
title: Online Optimization Methods
author: Fan Yao
date: 2020-08-30 11:33:00 +0800
categories: [Blogging, CS]
tags: [Optimization]
math: true
---


We study some no-regret learning dynamics and their convergence result in games. Most results come from Panayotis Mertikopoulos' book [ONLINE OPTIMIZATION AND LEARNING IN GAMES: THEORY AND APPLICATIONS](http://polaris.imag.fr/panayotis.mertikopoulos/files/HDR.pdf). 

The starting point is a question that lies on the heart of the multi-agent online learning: does learning lead to stability in multi-agent systems? Put it in a more specific setting, we are curious about whether no-regret learning converges to the Nash equilibrium of the game. In general, the answer is a resounding "no", because no-regret learning is only guaranteed to converge to coarse correlated equilibria (CCE), and there is counterexample showing that there exist CCE that are supported only on strictly dominated strategies. 

The basic model we consider here is online optimization, whose prototypical setting can be summarized by the following sequence of events:

1. At every stage \\(t=1,2,\cdots\\), the optimizer selects an action \\(X_t\\) from a closed convex subset \\(\mathcal{X}\\) of \\(n\\)-dimensional normed space \\(\mathcal{V}\\).
2. Once an action has been selected, the optimizer incurs a loss \\(l_t(X_t)\\) based on a priori unknown loss function \\(l_t:\mathcal{X}\rightarrow\mathbb{R}\\).
3. Based on the incurred loss and/or any other feedback received, the optimizer
updates their action and the process repeats.

Unlike a regular optimization problem that targets on a fixed loss function, online optimization involves a stream of loss functions. A reasonable metric to evaluate an online optimization algorithm is a notion called *regret*:

**Definition** : The *regret* incurred by a sequence of actions \\(X_t \in \mathcal{X}\\) against a sequence of loss function \\(l_t:\mathcal{X}\rightarrow \mathbb{R}, t=1,2,\cdots,\\) is defined as 

$$Reg(T)=\max_{x\in \mathcal{X}}\sum_{t=1}^T[l_t(X_t)-l_t(x)]=\sum_{t=1}^T l_t(X_t)-\min_{x\in \mathcal{X}}\sum_{t=1}^Tl_t(x).$$

When \\(Reg(T)=o(T)\\), we call the corresponding policy as *no-regret*.
Next we introduce some basic policies that might lead to no-regret under certain assumptions. 

## Follow-the-leader (FTL)
The first policy candidate, and also probably the most straightforward algorithm on can come up with is known as *follow-the-leader* (FTL), as described via the following updated rule:

$$X_{t+1} \in \text{argmin}_{x\in \mathcal{X}}\sum_{s=1}^tl_s(x).$$

However, FTL is to aggressive to guarantee no-regret. We can easily construct example in which the cumulative loss function exhibits significant oscillations to let FTL jiggle between two extremes, and thus result in a linear regret.

## Follow-the-regularized-leader (FTRL)
It turns out that the weakness of FTL can be resolved by simply adding a regularization term. The new policy concept is known as *follow-the-regularized-leader* (FTRL):

$$X_{t+1} \in \text{argmin}_{x\in \mathcal{X}}\Big\{\sum_{s=1}^tl_s(x)+\frac{1}{\gamma}h(x)\Big\},$$

where \\(h(x):\mathcal{X}\rightarrow \mathbb{R}\\) is a regularization function and \\(\gamma>0\\) is a tunable parameter that adjusts the weight of the regularization term. It has been shown that if each \\(l_t\\) is ***strongly convex*** and ***Lipschitz continuous***, FTRL's regret is bounded by 

$$Reg(T) \leq \frac{H}{\gamma}+\frac{\gamma}{K}\sum_{t=1}^TL_t^2,$$

where \\(H=\max(h)-\min(h)\\) denotes the *"depth"* of \\(h\\) over \\(\mathcal{X}\\), and \\(L_t\\) is the Lipschitz constant for \\(l_t\\), \\(K\\) is the constant in the strongly convex condition. By taking \\(\gamma=(1/L)\sqrt{HK/T}\\), the incurred regret bound is \\(Reg(T)\leq 2L\sqrt{(H/K)T}=O(\sqrt{T}).\\)

## Online gradient decient (OGD)
FTRL leads to no-regret, but it requires the optimizer to have full access to the loss functions revealed up to the previous stage, and also the feasibility of solving the minimization problem at each step. What if the agent can only observe the payoff and the gradient of the chosen action at each step? In this case, we may propose another straightforward approach that directly extends the gradient descent (GD) to the online setting. The recursive update rule writes

$$
\begin{equation}\label{eq:eg3}
    \begin{cases}
     X_{t+1}=\Pi(X_t+\gamma_t V_t), \\
     V_t=-\nabla l_t(X_t),
    \end{cases}
\end{equation}
$$

where \\(\gamma>0\\) is the step-size and \\(\Pi:\mathcal{V}\rightarrow \mathcal{X}\\) is the Euclidean projector 

$$\Pi(x)=\text{argmin}_{x' \in \mathcal{X}}\|x'-x\|^2.$$

For OGD, we have a similar result as FTRL: if the loss functions satisfy the ***strongly convex*** and ***Lipschitz continuous*** conditions, and \\(\gamma_t=\gamma>0\\) is a constant, OGD's regret is bounded by 

$$Reg(T) \leq \text{diam}(\mathcal{X})L\sqrt{T},$$

where \\(L\\) is the Lipschitz bound for all \\(l_t\\), and \\(\text{diam}(\mathcal{X})=\max_{x,x' \in \mathcal{X}}\\|x-x'\\|^2\\) is the diameter of \\(\mathcal{X}\\).

**Remarks** If the loss functions take the linear form and \\(h(x)=\frac{1}{2}\\|x\\|^2\\), FTRL is OGD.

## Online mirror descent (OMD)
The Euclidean projector in OGD can be replaced with a more general form, and this seemingly minor modification to OGD turns out to be non-trivial, because projection functions induced by different geometries give different Lipschitz constant and could further allow for considerably sharper regret guarantees. Formally, the *online mirror descent* (OMD) is defined as 

$$
\begin{equation}
    \begin{cases}
     X_{t+1}=P_{X_t}(\gamma_t V_t), V_t=-\nabla l_t(X_t),\\
     P_{x}(y)=\text{argmin}_{x'\in \mathcal{X}}\big\{y \cdot(x-x')+D(x',x)\big\},\\
     D(x',x)=h(x')-h(x)-\nabla h(x) \cdot (x'-x).
    \end{cases}
\end{equation}
$$

**Example 1** In the case \\(h(x)=\frac{1}{2}\\|x\\|^2\\), OMD is exatly OGD. 

**Example 2** In the case \\(\mathcal{X}=\Delta(\mathcal{A})\\) be the standard simplex of \\(\mathbb{R}^n\\), and \\(h(x)=\sum_{a \in \mathcal{A}}x_a\log x_a\\), we have entropic gradient descent (EGD), which is known as exponential weights algorithm in the multi-armed bandit literature.
 
The regret-bound result for OMD is 

$$Reg(T) \leq L\sqrt{(2H/K)T},$$

where \\(H,K,L\\) share the same meaning with FTRL's regret-bound result. Compared to OGD, we may carefully choose \\(h\\) to obtain a sharper bound by letting \\(\sqrt{2H/K} = o( \text{diam}(\mathcal{X})). \\) For example, in example 2, \\(H=\log n, K=1\\) and the \\(\text{diam}(\mathcal{X})=\sqrt{n}\\). So EGD improves on OGD by a factor \\(\Theta(n)\\). 

## Dual averaging (DA)
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


