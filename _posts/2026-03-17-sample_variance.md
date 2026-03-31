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

고등학교의 「확률과 통계」 과목에 보면 크기가 $n\ge2$인 표본 $\\{x_1,\cdots,x_n\\}$의 표본평균은

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

# 5. 파이썬 코드를 통한 실험

여기까지만 써도 깔끔한 글이 될 것이다.
표본분산을 구할 때 $n-1$로 구해야 함을 수식으로 증명했다.
하지만 글쎄, 머리로 이해는 되었을지언정 확실하게 와닿지 않는다.
그래서 [코드](https://github.com/govin08/travail/blob/master/2026/0319_sample_variance/sample_variance.ipynb)를 짜봤다.

평균이 0이고 ($\mu=0$) 모분산이 4인($\sigma^2=4$) 어떤 모집단에서 크기가 $n=10$인 표본 $\\{x_1,\cdots,x_{10}\\}$을 뽑는다고 해보자.
이러한 추출은 한 번만 하는 게 아니고 상당히 여러 번 ($N=10000$번) 수행하는 것이다.
각각의 표본에 대하여 표본의 평균과 표본의 분산을 구할 수 있고, 여러 번의 시행에 대하여 평균을 내면 기댓값도 계산해볼 수 있다.

코드는 다음과 같다.

    import numpy as np

    pm = 0 # 모평균 (population mean)
    pv = 4 # 모분산 (population variance)
    n = 10      # 표본 크기
    N = 10000   # 반복 횟수

    s_squareds = []
    var_of_samples = []

    for _ in range(N):
        x = np.random.normal(pm, np.sqrt(pv), n)
        sm = np.mean(x) # 표본평균 (sample mean)
        s_squared = np.sum((x-sm)**2) / (n-1) # 표본분산 (sample variance, s²)
        var_of_sample = np.sum((x-sm)**2) / n # 표본의 분산 (variance of sample)
        s_squareds.append(s_squared)
        var_of_samples.append(var_of_sample)

    exp_of_s_squared = np.mean(s_squareds) # 표본분산의 기댓값
    exp_of_var_of_samples = np.mean(var_of_samples) # 표본의 분산의 기댓값

    print(f"모분산: {pv}")
    print(f"1/(n-1) 평균: {exp_of_s_squared:.4f}")
    print(f"1/n 평균: {exp_of_var_of_samples:.4f}")

출력되는 결과는

```
모분산: 4
1/(n-1) 평균: 3.9888
1/n 평균: 3.5900
```

이다.
매우 놀라운 결과가 아닐 수 없다.
아니 놀라울 것이 없이, 당연한 결과이긴 한데, 막상 진실을 마주하니 매우 놀랍다.
표본의 분산을 평균내면 4가 나오지 않고 조금 적은 값이 나온다.
반면 표본분산은 4에 근접한 값이 나오는 것이다.

표본평균의 분산이 모분산보다 작다는 것은 합리적인 사실이다.
그런데 얼마나 작은가?
표본의 크기에 반비례하게?
거의 맞는 말이지만 정확하게는 표본의 크기에서 1을 뺀 값에 반비례하게 작다.