D. P. Kingma and J. Ba, “Adam: A method for stochastic optimization,” in Proc. 3rd Int. Conf. Learn. Representations (ICLR), San Diego, CA, USA, 2015. [Online]. Available: https://arxiv.org/abs/1412.6980

---

## Overview

Background

Related Works

Methodology

Experiments

Conclusion

---

## 1.Overview

---

## 2.Background

First Order optimization vs Second order optimization

### First Order optimization

한 번 미분한 weight만 Gradient에 반영

Gradient 수정이 제한적이지만 계산이 효율적

### Second order optimization

두 번 이상 미분을 해서 Gradient에 반영

데이터에 따라 효율적일 때도 있지만 대부분의 경우 시간 복잡도가 엄청나게 큼

### Exponential Moving Average

Exponential Moving Average

최근 데이터에 가중치를 두어 평균을 내는 방법

베타값을 조절함으로써 최근 데이터의 반영 정도를 조정할 수 있다.

---

## 3.Related Works

Optimization overview

### Stochastic Gradient Descent(SGD)

Data를 random sampling하여 Gradient Descent 수행

기존의 Gradient Descent 보다 효율적이지만, random sample의 특성 때문에 수렴 속도가 느리다는 단점이 있음.

지그재그로 수렴하는 그래프 특성

### Momentum

이전 시점의 Gradient들을 exponential moving average 방식으로 학습에 반영함

Momentum의 영향으로 인해 global optimum으로 수렴하지 못할 가능성이 있음

Learning rate(η) 가 고정되어 있음

### Adagrad

SGD와 달리 parameter별로 leaning rate를 다르게 적용함.

Frequent parameter -> Small update

Infrequent parameter -> Large update

단점으로 학습이 진행될수록 learning rate가 0에 수렴. t가 커지면 Gt가 커짐.

### RMSProp

Adagrad와 달리 exponential moving average 방식을 적용하여 최근 gradient에 더 높은 가중치를 부여함

분모의 Gt에 exponential moving average 방식을 적용

Learning rate가 0으로 수렴하지 않음

---

## 방법

기억 방식

비유

Adagrad  
모든 과거 기울기 누적  
매번 시험 본 점수를 전부 평균 내는 사람 (점점 변화가 둔해짐)

RMSProp  
최근 기울기만 조금씩 남김  
최근 몇 번의 점수만 참고하는 사람 (변화에 민감함)

---

## 4.Methodology

Adam Algorithm

기호 | 이름 | 역할 | 비유
-----|------|------|------
mₜ | 1차 모멘트(First Moment) | 기울기의 이동 평균 (Momentum 역할) | “속도” — 지금까지의 방향을 부드럽게 이어감
vₜ | 2차 모멘트(Second Moment) | 기울기의 제곱 이동 평균 (RMSProp 역할) | “진동 크기” — 얼마나 출렁이는지 추적함

Adam Update Rule


Initialization bias correction

Extensions : Adamax

Adam의 second momentum에 대한 term을 L2놈이 아닌 인피니티 놈으로 바꿈

---

## 5.Experiments

### Experiment : Logistic Regression

아래 왼쪽 그래프의 AdaGrad는 학습률만 조정하기 때문에, 방향이 같다면 수렴이 느리다는 것을 보여준다.

아래 오른쪽 그래프에서는 SGD가 확연하게 수렴이 느리다.

모든 실험에서 Adam 성능이 제일 높다.

### Experiment : Multi-layer Neural Networks



### Experiment : Convolutional Neural Networks


### Experiment : Bias-Correction Term



---

## 6.Conclusions

단순하고 계산 효율성이 높은 알고리즘 (fisrt order optimization algorithm)

AdaGrad(sparse한 gradient에 적합)와 RMSProp(non-stationary object에 적합)의 장점을 결합

적은 메모리 요구량

Non-convex optimization problem에서도 잘 작동하는 robust한 optimizer

---

https://youtu.be/sIjVu2xnTfI?si=fknD0g9F82FOkG3S

https://arxiv.org/abs/1412.6980?utm_source=chatgpt.com
