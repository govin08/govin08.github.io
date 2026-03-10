---
layout: single
title: "여러가지 이항분포"
categories: mathematics data-science
tags: [probability, discrete distribution]
use_math: true
published: true
author_profile: false
toc: true
---

요즘 빅데이터분석기사 시험을 준비하고 있다.
공부하다보니 여러가지로 블로그에 쓰고 싶은 주제들이 많이 보인다.
일례로 double exponential distribution (laplace distribution) 같은 것에 대해서도 쓰고 싶은데, 막상 써봤자 많은 이야기를 쓸 수는 없을 것 같아서, 그러니까 더 많은 내공이 있어야 의미있는 글을 쓸 수 있을 것 같아서 미루게 된다.

하지만 'UNIT 27 이산확률분포'에 다다르니 블로그에 글을 쓰지 않고는 배기지 못하는 상황이 됐다.
여러 이산확률분포들의 기댓값과 분산을 외워야 하는데, 이해하지 않고는 결코 외우고 싶지 않은 내 성향상 외우기가 싫었고 다 이해하기를 바랐다.
그리고 그 이해하는 게 그렇게까지 많은 수고가 들지 않아도 되는 것이, 이미 3년 전엔가 블로그에 [아주 상세하게 이것들을 정리해놓은 바](https://govin08.github.io/mathematics/kocw_stats)가 있기 때문이다.

해당 글은 너무 많은 양을 적어놔서, 글을 쓴 나조차도 다시 보기가 부담스럽다.
그러니, 딱 이항분포들에 대한 글만 추려서 적어놓으면 좋겠다는 생각이다.
여러 이항분포들에 대한 간략한 설명과 기댓값과 분산 식에 대한 유도를 적어보자.

다음은 notation에 대한 간단한 규칙이다.
표본공간은 $S$로 쓴다.
확률질량함수는 $P_X$로 쓴다, 즉

$$P(X=x)=P_X(x)$$

라고 쓴다.
$X$의 평균은 $E[X]$라고, 분산은 $V[X]$로 쓴다.
$X$는 이산확률변수이므로 countable한 값만을 가진다.
$X$가 가질 수 있는 값들을 $x_1$, $\cdots$, $x_n$, $(\cdots)$이라고 하면
평균의 정의는 당연히

$$E[X]=\sum_ixP_X(x_i)$$

이고, 분산의 정의는

$$V[X]=E\left[(X-E[X])^2\right]=E[X^2]-E[X]^2$$

이다.

# 1. 베르누이 분포

표본공간이 $S=\\{0,1\\}$인 시행을 베르누이 시행이라고 하자.
$0\le p\le 1$인 실수 $p$에 대하여 베르누이 분포는

$$
\begin{align*}
P_X(0)&=1-p\\
P_X(1)&=p
\end{align*}
$$

로 정의된다.
그러면

$$
\begin{align*}
E[X]
&=0\times P_X(0)+1\times P_X(1)\\
&=0\times(1-p)+1\times p\\
&=p\\
V[X]
&=(0-p)^2\times P_X(0)+(1-E[X])^2\times P_X(1)\\
&=p^2\times(1-p)+(1-p)^2\times p\\
&=p(1-p)
\end{align*}
$$

이다.

# 2. 이항분포

베르누이 시행을 $n$번 반복적으로 실행할 때 $1$이 나오는 횟수를 $X$라고 하면 $X$는 이항분포를 따른다고 한다.
$X$는 당연히 다음과 같은 확률질량함수를 가진다.

$$P_X(x)=\binom nxp^x(1-p)^{n-x}\qquad(x=0,1,\cdots,n)$$

$X$의 평균과 분산은 다음과 같이 계산된다.

$$
\begin{align*}
E[X]
&=\sum_{x=0}^nxP_X(x)\\
&=\sum_{x=0}^nx\binom nxp^x(1-p)^{n-x}\\
&=\sum_{x=1}^nx\binom nxp^x(1-p)^{n-x}\\
&=\sum_{x=1}^nn\binom{n-1}{x-1}p^x(1-p)^{n-x}\\
&=np\sum_{x=1}^n\binom{n-1}{x-1}p^{x-1}(1-p)^{(n-1)-(x-1)}\\
&=np\left(p+(p-1)\right)^{n-1}\\
&=np\\
E[X^2]
&=\sum_{x=0}^nx^2P_X(x)\\
&=\sum_{x=0}^nx^2\binom nxp^x(1-p)^{n-x}\\
&=\sum_{x=0}^n(x^2-x)\binom nxp^x(1-p)^{n-x}+\sum_{x=0}^nx\binom nxp^x(1-p)^{n-x}\\
&=\sum_{x=2}^nx(x-1)\binom nxp^x(1-p)^{n-x}+np\\
&=\sum_{x=2}^nn(n-1)\binom{n-2}{x-2}p^x(1-p)^{n-x}+np\\
&=n(n-1)p^2\sum_{x=2}^n\binom{n-2}{x-2}p^{x-2}(1-p)^{(n-2)-(x-2)}+np\\
&=n(n-1)p^2\left(p+(1-p)\right)^{n-2}+np\\
&=n(n-1)p^2+np\\
&=n^2p^2-np^2+np\\
V[X]
&=E[X^2]-E[X^2]\\
&=n^2p^2-np^2+np-(np)^2\\
&=np-np^2\\
&=np(1-p)
\end{align*}
$$

# 3. 기하분포

베르누이 시행을 반복적으로 시행할 때 처음으로 $1$이 나오기까지의 횟수를 $X$라고 하면 $X$는 기하분포를 따른다고 말한다.
기하분포의 확률질량함수는 당연히

$$P_X(x)=(1-p)^{x-1}p\qquad(x=1,2,\cdots)$$

이다.
평균을 구하는 식은

$$E[X]=\sum_{x=1}^\infty x(1-p)^{x-1}p$$

와 같다.
이 식의 우변은 $\sum_{x=1}^\infty(1-p)^x$를 $p$에 대하여 미분한 형태를 적절히 변형해서 얻을 수 있다.
등비급수의 식에 의해

$$\sum_{x=1}^\infty(1-p)^x=\frac{1-p}{1-(1-p)}=\frac{1-p}p=\frac1p-1$$

이고, 양변을 $p$에 대하여 미분한 뒤 마이너스를 붙이면

$$
\begin{equation}\tag{$*$}
\sum_{x=1}^\infty x(1-p)^{x-1}=\frac1{p^2}
\end{equation}
$$

이다.
따라서

$$E[X]=\frac1{p^2}\times p = \frac1p$$

이다.
$E[X^2]$의 식도 보면

$$E[X^2]=\sum_{x=1}^\infty x^2(1-p)^{x-1}p$$

인데, 이것은 식 ($\ast$)을 미분하면 비슷한 식을 얻을 수 있다.
($\ast$)을 미분한 뒤 마이너스를 붙이면

$$\sum_{x=1}^\infty x(x-1)(1-p)^{x-2}=\frac2{p^3}$$

이고, 따라서

$$
\begin{aligned}
\sum_{x=1}^\infty x^2(1-p)^{x-2}
&=\frac2{p^3}+\sum_{x=1}^\infty x(1-p)^{x-2}\\
&\stackrel{(\ast)}=\frac2{p^3}+\frac1{p^2(1-p)}\\
&=\frac{2(1-p)+p}{p^3(1-p)}\\
&=\frac{2-p}{p^3(1-p)}
\end{aligned}
$$

이고,

$$
E[X^2]=\sum_{k=1}^\infty x^2(1-p)^{x-1}p
=\frac{2-p}{p^3(1-p)}\times p(1-p)=\frac{2-p}{p^2}
$$

이다.
따라서

$$V[X]=E[X^2]-E[X]^2=\frac{2-p}{p^2}-\frac1{p^2}=\frac{1-p}{p^2}$$

이다.

# 4. 포아송 분포

단위시간동안 평균적으로 $\lambda$번의 사건이 일어난다고 기대될 때, 단위시간동안 사건이 일어난 횟수를 $X$라고 하면 $X$는 기하분포를 따른다고 말한다.

충분히 큰 $n$에 대하여 단위시간을 $n$개의 구간으로 자르면 해당 구간에서 사건이 일어날 확률은 $\frac\lambda n$이라고 생각할 수 있으므로 $X$는 $n$번의 독립시행에서 해당 사건이 일어난 횟수라고 생각할 수 있다.
정확하게는

$$P_X(x)
=\lim_{n\to\infty}\binom nx\left(\frac\lambda n\right)^x\left(1-\frac\lambda n\right)^{n-x}$$

이다.
그러면

$$
\begin{align*}
\lim_{n\to\infty}\binom nx\left(\frac\lambda n\right)^x\left(1-\frac\lambda n\right)^{n-x}
=&\lim_{n\to\infty}\frac{n(n-1)(n-2)\cdots(n-x+1)}{x!}\left(\frac\lambda n\right)^x\left(1-\frac\lambda n\right)^{n-x}\\
=&\lim_{n\to\infty}\frac{n(n-1)(n-2)\cdots(n-x+1)}{n^x}\frac{\lambda^x}{x!}\left(1-\frac\lambda n\right)^{n-x}\\
=&\frac{\lambda^x}{x!}
\lim_{n\to\infty}\frac{n-1}n
\times\lim_{n\to\infty}\frac{n-2}n
\times\cdots
\times\lim_{n\to\infty}\frac{n-x+1}n
\times\lim_{n\to\infty}
\left(1-\frac\lambda n\right)^{n-x}\\
=&\frac{\lambda^x}{x!}\lim_{n\to\infty}\left(1-\frac\lambda n\right)^{n-x}\\
=&\frac{\lambda^x}{x!}
\lim_{n\to\infty}\left(1-\frac\lambda n\right)^n
\lim_{n\to\infty}\left(1-\frac\lambda n\right)^{-x}\\
=&\frac{\lambda^x}{x!}e^{-\lambda}
\end{align*}
$$

이므로

$$
P_X(x)=\frac{\lambda^x}{x!}e^{-\lambda}\qquad(x=0,1,2,\cdots)
$$

이다.
이에 대한 자세한 설명은 [여기](https://govin08.github.io/mathematics/kocw_stats/#08-%EC%A7%80%EC%88%98%EB%B6%84%ED%8F%AC%EC%99%80-%EC%96%B4%EB%9E%91%EB%B6%84%ED%8F%AC)에 있다.

평균을 구해보면

$$
\begin{align*}
E[X]
&=\sum_{x=0}^\infty x\frac{\lambda^x}{x!}e^{-\lambda}\\
&=\lambda\sum_{x=1}^\infty\frac{\lambda^{x-1}}{(x-1)!}e^{-\lambda}\\
&=\lambda\sum_{m=0}^\infty\frac{\lambda^m}{m!}e^{-\lambda}\\
&=\lambda
\end{align*}
$$

이다.
사실 이것은 포아송 분포의 정의에 의해 당연하기도 하다.
분산을 구해보면

$$
\begin{align*}
E[X^2]
&=\sum_{x=0}^\infty x^2P_x(x)\\
&=\sum_{x=0}^\infty x^2\frac{\lambda^x}{x!}e^{-\lambda}\\
&=\sum_{x=0}^\infty x(x-1)\frac{\lambda^x}{x!}e^{-\lambda}
+\sum_{x=0}^\infty x\frac{\lambda^x}{x!}e^{-\lambda}\\
&=\lambda^2\sum_{x=2}^\infty\frac{\lambda^{x-2}}{(x-2)!}e^{-\lambda}+E[X]\\
&=\lambda^2\sum_{m=0}^\infty\frac{\lambda^m}{m!}e^{-\lambda}+E[X]\\
&=\lambda^2+\lambda\\
V[X]&=E[x^2]-E[x]^2=(\lambda^2+\lambda)-\lambda^2=\lambda
\end{align*}
$$

이다.

# 5. 초기하분포

초기하분포에 대해서는 공부한 바가 없다.
그러니 증명없이 정의와 결과만 적어보자.
내가 이해한 바는 이렇다.

$M$개의 구슬 중 $K$개가 검은 구슬이고 $N$개를 비복원추출할 때 추출한 검은 구슬의 개수를 $X$라고 하면 $X$는 초기하분포를 따른다고 한다.
확률질량함수는 당연히

$$
P(X=x)=\frac{\binom Kx\binom{M-K}{N-x}}{\binom MN}
$$

이다.
평균과 분산은

$$
\begin{align*}
E[X]&=\frac{KN}M\\
V[X]&=\frac{KN}M\frac{M-K}M\frac{M-N}{M-1}
\end{align*}
$$

이라고 한다.

# 6. 균등분포

$X$가 가질 수 있는 값들을 $x_1$, $\cdots$, $x_n$이고 각 값들이 될 가능성이 동등할 때 $X$는 균등분포를 따른다고 한다;

$$P_X(x_i)=\frac1n\quad(i=1,\cdots,n)$$

균등분포의 평균과 분산은 변량들이 등간격일 때에는 특별한 식이 있지만 그렇지 않은 일반적인 경우에는 다음과 같은 표준적인 식만 존재한다.

$$
\begin{align*}
E[X]&=\sum_{i=1}^n\frac{x_n}n\\
V[X]&=\sum_{i=1}^n\frac{x_n^2}n-\left(\sum_{i=1}^n\frac{x_n}n\right)^2
\end{align*}
$$
