---
layout: single
title: "Belllman optimal equation"
categories: machine-learning
tags: [reinforcement learing, Bellman optimal equation, partial order]
use_math: true
publish: false
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
미적분이나 선형대수, 해석학이나 집합론, 위상수학과 측도론, 확률론 등의 기본 개념들에 대해 깊이 이해하고 그 원리를 명확하게 알려고 노력하는 것을 그 말한 분이 얼마나 해봤을까, 하고 생각했다.
아마도 baby rudin 정도의 엄밀한 reasoning을 따라가본 적은 없을 것도 같은데.
보통 컴퓨터공학을 전공한 분들이 말하는 '수학을 잘한다', '머리가 좋다'는 건 어떤 개념일까, 하는 의문은 이쪽 업계에 있으면서 자주 든다.
새로 부임하셨던, 컴퓨터공학 출신의 교수님이 수학에 대해 포괄적으로 논하는 것도 들었지만, 글쎄, 내가 범접할 수 없는 천재라면 모를까 수학은 그렇게 쉽게 정복될 수 있는 대상이 아니라는 생각이다.
같이 일했던 어떤 분도 '수학을 잘하면 인공지능으로 돈을 많이 벌 수 있다'고 쉽게 말한 적이 있는데, 그분은 미적분도 잘 이해하지 못하실 것 같은데 어떤 의미로 그런 말씀을 하신 것인지 의아해 했던 적도 있다.

말이 길었는데, 나는 Sutton은 컴퓨터공학자임에도 불구하고 수학을 잘 아는 사람이라고 쓰려고 했다.
그 근거는 어떤 정책이 더 나은 정책이며, 가장 좋은 정책인 최적정책을 정의하는 데에 집합론의 partial ordering을 쓰고 있기 때문이다.
물론 이 개념은 학부 2학년 때 나오는 아주 기초적인 개념이고, 이해하는 데 몇 분 안 걸리는 개념일지도 모르겠다.
하지만 나는 poset으로 optimal policy를 설명한 이 방식을 보고 이 책이 좋아졌고 Sutton이 좋아졌다.
정말 적절하게 쓰였다고 생각되기 때문이다.

여기에 쓰는 것은 Sutton 책의 내용을 따라가되, 설명이 더 필요한 부분을 보충한 것이다.

# 4. Bellman optimal equations

어떤 finite MDP $\mathscr F(\mathcal S,\mathcal A,\mathcal R)$에 대하여 policy들의 집합을 $\Pi$라고 표시하자.

$$\Pi=\left\{\pi(\cdot|\cdot):\mathcal S\times\mathcal A\to[0,1]\,\vert\,\sum_{a\in\mathcal A}\pi(a|s)=1\right\}$$

$\Pi$에 partial order $\ge$를 다음과 같이 정의해 partially ordered set $\left(\Pi,\ge\right)$를 생각할 수 있다.
두 policy $\pi, \pi'\in\Pi$에 대하여 $\pi\ge\pi'$인 것의 정의는 모든 $s\in\mathcal S$에 대하여

$$v_\pi(s)\ge v_{\pi'}(s)$$

인 것이다.
$\left(\Pi,\ge\right)$는 분명히 totally ordered set은 아니다.
따라서 $\left(\Pi,\ge\right)$는 maximal의 존재는 보장되지만, maximum의 존재는 보장될 수 없다.
하지만 이 경우에는 maximum이 보장된다.
즉, 정책들의 최댓값, 혹은 최적 정책(optimal policy, $\pi^\ast$)의 존재가 보장된다.
다시 말해, 모든 $\pi\in\Pi$에 대하여 $\pi\le\pi^\ast$인 $\pi^\ast\in\Pi$가 존재한다. (Claim 1)

먼저, 함수 $v_\ast$를

$$v_\ast(s)=\max_{\pi\in\Pi}v_\pi(s)$$

로 정의하자.
저 정의가 조금 덜 엄밀할 것 같으면 ($\Pi$가 무한집합이라 최댓값이 존재하지 않을 수 있으니) $\max$를 $\sup$으로 바꿔도 될 듯하다.

그러면 모든 정책 $\pi\in\Pi$에 대하여

$$v_\ast(s)\le v_\pi(s)$$

가 성립한다.
그리고 이에 대응하는 함수 $q_\ast$를 (e3.13)와 비슷하게 다음과 같이 정의하고

$$q_\ast(s,a)=\sum_{r,s'}p(r,s'|s,a)(r+\gamma v_\ast(s'))$$

정책 $\pi^\ast$를 다음과 같이 정의한다.
주어진 $s\in\mathcal S$에 대하여
$A=\{a'\in\mathcal A:q_\ast(s,a')=\max_a q_\ast(s,a)\}$는 $|A|\ge1$을 만족하므로

$$\pi^\ast(a|s)=
\begin{cases}
\frac1{|A|}&a\in A\\
0&a\notin A
\end{cases}
$$

로 정의하면 $\pi^\ast$는 optimal policy가 된다.