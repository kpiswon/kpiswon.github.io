---
layout: post
title:  "BERT (PART II) : Attention 모델"
date:   2020-12-28 22:00:00
categories: Language Model
tags : [언어모델, 자연어처리, 자연어, Markov Chain, 마코프 체인, RNN, Seq2Seq, 시퀀스 투 시퀀스, Attention, Attention Model, BERT, 버트]
comments: true
---

이전 포스팅에서 RNN의 구조적 문제점에 대하여 언급하고, 이에 대한 보완책으로 Attention 모델을 짧게 설명하였다. 이번 포스팅에서는 Attention 모델을 좀 더 살펴보고, 이를 차용한 BERT 모델의 이론적인 부분까지 정리해본다.  

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

## Attention 모델 (Seq2Seq with attention model)
 
 인간이 사물을 인식하는 방법을 생각해보자. 사물을 인식할 때 사물 주변의 모든 사물들과 환경들을 한 번에 인식하지 않는다. 즉, 특정 사물임을 인지할 때 중요한 feature 정보만 인지한다면 그 사물이 무엇인지 특정하기에는 충분하다는 것이다. 이러한 인간의 정보처리와 마찬가지로, 중요한 feature는 더욱 중요하게 고려하는 것이 Attention 모델의 모티브이다.

 ![LM2-1](/assets/images/LM2-1.png)

 실제 예시를 들어 Attention 모델을 설명해보겠다. "How are you?"를 인코딩하고 이를 다시 "잘 지내?"로 디코딩하는 과정이다.  
 먼저 시작부분은 기존 RNN 모델에서 시작된다.  

 ![LM2-3](/assets/images/LM2-3.png)  

 위 그림에서 h3가 전통적인 Seq2Seq의 Context Vector이다. 하지만 Attention모델은 각각의 state에서 나온 vector h1, h2, h3를 모두 사용하여 context vector를 만들 것이다. h1, h2, h3를 Fully Connected Layer에 전달하여 각각의 score에 해당하는 s1, s2, s3를 이끌어낸다. 

 ![LM2-4](/assets/images/LM2-4.png) 
 
 위 그림에서 FC(h3)부분을 다시 넣은 이유는 아직 Decoder에서 나온 값이 없으므로 단순히 마지막 state값을 넣어준 것이다.(뒤에서 이어 설명) 이 FC Layer를 거쳐 계산된 score들은 0~1 사이의 값으로 변환해주는 Softmax를 취하여 상대적인 점수를 구할 수 있다.  

 ![LM2-5](/assets/images/LM2-5.png)  

 'How'는 90%, 'are'는 0%, 'you?'는 10%가 나온 것이라고 해석하면 된다. 이 값들은 우리가 얼마만큼 어디에 포커스를 해야하는 지를 나타내는 지표이다. 이 지표를 이용하여 첫 번째 Context Vector를 만들 수 있다.

 ![LM2-6](/assets/images/LM2-6.png) 

 위에서 구한 첫 번째 context vector(cv1)를 decoder의 첫 번째 RNN cell에 넣어준다. 지금껏 번역한 이전 state가 없기 때문에 {% raw %}<start>{% endraw %}라는 신호도 함께 input으로 넣어주게 된다.

 ![LM2-7](/assets/images/LM2-7.png)  

 context vector와 start 신호를 input으로 넣어준 RNN cell의 결과물로 How 0.9, are 0.0, you 0.1이 결합되어 번역된 '잘'이 output으로 나오게 된다. 여기까지가 한 바퀴 돌았다고 생각하면 된다. 

 ![LM2-8](/assets/images/LM2-8.png)   

 다음 사이클에서는 decoder에서의 현재 state값(dh1)이 FC layer에 input으로 들어가게 된다. 방금 전 처음 시작할 때에 decoder에서 나온 것이 없으므로 h3을 넣었다고 한 부분이다. 위에서 유의하게 보아야할 점은 FC layer의 input으로서 h1, h2, h3는 항상 쓰인다는 점이다. 이 값들 중 우리가 어떤 값에 중요하게 포커스를 해야할 지 사이클마다 계산을 하여야하기 때문이다.

 ![LM2-9](/assets/images/LM2-9.png)   

 다시 FC layer에서 나온 s1, s2, s3의 score값을 softmax를 취하면 새로운 Attention weight를 계산할 수 있다. 위 예시에서 2번째 단계에서는 How에 0.1, are에 0.0, you?에 0.9의 가중치를 계산하였다. 이를 이용하여 context vector (cv2)를 구하고, 두 번째 RNN cell에 전달한다. 이와 함께 이전 RNN cell의 output도 함께 전달하여 '지내?'라는 output을 도출하게 된다.

 한 번의 사이클을 더 돌게 되면 {% raw %}<end>{% endraw %}라는 output이 나오게 되며, 이 떄 사이클은 멈추게된다. 즉, 번역결과로 우리가 원하는 '잘 지내?'라는 번역결과가 나오는 것이다.

 여기까지 과정을 살펴보면, RNN의 기존 모델에서 Context Vector의 고정된 size로 인한 한계를 극복하고 Dynamic하게 context vector를 만드는 과정을 볼 수 있다. 이는 좀 더 방대한 양의 정보를 함축하는 데 있어서 효율적일 것이란 것을 체감할 수 있다. 

 정리를 해보자면,  
 > - Attention 모델의 모티브인 중요한 feature를 더 중요하게 고려하는 부분은 Attentin weight가 그 역할을 담당한다.  
 > - Context Vector가 각각 state별로 디코딩할 때마다 달라진다.  

 ### 만약 Prediction이 틀렸다면? - Teacher Forcing
 디코딩을 할 때 이전 RNN cell의 output을 가져다 쓰기 때문에, 이전 cell에서 잘못된 output을 도출하였다면 이후 값들은 안봐도 계속 틀릴 확률이 농후할 것이다. 이러한 점을 극복하고 학습을 좀 더 빠르고 효율적으로 진행하기 위하여 이전 RNN cell의 output을 가져다 input으로 넣는 대신, 정답을 넣어주는 방법이 있다. 이를 Teacher Forcing Method라고 하는 데, 이렇게 함으로서 과거에는 잘못된 결과를 도출했음에도 불구하고, 이 잘못된 결과를 가져다 쓰지 않고 정답을 input으로 학습을 하기 때문에 뒷부분은 올바른 prediction을 할 수 있게 된다.  

---

## Attention 모델의 한계점
 Attention 모델은 앞에서 언급했듯 **1. 기존 RNN의 고정된 Context Vector 크기로 인해 상대적으로 긴 Sequence의 정보를 함축할 수 없는 것을 해결한 점, 2. 중요한 정보만 가중치를 주어 Context Vecotor에 영향을 주게 하는 점, 3. 매우 긴 Sequence에 대해 앞부분의 정보가 뒤로갈수록 희석되지 않는다는 점**에서 기존 Seq2Se1의 encoder, decoder의 성능을 비약적으로 향상시켰다. 하지만 여전히 RNN이 순차적으로 연산이 이루어지고 있는 것은 변치 않는다. 이것이 내포하는 것은 아직도 연산 속도가 느리다는 점이다. 이를 해결하는 방안으로 그냥 RNN을 없애버리자! 이에 대해서는 다음 포스팅에서 Self-attention모델, 그리고 이어서 Transformer모델을 다루어보겠다. 

최종수정일 2020.12.28



> ###### References
> [Neural Machine Translation by Jointly Learning to Align and Translate](https://arxiv.org/pdf/1409.0473.pdf)
> [[딥러닝 기계번역] 시퀀스 투 시퀀스 + 어텐션 모델](www.youtube/watch?v=WsQLdu2JMgl)  
> T academy "자연어 언어모델 'BERT'"

{% highlight ruby %}
print("hello, neighbors!")
print("Sungwon Kim © 2020 • All rights reserved.")
{% endhighlight %}

[Github][githuburl]

[githuburl]: https://github.com/kpiswon

