---
layout: post
title: CRF(Contional Random Field)
date: 2017-11-30 17:00:00
categories: study
tags: CRF
---

## CRF 란?
저스틴 비버의 하루 일상을 순서대로 찍은 사진들이 있다고 상상해보자.  
우리는 각각의 사진에 한 단어로 설명(라벨)을 달고자 한다. (예> 식사 사진, 수면 사진, 운전 중 등등)  
어떻게 하면 될까?

한가지 방법은 사진들이 가지는 순서에 대한 정보는 무시하고 각 사진 자체만으로 분류를 해보는 것이 될 수 있겠다.
예를 들어 라벨이 달린 한 달 치의 사진이 주어졌다고 하면, 아침 6시 쯤에 찍힌 어두운 사진들은 '수면' 과 연관이 있는 사진들이고,
밝은 색깔이 많이 포함된 사진은 '춤' 에 대한 사진, 자동차가 나온 사진들은 '운전' 사진이라는 등의 사실을 배울 수 있을 것이다.

하지만 순서에 대한 관점을 무시하면 많은 양의 정보를 잃게 된다. 예를 들어 입만 크게 나온 사진이 있다고 하자. 그건 노래하는 사진일까 밥 먹는 사진일까?
만약 이전의 사진이 저스틴 비버가 밥을 먹거나 요리하는 사진이었다면, 그 입 사진은 '먹는' 행위에 관한 사진일 가능성이 더 높을 것이다.
하지만 이전 사진이 저스틴 비버가 노래나 춤 추는 사진이었다면, 입 사진도 마찬가지로 '노래하는' 사진일 가능성이 높다.

### Part-of-Speech Tagging
part-of-speech tagging 를 예로 좀 더 자세히 살펴보도록 하자.  
POS tagging 의 목적은 한 문장의 어휘들의 품사를 달아주는 것이다. (ex> 명사, 동사, 형용사, 명사)  
예를 들어 'Bob drank coffee at Starbucks.' 라는 문장은 'Bob(명사) drank(동사) coffee(명사) at(전치사) Starbucks(명사)' 가 될 것이다.
그럼 이제 CRF 를 이용해서 **문장에 대한** POS tagging 을 해보자. 먼저 다른 classifier 들과 마찬가지로 feature function $$f_i$$ 들을 정의해야 한다.

### Feature functions in a CRF
CRF 에서 각각의 feature function 은 아래와 같은 입력들을 받는 함수이다:
+ s : 문장
+ i : 단어의 문장 내 인덱스
+ $$l_i$$ : 현재 단어의 라벨
+ $$l_{i-1}$$ : 이전 단어의 라벨

그리고 출력 값은 실수 값이 된다.(하지만 실제로는 0 또는 1 인 경우가 많다.)

*참고 : 본 포스팅에서는 문장 전체에 대한 라벨이 아니라 현재 단어와 이전 단어의 라벨만 사용하도록 제한하고 있다.
이것은 **linear-chain CRF** 의 일종이라고 할 수 있다. 설명의 편의를 위해 범용적인 CRF 에 대한 내용은 생략하고자 한다.*

### Feature 를 Probabilties 로.
이제 각각의 feature function $$f_j$$ 에 weight $$\lambda_j$$ 를 할당해 보자.(데이터로부터 이 weight 들을 어떻게 학습하는 지는 아래에 설명하겠다.)  
문장 s 가 주어졌을 때, 문장 내 모든 단어에 대한 weighted feature 값을 더하여 s 에 라벨 l 이 부여될 수 있는지 점수를 매길 수 있다.

$$score(l|s) = \sum_{j = 1}^m \sum_{i = 1}^n \lambda_j f_j(s, i, l_i, l_{i-1})$$

(첫 번째 시그마는 각각의 feature function $$f$$ 에 대한 것이고, 안쪽 시그마는 $$i$$ 위치의 단어 각각에 대한 연산이다.)

그 다음 지수 연산(exponentiating)과 정규화(normalizing)를 통해 이 점수를 확률
$$p(l | s)$$ 로 환산할 수 있다.

$$
p(l | s) = \frac{exp[score(l|s)]}{\sum_{l’} exp[score(l’|s)]} = \frac{exp[\sum_{j = 1}^m \sum_{i = 1}^n \lambda_j f_j(s, i, l_i, l_{i-1})]}{\sum_{l’} exp[\sum_{j = 1}^m \sum_{i = 1}^n \lambda_j f_j(s, i, l’_i, l’_{i-1})]}
$$

### Feature Function 예시
그럼 이 feature function 들은 어떻게 생겼을까? POS tagging 의 feature 들은 다음과 같을 수 있다:  
+ $$
f_1(s, i, l_i, l_{i-1}) =
\begin{cases}
 1 & \mbox{if }l_i = \text{부사(ADVERB)} \mbox{ and the ith word ends in “-ly”} \\
 0 & \mbox{otherwise.}
 \end{cases}
$$
  + 이 feature 에 대한 weight $$\lambda_1$$ 값이 양수이고 큰 경우, 이 feature 는 -ly 로 끝나는 단어를 '부사' 로 라벨링할 거라는 의미가 될 것이다.
+ $$
f_2(s, i, l_i, l_{i-1}) =
\begin{cases}
1 & \mbox{if } i = 1, l_i = 동사(VERB), \mbox{ and the sentence ends in a question mark} \\
0 & \mbox{otherwise.}
\end{cases}
$$
  + 마찬가지로 feature 에 대한 weight $$\lambda_2$$ 값이 양수이고 큰 경우, ? 로 끝나는 문장의 첫 단어를 '동사' 로 라벨링하고자 할 것이다.
+ $$
f_3(s, i, l_i, l_{i-1}) =
\begin{cases}
1 & \mbox{if }l_{i-1} = \mbox{형용사(ADJECTIVE)} \mbox{, and }l_i = \mbox{명사(NOUN)} \\
0 & \mbox{otherwise.}
\end{cases}
$$
  + weight 가 양수 인 경우, 이 feature 는 형용사 뒤에는 명사가 온다는 것을 의미한다.
+ $$
f_4(s, i, l_i, l_{i-1}) = 1 \mbox{ if }l_{i-1} =\mbox{ 전치사(PREPOSITION) and }l_i = \mbox{전치사(PREPOSITION).}
$$
  + 이 함수에 대한 음수의 weight $$\lambda_4$$ 는 전치사 뒤에는 전치시가 오면 안되다는 것을 의미한다. 따라서 그런 라벨링이 발생하지 않도록 할 수 있을 것이다.

이게 끝이다!  
정리해보면 CRF 를 만들기 위해서는,
1. feature function 몇 개를 정의해주고, (feature function 은 전체 문장, 현재 위치, 그리고 주변의 라벨 값을 참조한다.)
2. weight 를 할당하고
3. 전체를 몽땅 더한 다음,
4. 필요하면 그 값을 확률로 환산하면 된다.

이제 CRF 이 다른 머신 러닝 기술들와 어떻게 다른지 비교해보도록 하자.

## Smells like Logistic Regression…
CRF 의 아래 식은 왠지 친숙해 보일 수도 있다. *[궁금하면 여기](https://en.wikipedia.org/wiki/Logistic_regression){:target='_blank'}*

$$
p(l | s) = \frac{exp[\sum_{j = 1}^m \sum_{i = 1}^n f_j(s, i, l_i, l_{i-1})]}{\sum_{l’} exp[\sum_{j = 1}^m \sum_{i = 1}^n f_j(s, i, l’_i, l’_{i-1})]}
$$

왜냐하면 CRF 는 근본적으로 Logistic Regression 의 순차 버전(sequential version)이기 때문이다.  
>
Logistic Regression 이 분류(claasification) 을 위한 log-linear 모델인데 반해,  
CRF 는 순차적인 라벨링(sequential labels) 을 위한 log-linear 모델이다.

## Looks like HMMs...
HMM([Hidden Markov Model](http://en.wikipedia.org/wiki/Hidden_Markov_model){:target='_blank'}) 도 POS tagging(또한 일반적인 순차 라벨링) 에 사용될 수 있는 모델임을 기억하자.  
CRF 가 라벨링 점수를 계산하기 위해 여러 개의 feature function 을 다 같이 사용하는데 비해,  
HMM 은 라벨링을 위해 생성적(generative) 방식을 취한다. 그 식은 아래와 같다:

$$
p(l,s) = p(l_1) \prod_i p(l_i | l_{i-1}) p(w_i | l_i)
$$

이 때,
+ $$
p(l_i | l_{i-1})
$$ 는 전이 확률(transition probabilities) - 예> 전치사 뒤에 명사가 나올 확률
+ $$
p(w_i | l_i)
$$ 는 방출 확률(emission probabilities) - 예> '명사' 라벨이 '아빠' 일 확률

그럼 HMM 과 CRF 는 어떻게 다른가?  
**CRF 가 더 강력하다!**  
CRF 는 HMM 이 할 수 있는 모든 것을 모델링할 수 있고, 그것보다 더 많은 일을 해낸다. 왜 그런지는 아래를 살펴보자.

HMM 확률 식에 log 를 취하면 아래와 같다.

$$
\log p(l,s) = \log p(l_0) + \sum_i \log p(l_i | l_{i-1}) + \sum_i \log p(w_i | l_i)
$$
