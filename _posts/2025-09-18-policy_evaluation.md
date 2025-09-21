---
layout: single
title: "(Sutton, 4.1절) Policy Evaluation"
categories: machine-learning
tags: [reinforcement learing, Bellman operator, contraction principle, operator norm]
use_math: true
published: true
author_profile: false
toc: true
---

이전 포스트에 이어 Sutton의 책을 읽어가보자.
늘 그렇듯 책에 생략된 내용에 대해서는 자료를 찾거나 직접 계산 또는 증명해서 채워나갈 것이다.
dynamic programming은, 이 책에서는 가장 기본적인 강화학습 알고리즘으로 소개된다.
환경모델이 완전히 주어진 상태에서 쓸 수 있는 가장 기본적인 알고리즘이라고 할만하다.

공부를 하면 할수록 강화학습의 fundamental이 되는 이 계산들이나 정리들이 꽤 만만치 않음을 느낀다.
이런 어려움은 나만 겪는 것이 아니라고도 생각해본다.
사람들이 때로, 아주 간단한 식에도 나처럼 심각하게 고민한 것을 [몇](https://mathoverflow.net/q/321701/573437) [번](https://stats.stackexchange.com/q/385713/398741) 봤다.

Sutton의 3장은 꽤 책의 내용과 비슷하게 썼다.
하지만 내가 관심있는 수학적인 부분은 책에 자세하게 설명되지 않고 있기 때문에 책 외의 내용이 많이 적힐 예정이다.
식번호는 책의 것을 그대로 따랐으나, 절은 다르게 썼다.

이 포스트에서 꼭 다루고자 하는 것은 Bellman operator와 contraction principle로 policy evaluation이 유효한 것임을 증명하는 것이다.
이에 관해서는 이미 [Ishwin Rao](https://web.stanford.edu/class/cme241/lecture_slides/BellmanOperators.pdf)나 [Carl Fredricksson](https://cfml.se/bellman-operators-are-contractions/)이 잘 써놨는데 아마 조금씩 참고할 것 같다.

정확한 증명을 위해 계속 파다보니 operator norm에 대해서도 이야기해야 했다.
그런데, 가만히 보면 contraction principle이나 operator norm 모두 내 석사논문에서 다뤘던 주제들이다.
사실 수학 전체로 보면 조금 기본적인 내용들을 내 부끄러운 석사논문에 넣었던 것인데 그때 공부했던 것이 직접적인 도움이 되었다.
어떻게 보면 policy evaluation은 Bellman operator의 Lipschitz constant가 1보다 작다는 사실을 이용하고 있다.

# 4. Dynamic Programming

4장의 맨 처음에 나오는 것은 Bellman optimal equation이다.
이 장에서는 DP를 이용해 optimal policy를 얻어내는 과정을 설명하고 있으니 optimal policy에 관한 다음 식이 중요한 것은 당연하다.
optimal value $v_\ast$, $q_\ast$에 대한 optimal equation들을 다시 써보면 다음과 같다.

$$
\begin{align*}
v_\ast(s)
&=\max_a\mathbb E\left[R_{t+1}+\gamma v_\ast(S_{t+1})|S_t=s, A_t=a\right]\\
&=\max_a\sum_{s',r}p(s',r|s,a)\left[r+\gamma v_\ast(s')\right]\tag{4.1}\\
q_\ast(s,a)
&=\mathbb E\left[R_{t+1}+\gamma\max_{a'}q_\ast(S_{t+1},a')|S_t=s, A_t=a\right]\\
&=\sum_{s',r}p(s',r|s,a)\left[r+\gamma\max_{a'}q_\ast(s',a')\right]\tag{4.2}
\end{align*}
$$

## 4.1 Bellman equation revisited

$v_\pi$에 대한 Bellman equation은 이전 포스트에서 썼지만 다시 적어보자.

$$
\begin{align*}
v_\pi(s)
&=\mathbb E_\pi\left[G_t|S_t=s\right]\\
&=\mathbb E_\pi\left[R_{t+1}+\gamma G_{t+1}|S_t=s\right]\\
&=\mathbb E_\pi\left[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s\right]\tag{4.3}\\
&=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\left[r+\gamma v_\pi(s')\right]\tag{4.4}
\end{align*}
$$

첫번째 줄과 두번째줄이 같다는 것, 그리고 그것이 네번째 줄과 같다는 것은 이전 포스트에서 증명했고, 그것을 Bellman equation이라고 했었다.
그러나 세번째 줄은 지금까지 없었던 식이다.
그래, 의미상으로는 당연히 그럴 것 같은데 왜 그런 지는 그렇게까지 쉽게 설명되지 않는다.
그러니, 세번째 줄로부터 시작하여 네번째 줄로 도출되는 계산을 해보려 한다.

$\pi$와 $p$의 정의, Markov property, 기댓값의 정의 등에 의해 다음과 같이 계산된다.

$$
\begin{align*}
&\mathbb E_\pi\left[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s\right]\\
=&\sum_a\pi(a|s)\mathbb E\left[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s,A_t=a\right]\\
=&\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\mathbb E_\pi\left[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s,A_t=a,R_{t+1}=r,S_{t+1}=s'\right]\\
=&\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\mathbb E_\pi\left[r+\gamma v_\pi(S_{t+1})|S_{t+1}=s'\right]\\
=&\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\mathbb E_\pi\left[r+\gamma v_\pi(s')\right]\\
\end{align*}
$$

## 4.2 policy evaluation

책의 4.1절에서 다루는 것은, 주어진 정책 $\pi$에 대하여 이에 대한 가치함수 $v_\pi$를 얻어내는 것이다.
즉 정책을 평가하는(policy evaluation, prediction problem) 것으로서 DP를 포함한 모든 강화학습에서의 중요한 두 과정 중 하나이다.

가치함수를 얻어내는 방식은 식 (4.4)을

$$
v_{k+1}(s)=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\left[r+\gamma v_k(s')\right]\tag{4.5}
$$

와 같이 변형해 가치함수들의 수열 $v_0, v_1, v_2, \cdots$을 만들어나가는 것이다.
$v_0$가 임의의 가치함수(e.g. $v_0\equiv0$)이고 $v_\pi$가 존재한다는 조건 하에 수열 $\\{v_i\\}$가 $v_\pi$로 수렴하는 것이 알려져 있고 이것을 Sutton은 증명하지 않고 넘어갔다.
이 포스트에서 이를 증명해보려 한다.
정확하게는 다음과 같다.

> $0\lt\gamma\lt1$인 finite MDP $\mathscr D\left(\mathcal S, \mathcal A, p, \gamma\right)$의 한 정책을 $\pi$라고 하자.
> 임의의 가치함수 $v_0:\mathcal S\to\mathbb R$에 대하여 가치함수의 수열 $\lbrace v_k\rbrace_{k=0}^\infty$를 식 (4.5)와 같이 정의하면 $k\to\infty$일 때 $v_k$는 $v_\pi$로 수렴한다.
> 
> $$\lim_{k\to\infty}v_k=v_\pi$$

수열의 수렴을 이야기하려면 함수 $v_k$가 속해있는 공간을 정의해야 할텐데, 여기서는 함수공간 $\mathcal V=\\{v:\mathcal S\to\mathbb R\\}$에 대하여 normed space $\left(\mathcal V,\vert\vert\cdot\vert\vert_\infty\right)$을 생각하는 것이다.
Sutton은 어느 순간엔가 $v_K=v_{K+1}=\cdots$이 되어 수렴한다고 적고 있는데, 위에 쓴 수렴은 Sutton이 말한 수렴의 의미를포함한다.
<!-- Sutton이 책에서 언급하는 '수렴'의 의미는 조금 다르다.
어느 순간엔가 $v_K=v_{K+1}=\cdots$이 되어 수렴한다고 적고 있는데, 위에 쓴 수렴은 Sutton이 말한 수렴의 의미를 포함한다. -->

## 4.3 Bellman operation

먼저 할 것은 식 (4.4) 버전의 Bellman equation을 Bellman operation으로 표현하는 것이다.
기본적으로 Carl Fredricksson의 자료를 따라갔다.

많은 계산들, 특히 marginalization이 포함되어 있으므로 천천히 계산하기 위해 일단 두 값 $A$, $B$로 분리하면 식 (4.4)에서

$$
\begin{align*}
v_\pi(s)
&=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\left[r+\gamma v_\pi(s')\right]\\
&=\sum_{a,s',r}r\pi(a|s)p(s',r|s,a)+\gamma\sum_{a,s',r}v_\pi(s')\pi(a|s)p(s',r|s,a)\\
&=A+\gamma B
\end{align*}
$$

이다.
먼저 $A$를 계산하면

$$
\begin{align*}
A&=\sum_{a,s',r}r\pi(a|s)p(s',r|s,a)\\
&=\sum_{a,s',r}r\frac{P\left(S_t=s,A_t=a\right)}{P\left(S_t=s\right)}\times
\frac{P\left(S_t=s,A_t=a,S_{t+1}=s',R_{t+1}=r\right)}{P\left(S_t=s,A_t=a\right)}\\
&=\sum_r r\sum_{a,s'}\frac{P\left(S_t=s,A_t=a,S_{t+1}=s',R_{t+1}=r\right)}{P\left(S_t=s\right)}\\
&=\sum_r r\frac{P\left(S_t=s,R_{t+1}=r\right)}{P\left(S_t=s\right)}\\
&=\sum_r r{P\left(R_{t+1}=r|S_t=s\right)}\\
&=\mathbb E_\pi\left[R_{t+1}|S_t=s\right]\\
&=r_\pi(s)
\end{align*}
$$

이다.
두번째 줄은 두 표현 $\pi(\cdot|\cdot)$, $p(\cdot,\cdot|\cdot,\cdot)$ 정의, 세번째 줄은 통분, 네번째 줄은 두 변수에 $a$, $s'$에 대한 marginalization, 다섯번째 줄과 여섯번째 줄은 각각 조건부확률과 조건부기댓값의 정의에 의해 계산되었다.
마지막 줄은 새롭게 $r_\pi$라는 함수를 정의한 것이다.

다음으로 $B$를 계산하는데 통분과 같은 과정은 빠르게 넘어가면서 계산해보려 한다.
계산해보면

$$
\begin{align*}
B
&=\sum_{a,s',r}v_\pi(s')\pi(a|s)p(s',r|s,a)\\
&=\sum_{s'}v_\pi(s')\sum_{a,r}\frac{P\left(S_t=s,A_t=a,S_{t+1}=s',R_{t+1}=r\right)}{P\left(S_t=s\right)}\\
&=\sum_{s'}v_\pi(s')\frac{P\left(S_t=s,S_{t+1}=s'\right)}{P\left(S_t=s\right)}\\
&=\sum_{s'}v_\pi(s')P\left(S_{t+1}=s'|S_t=s\right)
\end{align*}
$$

와 같다.
두번째 줄은 기호의 정의와 통분, 세번째 줄은 marginalization, 네번째 줄은 조건부확률의 정의에 의해 계산되었다.
마지막 식은 $\mathbb E\left[S_{t+1}|S_t=s\right]$로도 쓸 수 있지만 위의 계산결과까지만 사용할 것이다.
계산한 $A$와 $B$를 가지고 Bellman equation을 다시 쓰면

$$
v_\pi(s)=r_\pi(s)+\gamma\sum_{s'}v_\pi(s')P\left(S_{t+1}=s'|S_t=s\right)
$$

이 된다.
이전 포스트에도 언급했고, 책의 4장에도 다시 강조되지만 Bellman equation의 본질은 연립방정식, 그것도 선형(affine)연립방정식이다.
<!-- 변수의 개수와 식의 개수가 $|\mathcal S|$로 같으므로 이 연립방정식 $(\ast)$의 해가 단 하나 존재한다고 가정하자. -->
state space $\mathcal S$를 $\mathcal S=\\{s_1,\cdots,s_n\\}$으로 두고 위 식을 다시 쓰면 모든 $i,j$에 대하여 ($1\le i,j\le n)$

$$
v_\pi(s_j)=r_\pi(s_j)+\gamma\sum_{i=1}^nv_\pi(s_i)P\left(S_{t+1}=s_i|S_t=s_j\right)
$$

이 성립한다.
선형(affine)방정식이니, 벡터와 행렬로 표현하면 가장 적절하다.
행렬 $P\in\mathbb R^{n\times n}$를

$$
P=
\begin{bmatrix}
P\left(S_{t+1}=s_1|S_t=s_1\right)&\cdots&P\left(S_{t+1}=s_n|S_t=s_1\right)\\
\vdots&\ddots&\vdots\\
P\left(S_{t+1}=s_1|S_t=s_n\right)&\cdots&P\left(S_{t+1}=s_n|S_t=s_n\right)
\end{bmatrix}
$$

로 정의하고 벡터 $v_\pi,r_\pi\in\mathbb R^n$을

$$
v_\pi=
\begin{bmatrix}
v_\pi(s_1)\\
\vdots\\
v_\pi(s_n)
\end{bmatrix}
,\quad
r_\pi=
\begin{bmatrix}
r_\pi(s_1)\\
\vdots\\
r_\pi(s_n)
\end{bmatrix}
$$

으로 두면 Bellman equation은

$$v_\pi=r_\pi+\gamma Pv_\pi$$

가 된다.
이런 관점에서 Bellman operation $\mathcal T^\pi$를

$$\mathcal T^\pi(v)=r_\pi+\gamma Pv$$

로 정의할 수 있고 Bellman equation도 간단히

$$v_\pi=\mathcal T^\pi(v_\pi)\tag{$\ast$}$$

로 쓰일 수 있다.
그리고 policy evaluation 식 (4.5)도

$$v_{k+1}=\mathcal T^\pi(v_k)$$

로 표현될 수 있다.

## 4.4 the contraction principle

이제 순수하게 수학적인 정리가 나올 차례이다.
baby rudin (3ed)의 9.22, 9.23에 다음과 같은 내용이 있다.
먼저 9.22는 contraction에 대한 정의이다.

> (9.22 정의) 거리공간 $(X,d)$에 대하여 함수 $\phi:X\to X$가 어떤 실수 $0\le c\lt1$에 대해 모든 $x,y\in X$에 대하여
> 
> $$d\left(\phi(x),\phi(y)\right)\le cd(x,y)$$
> 
> 를 만족시키면 $\phi$를 $X$의 contraction이라고 부른다.

contraction principle은 9.23에 해당하는 다음의 정리이다.

> (9.23 정리) 만약 $X$가 complete space이고 $\phi$가 $X$의 contraction이면 $\phi(x^\ast)=x^\ast$를 만족시키는 $x^\ast$가 존재하고 그러한 $x$는 유일하다.

어떤 거리공간이 complete하다는 것은 코시수열이 수렴한다는 것을 말한다.
이 공간에서 거리가 점점 줄어드는 함수가 있으면, 그 함수는 고정점(fixed point)이 반드시 존재한다는 아주 강력하고 재미있는 정리이다.
이것은 [Banach fixed point theorem](https://en.wikipedia.org/wiki/Banach_fixed-point_theorem)이라는 이름도 가지고 있다.

증명은 비교적 쉽다.
그리고 그 fixed point를 찾아가는 과정이, 꼭 policy evaluation과도 비슷하다.
즉, 아무 점 $x_0\in X$를 잡더라도 $\phi$를 계속해서 취해나가면 일정한 점 $x^\ast$에 이른다는 것이다.

임의의 점 $x_0\in X$에서 출발하여 수열 $\\{x_n\\}_{n=0}^\infty$를

$$x_{n+1}=\phi(x_n)$$

로 반복적으로 정의하자.
그러면

$$
\begin{align*}
d(x_n,x_{n+1})
\le&d\left(\phi(x_{n-1}),\phi(x_n)\right)\\
=&cd(x_{n-1},x_n)\\
&\vdots\\
\le&c^nd(x_0,x_1)
\end{align*}
$$

가 된다.
그러면 $m\gt n$일 때

$$
\begin{align*}
d(x_n,x_m)
&\le d(x_n,x_{n+1})+d(x_{n+1},x_{n+2})+\cdots+d(x_{m-1},x_m)\\
&\le c^nd(x_0,x_1)+c^{n+1}d(x_0,x_1)+\cdots+c^{m-1}d(x_0,x_1)\\
&=\frac{c^n(1-c^{m-n})}{1-c}d(x_0,x_1)\\
&\le\frac{c^n}{1-c}d(x_0,x_1)\\
\end{align*}
$$

이다.
첫번째 줄은 삼각부등식, 두번째 줄은 바로 위의 부등식, 세번째 줄은 등비수열의 합 공식을 사용했고 네번째 줄은 당연하다.
그러면 임의의 양수 $\epsilon\gt0$에 대하여

$$\frac{c^N}{1-c}d(x_0,x_1)\lt\epsilon$$

을 만족시키는 자연수 $N$가 존재한다.
$m\gt n\gt N$이면

$$
d(x_n,x_m)
\le\frac{c^n}{1-c}d(x_0,x_1)
\le\frac{c^N}{1-c}d(x_0,x_1)\lt\epsilon
$$

이므로 수열 $\\{x_n\\}$은 코시수열이다.
그런데 $X$가 complete space이므로 이 수열은 어떤 점 $x^\ast$엔가에 수렴하게 된다.
이때, contraction이 연속함수라는 건 당연하므로 $\phi$가 연속함수라는 것을 활용하면

$$\phi(x^\ast)
=\phi\left(\lim_{n\to\infty}x_n\right)
=\lim_{n\to\infty}\phi(x_n)
=\lim_{n\to\infty}x_{n+1}
=x^\ast.$$

이다.
즉, $x^\ast$는 $\phi$의 고정점(fixed point)이다.

또한 만약 고정점이 두 개 $x^\ast$, $y^\ast$ 존재한다면

$$d(x^\ast,y^\ast)=d\left(\phi(x^\ast),\phi(y^\ast)\right)\le cd(x^\ast,y^\ast)$$

이 되어 $(1-c)d(x^\ast,y^\ast)\le0$, $d(x^\ast,y^\ast)=0$이 되어 $x^\ast=y^\ast$가 된다.
즉 고정점 $x^\ast$는 유일하다.
$\square$

## 4.5 operator norm : $||P||\le1$

policy evaluation 증명의 완성을 위해서는 $P$의 operator norm $||P||$가 1보다 작거나 같다는 사실이 필요하다.
여기서 말하는 operator norm이란 $P$를
$\left(\mathbb R^n, \lvert\lvert\cdot\rvert\rvert_\infty\right)$
에서
$\left(\mathbb R^n, \lvert\lvert\cdot\rvert\rvert_\infty\right)$
로 가는 operator로 보았을 때의 operator norm을 말한다.
이것은 [matrix norm](https://en.wikipedia.org/wiki/Matrix_norm#Matrix_norms_induced_by_vector_norms)이라고도 한다.

operator norm의 [여러가지 정의](https://en.wikipedia.org/wiki/Operator_norm#Equivalent_definitions) 중 

$$||A||=\sup\{Av:||v||\le1\}$$

을 사용하자.
<!-- 그리고 $||A^T||=||A||$라는 [잘 알려진 사실](https://math.stackexchange.com/a/3471127/746048)을 활용할 수 있다. -->
4.3에서 정의한 행렬 $P$는 각 행의 합이 1이다.
즉,

$$
P=[p_{ij}]_{n\times n}
=
\begin{bmatrix}
P\left(S_{t+1}=s_1|S_t=s_1\right)&\cdots&P\left(S_{t+1}=s_n|S_t=s_1\right)\\
\vdots&\ddots&\vdots\\
P\left(S_{t+1}=s_1|S_t=s_n\right)&\cdots&P\left(S_{t+1}=s_n|S_t=s_n\right)
\end{bmatrix}
$$

에서 $\sum_jp_{ij}=1$이다. ($i=1,\cdots,n$)
그러면

$$
\left|\left|p_{i1}v_1+\cdots+p_{in}v_n\right|\right|_\infty
\le p_{i1}||v||_\infty+\cdots+p_{in}||v||_\infty
=||v||_\infty
$$

이므로

$$
\left|\left|Pv\right|\right|_\infty
=\max\{\left|\left|p_{i1}v_1+\cdots+p_{in}v_n\right|\right|_\infty:i=1,\cdots,n\}
\le||v||_\infty
$$

이고, 따라서 

$$
||P||
=\sup\{||Pv||_\infty:||v||_\infty\le1\}
\le1
$$

이다.


<!-- $$
\begin{align*}
P^Tv
&=\left(
    ||p_{11}v_1+\cdots+p_{n1}v_n||_\infty,
    \cdots,
    ||p_{1n}v_1+\cdots+p_{nn}v_n||_\infty
    \right)\\
&=\left(
    \left|\left|
        p_{11}||v||_\infty+\cdots+p_{n1}||v||_\infty
    \right|\right|_\infty,
    \cdots,
    \left|\left|
        p_{1n}||v||_\infty+\cdots+p_{nn}||v||_\infty
    \right|\right|_\infty
    \right)\\
\end{align*}
$$

이므로

$$
\begin{align*}
\left|\left|P^Tv\right|\right|
&=
\end{align*}
$$ -->

## 4.6 proof (policy evaluation)

이제 policy evaluation의 증명이 가능하다.
<!-- 먼저 Bellman operator $\mathcal T^\pi$는 $\mathbb R^n$에서 $\mathbb R^n$으로 가는 contraction이다. -->
두 벡터 $v,w\in\mathbb R^n$에 대하여

$$
\begin{align*}
\left|\left|T^\pi(v) - T^\pi(w)\right|\right|_\infty
&=\left|\left|(r_\pi+\gamma Pv) - (r_\pi+\gamma Pw)\right|\right|_\infty\\
&=\gamma\left|\left|P(v-w)\right|\right|_\infty\\
&\le\gamma\lvert\lvert P\rvert\rvert\cdot\lvert\lvert v-w\rvert\rvert_\infty\\
&\le\gamma||v-w||_\infty\\
\end{align*}
$$

이다.
첫번째 줄은 bellman operator의 정의,
두번째 줄은 $P$의 선형성과 norm의 성질,
세번째 줄은 operator norm의 성질,
네번째 줄은 $\lvert\lvert P\rvert\rvert\le1$이 쓰였다.
이것을 다시 쓰면

$$
d\left(T^\pi(v),T^\pi(w)\right)
\le
\gamma d(v,w)
$$

이 된다.
$0\lt\gamma\lt1$ 이므로 $\mathcal T^\pi$는
$\left(\mathbb R^n, \lvert\lvert\cdot\rvert\rvert_\infty\right)$
에서의 contraction이다.
그러면 contraction principle에 의해

$$\mathcal T^\pi(v^\ast)=v^\ast$$

인 $v^\ast\in\mathbb R^n$이 유일하게 하나 존재하고 식 (4.5)로 정의된 수열 $\\{v_k\\}$에 대하여 $v_k\to v^\ast$이다.
즉 Bellman equation $(\ast)$이 유일한 해 $v^\ast$를 가진다.
따라서 $v^\ast=v_\pi$이다.
그러면

$$\lim_{k\to\infty}v_k=v^\ast=v_\pi$$

가 성립한다.