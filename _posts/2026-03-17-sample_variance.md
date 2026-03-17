---
layout: single
title: "표본분산의 정의"
categories: mathematics
tags: [statistics, sample mean, sample variance, sample standard deviation]
use_math: true
published: true
author_profile: false
toc: true
---

고등학교의 「확률과 통계」같은 과목에 보면 크기가 $n\ge2$인 표본 $\\{x_1,\cdots,x_n\\}$의 표본평균은

$$\bar x=\frac1n\sum_{i=1}^nx_i$$

으로 두고 표본분산은

$$
s^2=\frac1{n-1}\sum_{i=1}^n\left(x_i-\bar x\right)^2
$$

으로 둔다.
왜 $n$으로 나누지 않고 $n-1$로 나누지 않는가, 하는 질문은 너무나 당연한 질문이지만 대답하는 것이 쉽지만은 않다.
이 주제를 다뤄보려고 한다.

고등학교때 이 개념을 배울 때에도, 시간이 흘러 학생들을 가르칠 때에도 표본분산을 저렇게 정의하는 명확한 이유를 알지 못한 채로 계속 미뤄왔다.
그러다, 문득 제대로 이해하고 싶은 생각이 들어 관련 내용을 찾아보았고 여기에 정리하려 한다.

# 1. 문제상황 정의
