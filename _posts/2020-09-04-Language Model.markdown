---
layout: post
title:  "BERT (PART I) : Markov Chain, RNN, Seq2Seq, Attention 모델"
date:   2020-09-05 22:00:00
categories: Language Model
tags : [언어모델, 자연어처리, 자연어, Markov Chain, 마코프 체인, RNN, Seq2Seq, 시퀀스 투 시퀀스, Attention, Attention Model, BERT, 버트]
comments: true
---

최근 자연어처리의 강자인 BERT모델에 대해 설명하기 전에, BERT모델이 어떠한 배경에서 나왔는 지 이전의 언어 모델에 대해서 살펴보고자 한다. 

> * __언어모델__ : 언어 모델이란 **<u>"자연어의 법칙을 컴퓨터로 모사한 모델"</u>**을 의미한다.  
> * 주어진 단어들로부터 그 **<u>다음에 등장한 단어의 확률을 예측</u>**하는 방식으로 학습한다. (이전 state로 미래의 state를 예측)  
> * 다음의 등장할 단어를 잘 예측하는 모델은 그 **<u>언어의 특성이 잘 반영된 모델</u>**이자, **<u>문맥을 잘 계산하는 좋은 언어 모델</u>**이라고 할 수 있다.  

![LM1](/assets/images/LM1.png)

--- 

## Markov 확률 기반의 언어 모델 (마코프 체인 모델)
 
 초기의 언어 모델은 다음의 단어나 문장이 나올 확률을 통계와 단어의 n-gram을 기반으로 계산하였다. 딥러닝 기반의 언어모델은 해당 확률을 최대로 하도록 네트워크를 학습한다. 
 
 여기서 이용되는 **<u>마르코프 성질은 과거와 현재 상태가 주어졌을 때, 미래 상태의 조건부 확률 분포가 과거 상태와는 독립적으로 현재 상태에 의해서만 결정된다는 걸 뜻한다.</u>** 언어 모델에 적용시키면 현재 단어 뒤에 미래의 단어가 어떤 것이 오는 지 경우의 수를 나타낸 다음, 확률을 계산한다. 예를 들어보겠다.
 
 ![LM2](/assets/images/LM2.png)
 
 위와 같이 노드 다음에 어떤 노드가 오는 지 확률로 표현하였고, 이를 테이블로 정리해보면 다음과 같다.
 
 ![LM3](/assets/images/LM3.png)
 
 위 표를 이용하여 다음과 같은 문장들이 나올 확률을 계산할 수 있다.
 
 > "I like rabbits" : 0.66 x 0.33 = 0.22  
 > "I like turtles" : 0.66 x 0.33 = 0.22  
 > "I don't like snails" : 0.33 x 1.0 x 0.33 = 0.11

---

## Recurrent Neural Network(RNN) 기반의 언어 모델

 RNN은 히든 노드가 방향을 가진 엣지로 연결돼 순환구조를 이루는(directed cycle) 인공신경망의 한 종류이다. 이전 state의 정보가 다음 state를 사용됨으로써, 시계열 데이터 처리에 적합하다.  

 기본적인 RNN의 구조는 다음과 같다.

 ![LM4](/assets/images/LM4.png)

 INPUT은 character 단위가 될 수도 있고, word, sentence 등으로 설정할 수 있다. 예시는 `'hello'`를 character 단위(알파벳)로 끊어서 RNN의 학습과정을 들어보겠다.
 




---


최종수정일 2020.09.04



> ###### References
> T academy "자연어 언어모델 'BERT'" 강의자료  
> [[딥러닝 기계번역] 시퀀스 투 시퀀스 + 어텐션 모델](www.youtube/watch?v=WsQLdu2JMgl)  
> [KOCW](http://elearning.kocw.net/contents4/document/lec/2013/Konkuk/Honghyeoncheol/8.pdf)  

{% highlight ruby %}
print("hello, neighbors!")
print("Sungwon Kim © 2020 • All rights reserved.")
{% endhighlight %}

[Github][githuburl]

[githuburl]: https://github.com/kpiswon

