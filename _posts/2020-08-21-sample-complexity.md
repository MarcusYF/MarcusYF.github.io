---
title: Sample Complexity for Infinite Hypothesis Spaces
author: Fan Yao
date: 2020-08-21 11:33:00 +0800
categories: [Blogging, CS]
tags: [Learning Theory]
math: true
---

We study the sample complexity result for infinite hypothesis class in the lecture note of [theoretical machine learning](https://www.cs.princeton.edu/courses/archive/spring13/cos511/scribe_notes/0219.pdf) from Rob Schapire. 

## Result for Finite Hypothesis Class
For a finite hypothesis class \\( \mathcal{H} \\), we know that if an algorithm can find a hypothesis \\( h \\) consistent with \\(m= \frac{1}{\epsilon} (\ln|\mathcal{H}|+\ln \frac{1}{\delta}) \\) samples, then \\( Pr[err_D(h)\geq\epsilon] \leq \delta\\).

To see the connection among \\( \epsilon, \delta, m \\), and \\(\mathcal{H}\\), we define \\( \mathcal{B}=\{h \in \mathcal{H}: err_D(h) > \epsilon\} \\) and say that \\( h \\) is \\( \epsilon \\) bad if \\( h \in \mathcal{B} \\). Then 

$$
\begin{align*}
 & Pr[h \in \mathcal{H}: h \text{ is } \epsilon-\text{bad and } h \text{ is consistent}] \\ 
 \leq & Pr[\exists h \in \mathcal{H}: h \text{ is } \epsilon-\text{bad and } h \text{ is consistent}] \\
 = & Pr[\exists h \in \mathcal{B}:  h \text{ is consistent}] \\
 \leq & \sum_{h \in \mathcal{B}} Pr[h \text{ is consistent}] \\
 \leq & \sum_{h \in \mathcal{B}}(1-\epsilon)^m = |\mathcal{B}|(1-\epsilon)^m \\
 \leq &|\mathcal{H}|(1-\epsilon)^m \leq |\mathcal{H}|e^{-\epsilon m} \leq \delta.
\end{align*}
$$

Therefore, we conclude that with probability at least \\( 1-\delta \\), a hypothesis \\( h\in \mathcal{H} \\) consistent with \\( m \\) independently sampled from distribution \\( D \\) satisfies 

$$ err(h) \leq \frac{ \ln|\mathcal{H}| + \ln \frac{1}{\delta}}{m} .$$

## Result for Infinite Hypothesis Class

To generalize the sample complexity result to infinite hypothesis space \\(  \mathcal{H}\\), we need a new concept to characterize the size of \\( \mathcal{H}\\). Here we define the growth function

$$\Pi_{\mathcal{H}}(m)=\max_{S:|S|=m}|\Pi_{\mathcal{H}}(S)|,$$
where
$$\Pi_{\mathcal{H}}(S)=\{(h(x_1), \cdots, h(x_m)):h\in \mathcal{H}, S=(x_1, \cdots, x_m)\}.$$

The corresponding bound is of the form

$$err(h) \leq O\Big(\frac{\ln|\Pi_{\mathcal{H}}(2m)|+\ln \frac{1}{\delta}}{m}\Big).$$

The notations that will be used in the proof are listed in the following table:

| Notation   |      Definition     |
|:----------:|:-------------:|
|\\(\mathcal{H}\\)| A set of function from the function class \\(\mathcal{X} \rightarrow \{0,1\}\\)|
|\\(D\\)| A probability distribution on \\(\mathcal{X}\\)|
|\\(\delta, \epsilon\\)| Fixed value in (0, 1)|
|\\(S,S'\\)| Two independent sets of i.i.d samples from \\(\mathcal{X},\\) each is of size \\(m\\)|
|\\(T,T'\\)| The result after swaping the i-th elements of \\(S,S'\\) with prob.=0.5 for each \\(i\\)|
| \\( B \\) | The event \\( [\exists h \in \mathcal{H}: h \text{ consistent with }S \text{ and } h \text{ }\epsilon- \text{bad}] \\)  |
|\\( M(h, S) \\)|The number of mistakes \\(h\\) makes on \\(S\\)|
| \\( B' \\) |  The event \\( [\exists h \in \mathcal{H}: h \text{ consistent with }S \text{ and } M(h, S')>\frac{m\epsilon}{2}] \\)  |
| \\( B'\' \\) |  The event \\( [\exists h \in \mathcal{H}: h \text{ consistent with }T \text{ and } M(h, T')>\frac{m\epsilon}{2}] \\) | 
|\\(b(h)\\)|The event \\( [h \text{ consistent with }T \text{ and } M(h, T')>\frac{m\epsilon}{2}] \\)|

**Lemma 1.** If \\(m>\frac{8}{\epsilon}\\), \\( Pr[B' \| B] \geq \frac{1}{2}.\\)

By [Chernoff bound](https://en.wikipedia.org/wiki/Chernoff_bound) in the multiplicative form, \\(Pr[M(h,S')\leq\frac{m\epsilon}{2}] \leq (\sqrt{2} \exp(-\frac{1}{2}))^{m\epsilon} < \frac{1}{2}.\\)

**Lemma 2.** \\(Pr[B] \leq 2Pr[B'].\\)

\\(Pr[B'] \geq Pr[B \text{ and } B']=Pr[B]Pr[B' \| B]\geq \frac{1}{2}Pr[B].\\)

**Lemma 3.** \\(Pr[B'\'] = Pr[B'].\\)

The joint distribution of \\(T,T'\\) exactly equals those for \\(S,S'\\).

**Lemma 4.** \\(Pr[b(h) \| S,S'] \leq 2^{-\frac{m\epsilon}{2}}.\\)

Consider the label pattern \\(h\\) can induce on \\(S,S'\\). Let \\(r\\) denote the number of points in \\(S\\) with label different from the corresponding point in \\(S'\\). If \\(r<\frac{m\epsilon}{2}\\), \\(Pr[b(h) \|S,S']=0\\); otherwise, \\(Pr[b(h) \|S,S']=2^{-r}\leq2^{-\frac{m\epsilon}{2}}.\\)

**Lemma 5.** \\(Pr[B'\'] \leq \Pi_{\mathcal{H}}(2m)2^{-\frac{m\epsilon}{2}}.\\)

Let \\(\mathcal{H}'(S,S')\\) denotes a set of \\(\Pi_{\mathcal{H}}(2m)\\) representative hypothesis in \\(\mathcal{H}\\) that give all the different label patterns. We have

$$
\begin{align*}
  Pr[B''] =&Pr[\exists h \in \mathcal{H}: b(h)] \\ 
 =& E_{S,S'}Pr[\exists h \in \mathcal{H}: b(h) | S,S'] \\
 =& E_{S,S'}Pr[\exists h \in \mathcal{H}'(S,S'): b(h) | S,S'] \\
 \leq & E_{S,S'}\sum_{h \in \mathcal{H}'(S,S')} Pr[b(h) |S,S'] \\
 \leq & E_{S,S'}|\Pi_{\mathcal{H}}(2m)|2^{-\frac{m\epsilon}{2}}.
\end{align*}
$$

Given the bound of \\(Pr[B'\']\\), we can now bound \\(Pr[B]\\) by 

$$Pr[B] \leq 2Pr[B'] =2Pr[B'']\leq 2|\Pi_{\mathcal{H}}(2m)|2^{-\frac{m\epsilon}{2}}\leq \delta,$$

which gives

$$ \epsilon\leq O\Big(\frac{\ln|\Pi_{\mathcal{H}}(2m)|+\ln \frac{1}{\delta}}{m}\Big).$$