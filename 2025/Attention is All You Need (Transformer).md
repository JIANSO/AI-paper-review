# Attention Is All You Need — Paper Review

A. Vaswani, N. Shazeer, N. Parmar, J. Uszkoreit, L. Jones, A. N. Gomez, Ł. Kaiser, and I. Polosukhin,  
"Attention is all you need,"  
in *Advances in Neural Information Processing Systems (NeurIPS)*, vol. 30, pp. 5998–6008, 2017.

---

## 1. Background

### Attention Mechanism

어텐션을 통해서 현재 출력 대상에 대해, 모든 인코더의 hidden state를 고려할 수 있다.

---

## 2. Training Architecture

### Overview

트랜스포머 아키텍쳐

---

### Input into the Encoder

- 문장의 각 시퀀스(토큰)를 학습에 사용되는 벡터로 임베딩하는 embedding layer 구축  
- 임베딩 행렬 W에서 look-up을 수행하여 해당하는 벡터를 변환  
  (W: Trainable parameter)

트랜스포머는 입력 시퀀스들이 순서대로 들어가지 않으므로,  
**Positional Encoding(Positional Embedding)** 을 사용한다.

---

### Encoder (Attention)

문장 내 토큰들이 갖는 관계 학습 - Q, K, V 개념

#### Q, K, V로 나누는 이유

“입력의 다른 역할을 분리해서, 중요한 정보를 스스로 찾아내게 하려고”

트랜스포머의 Self-Attention은  
“입력의 각 단어가 다른 단어들을 얼마나 주의해야 하는지(weight)”를 학습하는 구조다.

이때 한 단어가 하는 역할이 세 가지라서  
입력을 세 가지 벡터로 ‘투영(projection)’시킨다.

#### 왜 하나의 인풋에서 세 가지(Q, K, V)가 필요할까?

자연어에서 단어가 하는 역할:

- 다른 단어를 찾는 역할 → Query  
- 자기 자신이 어떤 특징을 가지고 있는지 표현 → Key  
- 실제 전달되는 의미 정보 → Value  

따라서 입력 x 를 1개의 벡터로만 쓰면 Attention 불가능.

---

### 수학적으로 왜 Q, K, V로 나누는가?

입력 X(토큰 임베딩)를 세 개의 가중치 행렬로 선형변환:

여기서  
`W_K`, `W_V`, `W_Q` 는 학습되는 파라미터.

### Self-Attention은 이렇게 계산됨
 
$Attention(Q,K,V)=soft\max _{ }^{ }(\frac{Q\combi{K}^T}{\sqrt{\combi{d}^k}​​}​)V$  
Attention(Q,K,V)=softmax ( QKT √dk​​​​)V  

각 파트가 담당하는 역할  
QKT: “얼마나 주의해야 하는지” (score)  
softmax: score → 확률화  
그 확률로 V(정보)를 가중합  

만약 Q=K=V를 동일한 벡터로 쓰면?  
“어떤 단어를 기준으로 attention을 계산할지”와 “그 단어의 의미가 무엇인지”가 하나의 벡터에 섞여서 구분이 안 됨.  
그래서 '주의를 계산하는 공간'과 '의미를 담는 공간'을 분리하는 게 매우 중요.  

---

### Encoder(Multi-head Attention)

n_head로 Q, K, V를 나눔  
Attention head별로 다양한 task를 수행할 수 있도록 학습됨  

---

### Encoder(Residual Connection/Layer Norm)



---

### Encoder(Wrap Up)

아래 사진은 인코더의 전체 흐름  

(이미지 추후 추가 가능)  

---

### Decoder

인코더와 굉장히 동일함  

문장의 각 시퀀스(토큰)을 학습에 사용되는 벡터로 임베딩하는 Embedding Layer구축  

임베딩 행렬 W에서 look-up을 수행하여 해당하는 벡터를 반환(W : Trainable Parameter)  

---

### Decoder(Self Attention)

인코더와 Masking 구현이 다르다.  

디코더는 모든 Attention을 다 보지 않도록, 대각선 위 쪽을 모두 마스킹한다.  
Padding token의 위치에 대한 making  
Attention 가중치를 구할 때, 대상 토큰의 이 시점의 토큰은 참조하지 못하도록 masking  

---

### Last Layers

인코더와 디코더를 거친 최종 결과물을 예측에 사용하게 된다.  

---

### Regularization



---

## 3. Result



---

## 참고영상

https://www.youtube.com/watch?v=x_8cp4Vdnak

---

## 해당 논문을 코드로 구현한 github

https://github.com/jadore801120/attention-is-all-you-need-pytorch  

GitHub - jadore801120/attention-is-all-you-need-pytorch:  
A PyTorch implementation of the Transformer model in "Attention is All You Need".  
A PyTorch implementation of the Transformer model in "Attention is All You Need". - jadore801120/attention-is-all-you-need-pytorch  

github.com  

---

https://jalammar.github.io/illustrated-transformer/

The Illustrated Transformer  
Discussions: Hacker News (65 points, 4 comments), Reddit r/MachineLearning (29 points, 3 comments)  
Translations: Arabic, Chinese (Simplified) 1, Chinese (Simplified) 2, French 1, French 2, Italian, Japanese, Korean, Persian, Russian, Spanish 1, Spanish 2, Vietnamese  
Watch: MIT’s Deep Learning State ...  

jalammar.github.io  

---

https://youtu.be/Yk1tV_cXMMU?si=mqPTSuWYrEp5CO-w
