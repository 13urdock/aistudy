LLM
sophisticated mathematical function that predics what word comes next for any piece of text

한 단어를 정해서 예측하기보다 가능성이 있는 그 다음 단어를 확률을 적용하는 것 -> 확률이 높은 단어 선정

LLM 이 작동하는 방식은 이어지는 값들(parameters, weights)에 의해 정해짐
parameter 가 달라짐에 따라 인풋을 넣었을 때 다음으로 올 단어들의 확률이 달라짐

LLM은 많은 parameter들을 가질 수 있음
사람이 정하지 않음
-> 학습할 때 처음에는 아무말이나 출력하지만
예제(텍스트) 학습을 통해 반복적으로 정제됨

학습방법-pretraining
긴 문장이든 짧은 문장이든, 마지막 단어만 빼고 모델에 넣어 모델이 예측한 단어와 답을 비교하는 방식 -> backpropagation, 이 과정으로 모든 parameter들을 조정하게 되어 정답인 단어를 추측할 수 있도록 하는 방법
but in either case, the way this works is to pass in all but the last word from that example into the model and compare the prediction that it makes with the true last word from the example.

지금까지의 과정이 pretraining
AI assistant 의 목표와는 다른 방식.
The goal of auto-completing a random passage of text from the internet is very different from the goal of being a good AI assistant.

그래서 chatbot 같은 경우, 다른 트레이닝을 함:
RLHF
Reinforcement Learning with Human Feedback
사람이 직접 문제가 되거나 도움되지 않는 예측을 표시하고 고침으로써 모델의 parameter을 바꿈
-> user가 좋아할 수 있도록


2017년 전에는 대부분의 language model이 한번에 한 단어만 처리했었음
but google에서 bert라는 모델을 소개
bert는 문장을 순차로 읽지 않음, 병렬로 모든 문장을 동시에 읽음

Transformer
the very first step inside a transformer, and most other language models for that matter, is to associate each word with a long list of numbers

![[단어들이숫자로연결.png]]
대부분의 언어모델들이 이런 연속되는 숫자들로만 트레이닝이 가능하기 때문에 언어들을 숫자로 encode 해야함
그리고 이 숫자들이 다시 대응 되는 단어들로 encode 될 것 

transformer의 특징
attention
숫자배열(벡터)들이 서로 '대화'할 수 있도록 하여 맥락에 따라 인코드한 의미들을 정제함
모두 병렬로 이루어짐
ex) down by the river bank
-> bank가 encoding 하는 숫자들이 바뀔 수 있음 문장의 맥락에 따라 강둑(riverbank)라는 개념으로 만들기 위해서

feed-forward neural network
트레이닝하면서 모델이 더 많은 패턴을 저장할 수 있도록 함

데이터들이 이 두 과정을 반복하면서 학습이 진행되고, 각 단어에 대응되는 숫자 배열(벡터)이 그 다음 단어가 뭐가 될지 정확하게 예측할 수 있도록 필요한 모든 정보를 인코딩할 수 있음
ex) bank -> riverbank -> Beginning of a story, Establishing a setting
학습이 진행되면서 bank라는 단어에 대한 맥락과 정보가 쌓임

학습 마지막에 모델이 학습하며 얻은 정보와 맥락에 영향을 받아 다음 단어를 예측함