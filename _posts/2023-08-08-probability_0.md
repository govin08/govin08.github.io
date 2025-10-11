---
layout: single
title: "확률론 글쓰기"
categories: mathematics
tags: [statistics]
use_math: true
publish: true
author_profile: false
toc: false
---


올해 4월동안 [이상화 교수님의 『확률 및 통계』 강의에 대한 글](https://govin08.github.io/mathematics/kocw_stats/)을 열심히 적어보았습니다.
하지만, (1) 페이지가 불러와지는 데 몇 분씩 걸리고 (2) 내용상의 오류 혹은 오타가 있으며 (3) 전체적으로 구성이 매끄럽지 않다는 생각이 들었습니다.
글이 여러 개이면 정리나 식 등을 상호참조할 때 헷갈려서 모든 내용을 하나의 글에 적으려 했고 그러다보니 (1)의 문제가 발생했던 것인데, 이제 전체적인 윤곽을 한 번 잡았으니 여러 개의 글로 나누어서 다시 적어보려 합니다.

내용이나 구성 등은 전에 썼던 글을 기본적으로 따라가지만, 새로 적을 때에는 해당 강의를 염두에 두지 않고 적어나갈 생각입니다.
또, 예시를 먼저 들고 그 이유를 나중에 설명하는 귀납적인 방법보다는, 이론적인 사항들을 논리적으로 이야기하고 그 결과로서 예시를 나열하는 연역적인 방법을 사용할 것 같습니다.
글의 수준은 학부 1학년의 『확률과 통계』를 크게 벗어나지 않겠고 어차피 제 자신도 그 이상은 잘 모르지만, 확률공간을 설명할 때에는 measure 이야기 나올 것 같고, 컨볼루션을 말할 때에는 group 이야기가 나올 것 같으며, 피어슨 상관계수에 대해 쓸 때에는 Cauchy-schwarz inequality 이야기가 나올 것 같습니다.

생각해놓은 구성은 다음과 같습니다.
각각의 절들은 하나의 글을 구성할 것 같습니다.

1. 확률론 기초
<br>[1.1. 경우의 수와 이항정리](https://govin08.github.io/mathematics/probability_1_1/)
<br>[1.2. 확률의 정의](https://govin08.github.io/mathematics/probability_1_2/)
<br>[1.3. 조건부확률과 독립사건](https://govin08.github.io/mathematics/probability_1_3/)
<br>1.4. Sterling's formula

2. 확률변수와 확률분포
<br>2.1. 확률변수
<br>2.2. 이산확률분포
<br>2.3. 연속확률분포
<br>2.4. 평균과 분산
<br>2.5. 모멘트 생성 함수

3. 여러가지 분포
<br>3.1. 베르누이분포(이산)
<br>3.2. 기하분포(이산)
<br>3.3. 이항분포(이산)
<br>3.4. 푸아상분포(이산)
<br>3.5. 지수분포(연속)
<br>3.6. 균등분포(이산, 연속)
<br>3.7. 정규분포(연속)

4. 다변수 확률분포
<br>4.1. 이변수 확률분포
<br>4.2. 조건부 확률분포
<br>4.3. 조건부 평균
<br>4.4. 조건부 평균의 성질
<br>4.5. 공분산과 상관성
<br>4.6. 피어슨 상관계수
<br>4.7. 다변수 확률분포
<br>4.8. 확률과정
<br>4.9. 이변수 정규분포
<br>4.10. 다변수 정규분포

5. 확률변수의 변환
<br>5.1. 일변수 확률변수의 변환
<br>5.2. 연속확률변수의 합(컨볼루션)
<br>5.3. 지수분포와 컨볼루션 : 얼랑분포
<br>5.4. 기하분포와 컨볼루션 : 파스칼분포
<br>5.5. X+Y의 평균과 분산
<br>5.6. 이산확률변수의 합(컨볼루션)
<br>5.7. 이항분포, 푸아상 분포와 컨볼루션
<br>5.8. 다변수 확률변수의 변환

<div class="notice--info">
<b> 참고 </b> <br>
추후의 참조를 위해 적자면, "1. 확률론 기초", "2. 확률변수와 확률분포" 등은 <b>장</b>(章, chapter)이라고 부르고, "1.1. 경우의 수와 이항정리", "1.2. 확률공간" 등은 <b>절</b>(節, section)이라고 부를 것 같습니다.
각 절은 (1), (2), (3)등으로 구성될텐데 이것들은 <b>파트</b>라고 부를 것 같습니다.
</div>

이 글들을 쓰는 목적은 철저히 개인적인 목적에서입니다.
공부했던 것들을 대부분 머릿속에 정확하게 이해하고 있기는 하지만, 만족스러운 형태로 정리되어 있지 않습니다.
또한, 나중에 궁금한 게 생겨서 내용을 참고하려 한다고 할 때, 이전에 쓴 글은 (아무리 열심히 썼다고 하더라도) 크게 도움이 되지 않을 거라고도 생각됩니다.
최근에, 스페인 작가 "카밀로 호세 셀라"의 소설을 읽고 있는데, 매우 매우 잘읽힙니다.
노벨문학상을 받은 작가라 그런지 정말 잘 쓴 글인 것처럼 보이고 독자를 마구 빨려들게 합니다.
제가 쓰는 이 글들은 그정도의 글솜씨가 아닐 수 있을 것 같고, 이미 시중에 나온 많은 책들이 더 우수하리라는 것은 압니다.
하지만, 단지 개인적으로 공부를 하고 기록을 남기기 위해 이 글들을 적습니다.

다음은 이 글들과 직접적으로 관련이 있을만한 자료들의 목록입니다.

- [(강의정리) 확률 및 통계](https://govin08.github.io/mathematics/kocw_stats/)
- [statscraft의 wiki](https://github.com/hanishereandnow/Stats-Craft-LAB/wiki)

직접적으로 참고할만한 영문 교과서 목록입니다.
- [DeGroot, Probability and Statistics, 4ed](https://www.amazon.com/Probability-Statistics-4th-Morris-DeGroot/dp/0321500466)
- [The Book of Statistical Proofs](https://statproofbook.github.io/)
- [Introduction to Probability, Statistics and Random Process](https://www.probabilitycourse.com/)