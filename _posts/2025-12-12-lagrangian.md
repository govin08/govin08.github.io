---
layout: single
title: "Lagrangian"
categories: mathematics
tags: [lagrangian multiplier, vector calculus]
use_math: true
published: true
author_profile: false
toc: true
---

PCA에 대해 공부하다가 주성분을 찾는 대부분의 증명에서 lagrange multiplier method(라그랑지 승수법)를 사용한다는 것을 보았다.
문제는, 내가 이전에 [관련된 글](https://govin08.github.io/mathematics/lagrange_multiplier/)을 적었음에도 불구하고 그 증명이 바로 이해가 되지 않았다는 사실이다.

그 이유는 해당 증명들에서 lagrangian을 활용하여 라그랑지 승수법을 사용했기 때문이기도 했지만, 더 근본적으로는, 벡터 형태로 라그랑지 승수법이 사용된 것을 이해하지 못했기 때문이다.

PCA에 관한 글을 쓰기 전에 이것들을 정리하고 넘어가려 한다.
아마도 짧은 글이 될 것이다.

# 1. Lagrange multiplier revisited

먼저, [이전 글의 문제 정의](https://govin08.github.io/mathematics/lagrange_multiplier/#04-%EB%AC%B8%EC%A0%9C%EC%A0%95%EC%9D%98)를 벡터식으로 다시 써보자.
문체도 바꿔야지.

<!-- 
1. 다음과 같은 최적화 문제가 있다고 합시다.

   <div class="notice--info">
   Maximize $f(x_1,\cdots,x_n)$ subject to $\begin{cases}g_1(x_1,\cdots,x_n)&=k_1\\&\vdots\\g_m(x_1,\cdots,x_n)&=k_m\end{cases}$.
   </div>

2. 이 최적화문제를 풀기 위해서 다음과 같은 연립방정식을 풉니다.

   <div class="notice--success">
   $$
   \begin{cases}
   \nabla f(x_1,\cdots,x_n) + \lambda_1\nabla g_1(x_1,\cdots,x_n)+\cdots+\lambda_m\nabla g_m(x_1,\cdots,x_n)=0\\
   g_j(x_1,\cdots,x_n)=k_j\quad(j=1,2,\cdots,m)
   \end{cases}
   $$
   </div>

3. 연립방정식의 해가 $(x_1,\cdots,x_n)=\left({x_1}^{(1)},\cdots,{x_n}^{(1)}\right), \cdots, \left({x_1}^{(L)},\cdots,{x_n}^{(L)}\right)$ 과 같이 $L$개 주어진다면, 이 중에서 위 최적화문제의 답이 있습니다.
   그러니까,
   $f\left({x_1}^{(1)},\cdots,{x_n}^{(1)}\right)$,
   $\cdots$,
   $f\left({x_1}^{(L)},\cdots,{x_n}^{(L)}\right)$
   를 각각 계산하고 이 중에 가장 큰 값을 고르면 그 값이 구하고자 하는 최댓값이 됩니다.

이것을 벡터식으로 바꿔보자. -->

두 함수 $f:\mathbb R^n\to\mathbb R$와 $g:\mathbb R^n\to\mathbb R^m$에 대하여

1. 다음과 같은 최적화 문제가 있다고 하자.
    <div class="notice--info">
    Maximize $f(\boldsymbol x)$ subject to $g(\boldsymbol x)=\boldsymbol 0$.
    </div>

2. 이 최적화문제를 풀기 위해서 다음과 같은 연립방정식을 푼다.
(단, $\boldsymbol\lambda\in\mathbb R^m$)
    <div class="notice--success">
    $$
    \begin{cases}
    \frac{\partial f}{\partial\boldsymbol x}
    +
    \boldsymbol\langle\boldsymbol\lambda,\frac{\partial g}{\partial\boldsymbol x}\rangle
    =
    \boldsymbol0\\
    g(\boldsymbol x)=\boldsymbol 0
    \end{cases}
    $$
    </div>

3. 연립방정식의 해가 $\boldsymbol x=\boldsymbol x^{(1)}, \cdots, \boldsymbol x^{(L)}$ 과 같이 $L$개 주어진다면, 이 중에서 위 최적화문제의 답이 있다.
  그러니까, $f(\boldsymbol x^{(1)})$, $\cdots$, $f(\boldsymbol x^{(L)})$을 각각 계산하고 이 중에 가장 큰 값을 고르면 그 값이 구하고자 하는 최댓값이 된다.

이것을 정리의 형태로 쓰면 다음과 같다.

<div class="notice">
<b> 정리 </b> <br>
제약조건 $g(\boldsymbol x)=\boldsymbol 0$ 하에서 $f$가 $P^\ast$에서 최댓값(최솟값)을 가지면
$\frac{\partial f}{\partial\boldsymbol x}(P^\ast)
+\boldsymbol\langle\lambda,\frac{\partial g}{\partial\boldsymbol x}(P^\ast)\rangle
=\boldsymbol0$
를 만족시키는 벡터 $\boldsymbol\lambda\in\mathbb R^m$이 존재한다.
</div>

이 정리에 대한 증명은 조금 어렵다.
[이 곳](https://www.math.cmu.edu/~gautam/sj/teaching/2016-17/269-vector-analysis/pdfs/lagrange.pdf)에 증명이 엄밀하게 되어있는데 다 이해하지 못하겠다.
또한, 저차원의 몇 케이스에 대하여는 이전 글에서 증명했으니 증명은 생략하겠다.

# 2. Lagrangian

이 글의 주제가 라그랑지안이지만, 라그랑지 승수법 문제를 조금 다른 시각에서 보는 것에 지나지 않다.
무언가 새로운 것을 도입하는 것이 아니라, 기존 문제를 해석하는 또다른 시각을 제시하는 것뿐이다.

함수 $\mathcal L:\mathbb R^n\times\mathbb R^m\to\mathbb R$을

$$\mathcal L\left(\boldsymbol x,\boldsymbol\lambda\right)=f(\boldsymbol x)+\langle\boldsymbol\lambda, g(\boldsymbol x)\rangle$$

로 정의하자.