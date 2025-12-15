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

두 함수 $f:\mathbb R^n\to\mathbb R$와 $g:\mathbb R^n\to\mathbb R^m$에 대하여

1. 다음과 같은 최적화 문제가 있다고 하자.
    <div class="notice--info">
    $$\text{Maximize }f(\boldsymbol x)\text{ subject to }g(\boldsymbol x)=\boldsymbol 0.\tag1$$
    </div>

2. 이 최적화문제를 풀기 위해서 다음과 같은 연립방정식을 푼다.
    <div class="notice--success">
    $$
    \begin{cases}
    \frac{\partial f}{\partial\boldsymbol x}
    +
    \boldsymbol\lambda^T\frac{\partial g}{\partial\boldsymbol x}
    =
    \boldsymbol0\\
    g(\boldsymbol x)=\boldsymbol 0
    \end{cases}
    \tag2
    $$
    </div>

3. 연립방정식의 해가 $\boldsymbol x=\boldsymbol x^{(1)}, \cdots, \boldsymbol x^{(L)}$ 과 같이 $L$개 주어진다면, 이 중에서 위 최적화문제의 답이 있다.
  그러니까, $f(\boldsymbol x^{(1)})$, $\cdots$, $f(\boldsymbol x^{(L)})$을 각각 계산하고 이 중에 가장 큰 값을 고르면 그 값이 구하고자 하는 최댓값이 된다.

이때, $f,g\in C^1$이고 $\boldsymbol\lambda$는 $m$차원의 열벡터이다.
또 $\frac{\partial f}{\partial\boldsymbol x}$는 다변수 실함수에 대한 미분이므로 $n$차원 벡터, 즉 $f$의 gradient이다.
마찬가지로 $\frac{\partial g}{\partial\boldsymbol x}$는 $m\times n$ 행렬이 된다.
위의 풀이과정을 정리의 형태로 쓰면 다음과 같다.

<div class="notice">
<b> 정리 </b> <br>
제약조건 $g(\boldsymbol x)=\boldsymbol 0$ 하에서 $f$가 $P^\ast$에서 최댓값(최솟값)을 가지면
$\frac{\partial f}{\partial\boldsymbol x}(P^\ast)
+\boldsymbol\lambda^T\frac{\partial g}{\partial\boldsymbol x}(P^\ast)
=\boldsymbol0$
를 만족시키는 벡터 $\boldsymbol\lambda\in\mathbb R^m$가 존재한다.
</div>

이 정리에 대한 증명은 조금 어렵다.
[이 곳](https://www.math.cmu.edu/~gautam/sj/teaching/2016-17/269-vector-analysis/pdfs/lagrange.pdf)에 증명이 엄밀하게 되어있는데 다 이해하지 못하겠다.
또한, 저차원의 몇 케이스에 대하여는 이전 글에서 증명했으니 증명은 생략하겠다.

# 2. Lagrangian

이 글의 주제가 라그랑지안이지만, 라그랑지안을 사용하여 라그랑지 승수법을 푸는 것은 기존 문제를 조금 다른 시각에서 보는 것에 지나지 않다.
무언가 새로운 것을 도입하는 것이 아니라, 기존 문제를 해석하는 또다른 시각을 제시하는 것뿐이다.
하지만 라그랑지안을 활용하면 좀 더 깔끔하게 라그랑지 승수법을 서술할 수 있다.

함수 $\mathcal L:\mathbb R^n\times\mathbb R^m\to\mathbb R$을

$$\mathcal L\left(\boldsymbol x,\boldsymbol\lambda\right)=f(\boldsymbol x)+\boldsymbol\lambda^Tg(\boldsymbol x)$$

로 정의하자.
그러면 문제 (1)을 푸는 대신

<div class="notice--info">
$$\text{Maximize }\mathcal L\left(\boldsymbol x,\boldsymbol\lambda\right).\tag3$$
</div>

를 풀어도 된다.
(3)을 풀기 위해서는 $\mathcal L(\boldsymbol x,\boldsymbol\lambda)$의 모든 방향의 편미분이 0인 점들을 찾아야 한다.
즉,

$$
\begin{aligned}
\frac{\partial\mathcal L(\boldsymbol x,\boldsymbol\lambda)}{\partial\boldsymbol x}&=0\\
\frac{\partial\mathcal L(\boldsymbol x,\boldsymbol\lambda)}{\partial\boldsymbol \lambda}&=0
\end{aligned}
\tag4
$$

를 풀어야 한다.
그런데 (4)가

$$
\begin{align*}
\frac{\partial\mathcal f(\boldsymbol x)}{\partial\boldsymbol x}+\boldsymbol\lambda^T\frac{\partial g(x)}{\partial\boldsymbol x}&=0\\
g(\boldsymbol x)&=0
\end{align*}
$$

와 같으므로 (2)와 동등하다는 것이다.
즉, 제약조건이 있는 최적화 문제를, 변수가 늘어났지만 제약조건이 없어진 최적화 문제로 바꿀 수 있다.

# 3. Example

이전 글의 [예시 문제](https://govin08.github.io/mathematics/lagrange_multiplier/#11-%EB%AC%B8%EC%A0%9C)를 Lagrangian으로 풀어보자.
원래 문제는

<div class="notice--info">
제약조건 $x^2+y^2=1$ 하에서 $8x^2-2y$의 최댓값을 구하여라.
</div>

와 같았다.
$f(x,y)=8x^2-2y$, $g(x,y)=x^2+y^2-1$로 두면 $m=1$, $n=2$인 문제가 된다.
라그랑지안은

$$
\begin{align*}
\mathcal L\left(x,y,\lambda\right)
&=f(x,y)+\lambda g(x, y)\\
&=8x^2-2y+\lambda(x^2+y^2-1)
\end{align*}
$$

이다.
$\mathcal L$을 각각의 변수에 대해 편미분하고 0으로 두면

$$
\begin{align*}
\frac{\partial\mathcal L}{\partial x}=0&:16x+2\lambda x=0\\
\frac{\partial\mathcal L}{\partial y}=0&:-2+2\lambda y=0\\
\frac{\partial\mathcal L}{\partial\lambda}=0&:x^2+y^2=1\\
\end{align*}
$$

첫번째 식에서 $x=0$이거나 $\lambda=-8$이다.
만약 $x=0$이면, $y=\pm1$이고 따라서 $\lambda=\pm1$이다.
만약 $\lambda=-8$이면, $y=-\frac18$이고, $x=\pm\frac38\sqrt7$ 이다.
따라서 $(x,y)$는 $(x,y)=(0,1), (0,-1), (\frac38\sqrt7,-\frac18), (-\frac38\sqrt7,-\frac18)$이고, 그때마다의 $f(x,y)$는

$$
\begin{align*}
f(0,1)&=6\\
f(0,-1)&=10\\
f\left(\frac38\sqrt7,-\frac18\right)&=\frac{63}2+\frac14=\frac{65}2\\
f\left(-\frac38\sqrt7,-\frac18\right)&=\frac{63}2+\frac14=\frac{65}2
\end{align*}
$$

이다.
따라서 $f$의 최댓값은 $\frac{65}2$이다.