---
layout: single
title: "(Sutton, 4.2-6절) Policy Improvement 등"
categories: data-science
tags: [reinforcement learing, dynamic programming, policy improvement theorem, policy iteration]
use_math: true
published: true
author_profile: false
toc: true
---

더 시간이 지나기 전에 dynamic programming 포스팅을 끝내고 싶은 마음이 생겼다.
다른 주제들도 공부하여 포스팅을 남기고 싶은데 DP를 끝내지 않고 다른 것을 쓰기는 싫기 때문이다.
그러니까 일종의 의무감에서 이 글을 쓰고 있다.
당장 이전부터 PCA와 PLS에 대해 공부하고 싶었고 얼마 전에는 game theory나 control theory에 손을 댈까도 생각했었는데, 오늘은 MPC와 LQR을 배워야 할 필요가 생긴 것이다.
그러니 DP는 빠르게 공부하여 치워버리자.

그리고 사실 글을 쓸 준비가 되어있다고 생각한다.
[이전 글](https://govin08.github.io/data-science/policy_evaluation/)을 쓰고 나서 간간이 4.2절을 보았고 어느 정도 이해는 했던 터였다.
책의 내용대로 쭉 따라가서 GPI까지는 써볼까.

하지만 글쓰는 것이 쉽지는 않을 것 같다.
사실상 Sutton의 책에 쓰인 것보다 더 간결하고 잘 쓸 수는 없을테니, 글을 쓰고 나서도 나조차도 내 블로그 글을 보기보다는 Sutton의 책을 볼 것 같다.
게다가 당초의 목적은 policy evaluation 증명이었으니 원래의 동기부여는 달성된 셈이다.
그래도 DP를 시작했으니 끝을 보고는 싶다.
Sutton의 책을 비슷하게 쓰되 설명을 좀 잘 적어보는 식으로 하자.

## 4.2 Policy Improvement

<!-- 4.2절은 policy improvement theorem으로부터 시작한다. -->

policy evaluation을 통해 $v_\pi$ 또는 $q_\pi$를 추정한 것은 더 나은 정책을 찾기 위함이라고 봐도 과언이 아니다.
실제로 Q 함수를 얻어냈다면 그 Q함수에 의거하여 가장 괜찮은 정책, 즉 주어진 $s$에 대하여 $Q(s,a)$가 최대가 되는 $a$를 선택하게 하는 정책을 얻어낼 수 있다.
이것을 greedy policy라고 한다.
물론 Q 함수가 정확하다는 보장이 없기 때문에 greedy policy가 optimal policy라는 것은 아니지만, 그래도 주어진 $Q$에 대한 최선의 정책이겠다.

Sutton의 4.2절은 기존의 정책 $\pi$에 비해 $Q$의 관점에서 더 나은 정책 $\pi'$이 정말로 나은 정책이라는 정리로부터 시작한다.
즉, 상태 $s$에 대하여 정책 $\pi$를 따라갔을 때의 가치 $v_\pi(s)$에 비해, 새로운 행동 $a=\pi'(s)\ne\pi(s)$을 선택한 후 정책 $\pi$를 따라갈 때의 가치가 더 크다면, 새로운 정책 $\pi'$가 기존의 정책 $\pi$보다 낫다는 정리이다.

<div class="notice--info">
<b> policy improvement theorem (정책 개선 정리) </b> <br>
deterministic policy $\pi$, $\pi'$에 대하여 모든 $s\in\mathcal S$
$$q_\pi(s,\pi'(s))\ge v_\pi(s)\tag{4.7}$$
라면 모든 $s\in\mathcal S$에 대하여
$$v_{\pi'}(s)\ge v_\pi(s)\tag{4.8}$$
가 성립한다.
</div>

이에 대한 증명은 굉장히 straightforward한데, 책에 있는 그대로 따라 써보겠다.
엄밀하게 들어가려면 확률변수들의 급수를 어떻게 정의할지 하는 어려운 문제를 다뤄야 하겠지만 그런 것의 세부는 피하고, 다만 각각의 스텝이 어떻게 정당화될 수 있는지만 보자.

$$
\begin{align*}
v_\pi(s)
\le&q_\pi(s, \pi'(s))\\
=&\mathbb E\left[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s, A_t=\pi'(s)\right]\\
=&\mathbb E_{\pi'}\left[R_{t+1}+\gamma v_\pi(S_{t+1})|S_t=s\right]\\
\le&\mathbb E_{\pi'}\left[R_{t+1}+\gamma q_\pi(S_{t+1},\pi'(S_{t+1})\vert S_t=s\right]\\
=&\mathbb E_{\pi'}\left[R_{t+1}+\gamma\mathbb E_{\pi'}\left[R_{t+2}+\gamma v_\pi(S_{t+2})\vert S_{t+1},A_{t+1}=\pi'(S_{t+1})\right] \vert S_t=s\right]\\
=&\mathbb E_{\pi'}\left[R_{t+1}+\gamma R_{t+2}+\gamma^2 v_\pi(S_{t+2})\vert S_t=s\right]\\
\le&\mathbb E_{\pi'}\left[R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3}+\gamma^3 v_\pi(S_{t+3})\vert S_t=s\right]\\
&\vdots\\
\le&\mathbb E_{\pi'}\left[R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3}+\gamma^3 R_{t+4}+\cdots\vert S_t=s\right]\\
=v_{\pi'}(s)
\end{align*}
$$

첫째줄은 (4.7)을, 둘째줄은 이전 포스트의 (4.3)의 증명과 비슷하게 얻어낼 수 있으며, 셋째줄도 기호의 정의에 의해 당연하다.
넷째줄은 (4.7)을 다시 사용한 것이고 다섯째줄은 둘째줄과 비슷하다.
다섯째줄에 새로 생긴 expectation은 어떤 subscript도 없이 $\mathbb E$로 쓰일 수 있겠지만 $\mathbb E_{\pi'}$라고 쓰여도 무방하다.
그리고 $\mathbb E_{\pi'}$으로 적혔기 때문에 다음 여섯번째 식이, Markov property와 더불어 성립하게 된다.
이제, 셋째줄에서 여섯째줄로 변하는 과정을 한 번 더 반복한다고 하면 일곱번째 식이 되고, 이것을 무한히 반복한다고 생각하면 여덟번째 식이 된다.
그리고 급수의 정의를 생각하면 아홉번째 식이 이와 같고, 그것은 곧 열번째 식의 $v_{\pi'}(s)$의 정의와 정확히 일치한다.
이렇게 하여 policy improvement theorem이 증명되었다. $\square$

정책개선정리는 정책 $\pi$가 주어져 있을 때 이보다 더 좋은 정책 $\pi'$을 만들어낼 여지를 마련한다.
그러면, 이 정리를 최대한으로 활용하여 만들어낼 수 있는 새로운 정책 $\pi'$이 있다.
이것은 부등식 (4.7)의 좌변이 최대한으로 되는 경우이다.
이 정책을 greedy policy라고 부른다.

greedy policy $\pi'$는 통상 모든 $s$에 대하여

$$
\begin{align*}
\pi'(s)
&=\text{arg}\max_a q_\pi(s,a)\\
&=\text{arg}\max_a\mathbb E\left[R_{t+1}+\gamma v_\pi(S_{t+1})\vert S_t=s,A_t=a\right]\tag{4.9}\\
&=\text{arg}\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+\gamma v_\pi(s')\right]
\end{align*}
$$

인 정책 $\pi'$으로 정의한다.
즉, 여러 $a$들 중 $q_\pi(s,a)$가 최댓값을 가지는 $a$를 $\pi'(s)$로 정의하는 것이다.
최댓값을 가지는 $a$는 유일하지 않을 수 있으니 그러한 $a$들 중 하나를 $\pi'(a)$로 정의한다고 보면 되겠다.

이러한 $\pi'$는 deterministic policy이다.
반면, $\pi'$를 stochastic policy로서 다음과 같이 정의해도 되겠다.
주어진 $s$에 대해서 $q$값이 가장 큰 action(들)을 다 더해서 1이 되도록 확률을 주고 나머지 action에 대해서는 모두 0으로 주는 것이다.
예를 들어, $A_s=\\{a'\in A:q_\pi(s,a')=\max_{a\in\mathcal A}q_\pi(s,a)\\}$ 로 두고

$$
\begin{align*}
\pi'(a|s) =
\begin{cases}
\frac1{\left|A_s\right|}&a\in A_s\\
0                   &a\notin A_s\\
\end{cases}
\end{align*}
$$

와 같이 정의할 수 있다.

<div class="notice--info">
<b> policy improvement (정책 개선) </b> <br>
정책 $\pi$에 대하여 $\pi'\ge\pi$를 만족시키는 새로운 정책 $\pi'$를 선택하는 것을 <b> 정책개선 </b>이라고 부른다.
greedy policy는 정책개선 중 가장 적극적인 정책개선이다.
</div>

주어진 정책 $\pi$에 대한 greedy policy $\pi'$가 더이상 개선되지 않고 이전과 그 가치가 같다고, 즉 $v_{\pi'}=v_\pi$를 만족한다고 하자.
그러면 (4.9)에 의해

$$
\begin{align*}
v_{\pi'}(s)
&=q_{\pi'}(s,\pi'(s))\\
&=q_\pi(s,\pi'(s))\\
&=\max_a q_\pi(s,a)\\
&=\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+\gamma v_\pi(s')\right]\\
&=\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+\gamma v_{\pi'}(s')\right]
\end{align*}
$$

이다.
(이것은 Sutton 책에 써진 것을 더 세부적으로 써본 것이다.)
첫번째 줄은 기호의 정의에 의해 당연히 성립한다.
두번째 줄은 $v_{\pi'}=v_\pi$로부터 $q_{\pi'}=q_\pi$를 유도할 수 있기 때문에 그렇다(Bellman equation).
세번째와 네번째 줄은 (4.9)에서부터 나왔고, 마지막 다섯번째 줄은 $v_{\pi'}=v_\pi$이다.

그러면 위 식은 Bellman optimal equation (3.18)과 같다.
특별한 경우가 아닌 이상 연립방정식 Bellman optimal equation의 해가 유일할 것이므로, 이 정책 $\pi'$은 optimal policy라고 말할 수 있다.

## 4.3 Policy Iteration

어떤 $\pi$에 대하여 policy evaluation과 policy improvement를 반복할 수 있다.

![policy iteration]({{site.url}}\images\2025-10-13-policy_improvement\policy_iteration.png){: .img-80-center}

정책 평가와 정책 개선을 반복하는 것인데 이것을 policy iteration이라고 한다.
정책개선시 deterministic한 policy를 택한다고 가정하면, finite MDP의 deterministic policy의 개수는 유일하고, optimal policy 또한 deterministic하다고 가정할 수 있으므로 policy iteration는 언젠가 끝난다.
즉, 어느 순간 ($k$번째에) 최적정책 $\pi_\ast$에 도달하며 그때의 정책은 $v_{\pi_k}=v_{\pi_{k-1}}$를 만족할 것이다.

정책평가시 가치함수의 초깃값을 설정해야 했다.
그 초깃값이 어떻건 항상 $v_\pi$로 수렴한다는 것은 이전 포스트에서 증명했었다.
하지만 이 경우에는 초깃값을 직전 가치함수로 두면 수렴 속도에 있어서 유리하다는 것이 언급되고 있다.
즉, $\pi_i\stackrel{E}{\longrightarrow}v_{\pi_i}$의 정책평가를 진행할 때 가치함수의 초깃값을 $v_{i-1}$로 놓으면 수렴이 빨라진다는 것이다.
이것을 warm start라고도 부른다.

아래는 Sutton 책에 실린 pseudocode이다.
warm start 버전인 것을 확인할 수 있다.

![pseudocode : policy iteration]({{site.url}}\images\2025-10-13-policy_improvement\pseudocode-policy_iteration.png){: .img-80-center}

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

먼저 언급할 것은 $\bar{\mathcal T}$의 정의에 따르면 $v$에 대한 Bellman optimal equation (3.18)이

$$v_\ast(s)=(\mathcal Tv_\ast)(s)$$

로 쓰여질 수 있다는 사실이다.
또한, Bellman optimal equation이 식의 개수와 변수의 개수가 같은 (비선형) 연립방정식이므로 특수한 상황이 아닌 이상은 해가 하나이고, 따라서 $v=\bar{\mathcal T}v$를 만족시키는 어떤 $v$가 있다면 그 $v$는 optimal value function이라는 것도 알 수 있다.

증명에 앞서 절댓값과 최댓값에 관한 간단한 다음 식 $(\ast)$을 언급할 필요가 있다.

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

이제 $\bar{\mathcal T}$가 contraction mapping이라는 것을 증명한다.
임의의 $v$, $w$에 대하여 식 (4.10)에 의해 정의된 점화식인

$$
\begin{align*}
\left|(\bar{\mathcal T} v)(s)-(\bar{\mathcal T} w)(s)\right|
&=\left|
    \max_a\sum_{s',r}p(s',r\vert s,a)\left[r+v(s')\right]
    -\max_a\sum_{s',r}p(s',r\vert s,a)\left[r+w(s')\right]
\right|\\
&\stackrel{(\ast)}{\le}
\max_a\left|
    \sum_{s',r}p(s',r\vert s,a)\left[r+v(s')\right]
    -\sum_{s',r}p(s',r\vert s,a)\left[r+w(s')\right]
\right|\\
&\le\gamma\max_a\sum_{s',r}p(s',r\vert s,a)\left|v(s')-w(s')\right|\\
&=\gamma\sum_{s',r}p(s',r\vert s,a_\ast)\left|v(s')-w(s')\right|\\
&\le\gamma\max_{s'}|v(s')-w(s')|\\
&=\gamma\left\Vert v-w\right\Vert_{\infty}
\end{align*}
$$

이다.
첫번째 줄은 기호의 정의, 두번째 줄은 $(\ast)$, 세번째 줄은 단순 계산이다.
네번째 줄에서는 세번째 줄의 argmax를 $a_\ast$로 잡은 것이고 다섯번째와 여섯번째 줄은 기호의 정의로부터 당연하다.

이제 좌변에 $\Vert\cdot\Vert_\infty$를 취하면

$$
\left\Vert\bar{\mathcal T v}-\bar{\mathcal T w}\right\Vert_\infty
\le\gamma\Vert v-w\Vert_\infty
$$

이다.
따라서 $\mathcal T$는 contraction mapping이다.
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

