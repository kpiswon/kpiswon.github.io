---
layout: post
title:  "BERT (PART I) : Markov Chain, RNN, Seq2Seq 언어 모델"
date:   2020-09-19 22:00:00
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

 Markov 확률 기반의 언어 모델은 현재 상태에서만 독립적으로 의존하여 다음 상태를 예측하는 모델이기 때문에, 과거의 문맥을 반영하기 어렵다. 이를 보완하는 방법으로 RNN 기반의 언어모델이 등장하였는데, **<u>RNN은 히든 노드가 방향을 가진 엣지로 연결돼 순환구조를 이루는(directed cycle) 인공신경망의 한 종류이다. 이전 state의 정보가 다음 state를 사용됨으로써, 시계열 데이터 처리에 적합하다.</u>**  

 기본적인 RNN의 구조는 다음과 같다. 현재 단계의 Input에서 미래를 예측하는 Output를 예측하는 데에 있어서 현재의 히든레이어를 사용하는데, 현재의 히든레이어는 이전에 미래를 예측하는 데 쓰였던 전 단계의 히든레이어를 반영하여 계산하므로, 문맥을 반영할 수 있게 된다.

 ![LM4](/assets/images/LM4.png)

 INPUT은 character 단위가 될 수도 있고, word, sentence 등으로 설정할 수 있다. 예시는 `'hello'`를 character 단위(알파벳)로 끊어서 RNN의 학습과정을 들어보겠다.  
 먼저 각각의 알파벳은 one-hot-vector로 바꾸어 input으로 넣어준다. one-hot-vector는 0,1로 표현한 알파벳의 고유벡터라고 생각하면 된다.

 > h : (1,0,0,0)  
 > e : (0,1,0,0)  
 > l : (0,0,1,0)  
 > o : (0,0,0,1)  

 다음 문자를 학습하는 RNN encoder의 구조는 다음과 같다.

 ![LM5](/assets/images/LM5.png)
 
 위 RNN encoder를 구조를 인지해두고 char 단위가 아니라 띄어쓰기 단위로 학습을 시켜보자.

 ![LM6](/assets/images/LM6.png)

 **<u>output layer에서 최종 출력은 앞선 단어들의 문맥을 고려해서 만들어진 벡터로 'Context Vector'라고 부른다.</u>** 이 Context Vector를 이용하여 다양한 신경망 모델을 만들 수 있는데, 예를 들어 Context Vector가 긍정적인 감정에 대한 문장에서 나온 것이면 1, 부정적인 감정이면 0으로 라벨을 하여 학습을 시킨다면 감정 분류를 위한 모델을 만들 수 있다.

### RNN 모델 기반의 Seq2Seq  

 RNN을 기반으로한 Sequence to Sequence(Seq2Seq) 모델은 Encoding 된 언어를 다시 Decoding을 함으로써, 출력을 예측시킬 수 있다. 
 
 ![LM7](/assets/images/LM7.png)

 > Encoder Layer : Context Vector를 획득  
 > Decoder Layer : 획득된 Context Vector를 입력으로 다음 state를 예측  

 활용 예는 다양하게 이용할 수 있는데, 몇 가지 예를 들어보겠다.

 |ENCODER|DECODER|
 |:---:|:---:|
 |음성주파수|음성을 텍스트형태로 변환|
 |비디오프레임|무슨 장면인 지에 대한 라벨링|
 |한국어 텍스트|번역된 영어 텍스트|
 |"오늘 날씨 가 참 좋습니다"|명사 명사 조사 부사 동사|

---

## RNN 모델의 구조적 문제점  

 문맥을 파악하는 RNN을 모델을 만들었는 데 RNN 모델에도 구조적인 문제점이 존재한다. 

 - 입력 sequence의 길이가 매우 긴 경우, 처음에 나온 token의 대한 정보가 희석된다.
 - 고정된 Context vector 사이즈로 인해 긴 sequence에 대한 정보를 함축하기 어렵다.
 - 모든 token이 영향을 미치니, 중요하지 않은 token도 Context Vector에 영향을 준다. 

 이를 개선하기 위해 어텐션(Attention) 모델이 등장하는데, Attention 모델은 최종 output인 Context Vector만 활용하는 RNN과 달리, 각각의 output을 모두 이용하여 Context Vector를 Dynamic하게 생성하는 모델이다. 이에 대해서는 다음 포스팅에서 이어 설명하겠다.

  ![LM8](/assets/images/LM8.png)

---



최종수정일 2020.09.19



> ###### References
> T academy "자연어 언어모델 'BERT'" 강의자료  
> [[딥러닝 기계번역] 시퀀스 투 시퀀스 + 어텐션 모델](https://www.youtube.com/watch?v=WsQLdu2JMgI)  
> [KOCW](http://elearning.kocw.net/contents4/document/lec/2013/Konkuk/Honghyeoncheol/8.pdf)  

{% highlight ruby %}
print("hello, neighbors!")
print("Sungwon Kim © 2020 • All rights reserved.")
{% endhighlight %}

[Github][githuburl]

[githuburl]: https://github.com/kpiswon

