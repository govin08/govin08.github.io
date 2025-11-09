---
layout: single
title: "(Sutton, 4.4, 4.6절) Value Iteration 등"
categories: data-science
tags: [reinforcement learing, dynamic programming, value iteration theorem, generalized policy iteration]
use_math: true
published: true
author_profile: false
toc: true
---

policy iteration에 대한 글을 계속 이어나가다보니 양이 또 많아졌다.
- [DP1 : policy evaluation](https://govin08.github.io/data-science/policy_evaluation/)
- [DP2 : policy iteration](https://govin08.github.io/data-science/policy_iteration/)

심지어 코드도 추가했더니 글을 나누는 게 맞다는 판단이 생겼다.

## 4.4 Value Iteration

policy iteration은 정책평가와 정책개선의 반복이었지만, 정책개선도 그 자체로 반복 알고리즘이었다.
정책개선 한 번당 정책평가가 수렴할 때까지 기다리기에는 조금 지루할 수 있다.
그러니, 정책개선 한 번당 정책평가의 과정(sweep)을 한 번씩 수행하면 이 때에도 최적정책에 수렴하는 것이 알려져 있으며 이 과정을 value iteration이라고 한다고 Sutton은 쓰고 있다.
하지만 이건 비유적인 표현이라고 봐야 할 것 같다.
실제로 sweep과 PI를 반복적으로 적용하면 아래 식이 나오지는 않는다.

value iteration이란 가치함수를 다음과 같은 점화식으로 업데이트해나가는 방식을 말한다.

$$
\begin{align*}
v_{k+1}(s)
&=\max_a\mathbb E\left[R_{t+1}+\gamma v_k(S_{t+1})\vert S_t=s, A_t=a\right]\\
&=\max_a\sum_{s',r}p(s',r|s,a)\left[r+v_k(s')\right]\tag{4.10}
\end{align*}
$$

위의 식과 아래 식이 같다는 것은 다음 식에서 명백하다.
<!-- 기본적으로 (4.3)과 (4.4)이 같다는 이전 포스트의 증명과 거의 같다. -->

$$
\begin{align*}
&\mathbb E\left[R_{t+1}+\gamma v_k(S_{t+1})\vert S_t=s, A_t=a\right]\\
=&\sum_{s',r}p(s',r\vert s,a)\mathbb E\left[R_{t+1}+\gamma v_k(S_{t+1})\vert S_t=s,A_t=a,R_{t+1}=r,S_{t+1}=s'\right]\\
=&\sum_{s',r}p(s',r\vert s,a)\left[r+\gamma v_k(s')\right]
\end{align*}
$$

value iteration은 policy iteration과는 많이 다르게 정책평가와 정책개선을 동시에 수행하는 셈이고 그런 점에서 나중에 나오는 Q-learning과 비슷하다.

value iteration을 통해 optimal value function $v_\ast$를 얻을 수 있다는 것을 증명하기 위해서 operator $\bar{\mathcal T}$를 다음과 같이 정의하자.
[Dawei Li, Zikun Ye의 자료](http://127.0.0.1:4000/data-science/policy_improvement/#44-value-iteration)를 참고하였다.

$$
\begin{align*}
\bar{\mathcal T}v(s)
&=\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+v_\pi(s')\right]\\
&=\max_a\mathbb E\left[R_{t+1}+v(S_{t+1})\vert S_t=a, A_t=a\right]\\
\end{align*}
$$

먼저 언급할 것은 $\bar{\mathcal T}$의 정의에 따르면 $v$에 대한 Bellman optimal equation이

$$v_\ast(s)=(\mathcal Tv_\ast)(s)$$

로 쓰여질 수 있다는 사실이다.
또한, Bellman optimal equation이 식의 개수와 변수의 개수가 같은 (비선형) 연립방정식이므로 특수한 상황이 아닌 이상은 해가 하나이고, 따라서 $v=(\bar{\mathcal T}v)(s)$를 만족시키는 어떤 $v$가 있다면 그 $v$는 optimal value function이라는 것도 알 수 있다.

증명에 앞서 절댓값과 최댓값에 관한 간단한 다음 식 $(\ast)$을 참고하자.

$$
\begin{aligned}
\left|\max_af(a)-\max_ag(a)\right|
&=\max_af(a)-\max_ag(a)\\
&=f(a_\ast)-\max_ag(a)\\
&\le f(a_\ast)-g(a_\ast)\\
&\le\max_a\left|f(a)-g(a)\right|\\
\end{aligned}
\tag{$\ast$}
$$

이때, 일반성을 잃지 않고 $\max_a f(a)\ge \max_ag(a)$로 두었고 $a_\ast=\text{arg}\max_af(a)$로 두었다.

먼저, $\bar{\mathcal T}$가 contraction mapping이라는 것을 증명한다.
임의의 $v$, $w$에 대하여 식 (4.10)에 의해 정의된 점화식인

$$
\begin{align*}
\left|(\bar{\mathcal T^\pi v})(s)-(\bar{\mathcal T^\pi w})(s)\right|
&=\left|
    \max_a\sum_{s',r}p(s',r\vert s,a)\left[r+v(s')\right]
    -\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+w(s')\right]
\right|\\
&\stackrel{(\ast)}{\le}
\max_a\left|
    \sum_{s',r}p(s',r\vert s,a)\left[r+v(s')\right]
    -\sum_{s',r}p(s',r\vert s,a)\left[r+w(s')\right]
\right|\\
&=\gamma\max_a\sum_{s',r}p(s',r\vert s,a)\left|v(s')-w(s')\right|\\
&=\gamma\sum_{s',r}p(s',r\vert s,a_\ast)\left|v(s')-w(s')\right|\\
&=\gamma\mathbb E\left[
    \left|v(S_{t+1})-w(S_{t+1}))\right|\;\vert S_t=s, A_t=a_\ast
    \right]\\
&\le\gamma\max_{s'}|v(s')-w(s')|\\
&=\gamma\left\Vert v-w\right\Vert_{\infty}
\end{align*}
$$

이다.
첫번째 줄은 기호의 정의, 두번째 줄은 $(\ast)$, 세번째 줄은 단순 계산이다.
네번째 줄에서는 세번째 줄의 argmax를 $a_\ast$로 잡은 것이고 다섯번째 줄은 기호의 정의에 의해 당연하다.
여섯번째 줄은 $A_t=a_\ast$인 상황보다도 더 크게 최댓값을 잡을 수 있기 때문이며, 일곱번째 줄은 기호의 정의로부터 당연하다.

이제 좌변에 $\Vert\cdot\Vert_\infty$를 취하면

$$
\left\Vert\bar{\mathcal T^\pi v}-\bar{\mathcal T^\pi w}\right\Vert_\infty
\le\gamma\Vert v-w\Vert_\infty
$$

이다.
따라서 $\mathcal T^\pi$는 contraction mapping이다.
그러면 contraction principle에 의해, 임의의 $v_0:\mathcal S\to\mathbb R$에 대하여 점화식 (4.10) 혹은

$$
v_{k+1}=\bar{\mathcal T}v_k
$$

에 의해 정의된 수열 $\\{v_k\\}_{k=0}^\infty$는 수렴한다.

조금 더 정확하게는, 어떤 $K$에 대하여 $v_{K+1}=v_K$가 되는데 그러면
$v_K=\bar{\mathcal T}v_K$이 되어 $v_K$가 $\bar{\mathcal T}$의 fixed point가 된다.
그런데 $\bar{\mathcal T}$의 유일한 고정점은 $v_\ast$이므로 $v_K=v_\ast$이다.

$$
\lim_{k\to\infty} v_k=v_\ast
$$

이다.
여기서 극한은 $\Vert\cdot\Vert_\infty$의 관점에서의 극한이다.

Sutton 책의 pseudocode는 다음과 같다.

![policy iteration]({{site.url}}\images\2025-09-18-dynamic_programming\value_iteration.png){: .img-80-center}


<!-- 식 $(4.10)$에 의해 정의된 점화식

$$
\begin{aligned}
v_{k+1}(s)
&=\max_a\mathbb E\left[R_{t+1}+\gamma v_k(S_{t+1})\vert S_t=s, A_t=a\right]\\
&=\max_a\sum_{s',r}p(s',r|s,a)\left[r+v_k(s')\right]\\
&=(\bar{\mathcal T}v_k)(s)
\end{aligned}
\tag{4.10}
$$ -->

<!-- 이에 대한 Sutton의 식

$$
\begin{align*}
v_{k+1}(s)
&=\max_a\mathbb E\left[R_{t+1}+\gamma v_k(S_{t+1})\vert S_t=s, A_t=a\right]\\
&=\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+\gamma v_k(s')\right]\tag{4.10}
\end{align*}
$$

을 이해해보자.
가치함수 $v_k:\mathcal S\to\mathbb R$에 대한 greedy policy $\pi_{k+1}$은 식 (4.9)에 의해 ($v_{\pi_k}\longrightarrow\pi_{k+1}$)

$$
q_{\pi_{k+1}}(s,a) = \max_aq_{\pi_k}(s,a)
$$

$$q_{\pi_k}(s,\pi_{k+1}(s))=\max_a q_{\pi_k}(s,a)$$

이다.
이제 정책평가의 한 iteration을 취하면 ($\pi_{k+1}\longrightarrow v_{\pi_{k+1}}$)

$$
\begin{align*}
v_{\pi_{k+1}}(s)
&=\mathbb E\left[R_{t+1}+\gamma v_{\pi_k}(S_{t+1})\vert S_t=s\right]\\
&=\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+\gamma v_{\pi_k}(s')\right]
\end{align*}
$$

이고, 따라서

$$q_{\pi_1}(s,a)=\mathbb E\left[R_{t+1}+\gamma v_{\pi_0}(S_{t+1})\vert S_t=s, A_t=a\right]$$

이다.
여기서 다시 greedy policy를 취하면

$$
\begin{align*}
v_{\pi_1}\longrightarrow\pi_2:q_{\pi_1}(s,\pi_2(s))
&=\max_a q_{\pi_1}(s,a)\\
&=\max_a \mathbb E\left[R_{t+1}+\gamma v_{\pi_0}(S_{t+1}\vert S_t=s)\right]
\end{align*}
$$ -->

<!-- 가치함수의 초깃값 $v_0:\mathcal S\to\mathbb R$에 대한 greedy policy $\pi_1$은 식 (4.9)에 의해, 모든 $s$에 대해

$$v_{\pi_0}\longrightarrow\pi_1:q_{\pi_0}(s,\pi_1(s))=\max_a q_{\pi_0}(s,a)$$

이다.
이제 정책평가의 한 iteration을 취하면

$$\pi_1\longrightarrow v_{\pi_1}:v_{\pi_1}(s) = \mathbb E\left[R_{t+1}+\gamma v_{\pi_0}(S_{t+1})\vert S_t=s\right]$$

이고, 따라서

$$q_{\pi_1}(s,a)=\mathbb E\left[R_{t+1}+\gamma v_{\pi_0}(S_{t+1})\vert S_t=s, A_t=a\right]$$

이다.
여기서 다시 greedy policy를 취하면

$$
\begin{align*}
v_{\pi_1}\longrightarrow\pi_2:q_{\pi_1}(s,\pi_2(s))
&=\max_a q_{\pi_1}(s,a)\\
&=\max_a \mathbb E\left[R_{t+1}+\gamma v_{\pi_0}(S_{t+1}\vert S_t=s)\right]
\end{align*}
$$
 -->

<!-- sweep을 한 번 거치면 -->
<!-- 4.3절에서처럼 $\pi_0$부터 시작하자. -->
<!-- 식 (4.5)에 의해 -->

<!-- $$v_{\pi_0}=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\left[r+\gamma v_\right]$$ -->

<!-- 가치함수의 초깃값 $v_0:\mathcal S\to\mathbb R$에 대하여 sweep을 한 번 거치면 -->

