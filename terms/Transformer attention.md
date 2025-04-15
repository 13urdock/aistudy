bert
e: encoder
![[Pasted image 20250405190642.png]]

## 파라미터 계산: attention
![[Pasted image 20250405190713.png]]
it이라는 단어를 문장의 어디에 더 비중을 둘 지
## 행렬곱셈
행과 상대 열의 길이기 같아야함

3차원 행렬
![[Pasted image 20250405190951.png]]
면의 넓이가 같아야 

torchinfo - summary
-> embedding의 수, parameter들 볼 수 있음

embedding: 단어의 의미를 찾는 것 -> 벡터로 표시한 것
=> 파라미터 생성됨

# 임베딩
![[Pasted image 20250405191323.png]]
2) transformer에 저장되어 있는 vocab의 수가 30522개 있다는 뜻
![[Pasted image 20250405191516.png]]
seqeunce_length: 문장을 단어 단위로 쪼갠 단위

output: 임베딩을 한 결과

![[Pasted image 20250405192051.png]]
처음 임베딩 결과: 배는 같은 의미로 생각됨

임베딩에 위치값을 더해주면서, 앞의 배와 뒤의 배가 다른 의미를 가진 것이라는 것을 보여줌

![[Pasted image 20250405192310.png]]
단어간 관계를 학습시키기 위해 나온 것이 어텐션

input에 가중치를 곱한 결과 3개
![[Pasted image 20250405192523.png]]
view 함수: reshape

![[Pasted image 20250405192713.png]]
배에 대한 의미를 32차원에서 4그룹(head)로 나누어 의미를 각자 해석하도록함
![[Pasted image 20250405193135.png]]
눕히면서..? 독립축이 헤드가 됨

![[Pasted image 20250405193310.png]]

![[Pasted image 20250405193440.png]]
계산 후 다시 원래대로 돌린후

2차원으로 만듬
![[Pasted image 20250405193531.png]]

![[Pasted image 20250405193643.png]]


![[Pasted image 20250405193706.png]]

![[Pasted image 20250405193811.png]]

![[Pasted image 20250405193825.png]]
언어모델: layer normalization
batch normalization과 차이점
정규화 방향이 다름

감마와 베타: 각 열마다 있음, 총 64개
값이 너무 튀는 것을 보정해주기 위함 값
![[Pasted image 20250405194053.png]]
Feed Forward Network
d_model, dff
d_model < dff
전체를 늘렸다가(dff) 줄이면서 의미 관계도 재설정함

transformer layer를 여러번 쌓을 수 있음
normalization을 여러번 진행하는 것

miniLM-6v: 6개의 transformer layer

input, output 모양이 같아 layer 스케일링 가능
classifier: 분류기
![[Pasted image 20250405194843.png]]
cls: 문장의 시작을 알리는 토큰,
가중치 연산 후 바로 결과 그릴 수 있음

왜 이름들이 이렇게 붙었느냐 . . ..?
RNN
![[Pasted image 20250405195555.png]]
단어 위치가 멀면 앞단어를 까먹음
전체적인 의미 파악 어려윰

현재 시퀀스길이: 5,
임베딩후 :
![[Pasted image 20250405195819.png]]
단어에 대한 독립적인 의미만 있음

attention
![[Pasted image 20250405195955.png]]
루드D: dimension=32, 곱하면 계산 결과가 너무 커져서 나눠주는 것, 이 값이 제일 안정적이었음

Q: sequence by d
K^t: sequence의 전치행렬
-> 곱셈결과 : sequence by sequence의 행렬이 생김
![[Pasted image 20250405200135.png]]
-> 단어와 단어 사이의 관계가 나오게됨
해석한 결과를 쿼리키밸류

Qk^t: 단어의 관계
V: 단어의 독립적인 의미
둘을 곱한 결과물: 관련도와 의미가 결합되었다고 해석
-> attention에 대한 설명

![[Pasted image 20250405200743.png]]


normalization
![[Pasted image 20250405193932.png]]
# CONFIG
![[Pasted image 20250405191924.png]]


# 정리 - BERT 분류기 Encoder
sequence(문장 어절 수)
SV * VD -> 임베딩을 함
![[Pasted image 20250405201114.png]]

쿼리키밸류 d를 dqjs 곱함
-> 어텐션계산
-> layer normalization(감마베타) X DD 행렬(dense) X FFN
![[Pasted image 20250405201323.png]]

이 과정 계
![[Pasted image 20250405201428.png]]

이 과정 하나 더 하면 decoder