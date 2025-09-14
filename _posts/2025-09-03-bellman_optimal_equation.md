---
layout: single
title: "Policy Evaluation과 Contraction Principle"
categories: machine-learning
tags: [reinforcement learing, Bellman optimal equation, partial order]
use_math: true
published: false
author_profile: false
toc: true
---

(9월 3일에 다시 작성하기 시작) 쓰다보니 또 스크롤이 길어지고 있다.
늘 그렇듯 원래 정했던 목표보다 더 근본적인 것부터 써가고 있다.
이렇게 써가는게 맞긴 한데, 글이 길어지니 나눠버렸다.
이전 글까지는 finite MDP와 policy, value function, Bellman equation에 대해 썼고 이 글에서는 Bellman optimal equation에 대해 쓸 것 같다.
이후에는, 원래 쓰려고 했던 주제인 policy iteration과 contraction principle에 대해 쓸 것 같다.

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
그 근거는 어떤 정책이 더 나은 정책이며, 가장 좋은 정책인 최적정책을 정의하는 데에 집합론의 partial ordering과 비슷한 개념을 쓰고 있기 때문이다.
물론 이 개념은 학부 2학년 때 나오는 아주 기초적인 개념이고, 이해하는 데 몇 분 안 걸리는 개념일지도 모르겠다.
그리고 어쩌면, Sutton은 별 생각 없이 썼을지도 모르겠다.
결국 이 책에서 partial order라는 말이 나오진 않으니까.
하지만 나는 poset으로 optimal policy를 설명한 이 방식을 보고 이 책이 좋아졌고 Sutton이 좋아졌다.
정말 적절하게 쓰였다고 생각되기 때문이다.

여기에 쓰는 것은 Sutton 책의 내용을 따라가되, 설명이 더 필요한 부분을 보충한 것이다.

---

써나가다가, optimal policy의 존재성을 Sutton이 두리뭉실하게 넘어가고 있길래 한참 고민을 했다.
처음에는 직접 증명해보려고 했다.
optimal policy는 아니어도 optimal state value function인 $v_\ast$과 optimal action value function인 $q_\ast$는 정의할 수 있으니, $q_ast$로 greedy하게 정책을 만든 것이 최적정책임을 보이려 했다.
그런데 잘 되지 않았다.

다른 자료들을 찾아보니 [ASHWIN RAO의 자료](https://web.stanford.edu/class/cme241/lecture_slides/OptimalPolicyExistence.pdf)는 해당 증명을 하고 있는 듯보이나, 증명의 중요한 부분이 명확하게 다가오지 않았다. (혹은 이해되지 않았다. 두루뭉실하게 넘어가는 듯 느껴졌다.) stackoverflow의 [몇몇](https://mathoverflow.net/q/282492) [질문](https://stats.stackexchange.com/q/207780/398741)들도 신통한 답변이 없는 듯 보였다, 혹은 내가 이해를 못한 걸수도 있겠다.
그래도 가장 괜찮아보이는 [medium에서의 자료](https://medium.com/data-science/why-does-the-optimal-policy-exist-29f30fd51f8c)가 가장 괜찮아보였다.
여기서는 optimal policy의 존재성을 직접 증명하려 하지 않는 듯하다.
오히려, (정작 내가 쓰고 싶었던 주제인) policy evaluation과 contraction principle을 통해 optimal policy로 수렴할 수 있음을, (그래서 optimal policy가 존재함을) 보이고 있다.
그러니, Bellman optimal equation 절에서는 그냥 해당 식들만 나열하고, optimal policy를 증명하는 것은 그 다음 절에서 나아가면 괜찮겠다 싶다.

# 4. Bellman optimal equations

어떤 finite MDP $\mathscr F(\mathcal S,\mathcal A,\mathcal R)$에 대하여 policy들의 집합을 $\Pi$라고 표시하자.

$$\Pi=\left\{\pi(\cdot|\cdot):\mathcal S\times\mathcal A\to[0,1]\,\vert\,\sum_{a\in\mathcal A}\pi(a|s)=1\right\}$$

$\Pi$에 partial order $\le$를 다음과 같이 정의해 partially ordered set $\left(\Pi,\le\right)$를 생각할 수 있다.
두 policy $\pi, \pi'\in\Pi$에 대하여 $\pi\le\pi'$인 것의 정의는 모든 $s\in\mathcal S$에 대하여

$$v_\pi(s)\le v_{\pi'}(s)$$

인 것이다.
$\left(\Pi,\le\right)$는 분명히 totally ordered set은 아니다.
따라서 $\left(\Pi,\le\right)$는 maximal의 존재는 보장되지만, maximum의 존재는 보장될 수 없다.
하지만 이 경우에는 maximum이 보장된다.
즉, 정책들의 최댓값, 혹은 최적 정책(optimal policy, $\pi^\ast$)의 존재한다.
다시 말해, $\pi\le\pi^\ast$인 $\pi^\ast\in\Pi$가 존재한다. (Claim 1)

이것은 Sutton의 책에 언급만 되어있고 설명이나 증명이 있지는 않다.
그래서 간략하게 다음과 같이 증명해 해보려 한다.

먼저, 함수 $v_\ast$를

$$v_\ast(s)=\max_{\pi\in\Pi}v_\pi(s)$$

로 정의하자.
저 정의가 조금 덜 엄밀할 것 같으면 ($\Pi$가 무한집합이라 최댓값이 존재하지 않을 수 있으니) $\max$를 $\sup$으로 바꿔도 될 듯하다.
그러면 모든 정책 $\pi\in\Pi$에 대하여

$$v_\pi (s)\le v_\ast(s)$$

가 성립한다.
$v_\ast$에 대응하는 함수 $q_\ast$를 (e3.13)와 비슷하게

$$q_\ast(s,a)=\sum_{r,s'}p(r,s'|s,a)(r+\gamma v_\ast(s'))$$

로 정의하자.
그러면 $q_\ast$도 최대가 된다.
즉, 모든 $s$, $a$에 대하여 $v(s,a)\le q_\ast(s,a)$이다.
왜냐하면 모든 $\pi\in\Pi$에 대하여, $p(r,s'|s,a)\le0$, $\gamma\ge0$으로부터

$$
\begin{align*}
q_\pi(s,a)
&=\sum_{r,s'}p(r,s'|s,a)(r+\gamma v_\pi(s'))\\
&\le\sum_{r,s'}p(r,s'|s,a)(r+\gamma v_\ast(s'))\\
&=q_\ast(s,a)
\end{align*}
$$

이기 때문이다.
이제 이로부터 최적정책 $\pi^\ast$를 greedy하게 정의한다.
$q$함수가 주어졌으니, 주어진 $s$에 대해서 $q$값이 가장 큰 action(들)을 다 더해서 1이 되도록 양의 확률을 주고 나머지 경우는 모두 0으로 주는 것이다.
예를 들어, $A_s=\\{a'\in A:q_\ast(s,a')=\max_{\pi\in\Pi}q_\pi(s,a)\\}$ 로 두고

$$
\begin{align*}
\pi^\ast(a|s) =
\begin{cases}
\frac1{\left|A_s\right|}&a\in A_s\\
0                   &a\notin A_s\\
\end{cases}
\end{align*}
$$

로 할 수도 있고, 아니면 $q_\ast(s,a_\ast)=\max_{\pi\in\Pi}q_\pi(s,a)$를 만족시키는 action $a_\ast$에 대하여

$$
\begin{align*}
\pi^\ast(a|s) =
\begin{cases}
1   &a=a'\\
0   &a\ne a'\\
\end{cases}
\end{align*}
$$

로 둘 수도 있는 것이다.
<!-- 두번째 경우를 $\pi^\ast$로 사용하자. -->
그러면 모든 $\pi\in\Pi$에 대하여

$$
\begin{align*}
v_\pi(s)
&=\sum_{a\in\mathcal A}\pi(a|s)q_\pi(s,a),\\
&=q_{\pi_\ast}(s,a)\\
&\le\sum_{a\in\mathcal A}\pi^\ast(a|s)q_{\pi_\ast}(s,a),\\
&=q_\pi(s,a')\\
&=v_{\pi^\ast}(s)
\end{align*}
$$

이다.
따라서 Claim 1이 증명되었다.