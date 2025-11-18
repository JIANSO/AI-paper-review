# Long Short-Term Memory (LSTM) 논문 리뷰

S. Hochreiter and J. Schmidhuber,  
"Long Short-Term Memory,"  
in Neural Computation, vol. 9, no. 8, pp. 1735-1780, 15 Nov. 1997,  
doi: 10.1162/neco.1997.9.8.1735.

---

## Abstract

Learning to store information over extended time intervals by recurrent backpropagation takes a very long time, mostly because of insufficient, decaying error backflow. We briefly review Hochreiter's (1991) analysis of this problem, then address it by introducing a novel, efficient, gradient based method called long short-term memory (LSTM). Truncating the gradient where this does not do harm, LSTM can learn to bridge minimal time lags in excess of 1000 discrete-time steps by enforcing constant error flow through constant error carousels within special units. Multiplicative gate units learn to open and close access to the constant error flow. LSTM is local in space and time; its computational complexity per time step and weight is O. 1. Our experiments with artificial data involve local, distributed, real-valued, and noisy pattern representations. In comparisons with real-time recurrent learning, back propagation through time, recurrent cascade correlation, Elman nets, and neural sequence chunking, LSTM leads to many more successful runs, and learns much faster. LSTM also solves complex, artificial long-time-lag tasks that have never been solved by previous recurrent network algorithms.

---

## 시계열 데이터를 다루는 다양한 방법들

- Data Approach  
- Model Approach  
- Stochastic  
- Statistics  
- Deep Learning  
  - Vanilla RNN  
  - LSTM  
  - GRU  
  - Transformer  
  - TCN  
  - Neural ODE  
  - LTFS-Linear  

---

## 02. 이전 연구

### LSTM의 근본이 되는 RNN(Recurrent Neural Network)

- Vanila RNN  
- fully connected network와 비슷  

### 내부

- BPTT(Back Propagation Through Time)

### 기존 RNN 구조의 단점

#### 문제 현상
긴 time step에 대해 학습이 이루어지지 않음  
(보통 time-step이 10 이상이면 성능 저하)

#### 문제 원인: Vanishing Gradient and Exploding Gradient
BPTT 활용으로 인해 나타남

1) 긴 Time Dependency를 갖고 있을 경우,  
오차 역전파시 동일한 행렬곱(Wh)의 반복으로 인해  
Wh 값의 특이값에 따라  
- 1보다 작은 값을 가질 경우 → Gradient Vanishing  
- 1보다 큰 값을 가질 경우 → Exploding  

2) Activation Function인 tanh을 미분했을 경우  
값이 1보다 작기 때문에 Vanishing 될 확률이 높음.

---

## 03. 방법론

### LSTM (Long Short-Term Memory)

- 하나의 Unit에서 계산된 정보(Short-Term)를 길게(Long) 전파하는 Memory 구조를 제안함  
- 기존 RNN Unit 보다 Vanishing/Exploding Gradient를 막으며, 긴 Time Step을 학습 가능하게 함.

---

## 3가지의 Contribution

### 1. 장기기억 Cell 구조 도입
- 장기기억 Cell인 CEC(Constant Error Carousel)을 도입하여 Vanishing Gradient를 막음

### 2. Gate 구조의 도입
- Gate를 사용하여 학습 기반으로, 자동으로 Input, Output값에 대한 제어  
- (Original LSTM에는 Forget Gate가 없음)

### 3. Training Method 개선
- BPTT(Back Propagation Through Time)과  
  RTRL(Real-Time Recurrent Learning)의 Variation을 동시에 학습 사용

---

## Current Vanilla LSTM

- 최종적으로 현재의 Vanilla LSTM 구조는 아래와 같고, 우측은 계산 수식이다.  
- 학습 알고리즘은 Fully BPTT를 사용하고 있다.  
- 현재 구조는 **약 1,000 step까지 학습 가능**한 것으로 알려져 있다.

---

## 05. 결론

- LSTM : Long Short-Term Memory  
- RNN 구조에 Cell State(CEC)라는 Identity Mapping을 도입하여  
  긴 Step Time-Series Data에서 Gradient를 안정적으로 학습할 수 있는 구조를 제안  
- 이후 연구된 Deep Learning 구조들에 큰 영향을 미침  
- Gate 구조(Input Gate, Output Gate)를 통해  
  데이터의 입출력 비율을 학습 기반으로 자동 조절  
- 실험을 통해 Short Time Step 및 Long Time Step 데이터 모두에서  
  이전 연구보다 더 좋은 성능을 나타냄  
- Original LSTM은 이후 Forget Gate 추가 + Fully BPTT 방식으로 발전하여  
  현재의 LSTM 모델 구조로 확립됨  
- RTRL + BPTT로 학습한 방법에 대한 이론적 정당성은 일부 부족함

---

### 참고 영상

https://www.youtube.com/watch?v=IgIHjiCgECw&list=PLetSlH8YjIfUpPbSAfsY4zBJfztlH9CSQ&index=1
