---
layout: single
title: "Policy Evaluation과 Contraction Principle"
categories: machine learning
tags: [linear algebra, matrix]
use_math: true
publish: false
author_profile: false
toc: true
---

얼떨결에 강화학습 업무를 맡았을때는 매우 당황스러웠고 지금도 막막하지만, 그래도 하나 얻은 것이 있다.
대학원 시절에는 잘 읽히지 않던 Sutton의 책이 지금은 읽힌다는 것이었다.
입사 첫주에는 Sutton의 책을 열심히 읽어봤다.
결국은 DPG와 DDPG를 구현해야 하는 어려운 일로 가야하지만, 그전에 당연히 DQN을 알아야 할 것이고 또 그 전에는 SARSA, Q-learning을 알아야 한다.
그리고 Sutton 책에서는 Dynamic Programming도 RL의 맥락 속에 포함되어 있다.

이전에 공부했을 때처럼 2장의 Multi Armed Bandit은 읽지 않았다.
Sarsa와 Q-learning을 보려면 6장의 Temporal-Difference Learning을 알아야해서 여기부터 읽어보니, 어느 정도 잘 읽혔다.
사실 이전에는 TD라는 것의 의미 자체를 이해하지 못했는데, 이번에는 이해하게 되었다.
TD가 dynamic programing과 Monte Carlo의 절충이라고 할 때, 그렇다면 그 둘도 알아야 할텐데, MC는 별로 보기가 싫어서 DP를 먼저 보았다.
즉, 4장의 Dynamic Programming을 읽었다.
그리고 거의 다 아는 내용일지언정, 배경지식을 알아야 하니 3장도 쭉 다 읽었다.
3장에서는 늘 그렇듯 가장 중요하면서 조금 어려운 것이 Bellman equation 네 개 (v, q, optimal v, optimal q)인데 그걸 다시 보았다.
다 이해하지는 못했고, 그래도 (ordinary) Bellman equation 두 개는 이해했다.

그러니 4장의 DP 내용이 더 잘 보이기 시작했다.
여기서 설명하는 방법론들은 강화학습 전반에 사용되는 방법론이기 때문에 중요하다.
DP의 핵심이 되는 policy iteration의 두 방법 policy evaluation과 policy improvement 중 policy iteration을 보다가 의문이 생겼다.
저렇게 iterative한 방법을 통해 value function을 결정하는데, 그게 맞는 것인가.
수렴하는 게 맞는 것인가?
수렴한다면 옳은 값으로 수렴하는 게 맞는 것인가?

의문이 들어 인터넷을 검색해보니 나와 같은 의문을 가진 사람이 질문했고, likes를 가장 많이 받은 답변자는 Bellman operator를 contraction mapping으로 보아 contraction principle을 통해 증명했다.
그래서 이에 관해 정리해보려 한다.

Bellman operator에 관해서는 답변자가 쓴 대로 vector space를 쓸 필요는 없을 것 같다.
contraction principle은 어디서 본 것 같았다, 아마 Munkres의 Topology 책에서였던 것 같아 Topology 책을 뒤져보니 이때 나왔던 contraction과 fixed point는 $B^2$에서의 특수한 이야기였다.
대학원때 봤던 Rudin의 Real and Complex Analysis에서 Banach의 이름이 붙은 theorem이 있었던 것 같았는데 찾아보니 아니었다.
baby rudin을 보니 contraction principle이 본문에 떡하니 있었고 (왜 나는 그걸 기억하지 못하는가.) 정확히 해당 증명에 필요한 정리가 있었다.
사실 Munkres의 책에도 비슷한 정리가 있었지만 baby rudin에서의 설명이 더 쉬웠다.
정확하게는 baby rudin에서는 metric space에서의 증명을 하고 있었고 그 증명은 간결했다.
Munkres의 책에서는 꼭 metric space일 필요가 없는 상황에서 특정한 위상조건들이 주어졌을 때 contraction principle이 주어져있었고, 증명이 어려워보였기에 읽지 않았다.

그러니, 아마 MDP에 대한 간략한 설명과 더불어 bellman equation (and bellman optimal equation) for value function $v$, policy evaluation, contraction principle, fixed point, Bellman operator 등의 내용이 이 포스트에 쓰일 것 같다.

사실 이 블로그를 시작하게 된 건 머신러닝에 관한 여러 사항들을 정리하고 싶어서였다.
그런데 머신러닝보다는 수학을 좋아하는지라 제대로 된 머신러닝 글이 거의 없었는데 이번에 쓰게 되지 않을까 싶다.