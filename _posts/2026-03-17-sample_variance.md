---
layout: single
title: "표본분산의 정의"
categories: mathematics
tags: [statistics, sample mean, sample variance, sample standard deviation]
use_math: true
published: true
author_profile: false
toc: true
---

고등학교의 「확률과 통계」같은 과목에 보면 크기가 $n\ge2$인 표본 $\\{x_1,\cdots,x_n\\}$의 표본평균은

$$\bar x=\frac1n\sum_{i=1}^nx_i$$

으로 두고 표본분산은

$$
s^2=\frac1{n-1}\sum_{i=1}^n\left(x_i-\bar x\right)^2
$$

으로 둔다.
왜 $n$으로 나누지 않고 $n-1$로 나누는가, 하는 질문은 너무나 당연한 질문이지만 대답하는 것이 쉽지만은 않다.
이 주제를 다뤄보려고 한다.

고등학교때 이 개념을 배울 때에도, 시간이 흘러 학생들을 가르칠 때에도 표본분산을 저렇게 정의하는 명확한 이유를 알지 못한 채로 계속 미뤄왔다.
그러다, 문득 제대로 이해하고 싶은 생각이 들어 관련 내용을 공부했고 잘 이해가 되는 듯하여 여기에 정리하려 한다.

# 1. 통계적 추정

표본분산과 표본분산을 정의하는 이유에 대해 먼저 짚고 넘어가자.
모집단(population)의 분포를 모르는 상태에서 모집단의 분포를 추측하고 싶다고 하자.
통계적 추정이란, 모집단에서 $n$개의 변량을 독립적으로 복원추출하여 이를 통해 모집단의 분포를 추측하는 과정이다.

모집단의 분포를 설명하는 가장 대표적인 값은 평균과 분산이다.
이들을 모평균(population mean), 모분산(population variance)이라고 하고 각각 $\mu$, $\sigma^2$이라고 하자.
추출한 $n$개의 변량을 $x_1$, $\cdots$, $x_n$이라고 하고 이 변량들의 집합 $\\{x_1,\cdots,x_n\\}$을 표본(sample)이라고 하자.
또, $n$을 이 표본의 크기라고 하자.
각각의 $x_i$들은 확률변수라고 생각할 수 있고 앞문단에 의해 $x_i$들은 i.i.d.이다(independent and identically distributed).

# 2. 표본의 평균, 표본의 분산

그러면 모집단의 분포를 어떻게 추정할 것인가?
예를 들어 모평균을 어떻게 추정할 것이며 모분산을 어떻게 추정할 것인가?

가장 자연스러운 방법은, 표본의 평균과 표본의 분산을 모평균과 모분산으로 추정하는 것이다.
핵심을 먼저 말하면

> 모평균을 표본의 평균으로 추정하는 것은 적절하지만, 모분산을 표본의 분산으로 추정하는 것은 부적절하다.

즉, 두 종류의 용어를 명확히 구분해야 한다.
표본의 평균 $\frac1n\sum_{i=1}^nx_i$과 표본평균 $\bar x$을 구분해야 한다.
마찬가지로 표본의 분산 $\frac1n\sum_{i=1}^n\left(x_i-\frac1n\sum x_i\right)^2$과 표본분산 $s^2$도 구분해야 한다.
표본평균과 표본분산은 모평균과 모분산을 추정하기 위한 값인 것이지 표본의 평균과 표본의 분산과는 의미적으로 차이가 있다는 말이다.

그러면 표본평균과 표본분산은 어떻게 정의되어야 하는가?
모평균과 모분산을 추정할 수 있도록 해야 한다.
즉,

$$
\begin{align}
\mathbb E[\bar x]&=\mu\label{esm}\\
\mathbb E[s^2]&=\sigma^2\label{esv}
\end{align}
$$

이 만족되어야 한다.
그래야 편향되지 않은(unbiased) 추정을 수행할 수 있을테니까 말이다.

# 3. 표본평균의 정의

표본평균의 후보로 자연스럽게 생각할 만한 것은 당연히 표본의 평균이다.
표본의 평균 $\frac1n\sum_{i=1}^nx_i$에 대하여 기댓값을 계산해보면 $\mathbb E$의 선형성과 $x_i$들의 독립성에 의해

$$
\begin{align*}
\mathbb E\left[\frac1n\sum_{i=1}^nx_i\right]
&=\frac1n\sum_{i=1}^n\mathbb E[x_i]\\
&=\frac1n\sum_{i=1}^n\mu\\
&=\mu
\end{align*}
$$

이다.
<!-- *모평균을 표본의 평균으로 추정하는 것은 적절하다*. -->
그러면 표본평균은 그냥 표본의 평균으로 두어도 되는 것이다.
즉, 표본평균을

\begin{equation}
\bar x = \frac1n\sum_{i=1}^nx_i\label{dsm}
\end{equation}

로 정의하자.
그러면 (\ref{esm}) 조건은 자연스럽게 만족되는 것이다.
이후 계산을 위해 표본평균의 분산을 계산해놓자.

$$
\begin{align*}
\mathbb V[\bar x]
&=\mathbb V\left[\frac1n\sum_{i=1}^nx_i\right]\\
&=\frac1{n^2}\left(\sum_{i=1}^n\mathbb V[x_i]\right)\\
&=\frac1{n^2}\left(\sum_{i=1}^n\sigma^2\right)\\
&=\frac{\sigma^2}n\\
\end{align*}
$$

그리고 $\bar x^2$의 평균도 계산해놓자.

$$
\begin{align*}
\mathbb E\left[\bar x^2\right]
&=\mathbb V[\bar x]+\mathbb E\left[\bar x\right]^2\\
&=\frac{\sigma^2}n+\mu^2\\
\end{align*}
$$

# 4. 표본분산의 정의

표본평균과 마찬가지로 표본분산의 후보로 자연스럽게 생각할 만한 것은 당연히 표본의 분산이다.
즉 표본분산을

$$
\frac1n\sum_{i=1}^n\left(x_i-\bar x\right)^2
$$

로 두는 것이 자연스러워보인다.
이에 대해 기댓값을 계산해보면

$$
\begin{align*}
\frac1n\sum_{i=1}^n\left(x_i-\bar x\right)^2
&=\frac1n\sum_{i=1}^n{x_i}^2-2\frac1n\sum_{i=1}^nx_i\bar x+\frac1n\sum_{i=1}^n{\bar x}^2\\
&=\frac1n\sum_{i=1}^n{x_i}^2-2{\bar x}^2+{\bar x}^2\\
&=\frac1n\sum_{i=1}^n{x_i}^2-{\bar x}^2\\
\end{align*}
$$

으로부터

$$
\begin{align*}
\mathbb E\left[\frac1n\sum_{i=1}^n\left(x_i-\bar x\right)^2\right]
&=\mathbb E\left[\frac1n\sum_{i=1}^n{x_i}^2-{\bar x}^2\right]\\
&=\frac1n\sum_{i=1}^n\mathbb E\left[{x_i}^2\right]-\mathbb E[{\bar x}^2]\\
&=\frac1n\sum_{i=1}^n\left(\mathbb V[x_i]+\mathbb E\left[{x_i}\right]^2\right)-\mathbb E[{\bar x}^2]\\
&=\frac1n\sum_{i=1}^n\left(\sigma^2+\mu^2\right)-\left(\frac{\sigma^2}n+\mu^2\right)\\
&=\sigma^2+\mu^2-\left(\frac{\sigma^2}n+\mu^2\right)\\
&=\frac{n-1}n\sigma^2
\end{align*}
$$

이다.
다시 말해

$$
s^2=\frac1n\sum_{i=1}^n\left(x_i-\bar x\right)^2
$$

로 두면 편향이 발생한다 ; $\mathbb E[s^2]\ne\sigma^2$.
기댓값이 모분산과 달라지는 것이다 (\ref{esv})가 성립하지 않는다.

하지만 그 대신

\begin{equation}
s^2=\frac1{n-1}\sum_{i=1}^n\left(x_i-\bar x\right)^2
\end{equation}

로 두면

$$
\begin{align*}
\mathbb E[s^2]
&=\mathbb E\left[\frac1{n-1}\sum_{i=1}^n\left(x_i-\bar x\right)^2\right]\\
&=\frac1{n-1}\times(n-1)\sigma^2\\
&=\sigma^2
\end{align*}
$$

이 된다.

---

정리하면, 표본분산을 (4)처럼 정의해야 표본분산의 평균이 모분산과 같아져서 모평균과 모분산을 추정하는 데에 의미가 생긴다.
canonical하게 $n$으로 나누는 방식이면 편향이 생겨서 적절하지 못하다는 것이다.