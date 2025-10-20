---
layout: single
title: "(Sutton, 3장) Finite MDP"
categories: data-science
tags: [reinforcement learing, finite markov decision process, markov property, Bellman equation]
use_math: true
published: true
author_profile: false
toc: true
---

얼떨결에 강화학습 업무를 맡았을때는 매우 당황스러웠고 지금도 막막하지만, 그래도 하나 얻은 것이 있다.
대학원 시절에는 잘 읽히지 않던 Sutton의 책이 지금은 읽힌다는 것이었다.
입사 첫주에는 Sutton의 책을 열심히 읽어봤다.
결국은 DPG와 DDPG를 구현해야 하는 어려운 일로 가야하지만, 그전에 당연히 DQN을 알아야 할 것이고 또 그 전에는 SARSA, Q-learning을 알아야 했다.

<!-- Sutton 책에서는 dynamic programming도 강화학습의 맥락 속에 포함되어 있고 가장 기본적인 알고리즘으로 설명되고 있기에 DP도 아는 것이 좋다. -->
SARSA와 Q-learning을 보려면 6장의 Temporal-Difference Learning을 알아야해서 여기부터 읽어보니, 어느 정도 잘 읽혔다.
사실 이전에는 TD라는 것의 의미 자체를 이해하지 못했는데, 이번에는 이해하게 되었고 SARSA와 Q-learing도 분명히 이해하게 되었다.
TD가 dynamic programing과 Monte Carlo의 절충이라고 할 때, 그렇다면 그 둘도 알아야 할텐데, MC는 별로 보기가 싫어서 DP를 먼저 보았다.
즉, 4장의 Dynamic Programming을 읽었다.
(나중에 5장 Monte Carlo Simulation도 다 읽었다.)
그리고 거의 다 아는 내용일지언정, 배경지식을 알아야 하니 3장 Finite Markov Decision Process도 쭉 다 읽었다.
3장에서는 늘 그렇듯 가장 중요하면서 조금 어려운 것이 Bellman equation 네 개인데 그걸 다시 보았다.
다 이해하지는 못했고, 그래도 (ordinary) Bellman equation 두 개는 이해했다.
이전에 공부했을 때처럼 2장의 Multi Armed Bandit은 읽지 않았다.

그렇게 하고 나니 4장의 DP 내용이 더 잘 보이기 시작했다.
여기서 설명하는 방법론들은 강화학습 전반에 사용되는 방법론이기 때문에 중요하다.
DP의 핵심이 되는 policy iteration의 두 방법 policy evaluation과 policy improvement 중 policy evaluation을 보다가 의문이 생겼다.
저렇게 iterative한 방법을 통해 value function을 결정하는데, 그게 맞는 것인가.
수렴하는 게 맞는 것인가?
수렴한다면 옳은 값으로 수렴하는 게 맞는 것인가?

의문이 들어 인터넷을 검색해보니 나와 같은 의문을 가진 사람이 [질문](https://ai.stackexchange.com/q/20309/97276)한 적이 있었고, likes를 가장 많이 받은 답변자는 Bellman operator를 contraction mapping으로 보아 contraction principle을 통해 증명했다.
그래서 이에 관해 정리해보려 한다.

Bellman operator에 관해서는 답변자가 쓴 대로 vector space를 쓸 필요는 없을 것 같다.
contraction principle은 어디서 본 것 같았다.
아마 Munkres의 Topology 책에서였던 것 같아 뒤져보니 이때 나왔던 contraction과 fixed point는 $B^2$에서의 특수한 이야기였다.
대학원때 봤던 Rudin의 Real and Complex Analysis에서 Banach의 이름이 붙은 theorem이 있었던 것 같았는데 찾아보니 아니었다.
baby rudin을 보니 contraction principle이 본문에 떡하니 있었고 (왜 나는 그걸 기억하지 못하는가.) 정확히 해당 증명에 필요한 정리가 있었다.
사실 Munkres의 책에도 비슷한 정리가 있었지만 baby rudin에서의 설명이 더 쉬웠다.
정확하게는 baby rudin에서는 metric space에서의 증명을 하고 있었고 그 증명은 간결했다.
Munkres의 책에서는 꼭 metric space일 필요가 없는 상황에서 특정한 위상조건들이 주어졌을 때 contraction principle이 주어져있었고, 증명이 어려워보였기에 읽지 않았다.

그러니, 아마 MDP에 대한 간략한 설명과 더불어 bellman equation (and bellman optimal equation) for value function $v$, policy evaluation, contraction principle, fixed point, Bellman operator 등을 블로그에 쓸 것 같다.

**하지만 이 포스트에서는 (Sutton, 2018) 책의 3장, Finite MDP에 대한 내용만 담아보려 한다.**

사실 이 블로그를 시작하게 된 건 머신러닝에 관한 여러 사항들을 정리하고 싶어서였다.
그런데 머신러닝보다는 수학을 좋아하는지라 제대로 된 머신러닝 글이 거의 없었는데 이번에 쓰게 되지 않을까 싶다.

까뮈는 인간이 부조리한 상황에 대하여 반항할 수 있다고 했다.
여기서 말하는 까뮈적인 부조리는 군대 부조리나 임금체불같은 사회적인 부조리를 말하는 것은 아니겠고, 나 또한 어떤 사회적인 부조리에 처해있다고 생각하지는 않는다.
해야 하는 일이 지극히 공학적이고 실용적이며 불가능에 가까워보이는 일인 것에 비해, 내가 관심을 가지는 것은 수학적이고 추상적이며 확실히 알 수 있는 것이라고 한다면, 그 간극에서 발생하는 부조리는 어쩌면 까뮈적인 부조리가 아닐까.
따라서 블로그에 이런 글을 쓰는 것은 내 나름대로의 반항에 해당한다.
쉬거나 취미생활을 하는 시간이 거의 없는 일상에서 어떻게든 수학 글을 써내려가는 것은, 어떻게든 내 일의 가치를 부여하고 싶은 내 나름대로의 까뮈적인 반항이다.

# 3. Finite Markov Decision Process

## 3.1 finite MDP

Sutton의 3장에는 Finite MDP에 대한 내용이 나온다.
책을 읽어가면서 주요 식들은 책에서의 식번호와 같이 표기하려 한다.
하지만 책의 절 번호와는 다르게 쓰일 것이다.

수학에서 확률과정(random process, stochastic process)이란, 확률변수들의 나열이다.
혹은 collection of random variables라고 할 수도 있다.
Finite MDP 또한 확률과정의 일종으로 Markov property를 만족시키는 다음과 같은 확률변수들의 나열 

$$S_0,A_0,R_1,S_1,A_1,R_2,S_2,A_2,R_3,\cdots\tag{3.1}$$

로 정의되는데 이때, $S_t\in\mathcal S$, $A_t\in\mathcal A$, $R_t\in\mathcal R$이고 state space $\mathcal S$, action space $\mathcal A$, reward space $\mathcal R\subset\mathbb R$이 모두 유한집합이다.
이 확률변수들의 나열을 (조금 더 정확하게는 확률변수들의 값이 각각 지정된 상태를) trajectory라고도 한다.

Markov property란, state $S_t$와 reward $R_t$가 바로 이전의 state $S_{t-1}$와 action $A_{t-1}$에만 의존함을 뜻한다.
즉,

$$
\begin{align*}
&P\left\{S_t=s_t, R_t=r_t|\cdots, S_{t-2}=s_{t-2}, A_{t-2}=a_{t-2}, R_{t-1}=r_{t-1}, S_t=s_t, A_t=a_t\right\}\\
=
&P\left\{S_t=s_t, R_t=r_t|S_t=s_t, A_t=a_t\right\}
\end{align*}
$$

임을 뜻한다.
그 전에 어떤 과정을 거쳤건 간에, 다음 state과 reward를 예측하는 데에는 오로지 이전의 state와 action만이 영향을 미친다.
그렇다면 위 식의 우변에 해당하는 확률식은 이 MDP를 characterize하는 아주 중요한 값이 된다.
책에서의 식으로 쓰면

$$p(s',r|s,a)=P\left\{S_t=s',R_t=r|S_{t-1}=s,A_{t-1}=a\right\}\tag{3.2}$$

이 값은 transition dynamics라고 불린다.

그러니까 finite MDP는 $\mathcal S$, $\mathcal A$, $\mathcal R$, $p$의 네 집합 혹은 함수로 characterize될 수 있다.
David Silver의 자료에서는 discount factor $\gamma$까지 합쳐 다섯 개를 언급하고 있다.
하지만 여기서 $\mathcal R$이 그렇게까지 중요하진 않다.
그냥 $\mathbb R$으로 대체되어도 무관하기 때문이다.
따라서 이 포스트에서는 finite MDP들의 집합을 
$\mathscr D\left(\mathcal S, \mathcal A, p, \gamma\right)$와 같이 정의하려 한다.
각 trajectory
<!-- $$S_0,A_0,R_1,S_1,A_1,R_2,S_2,A_2,R_3,\cdots$$ -->
들은 $\mathscr D\left(\mathcal S, \mathcal A, p, \gamma\right)$의 한 원소라고 말할 수 있을 것 같다.


 <!-- (혹은 이 값보다는 덜 일반적인, 뒤에 나올 두 식 (3.4), (3.5)는) 여러 이름으로 불린다. -->
<!-- transition dynamics,  model function of environment 등의 이름이 있다. -->
실제로는, 위와 같이 일반적인 dynamics보다는 next state와 reward를 각각 따로 결정하는 다음의 두 식과 같이 주어지는 때가 더 많다.

$$
\begin{align*}
p(s'|s,a)&= P\left\{S_t=s'|S_{t-1}=s,A_{t-1}=a\right\}\tag{3.4}\\
r(s,a)&= \mathbb E\left[R_t|S_{t-1}=s,A_{t-1}=a\right]\tag{3.5}
\end{align*}
$$

위의 식은 state transition probabilty, 아래 식은 reward function이라고 불리며 이 둘을 합쳐 환경모델이라고도 한다.
그리고 물론, 식 (3.2)를 적절히 marginalize하면 (3.4)이 얻어지며, 그건 책에서도 잘 설명되고 있다.

사실 식 (3.5)는 좀 의아하다.
나같으면 식 (3.4)와 비슷하게 쓰기 위해

$$
p(r|s,a)= P\left\{R_{t+1}=r|S_{t-1}=s,A_{t-1}=a\right\}
$$

라고 쓸 것 같고, 이게 (3.5)보다 일반적인 식이 될 것이다.
하지만 물론, 식 (3.5)가 강화학습 상황에서 더 유용하고 자주 나올만한 식이 될 것이다.
예컨대 위의 식은 너무 추상적인 것이다.
심지어, 많은 강화학습 상황에서 reward는 action에 의존하지 않아서 더 간략하게 $r(s)$로 쓰는 것으로 충분한 경우가 많다.

마찬가지 관점에서, 식 (3.4)도 바꿔서 쓸 수 있다.

$$s'=f(s,a)$$

와 같이 쓸 수도 있는 것이다.
그리고 정말 많은 강화학습 상황에서 이렇게 next state가 deterministic하게 주어지지, (3.4)에서처럼 stochastic하게 주어지지 않을 수도 있다.

## 3.2 reward와 return

강화학습의 목적은 reward들의 cumulative sum $G_t$가 최대가 되도록 하는 것이다.
$G_t$를 return이라고 부른다.
trajectory는 일반적으로 끝날 수도 있고 (episodic task) 끝나지 않을 수도 있는데, 따라서 $G_t$는

$$G_t=R_{t+1}+R_{t+2}+\cdots+R_T\tag{3.7}$$

와 같이 유한합으로 정의될 수도 있고

$$G_t=R_{t+1}+R_{t+2}+\cdots$$

와 같이 급수로 정의될 수도 있다.
하지만 일반적으로는 discount factor $\gamma$를 적용하여 ($0\le\gamma\le1$)

$$G_t=\sum_{k=0}^\infty\gamma^kR_{t+k+1}\tag{3.8}$$

로 정의하며, 이 정의는 참으로 적절하다.
특히 $0\le\gamma\lt1$일 때가 절묘하다.
첫째로, 당연히 최근의 reward을 더 중요하게 여기고 예전의 reward는 덜 중요하게 여긴다는 점이 있지만, 그것보다는 둘째로, $G_t$가 well-defined된다는 점이 있다.
1보다 작은 discount factor를 적용하면, $G_t$가 마치 멱급수처럼 되어서, 항상 수렴하게 되는 것이다.
어떤 수학적인 거리낌도 없이, 자신있게 $G_t$를 쓸 수 있게 되는 것이다.
셋째로는, trajectory가 끝날 때와 끝나지 않을 때에 대하여 한꺼번에 쓸 수 있다는 장점이다.
$R_{T+1}=R_{T+2}=\cdots=0$
으로 정의하면 식 $(3.7)$이 되기 때문이다.
책에는 이런 말들이 3.4절에 잘 설명되어 있다.

## 3.3 policy와 value functions

transition dynamics 혹은 환경모델은 환경이 어떻게 구성되어있느냐를 나타낸다.
$S_t$, $A_t$가 주어졌을 때 $S_{t+1}$, $R_{t+1}$의 분포를 결정해준다.
하지만 아직 이것으로 식 (3.1)의 trajectory가 완전히 결정될 수는 없다.
즉, $S_t$가 주어졌을 때 $A_t$가 어떻게 결정될 것인가 하는 문제가 남는다.
이것을 결정하는 것이 정책(policy)이다.

transition dynamics가 agent를 둘러싼 state들이 어떻게 변화하느냐 하는 것을 나타낸다면, policy는 agent가 주어진 상태에 대해 어떤 행동을 취할것이냐 하는 것을 나타낸다.
policy $\pi$는 일반적으로 다음과 같이 조건부 확률

$$\pi(a|s)=P\left\{A_t=a|S_t=s\right\}$$

로 나타난다.
다시 말해 $\pi(\cdot|s)$는 주어진 state $s$에 대한 action의 확률분포를 의미한다.
이런 식의 stochastic policy가 일반적이지만, 좀 더 간단하게는 deterministic하게 action이 결정된다고 가정할 수도 있다.
특히 DPG, DDPG에서 이러한 determistic한 policy가 쓰이는데, 이때는 $\pi$ 대신 $\mu$를 써서

$$a=\mu(s)$$

라는 notation을 썼던 것 같다.

환경에 대한 모델 $p(s',r|s,a)$, $r(s,a)$와 agent의 정책 $\pi$이 주어지면, 이론적으로는 모든 종류의 trajectory의 가능성이 확률적으로 결정된다.
그러면, 어떤 시점 $t$에서 $G_t$의 기댓값, 즉 episode가 끝날때까지 혹은 영원히에 대한 보상의 합의 기댓값을 계산할 수 있다.
이것을 가치함수(value function)이라고 한다.
가치함수에는 두 가지가 있어서 state만 인자로 받는 state-value-function $v_\pi(s)$가 있고,  state과 action 두 개를 인자로 받는 action-value-function $q_\pi(s,a)$이 있다.
정확한 의미로 $q$는 state-action-value-function이라고 불러야 할테지만, 그냥 action-value-function이라고만 불러도 구분이 되니 그렇게 부르는 것이라고 대학원에서 배웠던 것 같다.
환경은 이미 주어졌다는 가정 하에, 정책 $\pi$가 명시되어야 하므로 아랫첨자로 달려있다.
상태 $s\in\mathcal S$에 대하여 $v_\pi(s)$를 식으로 적으면

$$v_\pi(s) = \mathbb E_\pi\left[G_t|S_t=s\right]\tag{3.12}$$

이며, 이것은 현재 상태 $s$에서 정책 $\pi$를 따라나갈 때의 return의 기댓값이다.
상태 $s\in\mathcal S$와 행동 $a\in\mathcal A$에 대하여 $q_\pi(s,a)$를 식으로 적으면

$$q_\pi(s,a)=\mathbb E_\pi\left[G_t|S_t=s,A_t=a\right]\tag{3.13}$$

이며, 이것은 현재 상태 $s$에서 행동 $a$를 취하고 정책 $\pi$를 따라나갈 때의 return의 기댓값이다.

Exercise 3.12, 13을 간단히 풀어보려 한다.
$v_\pi$를 $q_\pi$로 표현해보고 반대도 표현해보라는 문제로, 바로 다음 절의 Bellman equation의 증명과 깊숙히 연관되어 있다.
조금 헷갈리니까 자세히 쓰면 Exercise 3.12는

$$
\begin{align*}
v_\pi(s)
&=\mathbb E_\pi\left[G_t|S_t=s\right]\\
&=\sum_s gP\left\{G_t=g|S_t=s\right\}\\
&=\sum_s g\pi(a|s)P\left\{G_t=g|S_t=s, A_t=a\right\}\\
&=\sum_a\pi(a|s)\mathbb E_\pi\left[G_t|S_t=s,A_t=a\right]\\
&=\sum_a\pi(a|s)q_\pi(s,a)\tag{e3.12}
\end{align*}
$$

이 될 것이다.
Exercise 3.13은 간략하게

$$
\begin{align*}
q_\pi(s,a)
&=\mathbb E_\pi\left[G_t|S_t=s,A_t=a\right]\\
&=\sum_{s',r}p(s',r|s,a)\mathbb E_\pi\left[R_{t+1}+\gamma R_{t+2}|S_t=s,A_t=a, R_{t+1}=r,S_{t+1}=s'\right]\\
&=\sum_{s',r}p(s',r|s,a)\mathbb E_\pi\left[r+\gamma R_{t+2}|S_{t+1}=s'\right]\\
&=\sum_{s',r}p(s',r|s,a)\left(r+\gamma\mathbb E_\pi\left[R_{t+2}|S_{t+1}=s'\right]\right)\\
&=\sum_{s',r}p(s',r|s,a)\left(r+\gamma v_\pi(s')\right)\tag{e3.13}
\end{align*}
$$

와 같이 풀릴 수 있을 것이다.

## 3.4 Bellman equations

<!-- 모든 상태 $s\in\mathcal S$에 대하여 식(3.14)가 성립한다.
이것은 Bellman equation이라고 불린다. -->
<!-- 하지만 그 전에  -->
return에 대한 다음 식을 상기하자.

$$
\begin{align*}
G_t
&=R_{t+1}+\gamma R_{t+2}+\gamma^2R_{t+3}+\cdots\\
&=R_{t+1}+\gamma\left(R_{t+2}+\gamma R_{t+3}+\cdots\right)\\
&=R_{t+1}+\gamma G_{t+1}
\end{align*}
\tag{3.9}
$$

그러면 모든 $s\in\mathcal S$에 대하여 다음과 같은 Bellman equation이 성립한다.

$$
\begin{align*}
v_\pi(s)
&=\mathbb E_\pi\left[G_t|S_t=s\right]\\
&=\mathbb E_\pi\left[R_{t+1}+\gamma G_{t+1}|S_t=s\right]\\
&=\sum_a\pi(a|s)\mathbb E_\pi\left[R_{t+1}+\gamma G_{t+1}|S_t=s,A_t=a\right]\\
&=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\mathbb E_\pi\left[R_{t+1}+\gamma G_{t+1}|S_t=s,A_t=a,R_{t+1}=r,S_{t+1}=s'\right]\\
&=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\mathbb E_\pi\left[r+\gamma G_{t+1}|S_{t+1}=s'\right]\\
&=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\left(r+\gamma\mathbb E_\pi\left[G_{t+1}|S_{t+1}=s'\right]\right)\\
&=\sum_a\pi(a|s)\sum_{s',r}p(s',r|s,a)\left(r+\gamma v_\pi(s')\right)\tag{3.14}
\end{align*}
$$

두번째 줄에는 (3.9)가 쓰였고 세번째줄과 네번재 줄에는 conditional expectation의 의미가 쓰였다.
다섯번째 줄에는 Markov property가 쓰였고 여섯번째 줄에는 $\mathbb E$의 linearity가 쓰였다.
일곱번째 줄에는 $v_\pi$의 정의가 변형된 형태로 쓰였다.
이 중요한 식이, 처음 배울 때는 참 어렵게 느껴졌다.
지금은 잘 이해가 간다.

중요한 사실 중 하나는, 식 (3.14)가 복잡하게 생겼지만, 결국 $\lvert\mathcal S\rvert$개의 변수 $v_\pi(s)$에 대한 일차식이라는 것이다.
그리고 식이 $\lvert\mathcal S\rvert$개 있으므로, 변수가 $\lvert\mathcal S\rvert$개이고 식이 $\lvert\mathcal S\rvert$개인 일차연립방정식인 셈이다.
(3.14)은 서로 인접한 시간에서의 가치함수 값의 관계를 나타내는 식이다.
예컨대, 상태 $s$에서의 가치 $v_\pi(s)$를 다음 상태 $s'$에서의 가치 $v_\pi(s')$의 일차결합으로 표현하는 것이다.

방금 것은 $v$에 대한 Bellman equation이다.
$q$에 대해서도 Bellman equation이 있다.
모든 $s\in\mathcal S$와 모든 $a\in\mathcal A$에 대하여 다음 식이 성립한다.
책에는 이것이 Exercise 3.16로 되어있는 듯하고, 따로 식에 대한 라벨링이 되어 있지 않은데 e3.17로 표시해보려 한다.

$$
\begin{align*}
q_\pi(s,a)
&=\mathbb E_\pi\left[G_t|S_t=s, A_t=a\right]\\
&=\mathbb E_\pi\left[R_{t+1}+\gamma G_{t+1}|S_t=s, A_t=a\right]\\
&=\sum_{s',r} p(s', r|s,a)\mathbb E_\pi\left[R_{t+1}+\gamma G_{t+1}|S_t=s,A_t=a, R_{t+1}=r, S_{t+1}=s'\right]\\
&=\sum_{s',r} p(s', r|s,a)\mathbb E_\pi\left[r+\gamma G_{t+1}|S_{t+1}=s'\right]\\
&=\sum_{s',r} p(s', r|s,a)\left(r+\gamma\mathbb E_\pi\left[G_{t+1}|S_{t+1}=s'\right]\right)\\
&=\sum_{s',r} p(s', r|s,a)\left(r+\gamma\sum_{a'}\pi(a'|s')\mathbb E_\pi\left[G_{t+1}|S_{t+1}=s', A_{t+1}=a'\right]\right)\\
&=\sum_{s',r} p(s', r|s,a)\left(r+\gamma\sum_{a'}\pi(a'|s')q_\pi(s',a')\right)\tag{e3.17}
\end{align*}
$$

이것은 변수가 $\vert\mathcal S\vert\vert\mathcal A\vert$개이고 식의 개수도 $\vert\mathcal S\vert\vert\mathcal A\vert$개인 연립일차방정식이다.

그런데 증명을 하다보면 현재의 가치는 $v$로 두고 다음 상태의 가치는 $q$로 두고 싶어지고, 또 그 반대인 식도 만들어내고 싶어진다.
예를 들어 위의 증명을 조금만 바꾸면 다음 두 식이 성립한다 (exercise 3.18, 19).
식 (3.14)의 세번째줄로부터

$$
\begin{align*}
v_\pi(s)
&=\sum_a\pi(a|s)\mathbb E_\pi\left[R_{t+1}+\gamma G_{t+1}|S_t=s,A_t=a\right]\\
&=\sum_a\pi(a|s)q_\pi(s,a)
\tag{e3.18}
\end{align*}
$$

이고 식 (e3.17)의 다섯번째 줄로부터

$$
\begin{align*}
q_\pi(s,a)
&=\sum_{s',r} p(s', r|s,a)\left(r+\gamma\mathbb E_\pi\left[G_{t+1}|S_{t+1}=s'\right]\right)\\
&=\sum_{s',r} p(s', r|s,a)\left(r+\gamma v_\pi(s')\right)
\tag{e3.19}
\end{align*}
$$


## 3.5 optimal policies

(9월 3일에 다시 작성하기 시작) 쓰다보니 또 스크롤이 길어지고 있다.
늘 그렇듯 원래 정했던 목표보다 더 근본적인 것부터 써가고 있다.

꽤 자세히 적어나가면서 Sutton의 책을 열심히 보게된다는 점은 좋다.
마치 공자의 『논어』에 대하여 위나라의 하안이 『논어집해』를 쓰며 주석을 달아가는 느낌이랄까.


---

Sutton은 컴퓨터공학자라고 한다.
첫 회사에서 만난, Texas A&M 대학교에서 석박통합과정으로 공부하고 있다는 컴공출신 분이, 어떤 사람이 '수학을 잘한다'라는 표현을 썼었다.
그 분에게는 아무 말도 하지 않았지만 '수학을 잘한다'는 것에 대한 말의 의미에 대해 한참동안 고민했다.
미적분이나 선형대수, 해석학이나 집합론, 위상수학과 대수학, 측도론과 확률론 등의 기본 개념들에 대해 깊이 이해하고 그 원리를 명확하게 알려고 노력하는 것을 그 말한 분이 얼마나 해봤을까, 하고 생각했다.
아마도 baby rudin 정도의 엄밀한 reasoning을 따라가본 적은 없을 것도 같은데.
보통 컴퓨터공학을 전공한 분들이 말하는 '수학을 잘한다', '머리가 좋다'는 건 어떤 개념일까, 하는 의문은 이쪽 업계에 있으면서 자주 든다.
새로 부임하셨던, 컴퓨터공학 출신의 교수님이 수학에 대해 포괄적으로 논하는 것도 들었지만, 글쎄, 내가 범접할 수 없는 천재라면 모를까 수학은 그렇게 쉽게 정복될 수 있는 대상이 아니라는 생각이다.
직전 직장에서 같이 일했던 어떤 분도 '수학을 잘하면 인공지능으로 돈을 많이 벌 수 있다'고 쉽게 말한 적이 있는데, 그분은 미적분도 잘 이해하지 못하실 것 같은데 어떤 의미로 그런 말씀을 하신 것인지 의아해 했던 적도 있다.

말이 길었는데, 나는 Sutton은 컴퓨터공학자임에도 불구하고 수학을 잘 아는 사람이라고 쓰려고 했다.
그 근거는 어떤 정책이 더 나은 정책이며, 가장 좋은 정책인 최적정책을 정의하는 데에 집합론의 partial ordering을 쓰고 있기 때문이다.
물론 이 개념은 학부 2학년 때 나오는 아주 기초적인 개념이고, 이해하는 데 몇 분 안 걸리는 개념일지도 모르겠고, 실제로 Sutton은 partial order라는 말을 명시적으로 쓰고 있지는 않다.
하지만 나는 poset으로 optimal policy를 설명한 이 방식을 보고 이 책이 좋아졌고 Sutton이 좋아졌다.
정말 적절하게 쓰였다고 생각되기 때문이다.

---


어떤 $\mathscr D(\mathcal S,\mathcal A, p, \gamma)$에 대하여 policy들의 집합을 $\Pi$라고 표시하자.

<!-- $$\Pi=\left\{\pi(\cdot|\cdot):\mathcal S\times\mathcal A\to[0,1]\,\vert\,\sum_{a\in\mathcal A}\pi(a|s)=1\right\}$$ -->

$$\Pi=\left\{\pi:\mathcal S\times\mathcal A\to[0,1]\,\vert\,\sum_{a\in\mathcal A}\pi(a|s)=1\right\}$$

$\Pi$에 partial order $\le$를 다음과 같이 정의해 partially ordered set $\left(\Pi,\le\right)$를 생각할 수 있다.
두 policy $\pi, \pi'\in\Pi$에 대하여 $\pi\le\pi'$인 것의 정의는 모든 $s\in\mathcal S$에 대하여

$$v_\pi(s)\le v_{\pi'}(s)$$

인 것이다.
$\left(\Pi,\le\right)$는 분명히 totally ordered set은 아니다.
따라서 $\left(\Pi,\le\right)$는 maximal의 존재는 보장되지만, maximum의 존재는 보장될 수 없다.
하지만 이 경우에는 maximum이 보장된다는 것을 Sutton은 말하고 있다.
즉, 정책들의 최댓값, 혹은 최적 정책(optimal policy, $\pi_\ast$)의 존재한다.
다시 말해,

<div class="notice--info" markdown="1">
모든 $\pi\in\Pi$에 대하여 $\pi\le\pi_\ast$인 $\pi_\ast$가 존재한다.
</div>

이것은 Sutton의 책에 언급만 되어있고 설명이나 증명이 있지는 않다.
그래서 간략하게 다음과 같이 증명해 해보려고도 했다.

### 3.5.1 직접 증명 시도
함수 $v_\ast$를

$$v_\ast(s)=\sup_{\pi\in\Pi}v_\pi(s)$$

로 정의하자.
그러면 모든 정책 $\pi\in\Pi$에 대하여

$$v_\pi (s)\le v_\ast(s)$$

가 성립한다.
$v_\ast$에 대응하는 함수 $q_\ast$를 (e3.13)와 비슷하게

$$q_\ast(s,a)=\sum_{s',r}p(s',r|s,a)(r+\gamma v_\ast(s'))$$

로 정의하자.
그러면 $q_\ast$도 최대가 된다.
즉, 모든 $s$, $a$에 대하여 $q(s,a)\le q_\ast(s,a)$이다.
왜냐하면 모든 $\pi\in\Pi$에 대하여, $p(s',r|s,a)\le0$, $\gamma\ge0$으로부터

$$
\begin{align*}
q_\pi(s,a)
&=\sum_{s',r}p(s',r|s,a)(r+\gamma v_\pi(s'))\\
&\le\sum_{s',r}p(s',r|s,a)(r+\gamma v_\ast(s'))\\
&=q_\ast(s,a)
\end{align*}
$$

이기 때문이다.
이제 이로부터 최적정책 $\pi_\ast$를 greedy하게 정의한다.
$q$함수가 주어졌으니, 주어진 $s$에 대해서 $q$값이 가장 큰 action(들)을 다 더해서 1이 되도록 양의 확률을 주고 나머지 경우는 모두 0으로 주는 것이다.
예를 들어, $A_s=\\{a'\in A:q_\ast(s,a')=\max_{a\in\mathcal A}q_\pi(s,a)\\}$ 로 두고

$$
\begin{align*}
\pi_\ast(a|s) =
\begin{cases}
\frac1{\left|A_s\right|}&a\in A_s\\
0                   &a\notin A_s\\
\end{cases}
\end{align*}
$$

로 할 수도 있고, 아니면 $q_\ast(s,a_\ast)=\max_{a\in\mathcal A}q_\pi(s,a)$를 만족시키는 action $a_\ast$에 대하여

$$
\begin{align*}
\pi_\ast(a|s) =
\begin{cases}
1   &a=a'\\
0   &a\ne a'\\
\end{cases}
\end{align*}
$$

로 둘 수도 있는 것이다.
<!-- 두번째 경우를 $\pi_\ast$로 사용하자. -->
<!-- 그러면 모든 $\pi\in\Pi$에 대하여 -->

그러면, $\pi_\ast$는 주어진 상황에서의 가장 greedy한 정책일 수 있다.
하지만 정말로 이 정책이 optimal한지를 밝히는 것은 쉽지 않아보인다.
증명이 잘 되지 않아, [두](https://ai.stackexchange.com/q/48963/97276) [곳](https://math.stackexchange.com/q/5096132/746048)에 질문을 올려놓았다.
누군가 풀어준다면 좋겠다.

<!-- 
그러면, 특정한 면에서는 $\pi_\ast$가 일반적인 $\pi$보다 나은 점이 있기는 하지만, 그렇다고 해서 $v_\pi\le v_{\pi_\ast}$를 증명할 수는 없는 것으로 보인다. -->

<!-- $$
\begin{align*}
v_\pi(s)
&=\sum_{a\in\mathcal A}\pi(a|s)q_\pi(s,a),\\
&=q_{\pi_\ast}(s,a)\\
&\le\sum_{a\in\mathcal A}\pi_\ast(a|s)q_{\pi_\ast}(s,a),\\
&=q_\pi(s,a')\\
&=v_{\pi_\ast}(s)
\end{align*}
$$

이다.
따라서 Claim 1이 증명되었다. -->

<!-- optimal policy의 존재성, 그것을 증명하는 건 꽤 만만치 않은일이다. -->

### 3.5.2 some articles

이 주제에 대해 몇몇 읽을만한 글들이 있어서 읽어봤다.
[Ashwin Rao](https://web.stanford.edu/class/cme241/lecture_slides/OptimalPolicyExistence.pdf)는 말로서 적절히 optimal policy의 존재성을 증명하고 있다.
다음과 같이 쓰고 있다.

![lemma_f]({{site.url}}\images\2025-08-20-finite_mdp\ashwin_rao.png){: .img-90-center}

이 증명을 적절히 비슷하게 서술하면 어쨌든 optimal policy가 존재함을 증명하는 듯이 서술하고 넘어갈 수도 있다.
하지만, 문제는 내 스스로가 잘 와닿지 않는다는 점이다.

[Alireza Modirshanechi](https://medium.com/data-science/why-does-the-optimal-policy-exist-29f30fd51f8c)는 정책이 한 걸음 더 나아갈 수 있음을 아주 멋지게 설명하고 있는데 (Theorem 1) 결국 이건 Sutton 책에서의 policy improvement를 다시 쓴 것에 불과하다.
그리고 이를 통해 optimal policy의 존재성을 증명하고 있지는 않다.

---

직접 증명하려고 했던 것도 성공하지 못했고, 기존 자료를 이해하는 것도 실패했다.
하지만 책의 내용을 따라가면 policy iteration으로부터 optimal policy의 존재성을 증명할 수 있을지도 모르겠다.

## 3.6 Bellman optimal equations

(9/14)이 이론들을 처음 본 건 벌써 5년 전이다.
그런데도 아직도 개념들이 헷갈리고 계산에 확신이 없는 걸 보면 당시에 제대로 공부하지 않았던 것이 확실하다.
지금도 그냥 Bellman equation에 적당히 maximum을 달면 되는 게 아닌가 하는 안일한 생각을 가지고 있으면서, 얼른 Bellman operator와 contraction principle로 넘어가려 했던 것이다.
하지만 Sutton의 책 내용을 잘 따라가는 것만으로도 솔직히 조금 벅차다.
그렇다고 Sutton 책의 내용이 이해가 안간다는 건 아니다.
그러니 욕심을 내지는 말고, 잘 따라가보자.

---

optimal policy $\pi_\ast$가 존재한다고 가정하자.
$\pi_\ast$에 따른 state value function과 action value function은 비슷한 종류의 maximality를 가진다.
즉, $v_\ast=v_{\pi_\ast}$, $q_\ast=q_{\pi_\ast}$라고 표기하면, 모든 $\pi\in\Pi$에 대하여

$$
\begin{align*}
v_\pi(s)&\le v_\ast(s)&&\forall s\in\mathcal S\\
q_\pi(s,a)&\le q_\ast(s,a)&&\forall s\in\mathcal S,\forall a\in\mathcal A
\end{align*}
$$

이다.
혹은, 조금 더 정확하게

$$
\begin{align}
v_\ast(s)&=\max_\pi v_\pi(s)&&\forall s\in\mathcal S\tag{3.15}\\
q_\ast(s,a)&=\max_\pi q_\pi(s,a)&&\forall s\in\mathcal S,\forall a\in\mathcal A\tag{3.16}
\end{align}
$$

이다.
<!-- 을 만족시키는 정책 $\pi_\ast$가 존재한다. -->

이때, 다음과 같은 Bellman optimal equation이 성립한다.
어떤 정책에도 의존하지 않는 식이라고 Sutton은 강조한다.
순수하게 $v_\ast(s)$들 사이의 관계식으로서 다음 식이 성립한다.
모든 $s\in\mathcal S$에 대하여,

$$
\begin{align*}
v_\ast(s)
&=\max_a q_\ast(s,a)\\
&=\max_a\mathbb E_{\pi_\ast}\left[G_t|S_t=s,A_t=a\right]\\
&=\max_a\mathbb E_{\pi_\ast}\left[R_{t+1}+\gamma G_{t+1}|S_t=s,A_t=a\right]\\
&=\max_a\sum_{s',r}\mathbb E_{\pi_\ast}\left[R_{t+1}+\gamma G_{t+1}|S_t=s,A_t=a,S_{t+1}=s',R_{t+1}=r\right]p(s',r|s,a)\\
&=\max_a\sum_{s',r}\mathbb E_{\pi_\ast}\left[r+\gamma G_{t+1}|S_{t+1}=s'\right]p(s',r|s,a)\\
&=\max_a\sum_{s',r}\left(r+\gamma \mathbb E_{\pi_\ast}\left[G_{t+1}|S_{t+1}=s'\right]\right)p(s',r|s,a)\\
&=\max_a\sum_{s',r}\left(r+\gamma v_\ast(s')\right)p(s',r|s,a)\tag{3.18}\\
&=\max_a\mathbb E\left[R_{t+1}+\gamma v_\ast\left(S_{t+1}\right)|S_t=s,A_t=a\right]\tag{3.19}
\end{align*}
$$

이다.
좌변이 (3.18) 또는 (3.19)와 같다는 것이 state value function에 대한 Bellman optimal equation이다.
두 식은, 정말로 정책에 independent한 식이다.
(3.19)에 subscript로 $\pi$같은게 없다는 것이다.

(3.18)은 $|\mathcal S|$개의 변수 $v_\ast(s)$에 대한 $|\mathcal S|$개의 식이다.
즉, 연립방정식인데 $\max$를 포함하고 있어 일차연립방정식은 아닌 비선형 연립방정식이다.
$q_\ast$ 버전의 Bellman optimal equation은 다음과 같다.

$$
\begin{align*}
q_\ast(s,a)
&=\mathbb E_{\pi_\ast}\left[G_t|S_t=s,A_t=a\right]\\
&=\mathbb E\left[R_{t+1}+\gamma v_\ast(S_{t+1})|S_t=s,A_t=a\right]\\
&=\mathbb E\left[R_{t+1}+\gamma\max_{a'}q_\ast\left(S_{t+1},a\right))|S_t=s,A_t=a\right]\\
&=\sum_{s',r}p(s',r|s,a)\left[r+\gamma\max_{a'}q_\ast(s',a')\right]\tag{3.20}
\end{align*}
$$

이 식, 특히 세번째 줄은 Q-learning에 대한 이론적 근거가 된다.