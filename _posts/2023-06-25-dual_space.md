---
layout: single
title: "dual space"
categories: mathematics
tags: [linear algebra]
use_math: true
publish: false
author_profile: false
toc: true
---

문득 궁금해져서 linear functional과 dual space에 대해 정리해봤습니다.
그저께였던 금요일 저녁에 Oxford에서 제작한 것으로 보이는 [어떤 자료](https://people.maths.ox.ac.uk/flynn/genus2/alg0506/LALect03.pdf)를 따라가면서 그 증명을 채워넣었습니다.
어제 아침에 더 괜찮아보이는 notation으로 다시 적어보았고, 그리고 오늘 (새벽 4시) 일찍 일어난 김에 포스팅을 작성해봅니다.

dual space에 대해서는 당연히, 학부 2학년 때 배운 바가 있기는 하지만, 늘 그렇듯이 수업시간에 빠르게 지나간 정도였고, 깊이 고민해본 적은 없습니다.
해당 자료에 등장하는 annihilator라는 개념에 대해서는 대학원 대수 수업때 잠깐 접었던 적이 있습니다.
이 포스트의 마지막 결론이 될 것으로 보이는 $\text{dim}(V)=\text{dim}(U)+\text{dim}(U^0)$는, (Strang 책의) fundamental theorem of linear algebra 같은 걸 떠올리게 합니다.
혹은, $U<V$일 때, $\text{dim}(V)=\text{dim}(U)+\text{dim}(U^\perp)$가 성립한다는 식과도 유사해보입니다. (같은 것일지도 모르겠습니다.)

간단한 수학이고, 그 정도의 목적을 위해서는 영어가 훨씬 편할 것 같아서 영어로 작성했습니다.
간략히 적기 위해서 universal / existential quantifier ($\forall, \exists$)를 적극적으로 활용했습니다.

1에서는 관련된 기본 배경개념을 적어봤는데 vector space와 field, basis와 dimension 정도에 대해 간단히 적었습니다.
2에서는 linear functional과 dual space에 대해 적었습니다.
3에서는 linear functional의 해석학적인 예시에 대해 적어보았는데, 관련된 질문이 해소되지 않은 것이 있어서 stackexchange에 [질문](https://math.stackexchange.com/questions/4725056/what-is-the-dual-of-integration-over-the-unit-interval)도 올려보았습니다.

<style id="compiled-css" type="text/css">
ol.parenthesis {
    counter-reset: list-in-thm;
    margin-left: 0;
    padding-left: 0;
}
ol.parenthesis li {
    list-style: none;
    position: relative;
    padding-left: 0;
    margin-left: 2.2em;
}
ol.parenthesis li:before {
    content: "(" counter(list-in-thm) ")";
    counter-increment: list-in-thm;
    position: absolute;
    width: 3.0em;
    left: -3.5em;
    text-align: right;
}
</style> 


# 1. Preliminaries

## 1.1 Field

$(F,+,\times)$ is a field if the following 11 conditions hold ;

<ol class="parenthesis">
<li>$\forall a,b\in F$, $a+b\in F$</li>
<li>$\forall a,b,c\in F$, $(a+b)+c=a+(b+c)$</li>
<li>$\forall a,b\in F$, $a+b=b+a$</li>
<li>$\exists 0\in F$ s.t. $\forall a\in F$, $a+0=0+a=a$</li>
<li>$\forall a\in F$, $\exists -a\in F$ s.t. $a+(-a)=(-a)+a=0$</li>
<li>$\forall a,b\in F$, $ab\in F$</li>
<li>$\forall a,b,c\in F$, $(ab)c=a(bc)$</li>
<li>$\forall a,b\in F$, $ab=ba$</li>
<li>$\exists 1\in F$ s.t. $\forall a\in F$, $a\times1=1\times a=a$</li>
<li>$\forall a(\ne0)\in F$, $\exists a^{-1}\in F$ s.t. $aa^{-1}=a^{-1}a=1$</li>
<li>$\forall a,b,c\in F$, $(a+b)c=ac+bc$</li>
</ol>

That is, $F$ is an abelian group under the addition(1-5), $F\setminus\\{0\\}$ is an abelian group under the multiplication(6-10) and the distribution law holds(11).
An element $a$ of a field $F$ is called a *scalar*.

## 1.2 Vector space

$(V,+,\cdot)$ is a vector space if the following 10 conditions hold;

<ol class="parenthesis">
<li>$\forall v,w\in V$, $v+w\in V$</li>
<li>$\forall u,v,w\in V$, $(u+v)+w=u+(v+w)$</li>
<li>$\forall v,w\in V$, $v+w=w+v$</li>
<li>$\exists 0\in V$ s.t. $\forall v\in V$, $v+0=0+v=v$</li>
<li>$\forall v\in V$, $\exists -v\in V$ s.t. $v+(-v)=(-v)+v=0$</li>
<li>$\forall a\in F$, $\forall v\in V$, $av\in V$</li>
<li>$\forall a,b\in F$, $\forall v\in V$, $(a+b)v=av+bv$</li>
<li>$\forall a\in F$, $\forall v,w\in V$, $a(v+w)=av+aw$</li>
<li>$\forall a,b\in F$, $\forall v\in V$, $a(bv)=(ab)v$</li>
<li>$\forall v\in V$, $1v=v$</li>
</ol>

That is, $V$ is an abelian group under the addition(1-5) which satisfies several conditions(6-10) with respect to the scalar multiplication.
An element $v$ of a vector space $V$ is called a *vector*.

**1.2.1**

If $F$ is a field, then $F$ itself is a vector space over $F$.

**1.2.2**

If $F$ is a field, then the set

$$F^n=\left\{\begin{bmatrix}a_1&\cdots&a_n\end{bmatrix}^T\mid a_1,\cdots,a_n\in F\right\}$$

of all column vectors of $F$ is a vector space.

## 1.3 Basis and dimension

Suppose that $V$ is a vector space over a field $F$.
The set $\\{v_1,\cdots,v_n\\}$ of vectors is called the basis for $V$ if $\\{v_1,\cdots,v_n\\}$ is linearly independent and $\\{v_1,\cdots,v_n\\}$ spans $V$.
By the linear independence of $\\{v_1,\cdots,v_n\\}$, we mean

$$a_1v_1+\cdots+a_nv_n\quad\Longrightarrow\quad a_1=\cdots=a_n=0,$$

where $a_i\in F$ for all $i$.
We say $\\{v_1,\cdots,v_n\\}$ spans $V$ or

$$V\subset\text{span}(\{v_1,\cdots,v_n\}),$$

if every vector $v\in V$ can be expressed as a linear combination

$$v=a_1v_1+\cdots+a_nv_n.$$

of vectors $v_i$'s.
The basis $\\{v_1,\cdots,v_n\\}$ is often denoted by $\\{v_i\\}_{i=1}^n$ for brevity.

The cardinality of a basis for a vector space is unique and is called the dimension of $V$ ; 

$$\text{dim}(V)=n.$$

## 1.4 Subspace

If $U$ is a subset of a vector space $V$ over a field $F$ and $U$ is itself a vector space, $U$ is called a subspace of $V$.
We write

$$U<V.$$

A necessary and sufficient condition that a subset $U$ of $V$ is a subspace of $V$ is that $U$ is nonempty and closed under the two operations.
That is, if $U\subset V$, then $U\lt V$ if and only if

<ol class="parenthesis">
<li> $U\ne\varnothing$ </li>
<li> $\forall u,v\in U, u+v\in U$ </li>
<li> $\forall a\in F,\forall u\in U, au\in U$. </li>
</ol>

# 2. Dual space

Let $V$ be a vector space over a field $F$.

## 2.1 Linear functional

A linear functional (linear form) is a linear function of $V$ into $F$.
That is, $f:V\to F$ is called a linear functional if 
<ol class="parenthesis">
<li> $\forall v,w\in V, f(v+w)=f(v)+f(w)$ </li>
<li> $\forall a\in F,\forall v\in V, f(av)=af(v)$. </li>
</ol>

**2.1.1**

Let $y=\begin{bmatrix}y_1&\cdots&y_n\end{bmatrix}$ be a row matrix where $y_i\in F$ for all $i\in\\{1,\cdots,n\\}$.
The left multiplication $f_y:F^n\to F$ by $y$ which maps each column vector

$$v=\begin{bmatrix}v_1\\\vdots\\v_n\end{bmatrix}$$

in $F^n$ to a scalar 

$$yv=\begin{bmatrix}y_1&\cdots&y_n\end{bmatrix}\begin{bmatrix}v_1\\\vdots\\v_n\end{bmatrix}=\sum_{i=1}^ny_iv_i$$

is a linear functional.
(i.e. $f_y(v)=yv$)

**2.1.2**

The set of all $n\times n$ matrices, denoted by $F^{n\times n}$ is a vectors space.
For a fixed column vector $v\in F^n$, the map $A\mapsto v^TAv$, which maps a $n\times n$ matrix $A$ into the quadratic form $v^TAv$ is a linear functional.

The left multiplication by $y$ (2.1.1) and the quadratic form (2.1.2) for each matrix $A$ are indeed linear functional since matrix operation is linear.
But quadratic form *for each vector* $v\mapsto v^TAv$ is not a linear functional.

## 2.2 Dual space

For a vector space $V$ over a field $F$, define $V^\ast$ be the collection of all linear functionals of $V$ into $F$.
It is called the **dual space** of $V$.
We can impose algebraic structure $(V^\ast,+,\cdot)$ in such a canonical way that

<ol class="parenthesis">
<li> for all $f,g\in V^\ast$, define $f+g$ as follows : $\forall v\in V, (f+g)(v)=f(v)+g(v)$ </li>
<li> for all $a\in F$, and $f\in V^\ast$, define $af$ as follows : $\forall v\in V, (af)(v)=af(v)$ </li>
</ol>

**2.2.1 $V^\ast$ is a vector space.**

To prove that $V^\ast$ is a vector space over $F$, it is enough to check the ten conditions illustrated in **1.2** ;

<ol class="parenthesis">
<li>
    $\forall f,g\in V^\ast$, $f+g\in V^\ast$<br>
    Let $f,g\in V^\ast$.
    For $v,w\in V$, 
    \begin{align*}
    (f+g)(v+w)
    &=f(v+w)+g(v+w)\\
    &=\left(f(v)+f(w)\right)+\left(g(v)+g(w)\right)\\
    &=\left(f(v)+g(v)\right)+\left(f(w)+g(w)\right)\\
    &=(f+g)(v)+(f+g)(w).
    \end{align*}
    And for all $a\in F$ and $v\in V$,
    \begin{align*}
    (f+g)(av)
    &=f(av)+g(av)\\
    &=af(v)+ag(v)\\
    &=a\left(f(v)+g(v)\right)\\
    &=a(f+g)(v).
    \end{align*}
    Thus, $f+g$ is a linear functional and $f+g\in V^\ast$.
</li>

<li>
    $\forall f,g,h\in V^\ast$, $(f+g)+h=f+(g+h)$<br>
    Let $f,g,h\in V^\ast$.
    For each $v\in V$,
    \begin{align*}
    \left((f+g)+h\right)(v)
    &=(f+g)(v)+h(v)\\
    &=\left(f(v)+g(v)\right)+h(v)\\
    &=f(v)+\left(g(v)+h(v)\right)\\
    &=f(v)+(g+h)(v)\\
    &=\left(f+(g+h)\right)(v).
    \end{align*}
    Thus, $(f+g)+h=f+(g+h)$.
</li>
<li>
    $\forall f,g\in V^\ast$, $f+g=g+f$<br>
    Let $f,g\in V^\ast$.
    For each $v\in V$,
    \begin{align*}
    \left(f+g\right)(v)
    &=f(v)+g(v)\\
    &=g(v)+f(v)\\
    &=\left(g+f\right)(v).
    \end{align*}
    Thus, $f+g=g+f$.
</li>
<li>
    $\exists 0\in V^\ast$ s.t. $\forall f\in V^\ast$, $f+0=0+f=f$<br>
    Let $0:V\to F$ be defined by $0(v)=0$ for all $v\in V$.
    Then $0\in V^\ast$ since $0(v+w)=0=0+0=0(v)+0(w)$ and $0(av)=0=a\cdot0=a0(v)$ for all $v,w\in V$ and for all $a\in F$.
    Now, fix $f\in V^\ast$.
    Then,
    \begin{align*}
    (f+0)(v)
    &=f(v)+0(v)\\
    &=f(v)+0\\
    &=f(v)
    \end{align*}
    for each $v\in V$.
    Thus, $f+0=f$.
    Similarly, $0+f=f$.
    </li>
<li>
    $\forall f\in V^\ast$, $\exists -f\in V^\ast$ s.t. $f+(-f)=(-f)+f=0$<br>
    For each $f\in V^\ast$, define $-f:V\to F$ by $(-f)(v)=-f(v)$ for each $v\in V$.
    For every $v\in V$,
    \begin{align*}
    (f+(-f))(v)
    &=f(v)+(-f)(v)\\
    &=f(v)-f(v)\\
    &=0\\
    &=0(v).
    \end{align*}
    Thus, $f+(-f)=0$.
    Similarly, $(-f)+f=0$.
    </li>
<li>
    $\forall a\in F$, $\forall f\in V^\ast$, $af\in V^\ast$ <br>
    Let $a\in F$ and $f\in V^\ast$.
    For $v,w\in V$,
    \begin{align*}
    (af)(v+w)
    &=af(v+w)\\
    &=a\left(f(v)+f(w)\right)\\
    &=af(v)+af(w)\\
    &=(af)(v)+(af)(w).
    \end{align*}
    And for all $b\in F$ and $v\in V$,
    \begin{align*}
    (af)(bv)
    &=af(bv)\\
    &=a(bf(v))\\
    &=abf(v)\\
    &=b(af(v))\\
    &=b(af)(v).
    \end{align*}
    Thus, $af$ is a linear functional and $af\in V^\ast$.
    </li>
<li>
    $\forall a,b\in F$, $\forall f\in V^\ast$, $(a+b)f=af+bf$<br>
    Let $a,b\in F$ and $f\in V^\ast$.
    For each $v\in V$,
    \begin{align*}
    \left((a+b)f\right)(v)
    &=(a+b)f(v)\\
    &=af(v)+bf(v)\\
    &=(af+bf)(v)\\
    \end{align*}
    Thus, $(a+b)f=af+bf$.
    </li>
<li>
    $\forall a\in F$, $\forall f,g\in V^\ast$, $a(f+g)=af+ag$ <br>
    Let $a\in F$ and $f,g\in V^\ast$.
    For each $v\in V$,
    \begin{align*}
    \left(a(f+g)\right)(v)
    &=a(f+g)(v)\\
    &=a\left(f(v)+g(v)\right)\\
    &=af(v)+ag(v)\\
    &=(af+ag)(v)
    \end{align*}
    Thus, $a(f+g)=af+ag$.
    </li>
<li>
    $\forall a,b\in F$, $\forall f\in V^\ast$, $a(bf)=(ab)f$ <br>
    Let $a,b\in F$ and let $f\in V^\ast$.
    For each $v\in V$,
    \begin{align*}
    \left(a(bf)\right)(v)
    &=a(bf)(v)\\
    &=a\left(bf(v)\right)\\
    &=abf(v)\\
    &=\left((ab)f\right)(v)
    \end{align*}
    Thus, $a(bf)=(ab)f$.
    </li>
<li>
    $\forall f\in V^\ast$, $1f=f$ <br>
    Let $f\in V^\ast$.
    For each $v\in V$, $(1f)(v)=1\cdot f(v)=f(v)$.
    Thus, $1\cdot f=f$.
    </li>
</ol>

Indeed, $V^\ast$ satisfies all the ten conditions.
Therefore, $V^\ast$ is a vector space over $F$.

## 2.3 Dual basis

Let $\\{v_i\\}_{i=1}^n$ be a basis for $V$.
For each $i\in\\{1,\cdots,n\\}$, let $f_i:V\to F$ be defined by

$$f_i\left(\sum_{j=1}^na_jv_j\right)=a_i.$$

It is equivalent to saying that $f_i$'s are linear functionals such that $f_i(v_j)=\delta_{ij}$ for all $j\in\\{1,\cdots,n\\}$, where $\delta_{ij}$ is the Kronecker delta defined by

$$
\delta_{ij}=
\begin{cases}
1&(i=j)\\
0&(i\ne j).
\end{cases}
$$

**2.3.1 $f_i\in V^\ast$**

So defined $f_i$ is a linear functional.
To this aim, we prove that $f_i(v+w)=f_i(v)+f_i(w)$ and $f_i(av)=af_i(v)$ for all $v,w\in V$ and for all $a\in F$.
The proofs are straightforward.
Denote

$$
v=\sum_{j=1}^na_jv_j,\quad w=\sum_{j=1}^nb_jv_j.
$$

Then,

$$
\begin{align*}
f_i(v+w)
&=f_i\left(\sum_{j=1}^na_jv_j+\sum_{j=1}^nb_jv_j\right)\\
&=f_i\left(\sum_{j=1}^n(a_j+b_j)v_j\right)\\
&=a_i+b_i\\
&=f_i(v)+f_i(w)\\
f_i(av)
&=f_i\left(a\sum_{j=1}^na_jv_j\right)\\
&=f_i\left(\sum_{j=1}^naa_jv_j\right)\\
&=aa_i\\
&=af_i(v)
\end{align*}
$$

Therefore, $f_i\in V^\ast$.

**2.3.2 $\\{f_i\\}_{i=1}^n$ is a basis for $V^\ast$**

It is enough to prove the linear independence and spanning of $\\{f_i\\}_{i=1}^n$.

To prove the linear independence of $\\{f_i\\}_{i=1}^n$, set

$$\sum_{i=1}^na_if_i=0.$$

For each $j\in\\{1,\cdots,n\\}$,

$$
\begin{align*}
0
&=0(v_j)\\
&=\left(\sum_{i=1}^na_if_i\right)(v_j)\\
&=\sum_{i=1}^na_if_i(v_j)\\
&=\sum_{i=1}^na_i\delta_{ij}\\
&=a_j
\end{align*}
$$

Thus, $a_1=\cdots=a_n=0$ and $V^\ast$ is linearly independent.

To prove $\text{span}\left(\\{f_i\\}_{i=1}^n\right)=V^\ast$, pick $f\in V^\ast$.
That is, $f:V\to F$ is a linear functional.

Since

$$
\begin{align*}
\left(\sum_{i=1}^nf(v_i)f_i\right)(v_j)
&=\sum_{i=1}^nf(v_i)f_i(v_j)\\
&=\sum_{i=1}^nf(v_i)\delta_{ij}\\
&=f(v_j)
\end{align*}
$$

for each $j\in\\{1,\cdots,n\\}$,

$$\left(\sum_{i=1}^nf(v_i)f_i\right)(v)=f(v)$$

for all $v\in V$.
Thus, $\sum_{i=1}^nf(v_i)f_i=f$ and $f$ is now expressed as a linear combination of $f_i$'s.
Therefore, $\\{f_i\\}_{i=1}^n$ spans $V^\ast$ and is a basis for $V^\ast$.

So defined $\\{f_i\\}_{i=1}^n$ is called the **dual basis** of the original basis.
 <!-- of $\\{v_i\\}_{i=1}^n$. -->

**2.3.3 Two spaces have the same dimension**
<!-- $\text{dim}(V)=\text{dim}(V^\ast)$ -->

<!-- Since both of the bases $\\{v_i\\}_{i=1}^n$ and $\\{f_i\\}_{i=1}^n$, one for $V$ and the other for $V^\ast$ have the same cardinality, $V$ and $V^\ast$ are vector spaces of same dimension. -->

Since both of the bases have the same cardinality, $V$ and $V^\ast$ are vector spaces of the same dimension.

**2.3.4 Two spaces have the same cardinality**

Moreover, $V$ is equivalent to $V^\ast$ in the sense of cardinality, or $V\sim V^\ast$.
That is, the map $\mathcal D:V\to V^\ast$ defined by

$$\mathcal D\left(\sum_{j=1}^na_jv_j\right)=\sum_{j=1}^na_jf_j.$$

is a one-to-one correspondence.
To prove the injectivity of $\mathcal D$, let

$$\mathcal D\left(\sum_{j=1}^na_jv_j\right)=\mathcal D\left(\sum_{j=1}^nb_jv_j\right)$$

so that

$$\sum_{j=1}^na_jf_j=\sum_{j=1}^nb_jf_j$$

Then, $a_j=b_j$ for all $j\in\\{1,\cdots,n\\}$.
Thus,

$$\sum_{j=1}^na_jv_j=\sum_{j=1}^nb_jv_j$$

and $\mathcal D$ is injective.
Surjectivity follows immediately from the spanning property.
 <!-- of $\\{f_i\\}_{i=1}^n$. -->
Let $f\in V^\ast$.
Since $\\{f_i\\}_{i=1}^n$ spans $V^\ast$, $f$ can be expressed as

$$f=\sum_{i=1}^na_if_i.$$

It follows that

$$\mathcal D\left(\sum_{i=1}^na_iv_i\right)=\sum_{i=1}^na_if_i=f$$

and $\mathcal D$ is surjective.

**2.3.5 $\mathcal D$ is linear**

The conversion map $\mathcal D$ of $V$ onto $V^\ast$ is a linear transformation ;

$$
\begin{align*}
\mathcal D\left(\sum_{i=1}^na_iv_i+\sum_{i=1}^nb_iv_i\right)
&=\mathcal D\left(\sum_{i=1}^n(a_i+b_i)v_i\right)\\
&=\sum_{i=1}^n(a_i+b_i)f_i\\
&=\sum_{i=1}^na_if_i+\sum_{i=1}^nb_if_i\\
&=\mathcal D\left(\sum_{i=1}^na_iv_i\right)+\mathcal D\left(\sum_{i=1}^nb_iv_i\right)\\
\mathcal D\left(a\sum_{i=1}^na_iv_i\right)
&=\mathcal D\left(\sum_{i=1}^naa_iv_i\right)\\
&=\sum_{i=1}^naa_if_i\\
&=a\sum_{i=1}^na_if_i\\
&=a\mathcal D\left(\sum_{i=1}^na_iv_i\right)
\end{align*}
$$

Combining 2.3.4 and 2.3.5, $V$ is isomorphic to $V^\ast$.


## 2.4 Annihilator

For $U\lt V$, let

$$U^0=\{f\in V^\ast\mid f(u)=0\text{ for all } u\in U\}.$$

$U^0$ is called the **annihilator** of $U$ in $V$.
Elements $f$ of $U^0$ literally annihilate vectors of $U$.

**2.4.1 $U^0\lt V^\ast$**

The annihilator $U^0$ is a subspace of $V^\ast$.
To this aim we prove the three conditions illustrated in 1.4.

(1) Since $0\in U^0$, $U^0$ is not empty.
(2) Pick $f,g\in U^0$.
Then $(f+g)(u)=f(u)+g(u)=0+0=0$ for all $u\in U$.
Thus, $f+g\in U^0$.
(3) Pick $a\in F$ and $f\in U^0$.
Then, $(af)(u)=af(u)=a\cdot0=0$ for all $u\in U$.
Thus, $af\in U^0$.

Therefore, $U^0$ is a subspace of $V^\ast$ as expected.

Now we prove the following main result,

<div class="notice--success">
$$\text{dim}(V)=\text{dim}(U)+\text{dim}(U^0).$$
</div>

which takes several steps as follows.

**2.4.2 Two types of basis vectors**

Suppose that $U\ne\\{0\\}$ and $U\ne V$.
Let $B_0=\\{v_1,\cdots,v_k\\}$ be a basis for $U$ and $F_0=\\{f_1,\cdots,f_k\\}$ be the corresponding dual basis where $0\lt k\lt n$.
We can extend the basis $B_0$ for $U$ to a basis $B=\\{v_1,\cdots,v_k,v_{k+1},\cdots,v_n\\}$ for $V$.
Let $B^0=\\{v_{k+1},\cdots,v_n\\}$ and let $F^0=\\{f_{k+1},\cdots,f_n\\}$ be the set of the coresponding linear functionals.

RECAP : 

$$
\begin{align*}
B_0&=\{v_1,\cdots,v_k\}&&:\text{basis for $U$}\\
B^0&=\{v_{k+1},\cdots,v_n\}\\
F_0&=\{f_1,\cdots,f_k\}\\
F^0&=\{f_{k+1},\cdots,f_n\}\\
\end{align*}
$$

**2.4.3 $f_{k+1},\cdots,f_n\in U^0$.**

Fix $i\in\\{k+1,\cdots,n\\}$.
Here, we prove $f_i\in U^0$.
To this aim, let $u\in U$ so that

$$u=\sum_{j=1}^ka_jv_j$$

for some $a_1$, $\cdots$, $a_k$.
Then

$$
\begin{align*}
f_i(u)
&=f_i\left(\sum_{j=1}^ka_jv_j\right)\\
&=\sum_{j=1}^ka_jf_i(v_j)\\
&=\sum_{j=1}^ka_j\delta_{ij}\\
&=0
\end{align*}
$$

Therefore, $f_i\in U^0$, as desired.

**2.4.4 $F^0$ is a basis for $U^0$**

That $F^0=\\{f_{k+1},\cdots,f_n\\}$ is linearly independent is trivial.
To prove $\text{span}(F^0)=U^0$, let $f\in U^0$ with

$$f=\sum_{i=1}^na_if_i$$

For every $j\in\\{1,\cdots,k\\}$,

$$0=f(v_j)=\sum_{i=1}^na_if_i(v_j)=a_j.$$

Thus,

$$f=\sum_{i=k+1}^na_if_i$$

and $F^0=\\{f_{k+1},\cdots,f_n\\}$ spans $U^0$.

RECAP : 

$$
\begin{align*}
B_0&=\{v_1,\cdots,v_k\}&&:\text{basis for $U$}\\
B^0&=\{v_{k+1},\cdots,v_n\}\\
F_0&=\{f_1,\cdots,f_k\}\\
F^0&=\{f_{k+1},\cdots,f_n\}&&:\text{basis for $U^0$}\\
\end{align*}
$$

**2.4.5 $\text{dim}(V)=\text{dim}(U)+\text{dim}(U^0)$**

If $U\ne\\{0\\}$ and $U\ne V$ as supposed at the beginning of 2.4.2, then

$$\text{dim}(U)+\text{dim}(U^0)=k+(n-k)=n=\text{dim}(V).$$

If $U=\\{0\\}$, then $U^0=V^\ast$ and 

$$\text{dim}(U)+\text{dim}(U^0)=0+n=n=\text{dim}(V).$$

If $U=V$, then $U^0=\\{0\\}$ and 

$$\text{dim}(U)+\text{dim}(U^0)=n+0=n=\text{dim}(V).$$

This proves the statement in 2.4.1 in general.
$\square$

# 3. Examples of linear functionals and duals of them

## 3.1 Integration

Consider a function space $C^0[0,1]$ of all continuous functions on the unit closed interval $[0,1]$.
Let $\Phi:C^0[0,1]\to\mathbb R$ be defined by

$$\Phi(f)=\int_0^1f(x)\,dx$$

Then $\Phi$ is a linear functional since

$$
\begin{align*}
\Phi(f+g)
&=\int_0^1(f+g)(x)\,dx\\
&=\int_0^1\left(f(x)+g(x)\right)\,dx\\
&=\int_0^1f(x)\,dx+\int_0^1g(x)\,dx\\
&=\Phi(f)+\Phi(g)\\
\Phi(af)
&=\int_0^1(af)(x)\,dx\\
&=\int_0^1af(x)\,dx\\
&=a\int_0^1f(x)\,dx\\
&=a\Phi(f).
\end{align*}
$$

## 3.2 Differentiation

Let $C^1(0,1)$ be the function space of all differentiable functions on the unit open interval $(0,1)$.
Let $\Psi:C^1(0,1)\to\mathbb R$ be defined by

$$\Psi(f)=f'\left(\frac12\right)$$

Then $\Psi$ is a linear functional since

$$
\begin{align*}
\Psi(f+g)
&=(f+g)'\left(\frac12\right)\\
&=f'\left(\frac12\right)+g'\left(\frac12\right)\\
&=\Psi(f)+\Psi(g)\\
\Psi(af)
&=(af)'\left(\frac12\right)\\
&=af'\left(\frac12\right)\\
&=a\Psi(f).
\end{align*}
$$

## 3.3 Question

What is the *dual* of $\Phi$?
What is the *dual* of $\Psi$?

잘 모르겠어서 [이 곳](https://math.stackexchange.com/questions/4725056/what-is-the-dual-of-integration-over-the-unit-interval)
에 질문을 올려보았다.

# 참고한 자료들
1. [Linear Algebra 3 : Dual space](https://people.maths.ox.ac.uk/flynn/genus2/alg0506/LALect03.pdf)
