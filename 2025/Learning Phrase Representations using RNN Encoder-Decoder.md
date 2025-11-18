K. Cho, B. van Merrienboer, C. Gulcehre, D. Bahdanau, F. Bougares, H. Schwenk, and Y. Bengio,  
“Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Translation,”  
in Proceedings of the 2014 Conference on Empirical Methods in Natural Language Processing (EMNLP), Doha, Qatar, pp. 1724–1734, Oct. 2014,  
doi: 10.3115/v1/D14-1179.

---

## 1.Introduction

Translation이란?

Translation 목표

Source 문장을 Target 문장으로 변환하는 것.

Statistical Machine Translation이란?

통계적 방법론을 활용하여 번역을 하는 방법

다양한 feature들의 확률을 선형 조합하여 제일 높은 확률을 갖는 문장으로 번역

Translation의 특징

Source 문장의 길이와 Target 문장의 길이가 가변적임

문법에 따라 Source 문장과 Target 문장의 단어 등장 순서가 다름

한 개의 Source 문장이 2개 이상의 Target 문장과 의미적으로 매핑될 수 있음

논문의 Key Point

Gated Recurrent Unit(GRU) 아키텍처 제시

순차적으로 입력(가변적)을 받아서, 고정된 크기의 벡터 형태로 지식을 압축하는 아키텍처

LSTM과 비슷하게 작동하지만, 보다 간단한 구조를 갖고 있으며, 계산량을 줄인 아키텍처

Sequence-to-Sequence 아키텍처 제시

가변적인 길이의 Source 문장의 문법적, 의미적 특징을 고정된 크기의 벡터로 압축할 수 있는 아키텍처.

고정된 크기의 벡터로부터 문법적, 의미적을 고려한 가변적인 길이의 Target 문장을 생성할 수 있는 아키텍처.

Conditional log-likelihood를 활용하여 학습할 수 있는 아키텍처.

---

## 2.Model

Gated Recurrent Unit(GRU) 아키텍처

Gated Recurrent Unit(GRU) 아키텍처 목표

순차적으로 입력(단어열)을 받아서 고정된 크기의 벡터 형태로 지식을 압축

GRU 아키텍처의 출력은 항상 동일한 크기의 벡터 형태

단어가 임베딩 되어 들어간다.

Gated Recurrent Unit(GRU) 아키텍처 상세

전체 이미지

특정 시점 t에서 GRU의 입력은 $x'_t$ (임베딩 벡터)로 표기된다.

GRU 이외의 히든 벡터 입력은 $h'_{t-1}$ (이전 시점의 히든 벡터)로 표기된다.

출력인 히든 벡터는 $h'_t$로 표기된다.

1. Reset Gate : Candidate 계산 과정에서 과거의 정보를 어느 정도 제거할지에 대한 값을 도출하는 역할  
0~1 사이의 값으로 이루어진 벡터를 도출

2. Candidate : 현 시점의 정보와 Reset Gate를 통해 줄어든 과거 정보를 취합하여, 정보 후보군을 계산하는 단계

3. Update Gate : Hidden을 계산하는 과정에서 과거의 정보와 현재 정보 결합 비율에 대한 값을 도출하는 역할.  
0~1 사이의 값으로 이루어진 벡터를 도출

4. Hidden : Update Gate를 통해 현재 정보와 과거 정보를 조합하여 GRU의 최종 결과인 hidden vector를 계산하는 과정.

Gated Recurrent Unit(GRU) 아키텍처 결과

GRU의 마지막 Hidden vector에는 이전 정보들이 압축되어 저장되어 있음.

---

## Sequence-to-Sequence 아키텍처

Sequence-to-Sequence 아키텍처 목표

가변적인 길이의 Source 문장의 문법적, 의미적 특징을 고정된 크기의 벡터로 압축할 수 있는 아키텍처

Encoder와 Decoder로 구성되어 있으며, Encoder는 문장을 압축하는 역할, Decoder는 문장을 생성하는 역할

Encoder의 역할

가변적인 길이의 Source 문장의 문법적, 의미적 특징을 고정된 크기의 벡터로 압축하는 역할

GRU 아키텍처를 활용하여 압축

Encoder 마지막에 도출된 Vector를 Context Vector라고 지칭.

Decoder의 역할

Context Vector를 추가로 입력받는 GRU 아키텍처를 활용

Context Vector를 활용하여 Target 문장을 생성하는 역할

Decoder GRU 아키텍처 상세

Encoder와 아키텍처는 동일하나 Context Vector가 추가됨

Decoder로부터 문장 생성 방법

Hidden vector에 Linear Layer를 추가하여 Vocabulary 개수만큼 Vector를 생성

최종 Vector에 Softmax 함수를 취하면, 특정 단어가 나올 확률을 의미하는 Vector를 생성할 수 있음

Sequence to Sequece 학습하는 방법

Target 문장을 Label로 활용하여 Decoder로부터 나온 확률을 기반으로 학습

Target 문장의 Negative log Likelihood를 계산하고 Gradient Decent 방식으로 학습

Gradient Decent 방식으로 Decoder 뿐만 아니라 Encoder를 함께 학습

학습된 모델을 활용 방법

학습된 모델로부터 Source 문장에 대한 Target 문장의 확률을 도출

---

## 4.Experiment

English French translation

TASK(Dataset)

WMT'14 workshop 데이터를 활용하여 영어로부터 프랑스어로 번역 Task 수행

---

## 5.Conclusion

Summary

GRU 아키텍처

LSTM보다 간단하며 계산량을 줄인 아키텍처를 제안.

Seq-to-Seq 아키텍처

Encoder와 Decoder로 구성된 가변적인 길이를 압축 생성할 수 있는 아키텍처를 제안

번역 Task에서 딥러닝의 가능성

딥러닝 아키텍처를 활용하여 번역 Task를 학습하고 Inference할 수 있는 가능성을 제시

---

https://arxiv.org/abs/1406.1078

Learning Phrase Representations using RNN Encoder-Decoder for Statistical Machine Translation  
In this paper, we propose a novel neural network model called RNN Encoder-Decoder that consists of two recurrent neural networks (RNN). One RNN encodes a sequence of symbols into a fixed-length vector representation, and the other decodes the representation into another sequence of symbols. The enco...

arxiv.org

---

https://youtu.be/T-w0X0R0pPY?si=g-5USDW38msZW7_f
