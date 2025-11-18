S. Sukhbaatar, A. Szlam, J. Weston, and R. Fergus,  
"End-to-end memory networks,"  
in *Advances in Neural Information Processing Systems (NeurIPS)*,  
vol. 28, pp. 2440–2448, 2015.

---

## End To End Memory Networks란?

기존 Memory Networks의 역전파 적용 어려움을 개선하여 엔드 투 엔드 학습이 가능하도록 만든 모델이다. 기존 Memory Networks의 4개 컴포넌트 파이프라인을 단일 레이어 또는 다중 레이어 구조로 통합하고, 입력으로 들어간 질문 정보도 메모리 업데이트 시 고려하여, 역전파가 용이하도록 개선했다.

---

## 핵심 아이디어

외부 메모리를 가진 신경망 구조를 만들어, 입력된 정보들을 메모리에 저장하고, 질의(Query)에 대해 메모리를 여러 번 참조(hops)하여 답변(Answer)를 생성하는 방식. 즉 입력+ 질의 -> 메모리 저장 -> 연속적인 임베딩으로 변환 -> 여러 홉(hops)을 통해 메모리 접근 -> 출력.

이전의 Memery Networks 구조가 각 단계별로 중간 지도(supervision)을 필요로 했던 데 비해, 이 논문 구조는 end-to-end 학습을 가능하게 만들어 중간 지도 없이도 학습할 수 있다는 점이 특징임.

---

## 적용 분야

인공적인 질문응답(synthetic QA) 태스크에서 실험

언어 모델링(Language modeling) 태스크에서 일반 RNN/LSTM 수준의 성능을 보여줌.

---

## 구조 및 작동 방식

### 메모리 저장 및 임베딩

입력 x1,…,xn은 메모리 슬롯에 저장됨. 메모리의 크기(슬롯 수)는 고정된 buffer size를 갖는다.

각 입력 문장 xi와 질의 q는 단어사전 V의 토큰들로 구성되어 있다. 이것들은 학습 가능한 임베딩 행렬 A, B를 통해 벡터로 변환된다.

변환된 mi,ci,u를 통해 메모리 접근이 수행된다.

입력 임베딩: mi=Axi  
출력 임베딩: ci=Cxi  
질의 임베딩: u=Bq

### 메모리 접근(hops)

메모리를 단 한 번 참조하는 게 아니라, 여러 번 반복하여 참조(hops)함.

각 hop에서, 질의 uk와 메모리 임베딩 mi의 내적을 통해 관련도를 계산하고, Softmax를 통해 확률 분포 Pi를 구한다.

그 다음, 출력 임베딩 Ci들의 가중합을 계산하여 Ok를 얻는다.

이후, 다음 hop 질의는 이전 질의에 이 Ok를 더해 업데이트 한다.

이 과정을 K번 반복(hop)한 후, 최종 출력 a는 다음과 같이 softmax로 예측한다.

---

## 학습

전체가 미분 가능(differentiable)하게 설계되어 있으며, 입력-출력 쌍만으로 학습된다.

중간 단계 지도(supervision)없이 학습 가능.

학습은 표준 cross-entropy 손실 함수를 사용하며 아래와 같은 수식 형태로 최소화된다.

여러 hop을 사용한 구조가 단일 hop 구조보다 성능 향상을 보임.

---

## 예시

1. John went to the kitchen.  
2. John picked up the apple.  
3. John went to the garden.

Question: Where is the apple?  
Answer: garden

입력 문장 1–3  
“사실(facts)”로서 메모리 슬롯에 저장됨

질문 문장  
query q 로 변환되어 메모리를 탐색

attention  
“apple” 관련 문장(slot 2, 3)에 높은 확률 부여

hop 1 결과  
John이 apple을 들고 있음 → 위치 관련 문장(slot 3)을 다시 참조

hop 2 결과  
garden이라는 답 도출

즉, 입력 문장 = 기억 내용/저장, 질문 = 메모리 검색 키(key)/검색 역할이다.

---

## 1.Introduction

Challenge

Multiple Computational steps

Long term dependency in sequential data

Propose

Extension of RNN architecture with multiple computational steps('hops')

이번 논문은 MeMory Networks의 후속 연구이다.

기존 Memory Networks는 4개의 컴포넌트가 있었다.

하지만 Max operation 때문에 BackPropagation을 적용하기 어렵다는 문제가 있었다.

---

## 2.Approach

End To End Memory Networks는 단일 레이어와 다중 레이어로 구성됨.

입력으로 들어간 질문, 쿼리에 대한 정보도 함께 고려해서 메모리를 업데이트 함.

### Single Layer Version

(원문 그대로 – 이미지 없음)

### Tree Layer Version / Multiple Layer

Complextity가 높아지는 것을 보완하기 위해 2가지 방법을 사용함.

Trick 1. Adjacent  
학습 파라미터를 줄임

Trick 2. Layer-wise(RNN-like)  
각 레이어 별로 임베딩 매트릭스를 동일한 것을 사용함.

---

## 3.Syntetic Question and Answering Experiments

### 1.QA Dataset

실험 데이터로 bAbI 데이터 사용

### 2.Model Details

Sentence Representation

Back-of-Words(Bow)

Position Encoding(PE)

Temporal Encoding(TE)

Random Noise(RN)

### 3.Training Details

Linear Start(LS) : if validation loss stopped decreasing then the softmax layers were re-inserted.

### 4. Baselines

MemNN(AM+NG+NL Memory Networks)

Adaptive Memories(AM) : performing a variable number of hops rather than2

N-grams(NG)

Nonlinearity(NL) : Embedding using classical two-layer neural network

MemNN-WSH(Weakly Supervised Heuristic)

LSTM(Weakly Supervised)

### 5. Results

Memory hop이 클수록 좋음

### 6. Demo


---

https://youtu.be/CM5lRnNad2Q?si=NqsgXeJWWgovIss9
