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

더 시간이 지나기 전에 Dynamic programming 포스팅을 끝내고 싶은 마음이 생겼다.
다른 주제들도 공부하여 포스팅을 남기고 싶은데 DP를 끝내지 않고 다른 것을 쓰기는 싫기 때문이다.
그러니까 일종의 의무감에서 이 글을 쓰고 있다.
당장 이전부터 PCA와 PLS에 대해 공부하고 싶었고 얼마 전에는 game theory나 control theory에 손을 댈까도 생각했었는데, 오늘은 MPC와 LQR을 배워야 할 필요가 생긴 것이다.
그러니 DP는 빠르게 공부하여 치워버리자.

그리고 사실 글을 쓸 준비가 되어있다고 생각한다.
[이전 글](https://govin08.github.io/machine-learning/policy_evaluation/)을 쓰고 나서 간간이 4.2절을 보았고 어느 정도 이해는 했던 터였다.

책의 내용대로 쭉 따라가서 GPI까지는 써볼까.

## 4.2 Policy Improvement

policy evaluation을 통해 $v_\pi$ 또는 $q_\pi$를 추정한 것은 더 나은 정책을 찾기 위함이라고 봐도 과언이 아니다.
특히 action function $q_\pi$를 알고 있다면 더 나은 정책을 

<div class="notice--info">
<b> 정책 개선 정리 (policy improvement theorem) </b> <br>
사건 $A$가 일어났다는 것을 가정했을 때 사건 $B$가 일어날 확률을 조건부확률이라고 부르고, $P(B\mid A)$로 적습니다.
$P(B\mid A)$의 값은
$$P(B\mid A)=\frac{P(A\cap B)}{P(A)}$$
으로 정의됩니다.
</div>
