---
layout: single
title: "Policy Evaluation"
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

그러니 4장의 Dynamic Programming 내용이 더 잘 보이기 시작했다.
여기서 설명하는 방법론들은 강화학습 전반에 사용되는 방법론이기 때문에 중요했다.
Dynamic Programming의 핵심이 되는 두 방법 중