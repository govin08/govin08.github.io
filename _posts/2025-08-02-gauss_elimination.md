---
layout: single
title: "가우스 소거법"
categories: mathematics
tags: [linear algebra, matrix]
use_math: true
publish: false
author_profile: false
toc: true
---

2025년 전반기는 나름대로 유익하고 열심히 살았지만 경력상으로는 빈 구간이 되었다.
<!-- 이제 8월 1일부로 다시 경력을 채우게 될 것 같지만, 그 전에 -->
다시 경력을 채우기 직전, 통장 잔고가 줄어드는 것을 피하기 위해 수학을 가르치는 능력을 활용해보았다.
살면서 학원에서 일한 적은 이번이 처음이었는데 이 일은 매우 재미있었다.

2개월 반 동안 괜찮은 원장선생님 밑에서 일하고 나서 거의 다 끝날때쯤, 원장님께서 가우스 소거법에 대해 물어보셨다.
대학원 진학을 염두에 두고 있으셔서 선형대수를 배우시는 것 같았다.
마침 중학교 2학년 학생들에게 연립방정식을 가르쳐줬었으므로 그걸 상기하면서 가우스 소거법을 알려드리고 나니 블로그에도 관련 글을 좀 적어보면 어떨까 하는 생각이 들었다.

아주 길게 쓸 건 아니고 짧게만 적어보자.
블로그 글을 시작해놓고 제대로 끝내지 않은 글들이 한가득이라, 이번 글은 좀 마무리하도록 해보자.
하지만 별 이야기를 많이 쓰진 않을 것이다.

가우스 소거법은 결국 일차연립방정식을 푼다는 것이고, 그건 선형대수 관점에서 보면 affine space들의 교집합을 구해나간다는 뜻이며, 당연히 affine space의 basis를 구하는 것이 중요한 문제가 된다.
그리고 가우스 소거법을 하고 나면 이 모든 것이 아주 systematic하게 구해진다.

변수가 두 개이고 식이 두 개인 nontrivial한 연립방정식은 풀린다.
affine space들의 교집합은 one-point-set이 된다.
변수가 세 개이고 식이 세 개여도 마찬가지이다.
변수가 세 개이고 식이 두 개이면 affine space들의 교집합은 삼차원에서의 (일차원) 직선이 되고, 변수가 네 개이고 식이 두 개이면 affine space들의 교집합은 사차원에서의 이차원 평면이 된다.

# 1. 아핀공간(affine space)

가우스 소거법은 지극히 쉬운데 괜히 affine space라는 어려워보이는 용어를 도입하는 것이 적절할 지 모르겠다.
하지만 그래도 연립방정식의 상황과 가우스 소거법의 결과를 기하학적으로 묘사하는 데에는 아핀공간에 대한 이야기가 필요하지 않을까 한다.
아핀공간의 정의는 컨텍스트마다 그 형태가 다를 수 있겠지만 여기서 필요한 건 다음과 같이 간단하다.

벡터공간 $\mathbb R^n$ 위의 벡터 $v$와 부분공간(subspace) $W$에 대하여

$$v+W=\{a+w:w\in W\}$$

를 아핀공간이라고 한다.

예를 들어, 일차방정식 $ax+by+cz=d$이 나타내는 집합

$$A=\{(x,y,z):ax+by+cz=d\}$$

는 아핀공간이다.
왜냐하면 벡터 $n=(a,b,c)$에 대한 orthogonal complement

$$n^\perp=\{(x,y,z):ax+by+cz=0\}$$

는 $\mathbb R^3$의 부분공간인데 $v=(-\frac da,0,0)$로 두면 $A=v+n^\perp$이기 때문이다.

다시 말해, 이차원 평면 상의 직선은 아핀공간이고 삼차원 공간 상의 직선과 평면도 아핀공간이다.
한점집합(one-point-set)도 아핀공간이다.
또한, 일차연립방정식의 해를 구한다는 것은 아핀공간들의 교집합을 구한다는 것이다.
그런데 아핀 공간들의 교집합은 여전히 아핀공간이므로 ([증명생략](https://math.stackexchange.com/q/297201)) 연립방정식의 해는 항상 아핀공간을 이루게 된다.

# 2. 일차연립방정식

## 2.1 변수 2개, 식 2개

중학교 2학년 과정에서는 이원일차연립방정식을 배운다.
교과과정에는 가감법과 대입법이 소개되며 경우에 따라서는 등치법을 소개하기도 한다.
하지만 대입법이나 등치법으로 풀릴 수 있는 문제는 모두 가감법으로 풀릴 수 있고 따라서 가감법만 논해도 된다.
그리고 gauss elimination은 단순히 가감법을 해나가는 것과 다름없다.

연립방정식

$$
\begin{cases}
2x+3y&=5\\
5x+4y&=16
\end{cases}
$$

를 푸는 것은 쉽다.
위의 식을 (1), 아래 식을 (2)라고 하면 $x$를 소거하기 위해 $(1)\times 5 - (2)\times 2$를 하여 $7y=-7$, 즉 $y=-1$을 얻고 이를 (1) 또는 (2)에 대입해 $x=4$를 얻는다.
정확하게는

$$
\begin{cases}
2x+&&3y&=5\\
&&7y&=-7
\end{cases}
$$

의 과정을 거쳐

$$
\begin{cases}
x&&&=4\\
&&y&=-1
\end{cases}
$$

를 얻게 되는 것이다.

원래의 문제를 augmented matrix(확대행렬)로 표현하면

$$
\begin{bmatrix}
2&3&|&5\\
5&4&|&16
\end{bmatrix}
$$

이다.
가감법을 쓴다는 것은 augmented matrix의 row operation을 해나간다는 것이다.
row operation이란 row를 하나의 벡터로 보아 덧셈과 스칼라곱을 하는 것을 말한다.
위의 연립방정식 풀이는 두 번의 row operation을 거치는 것이라고 말할 수 있다.
즉

$$
\begin{bmatrix}
2&3&|&5\\
5&4&|&16
\end{bmatrix}
\quad\rightarrow\quad
\begin{bmatrix}
2&3&|&5\\
0&7&|&-7
\end{bmatrix}
\quad\rightarrow\quad
\begin{bmatrix}
1&0&|&5\\
0&1&|&-1
\end{bmatrix}
$$

의 과정을 거친 것이고, 좀 더 상세하게 쓰면 다음과 같다.

$$
\begin{align*}
\begin{bmatrix}
2&3&|&5\\
5&4&|&16
\end{bmatrix}
&\rightarrow
\begin{bmatrix}
2&3&|&5\\
0&7&|&-7
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow 5R_1-2R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
2&3&|&5\\
0&1&|&-1
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow \frac17R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
2&0&|&8 \\
0&1&|&-1
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow R_1-3R_2\\
\,
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1&0&|&4 \\
0&1&|&-1
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow\frac12R_1\\
\,
\end{matrix}
\end{align*}
$$

각 과정마다 오른쪽에는 row operation을 표현했다.
예를 들어, 맨 처음의 $R_2 \leftarrow 5R_1-2R_2$는 두번째 일차식을 $(1)\times 5 - (2)\times 2$로 대체한다는 뜻이다.

결국 augmented matrix의 왼쪽부분을 항등행렬로 만들면 연립방정식이 다 풀리는 것이고, 따라서 왼쪽 행렬을 항등행렬로 만드는 것이 목적이 되어야 한다.
이를 위해서는 첫번째 행의 첫번째 계수를 1로 만들어주는 것을 먼저 하는 것이 더 적절할 수 있다.
그런 관점에서, 위 과정과 equivalent하면서도 (그리고 일견 더 복잡해보이면서도) 더 바람직한 풀이는 다음과 같다.

$$
\begin{align*}
\begin{bmatrix}
2&3&|&5\\
5&4&|&16
\end{bmatrix}
&\rightarrow
\begin{bmatrix}
1&\frac32&|&\frac52\\
5&4&|&16
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow \frac12R_1\\
\,
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1&\frac32&|&\frac52\\
0&-\frac72&|&\frac72
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow -5R_1+R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1&\frac32&|&\frac52\\
0&1&|&-1
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow -\frac27R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1&0&|&4 \\
0&1&|&-1
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow R_1-\frac32R_2\\
\,
\end{matrix}
\end{align*}
$$

첫번째 행의 첫번째 계수를 1로 만들어주는 과정은 첫번째 과정($\rightarrow$)에 있었다.
$x$를 소거하는 두번째 과정을 거치고 나서는 두번째 행의 두번째 계수를 1로 만들어주는 과정이 있고 이게 세번째 과정이다.
마지막으로 $y$를 소거하는 네번째 과정을 거치면 연립방정식의 해가 구해진다.

위와 같은 과정이 가우스 소거법이다.
일반적으로 다음과 같은 과정을 거친다.
- (1) 첫번째 행의 첫번째 열 값을 1로 만든다.
- (2) 첫번째를 제외한 모든 행의 첫번째 열 값을 0으로 만든다.
- $n\gt1$인 $n$에 대하여 $n$번째 행의 $n-1$번째까지의 열 값이 모두 0이면 다음의 (3), (4)를 반복한다.
- (3) $n$번째 행의 $n$번째 계수를 1로 만든다.
- (4) $n$번째 행을 제외한 모든 행의 $n$번째 계수를 0으로 만든다.

이 과정을 거치면 왼쪽 행렬을 항등행렬로 바꾸는 목적을 달성할 수 있고, 따라서 연립방정식의 해를 구해낼 수 있다.

단, 문제가 '정상적'으로 풀리는 경우는 왼쪽행렬의 첫번째 행과 두번째 행이 일차독립인 경우, 즉 왼쪽행렬의 rank가 2인 경우이며, 다시말해 가감법을 했을 때 두 변수가 한꺼번에 소거되지 않는 경우이다.
두 변수가 한꺼번에 소거되는 경우 또한 중학교 2학년 교과과정에서 간접적으로 소개되는데

$$
\begin{cases}
ax+by&=c\\
a'x+b'y&=c'\\
\end{cases}
$$

에서, $\frac a{a'}\ne\frac b{b'}$이면 해가 한 쌍 존재하고, $\frac a{a'}=\frac b{b'}=\frac c{c'}$이면 해가 무수히 많으며 $\frac a{a'}=\frac b{b'}\ne\frac c{c'}$이면 해가 없다고 배우게 된다.
여기서 $\frac a{a'}=\frac b{b'}$인 조건은 정확히 왼쪽행렬의 판별식이 0인 것과 일치하는 것이다.
즉, $\frac a{a'}=\frac b{b'}$이면 왼쪽행렬은 비가역행렬이 되고 rank가 1이 되어 두 행이 일차독립이고, 간단히 말하면 윗행의 실수배가 아랫행과 일치하게 되어 두 변수가 한꺼번에 소거되는 경우에 해당하게 된다.

마지막으로, 가장 간단하면서도 nontrivial한 이 경우에 대해, 아까 장황하게 늘어놓은 유클리드공간이나 affine space, one-point-set 이야기를 할 필요가 있겠다.
이 문제는 변수가 두 개이므로 이차원 유클리드 공간 $\mathbb R^2$이 그 배경이 된다.
두 식 $2x+3y=5$, $5x+4y=16$은 평면 위의 두 직선을 의미하며 이 직선들 각각은 affine space이다.
두 affine space의 교집합은 one-point-set인 $\\{(4,-1)\\}$이며, 이것은 $\mathbb R^2$ 위의 한 점(두 직선의 교점)이 되는 것이다.

## 2.2 변수 3개, 식 3개

이와 같은 이야기는 삼원일차연립방정식에도 똑같이 적용된다.
연립방정식

<!-- 2, 3, -1 -->

$$
\begin{align*}
 x-2y+ z=&-5\\
2x+y +3z=&4\\
-x+3y-2z=&9\\
\end{align*}
$$

을 중고등학교 수준에서 푸는 방식은 다음과 같다.
먼저 $-2\times(1)+(2)$와 $(1)+(3)$으로부터 두번째 식과 세번째식에서 변수 $x$를 없앤다.

$$
\begin{align*}
 x-2y+ z=&-5\\
   5y+ z=&14\\
    y- z=&4\\
\end{align*}
$$

여기서 (2)+(3)을 하여 변수 $z$을 없애면 $6y=18$, $y=3$이 되고, 이것을 기존 식들에 계속 대입하면 $z=-1$, $x=2$을 얻을 수 있다.
하지만, 일관된 방식으로 문제를 풀어나가려면 $z$를 소거하기보다는 $y$를 소거하는 편이 좋을 것이다.
즉, (2)의 양변을 5로 나눈 다음, 그것을 (3)에서 빼주면

$$
\begin{align*}
 x-2y+ z=&-5\\
    y+\frac15z=&\frac{14}5\\
     -\frac65z=&\frac65\\
\end{align*}
$$

이 된다.
이와 같이 진행하여도 같은 해 $(x,y,z)=(2,3,-1)$를 얻을 수 있다.

가우스 소거법으로 이 문제를 풀면 다음과 같다.

$$
\begin{align*}
\begin{bmatrix}
 1&-2& 1&|&-5\\
 2& 1& 3&|&4\\
-1& 3&-2&|&9
\end{bmatrix}
&\rightarrow
\begin{bmatrix}
1&-2& 1&|&-5\\
0& 5& 1&|&14\\
0& 1&-1&|&4
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow -2R_1+R_2\\
R_3 \leftarrow R_1+R_3
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1&-2& 1&|&-5\\
0& 1& \frac15&|&\frac{14}5\\
0& 1&-1&|&4
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow \frac15R_2\\
\,
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1& 0& \frac75&|&\frac35\\
0& 1& \frac15&|&\frac{14}5\\
0& 0&-\frac65&|&\frac65
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow R_1+2R_2\\
\\
R_3 \leftarrow -R_2+R_3
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1& 0& \frac75&|&\frac35\\
0& 1& \frac15&|&\frac{14}5\\
0& 0& 1&|&-1
\end{bmatrix}
&&
\begin{matrix}
\\
\,\\
R_3 \leftarrow -\frac56R_3
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
1& 0& 0&|&2\\
0& 1& 0&|&3\\
0& 0& 1&|&-1
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow R_1-\frac75R_3\\
R_2 \leftarrow R_2-\frac15R_3\\
\,
\end{matrix}
\end{align*}
$$

따라서, 연립방정식의 해 $(x,y,z)=(2,3,-1)$가 구해졌다.

이 문제가 nontrivial한 이유는, 그러니까 해가 '정상적'으로 한 개가 나오는 까닭은 왼쪽의 행렬이 full rank 3을 가지기 때문이며, 다시 말해 세 개의 행들이 서로 일차독립이기 때문이다.
이 문제는 변수가 세 개 이므로 삼차원 유클리드 공간 $\mathbb R^3$이 그 배경이 되며, $x-2y+ z=-5$와 같은 세 일차식들은 공간 위의 세 평면이 되고 이것들 각각이 affine space이다.
세 affine space의 교집합은 이번에도 one-point-set인 $\\{(2,3,-1)\\}$로 주어졌다.

## 2.2 변수 3개, 식 2개

앞의 두 사례에서는 nontrivial한 사례를 봤다.
즉, 해가 하나인 연립방정식들만을 고려했다.
이번에는 trivial한 경우, 즉 해가 무한히 많은 경우를 다뤄보려 한다.
하지만 해가 무수히 많다고 하여, 그것들이 모두 같은 것은 결코 아니다.
해가 무수히 많은 경우도 그 경우마다 다 다른데 그 상황은 affine space와 그 basis로 가장 잘 묘사될 수 있을 것이다.

trivial한 사례는 변수의 개수와 식의 개수가 같은 앞의 두 사례와 같은 세팅에서 왼쪽행렬이 full rank를 가지지 않도록 하면 된다.
3 by 3 case에서 rank 2짜리 행렬로 연립방정식을 만들어도 좋은 예가 될 것이다.
하지만 여기서는 그냥 변수의 개수와 식의 개수가 다른, 변수의 개수가 식의 개수보다 많은 상황을 생각하려 한다.
이 상황은 3 by 3인 rank 2 행렬이 나오는 경우와 근본적으로 같을 것이다.

다음과 같은 연립방정식

<!-- typical point : (2,-1,1) -->

$$
\begin{align*}
x-y+2z&=5\\
2x+y-4z&=-1
\end{align*}
$$

은 변수가 세 개이고 식이 두 개이다.
그러면 연립방정식이 풀리지 않는다.
즉, $x$, $y$, $z$를 deterministic하게 확정하는 것이 불가능하다.
왜나하면, 삼차원 평면에서 두 평면이 만나는 도형은 일반적으로 직선(교선)이 될 것이기 때문이다.
하지만 지금까지 하던 대로 gauss elimination을 해보면 의미있는 결과를 얻을 수 있다.
즉, 직선(affine space)을 효율적으로 묘사할 수 있는 표현식(basis)을 찾을 수 있다.

가우스 소거법은 다음과 같이 진행된다.

$$
\begin{align*}
\begin{bmatrix}
 1&-1& 2&|&5\\
 2& 1&-4&|&-1
\end{bmatrix}
&\rightarrow
\begin{bmatrix}
 1&-1& 2&|&5\\
 0& 3&-8&|&-11
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow -2R_1+R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
 1&-1& 2&|&5\\
 0& 1&-\frac83&|&-\frac{11}3
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow \frac13R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
 1& 0&-\frac23&|&\frac43\\
 0& 1&-\frac83&|&-\frac{11}3
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow R_1+R_2\\
\,
\end{matrix}
\end{align*}
$$

그렇게 해서 얻어진 augmented matrix는, 바라던 대로 왼쪽행렬이 reduced row echelon form이 되었다.
이 augmented matrix를 연립방정식의 형태로 다시 써보면

$$
\begin{align*}
x&&&&-\frac23z&=\frac43\\
&&y&&-\frac83z&=-\frac{11}3
\end{align*}
$$

이 된다.
그러면 $x=\frac23z+\frac43$, $y=\frac83z-\frac{11}3$이고, 따라서 이 연립방정식을 만족시키는 점 $(x,y,z)$들의 집합 $A$은

$$
\begin{align*}
A
&=\left\{(x,y,z):x-y+2z=5,2x+y-4z=-1\right\}\\
&=\left\{(x,y,z):x=\frac23z+\frac43,y=\frac83z-\frac{11}3\right\}\\
&=\left\{\left(\frac23z+\frac43,\frac83z-\frac{11}3,z\right):z\in\mathbb R\right\}\\
&=\left\{z\left(\frac23,\frac83,1\right)+\left(\frac43,-\frac{11}3,0\right):z\in\mathbb R\right\}\\
&=\left(\frac43,-\frac{11}3,0\right) + \left\langle\left(\frac23,\frac83,1\right)\right\rangle\\
&=\left(\frac43,-\frac{11}3,0\right) + \left\langle\left(2,8,3\right)\right\rangle\\
\end{align*}
$$

이 된다.
이때 $\left\langle\left(\frac23,\frac83,1\right)\right\rangle\$는 벡터 $\left(\frac23,\frac83,1\right)$의 span이다.
즉 해당 벡터를 basis로 하는 일차원 부분공간을 뜻한다.
따라서 $A$는 일차원 affine space가 된다.
즉, 이것은 $\left(\frac43,-\frac{11}3,0\right)$을 지나고 벡터 $(2,8,3)$의 방향을 향하는 직선이다.

## 2.2 변수 4개, 식 2개

2.3의 사례도 재미있지만, affine space가 1차원이라는 점에 있어서는 너무 간단한 예이기도 하다.
affine space가 2차원이 되는 경우, 즉 대응되는 subspace의 basis vector가 두 개 이상인 경우를 살펴보는 것도 의미있을 것이다.
이번에도 full rank를 가지지 않는 적절한 linear system을 생각하기보다는, 변수의 개수 자체를 식의 개수보다 많게 두려고 한다.

고려하는 것은 다음의 연립방정식이다.

<!-- (1, 3, 4, -1) -->

$$
\begin{cases}
2x-y+2z+w   &=6\\
6x+2y-5z-4w &=-4
\end{cases}
$$

가우스 소거법을 적용시키면

$$
\begin{align*}
\begin{bmatrix}
 2&-1& 2& 1&|&6\\
 6& 2&-5&-4&|&-4
\end{bmatrix}
&\rightarrow
\begin{bmatrix}
 1&-\frac12& 1& \frac12&|&3\\
 6& 2&-5&-4&|&-4
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow \frac12R_1\\
\,
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
 1&-\frac12& 1& \frac12&|&3\\
 0& 5&-11&-7&|&-22
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow -6R_1+R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
 1&-\frac12& 1& \frac12&|&3\\
 0& 1&-\frac{11}5&-\frac75&|&-\frac{22}5
\end{bmatrix}
&&
\begin{matrix}
\\
R_2 \leftarrow \frac15R_2
\end{matrix}\\
&\rightarrow
\begin{bmatrix}
 1& 0&-\frac1{10}& \frac15&|&\frac45\\
 0& 1&-\frac{11}5&-\frac75&|&-\frac{22}5
\end{bmatrix}
&&
\begin{matrix}
R_1 \leftarrow R_1+\frac12R_2\\
\,
\end{matrix}
\end{align*}
$$

이다.
따라서

$$
\begin{align*}
x&&&&-\frac1{10}z+\frac15w&=\frac45\\
&&y&&-\frac{11}5z-\frac75w&=-\frac{22}3
\end{align*}
$$

이고 해집합 $A$는

$$
\begin{align*}
A
&=\left\{(x,y,z,w):2x-y+2z+w=6,6x+2y-5z-4w=-4\right\}\\
&=\left\{(x,y,z,w):x=\frac1{10}z-\frac15w+\frac45,y=\frac{11}5z+\frac75w-\frac{22}3\right\}\\
&=\left(\frac45,-\frac{22}3,0,0\right)
    +\left\langle\left(\frac1{10},\frac{11}5,1,0\right),
    \left(-\frac15,\frac75,0,1\right)\right\rangle\\
&=\left(\frac45,-\frac{22}3,0,0\right)
    +\left\langle\left(1,22,10,0\right),
    \left(-1,7,0,5\right)\right\rangle
\end{align*}
$$

이 된다.
따라서, $A$는 4차원 공간 상에서 $\left(\frac45,-\frac{22}3,0,0\right)$를 지나고 두 벡터 $\left(1,22,10,0\right)$, $\left(-1,7,0,5\right)$를 basis로 하는 평면이다.


# 3. code implementation