S. Ioffe and C. Szegedy,  
"Batch normalization: Accelerating deep network training by reducing internal covariate shift,"  
in *Proceedings of the 32nd International Conference on Machine Learning (ICML)*,  
vol. 37, pp. 448–456, 2015.

---

## Background

Nomalization vs. Standardization

Nomalization

값 자체를 0과 1사이의 값으로 조정

Standardization

평균이 0, 분산이 1이 되도록 조정

본 논문에서 말하는 Normalization은 Standardization

Dataset Shift

분포가 다른 데이터로 인한 성능 하락 문제

크게 세 가지 종류로 나뉨

Covariate Shift

독립 변수에서 분포 변화

본 논문에서 말하는 부분. 현실 상황에서 가장 많이 발생하는 부분.

Prior Probability Shift

목적 변수에서의 분포 변화

Concept Shift

독립 변수와 목적 변수 사이의 관계 변화

---

## Introduction

Internal Covariate Shift

각 layer를 통과하는 Input의 분포가 변화하는 현상

Gradient Vanishing Problem

서로 다른 분포에 최적화하는 과정에서 Sigmoid의 양 극단으로 치우칠 확률 증가

대안

ReLU / Careful Initialization / Small Learning Rate

본 논문 : 분포를 안정화시킴으로써 위 문제를 해결함과 동시에 학습 속도 개선

---

## Method

Batch Normalization

1) Channel 별 Feature를 독립적으로 Normalize

x(hat)에 감마를 곱하고 베타를 더한 이유는, 시그모이드의 논리니어 특성을 사용하기 위함. (그림 빨간 부분)

2) Mini-batch 단위로 Normalize

아래 수식은 y를 구하기 위함

로 Normaliz

BN Layer

Training / Inference

Inference를 수행할 때 BN는 조금 다르게 수행된다. Inference의 목적은, Input이 들어왔을 때 deterministic한 output을 도출하는 것이 목적이기 때문이다.

Training과정에서 statistics(μ, σ)의 avarage/moving average 계산하여 Inference시 사용한다.

---

## Experiments

(a) 그림

BN를 적용하면 Training 스텝이 적을 때에도 높은 성능을 유지한다.

---

## Conclusion

배치 노멀라이제이션(BN)이 해결하는 점

각 배치마다 입력 값을 정규화(평균 0, 분산 1) 해줘서

값의 분포가 계속 변하는 문제(Internal Covariate Shift) 를 줄여주고

2. 덕분에 시그모이드가 포화 영역에 들어갈 가능성이 낮아짐.

즉, BN은 입력이 적당한 범위에 머물도록 하여, 기울이가 0 근처로 줄어드는 것을 막는다.

---

https://youtu.be/4jAyXi7byd8?si=85cJjlbHgrEZePuA
