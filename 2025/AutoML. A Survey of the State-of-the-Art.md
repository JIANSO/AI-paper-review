X. He, K. Zhao, and X. Chu, “AutoML: A Survey of the State-of-the-Art,” Knowledge-Based Systems, vol. 212, p. 106622, Jan. 2021. doi: 10.1016/j.knosys.2020.106622

## Abstract 

Deep learning (DL) techniques have obtained remarkable achievements on various tasks, such as image recognition, object detection, and language modeling. However, building a high-quality DL system for a specific task highly relies on human expertise, hindering its wide application. Meanwhile, automated machine learning (AutoML) is a promising solution for building a DL system without human assistance and is being extensively studied. This paper presents a comprehensive and up-to-date review of the state-of-the-art (SOTA) in AutoML. According to the DL pipeline, we introduce AutoML methods –– covering data preparation, feature engineering, hyperparameter optimization, and neural architecture search (NAS) –– with a particular focus on NAS, as it is currently a hot sub-topic of AutoML. We summarize the representative NAS algorithms’ performance on the CIFAR-10 and ImageNet datasets and further discuss the following subjects of NAS methods: one/two-stage NAS, one-shot NAS, joint hyperparameter and architecture optimization, and resource-aware NAS. Finally, we discuss some open problems related to the existing AutoML methods for future research. 

Keywords: deep learning, automated machine learning (AutoML), neural architecture search (NAS), hyperparameter optimization (HPO) 

“딥러닝 모델을 만들 때 필요한 수많은 결정(데이터 전처리, 하이퍼파라미터, 네트워크 구조 등)을 사람이 일일이 하지 않고, 자동으로 잘 찾는 방법을 체계적으로 정리한다.”

AutoML(Automated Machine Learning)의 목표는 “비전문가도 고성능 모델을 쉽게 만들게 하는 것”이다.

이 논문은 그중에서도 특히 딥러닝 중심의 AutoML 기술에 초점을 맞춰서, 딥러닝 시스템 구축의 자동화를 다룬다.

---

## 단계

| 단계 | 설명 | 대표 기술 |
|------|------|-----------|
| ① 데이터 준비(Data Preparation) | 데이터 정제, 정규화, 증강(Augmentation) 자동화 | 자동 결측치 처리, 자동 스케일링 |
| ② 특징 엔지니어링(Feature Engineering) | 입력 특징 자동 생성 및 선택 | Feature synthesis, feature selection |
| ③ 하이퍼파라미터 최적화(HPO) | 학습률, 배치 크기 등 수많은 하이퍼파라미터를 자동 탐색 | Bayesian Optimization, TPE, Hyperband |
| ④ 신경망 구조 탐색(NAS) | CNN, RNN, Transformer 같은 구조 자체를 자동으로 설계 | Reinforcement Learning, Evolutionary Search, Gradient-based NAS |

---

## 0. Contents

1.Introduction  
2.Data Preparation  
3.Feature Engineering  
4.Model Generation  
5.Model Estimation  
6.NAS Performance Summary  
7.Open Problems and Future Work  

---

## 1.Introduction

AutoML이란?

Automated Machine Learning

Data scientists와 Domain experts의 업무를 줄이고, 자동으로 ML application을 설계할 수 있게 함.

제한된 자원으로 ML Pipeline을 자동으로 구축함

AutoML 시스템은 다양한 기술들의 조합을 활용해 ML Pipeline을 설계할 수 있음(ex. Cloud AutoML by Google)

AutoML pipeline은 data preparation, feature engineering, model generation, model estimation으로 구성됨

AutoML Pipeline

아래 Figure 1에서 Neural Architecture Search (NAS)에 대해 확인할 수 있다.

---

## 2.Data Preparation

Data Preparation

Data Augmentation

Figure 3 classifies DA techniques from the perspective of data type (image, audio, and text), and incorporates automatic DA techniques that have recently received much attention.

---

## 3. Feature Engineering

Feature Engineering

Feature Engineering은 Raw data로부터 알고리즘과 모델이 사용할 적절한 feature를 추출하는 것을 목적으로 한다.

### (1) Feature Selection

불필요한 feature를 제거하여 feature subset을 구축함

일부만 사용하기에, 모델을 단순화하고 오버 피팅을 방지하여 성능을 향상시킬 수 있음

Complete Search, Heuristic Search(ex. 전방 선택, 후방 소거, Bidirectional), Random Search(ex. 유전 알고리즘)

### (2) Feature Construction

모델의 일반화 성능을 향상시키기 위해 새로운 feature를 생성함

Representation 성능 향상을 위해 기존에는 standardization, normalization, feature discretization 같은 전처리 방식들이 사용됨

Feature 생성을 자동화하기 위해 사전 정의된 transformation 종류 중, 선택하는 과정에 의사결정나무나 유전 알고리즘이 사용해 자동화 하려는 연구들이 등장

### (3) Feature Extraction

차원축소 + 새로운 feature 생성

PLA, LDA 같은 mapping function을 사용하는 방식에서, 딥러닝 등장 이후 Feed forward neural network, Autoencoder 기반 방식들이 사용되고 있음

---

## 4. Model Generation 

Model Generation 

논문의 이후 내용은 NAS에 대해 중점적으로 설명한다.

Neural Architecture Search pipeline

### (1) Search Space

Architecture Search를 할 범위를 지정함

NAS에선 neural network으로 제한

### (2) Architecture Optimization Method

정해진 Search space 안에서 모델 최적 architecture를 탐색

효율성과 탐색된 architecture의 성능을 높이기 위한 알고리즘들이 사용됨

### (3) Model Estimation Method

탐색된 architecture의 성능을 평가

일반적인 학습 -> 평가의 과정은 많은 시간이 소요됨

평가 정확성을 유지한 채 효율적으로 평가할 수 있는 알고리즘들이 제안됨.

---

### Search Space

일반적인 convolution, pooling, activation, skip connection, concatenation, addition 등의 operation과 이전 연구들에서 성능이 입증된 separable convolution, dilated convolution, sequeeze-and-excitation block 등이 사용됨.

#### (1) Entire-structured Search Space

Operation 단위로 architecture를 설계할 수 있음

사전 정의된 node(operation)들로 architecture를 구성

가장 직관적이고 많은 모델을 탐색할 수 있음

깊은 모델을 탐색하기에는 계산 비용이 큼

탐색된 모델의 전이 가능성(transferability)가 낮음

Small dataset으로 탐색한 모델이 large dataset에는 적합하지 않을 수 있음

#### (2) Cell-Based Search Space

동일한 cell 단위로 구성된 architecture를 사용한 다음 cell을 optimize하는 방

VGG, ResNet, Inception등에서 사용되는 방식

단순한 모델에서 cell을 추가하는 것으로 깊은 모델을 생성할 수 있음

EfficientNet에서 증명된 방식

Search -> Estimation의 2-stage 방식이 많이 사용됨

P-DARTS에선 stack된 cell을 점차 늘려가는 방식으로 깊은 모델을 탐색

#### (3) Hierarchical Search Space

Cell level과 Network level을 모두 고려한 architecture를 사용

반복된 cell 형태를 사용하고 layer마다 cell 내부 구조를 달리함

모델이 깊어질 수록 다양한 cell을 효율적으로 탐색하기 위한 방법들이 제안됨

HierNAS : Higher-level cell은 lower-level cell을 조합하여 구성

Progressive NAS : 간단한 cell을 먼저 탐색한 후, 점차 block을 추가하고 탐색하는 방식으로 Higher-level cell을 탐색함

#### (4) Morphism-Based Search Space

Function은 유지하고 architecture의 형태만 변형

Net2Net : 기존 모델 구조를 활용해 Deeper Net, Wider Net를 설계함

Network morphism : Knowledge distillation을 활용하기에 Depth, width, kernel size등을 한번에 morphing 할 수 있음

EfficientNet : CNN의 depth, width, resolution을 적절한 비율로 구성하는 것이 효율적임을 입증

---

## Architecture Optimization

정의된 search space 내에서 가장 성능이 좋은 신경망 구조(architecture)를 탐색하는 과정

### (1) Evolutionary Algorithm

### (2) Reinforcement Learing

NAS  
RNN Agent를 이용하여 architecture를 설계함  
Validation accuracy를 강화학습의 reward로 하여 더 나은 모델 탐색  

Agent : 학습하는 대상  
State : 현재 상황  
Environment : Agent 제외 요소  
Reward : 보상  
Action : 선택  

### (3) Gradient Descent

DARTS (Differentiable Architecture Search)

Softmax function을 사용해 search space를 continuous, differentiable 하도록 함

### (4) Surrogate Model-Based Optimization

입력 architecture의 성능을 예측하는 surrogate function을 사용함

### (5) Grid and Random Search

### (6) Hybrid Optimization Method

---

## 5. Model Estimation

(1) Low fidelity

(2) Weight Sharing

(3) Surrogate

(4) Early stopping

(5) Resource-aware

---

## 6. NAS Perfomance Summary

(내용 그대로)

---

## 7. Open Problems and Future Work

(1) Flexible Search Space  
(2) Applying NAS to More Areas  
(3) Interpretability  
(4) Reproducibility  
(5) Robustness  
(6) Joint Hyperparameter and Architecture Optimization  
(7) Complete AutoML Pipeline  
(8) Lifelong Learing  

---

https://youtu.be/z3dIbE586JE?si=wKuGKVgMWFhzWJKv
