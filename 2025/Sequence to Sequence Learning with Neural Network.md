I. Sutskever, O. Vinyals, and Q. V. Le,  
"Sequence to sequence learning with neural networks,"  
in *Advances in Neural Information Processing Systems (NeurIPS)*,  
vol. 27, pp. 3104–3112, 2014.

---

## 1. Introduction

01) Basic Concept : Machine Translation

SMT

통계적인 방법을 활용하여 기계 번역

---

## 2. Main Contribution

기계 번역의 주요한 방법으로서, 새로운 neural machine translation 구조 제안

Multi-layer LSTM 구조의 Encoder-Decoder 제안

문장을 역순으로 넣어 성능 향상

BLEU score 34.8 달성

SMT BLEU score : 33.3

---

## 3. Model : Seq2Seq

01) encoder-decoder 구조

기존 연구에서 RNN Cell을 썼던 것과 다르게, 본 논문에서는 LSTM 사용

기본적인 인코더-디코더 구조는 동일

---

## 4. Model Architecture

1) Embedding

모든 언어 모델은 그대로 넣을 수 없고, 컴퓨터가 이해할 수 있는 방식으로 변환해야 함. 바로 그 과정이 임베딩이다.

Source sentence와 target sentence 각각 임베딩하여, encoder와 decoder의 input으로 활용한다.

2) Encoder

임베딩된 단어들을 순차적으로 인코더에 입력

임베딩 단어와 hidden state를 LSTM cell로 연산해서 hidden state를 업데이트

마지막 hidden state를 context vector로 지정

3) Decoder

임베딩된 target 단어들을 순차적으로 디코더에 입력

임베딩 단어와 hidden state를 LSTM cell로 연산해서 hidden state를 업데이트 하고, 다음 나올 확률이 높은 단어 예측

Target sentence의 임베딩은 학습 시에만 필요. (추론 시 정답 번역 문장은 주어지지 않기 때문)

4) Softmax Layer

LSTM의 output을 dense layer에 거치고, softmax를 활용해 각 단어가 나올 확률값을 계산

Softmax Layer를 거치며, 각 토큰이 나올 확률값이 output으로 나오게 됨.

이 예시에서는 noir가 나올 확률이 가장 높으므로, 다음 단어는 'noir'로 선택

---

## 5. Training Stage

01) Teacher Forching

chiot(잘못 예측한 값)를 input으로 넣어준다는 건가? 안 넣어준다는 건가? -> 

모델이 잘못 예측한 값이 아닌, 실제 정답을 다음 스텝 입력으로 넣어줌.

학습이 빠르고 안정적이도록 도와줌.

02) Reversed Source Sentence as Input

---

## 6. Inference Stage

Decoder가 예측한 값을 다음 input으로 활용하여 번역을 진행

앞서 teacher-forcing ratio=0으로 두어 예측 진행

Decoder 단계에서 임베딩이 필요하지 않으며, <SOS> 시작을 알리는 토큰을 입력으로 받으며 바로 다음 토큰에 대해 예측

훈련 단계처럼 정답 문장의 임베딩값이 필요하지 않음.

Beam Search

디코딩에서 활용하는 Search Strategy

---

## 7. Experiment

Evaluation Metric : BLUE score

---

https://youtu.be/PipiRRL50p8?si=YR7SSR5gdTvnFxLb
