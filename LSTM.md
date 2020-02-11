이 포스트의 내용은 https://towardsdatascience.com의 '[Illustrated Guide to LSTM’s and GRU’s: A step by step explanation](https://towardsdatascience.com/illustrated-guide-to-lstms-and-gru-s-a-step-by-step-explanation-44e9eb85bf21)'을 기반으로 정리한 글입니다. 개인적인 공부차원에서 작성한 것임을 참고하시기 바랍니다.



# LSTM(Long Short Term Memory)

![](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img015.gif)

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img013.png" style="zoom:200%;" />

![](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img014.png)

**LSTM(Long Short Term Memory)**는 RNN에서 Short-Term Memory문제점을 극복하고자 만들어진 층(Layer)이다. LSTM은 필요한 데이터와 불필요한 데이터를 구분하여 학습하여 학습의 효율을 올린다. 



대표적인 예시를 통해 알아보자. 아래 그림은 시리얼에 대한 타노스의 리뷰이다.

![](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img016.png)

우리는 보통 물건을 사기위해 다른 사람이 작성한 리뷰를 통해 물건을 살지말지 결정하기도 한다. 리뷰를 읽어보고 긍정적인 말들이 있으면 사고 싶을 것이고 부정적인 말들이 있으면 사지 않을 것이다. 전부 읽어보니 대략 긍정적인 리뷰인 듯하다. 리뷰에서 우리의 구매욕을 자극하는 중요한 단어나 문장을 찾아보자. 

![](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img017.gif)

아마도 **“Amazing!”** / **“perfectly balanced breakfast”** / **will definitely be buying again!** 이 리뷰에서 가장 중요한 어구인 것을 쉽게 알 수 있다. 나머지 단어인 "this" / "gave" / "all" / "should" 등은 위의 어구에 비해 중요도가 떨어진다. 

이것이 LSTM의 기본 원리이다. 예측을 하는데 상관도가 높은 정보들만 유지하고 상관도가 낮은 정보들은 잊어버린다. 





# LSTM의 데이터 처리과정

### RNN 데이터 처리과정

먼저 RNN의 데이터 처리과정을 간단하게 살펴보자.

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img019.gif" style="zoom:200%;" />

RNN은 단어 데이터들을 전부 컴퓨터가 읽을 수 있는 벡터(Vector)로 만들어주면 한 단어씩 처리를 한다. 데이터 처리가 이루어지는 동안 이전의 Hidden State는 다음 단계로 넘어간다. Hidden State는 neural networks memory(신경망 기억장치?)역할을 하는데 누적된 정보들을 가지고 있다. 

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img020.gif" style="zoom:200%;" />



새로운 Hidden State가 나오기 위해 이전에 어떤 과정을 거치는 알아보자. 

처음에는 Input과 이전의 Hidden State가 결합한 벡터를 만든다. 새롭게 만들어진 벡터는 tahn 함수를 거쳐 새로운 Hidden State를 만들게 된다. 

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img021.gif" style="zoom:200%;" />





그럼  tahn 함수는 무엇일까?



### Tahn Function

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img022.gif" style="zoom:200%;" />

활성화 함수 종류 중 하나인 Tahn Function은 데이터의 값들이 거대해지는 것을 막기위해 항상 -1에서 1로 유지하는 역할을 한다. 무수히 많은 벡터 데이터들이 학습을 거치면서 많은 수학적인 연산을 거치게 된다. 만약에 매 과정마다 데이터가 3씩 곱해지는 연산을 하게 된다면 어떻게 될까? 그렇다면 데이터값이 겉잡을 수 없이 커지게 되어 처리 속도가 그만큼 느려지게 될 것이다. 

![*3 그림넣기](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img023.gif)

![tahn함수 그림 넣기](/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&LSTM/img024.gif)





### LSTM

<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img013.png" style="zoom:200%;" />



<img src="/Users/youngjunyoon/Desktop/Github/WeeklyStudy002_RNN&amp;LSTM/img014.png" style="zoom:200%;" />

**Cell State** : 

> '고속도로'같은 역할을 한다. 연관성이 높은 데이터들만 모아서 바로 이동하기 때문이다.

**Forget State** : 

> Sigmoid함수를 이용하여 값이 0인 데이터는 버리고 1인 데이터를 남기게 된다. 이전의 Hidden State와 현재 Input과 함께 Sigmoid함수를 거치게 된다. 그래서 나온 결과값이 0에 가까우면 버리고 1에 가까우면 유지하게 된다.

**Input Gate** : 

> 