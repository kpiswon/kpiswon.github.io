---
layout: post
title:  "BERT (PART II) : Attention 모델, BERT 모델"
date:   2020-12-28 22:00:00
categories: Language Model
tags : [언어모델, 자연어처리, 자연어, Markov Chain, 마코프 체인, RNN, Seq2Seq, 시퀀스 투 시퀀스, Attention, Attention Model, BERT, 버트]
comments: true
---

이 전 포스팅에서 RNN의 구조적 문제점에 대하여 언급하고, 이에 대한 보완책으로 Attention 모델을 짧게 설명하였다. 이번 포스팅에서는 Attention 모델을 좀 더 살펴보고, 이를 차용한 BERT 모델의 이론적인 부분까지 정리해본다.  

## 다시 한 번, RNN 모델의 문제점

 - 입력 sequence의 길이가 매우 긴 경우, __처음에 나온 token의 대한 정보가 희석__된다.  
 - __고정된 Context vector 사이즈__로 인해 긴 sequence에 대한 정보를 함축하기 어렵다.  
 - 모든 token이 영향을 미치니, __중요하지 않은 token도 Context Vector에 영향__을 준다. 

 상기 언급한 RNN 모델의 문제점을 해결할 방법은 무엇이 있을까? 인코더에서 나온 모든 state의 정보를 활용하는 것이다. 이를 통해 우리가 얻을 수 있는 장점은 크게 2가지가 있다. 
 
 ![LM2-2](/assets/images/LM2-2.png)
 
 1. 고정된 Context Vector 사이즈로 인한 문제를 해결할 수 있다.  
 2. 인코더에서 나온 모든 state 중에서 우리가 집중하여야 할 부분만 집중할 수 있도록 설계할 수 있다.  

 이 점을 기억하고 Attention 모델에 대해서 자세히 살펴보도록 한다.  


--- 

## Attention 모델
 
 인간이 사물을 인식하는 방법을 생각해보자. 사물을 인식할 때 사물 주변의 모든 사물들과 환경들을 한 번에 인식하지 않는다. 즉, 특정 사물임을 인지할 때 중요한 feature 정보만 인지한다면 그 사물이 무엇인지 특정하기에는 충분하다는 것이다. 이러한 인간의 정보처리와 마찬가지로, 중요한 feature는 더욱 중요하게 고려하는 것이 Attention 모델의 모티브이다.

 ![LM2-1](/assets/images/LM2-1.png)

 실제 예시를 들어 Attention 모델을 설명해보겠다. "How are you?"를 인코딩하고 이를 다시 "잘 지내?"로 디코딩하는 과정이다.  
 먼저 시작부분은 기존 RNN 모델에서 시작된다.  

 ![LM2-3](/assets/images/LM2-3.png)  

 위 그림에서 h3가 전통적인 Seq2Seq의 Context Vector이다. 하지만 Attention모델은 각각의 state에서 나온 vector h1, h2, h3를 모두 사용하여 context vector를 만들 것이다. h1, h2, h3를 Fully Connected Layer에 전달하여 각각의 score에 해당하는 s1, s2, s3를 이끌어낸다. 

 ![LM2-4](/assets/images/LM2-4.png) 
 
 위 그림에서 FC(h3)부분을 다시 넣은 이유는 아직 Decoder에서 나온 값이 없으므로 단순히 마지막 state값을 넣어준 것이다. 이 FC Layer를 거쳐 계산된 score들은 0~1 사이의 값으로 변환해주는 Softmax를 취하여 상대적인 점수를 구할 수 있다.  

 ![LM2-5](/assets/images/LM2-5.png)  

 'How'는 90%, 'are'는 0%, 'you'는 10%가 나온 것이라고 해석하면 된다. 이 값들은 우리가 얼마만큼 어디에 포커스를 해야하는 지를 나타내는 지표이다. 이 지표를 이용하여 첫 번째 Context Vector를 만들 수 있다.

 ![LM2-6](/assets/images/LM2-6.png) 

 위에서 구한 첫 번째 context vector(cv1)를 decoder의 첫 번째 RNN cell에 넣어준다. 지금껏 번역한 이전 state가 없기 때문에 {% raw %}<start>{% endraw %}라는 신호도 함께 input으로 넣어주게 된다.






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

