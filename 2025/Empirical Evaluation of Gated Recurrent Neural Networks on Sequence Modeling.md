J. Chung, C. Gulcehre, K. Cho, and Y. Bengio, “Empirical Evaluation of Gated Recurrent Neural Networks on Sequence Modeling,” arXiv preprint arXiv:1412.3555, 2014.

---

## Abstract 

In this paper we compare different types of recurrent units in recurrent neural networks (RNNs). Especially, we focus on more sophisticated units that implement a gating mechanism, such as a long short-term memory (LSTM) unit and a recently proposed gated recurrent unit (GRU). We evaluate these recurrent units on the tasks of polyphonic music modeling and speech signal modeling. Our experiments revealed that these advanced recurrent units are indeed better than more traditional recurrent units such as tanh units. Also, we found GRU to be comparable to LSTM.

이 논문은 LSTM만큼 잘 되냐?”를 실험적으로 검증한, RNN 계열에서 매우 유명한 경험적 비교 연구(benchmark paper)이다. 한 문장으로 말하면 LSTM과 GRU 같은 “게이트 기반 RNN"이 전통적 RNN보다 훨씬 좋고, GRU는 LSTM과 거의 동등한 성능을 낸다를 실제 실험으로 증명한 논문이다.

---

## 연구 목적

게이트(gate)를 사용하는 RNN(LSTM, GRU)이 정말로 기존 RNN보다 좋은가?

LSTM과 GRU 중 무엇이 더 나은가?

GRU는 LSTM보다 구조가 간단한데, 성능도 비슷하거나 좋은가?

---

## 실험한 데이터셋 / 과제(Task)

논문은 두 가지 시퀀스 분야에서 비교함.

1) Polyphonic Music Modeling  

음악을 시퀀스로 보고, 다음 음을 예측하는 작업  

다양한 악기 음성의 시간적 패턴을 학습해야 해서 시퀀스 학습 난이도가 높음  

2) Speech Signal Modeling  

음성 신호를 시퀀스로 보고, 신호의 다음 상태를 예측하는 문제  

시간/주파수 패턴을 잡아야 해서 역시 RNN 성능 차이가 잘 나타남  

---

## 결과 (핵심 결론)

1) LSTM vs GRU vs tanh RNN 비교  

LSTM과 GRU는 전통적 RNN(tanh)보다 훨씬 우수함  

긴 시퀀스 패턴 학습에 있어서 게이트 구조가 필수적임을 보여줌  

2) LSTM vs GRU  

GRU는 LSTM과 거의 동등한 성능  

어떤 데이터셋은 GRU가 더 좋고, 어떤 데이터셋은 LSTM이 약간 더 좋음  

전반적으로 둘 다 잘 됨  

3) GRU의 장점  

구조가 더 심플함(파라미터 수 적음)  

학습 속도 빠름  

그럼에도 성능은 LSTM급  

이 논문이 나오고 나서 GRU가 RNN 분야에서 엄청 유명해짐.

---

## 이 논문의 의의

당시 GRU는 새로 나온 구조였는데,[실제로 LSTM을 대체할 수 있다] 는 강력한 근거가 된 논문.

RNN 구조 비교 연구의 대표적인 실험 논문이라 딥러닝 교재·강의·리뷰 논문에서도 자주 인용됨.

---

## 1.Approach

RNN이 무엇이고, LSTM, GRU가 무엇인지 파악, 장단점들 파악

RNN의 구조, LSTM의 구조를 하나 하나씩 뜯어보며 다른 구조가 왔을 때 이번 경험을 토대로 그림을 쉽게 그리고 쉽게 이해할 수 있도록 하는 것 

모델의 세부적인 구조 이해

각 단계별 input의 구조

---

## 2.Introduction

RNN

(input + prev_hidden) -> hidden -> output

RNN의 단점

Back propagation할 때의 Gradient vanishing problem 

Long-term dependency를 capture하기 힘듦

이를 해결하기 위해 more sophisticated activation function(일명 'Gated Recurrent Neural Network)

Gate : 선택적으로 정보를 추가하거나 제외시키게 하는 장치

Ex. LSTM, GRU

---

## 3.Methodology

Gated Recurrent Neural Network

---

### 3.1 Long Short-Term Memory Unit 

LSTM의 모든 게이트(i, f, o)는 “cell state(c_t)”를 업데이트하고 관리하기 위한 장치들이다.

LSTM은 크게 3개 정보가 있음

1.이전 memory  

$​\combi{c}_{t−1}$​ct−1​​  

2.후보 memory  

$\tilde{\combi{c}_{t​}}=\tanh (\combi{W}_c​\combi{x}_t​+\combi{U}_c​\combi{h}_{t−1}​)$  
~  
ct​​=tanh(Wc​​xt​​+Uc​​ht−1​​)​  

3.게이트 (f_t, i_t)

게이트들의 정확한 역할 요약

LSTM에는 세 가지 게이트가 있음

1) Forget gate  

이전 cell state c_{t-1} 를 얼마나 유지할지(=얼마나 지울지) 결정  

$​\combi{c}_{t−1}\to \times ft​​\ \to 일부만\ 유지$​ct−1​→×ft​​ →일부만 유지​  

2) Input gate  

새로운 정보를 cell state에 얼마나 추가할지 결정  

$\tilde{\combi{c}_t}​\to \times \combi{i}_t\to ​​일부만\ 추가$  
~  
ct​​→×it​→​​일부만 추가​  

3) Output gate  

업데이트된 cell state 를 외부 출력(h_t)로 얼마나 노출할지 결정  

$​\combi{h}_t=\combi{o}_t⋅\tanh (\combi{c}_{t​}​)$​ht​=ot​⋅tanh(ct​​​)​  

    입력 x_t                 이전 h_{t-1}
       │                        │
       └─── W_c, U_c ───┬───────┘
                        ▼
                tanh() =  후보 c̃_t   →→→ i_gate로 조절 →→+
                                                               |
이전 memory c_{t-1} →→ f_gate로 조절 →→─────────────────────── +
                                                             ▼
                                                   최종 memory c_t

---

### 3.2 Gated Recurrent Unit 

GRU는 LSTM과 같은 기능을 훨씬 간단한 구조로 구현한 버전이다. LSTM의 핵심은 cell state를 게이트(i, f)로 조절하는 것.

GRU는 cell state가 없지만 대신

reset gate  

update gate  

candidate activation  

을 사용해서 “이전 상태 유지/삭제”, “새로운 정보 추가”를 hidden state 하나만으로 해결한다.

즉, GRU = LSTM의 기능을 hidden state 안에 다 넣은 간소화 버전이다.

GRU는 한 타임스텝에서 두 단계 계산을 한다.

(1) 후보 hidden state(새 정보) 를 만든다 → 

$\tilde{\combi{h}_{t​}}=\tanh (W\combi{x}_{t​}+U(\combi{r}_t⊙\combi{h}_{t−1​}​))$  
~  
ht​​=tanh(Wxt​​+U(rt​⊙ht−1​​​))​  

(2) 게이트 z_t(업데이트 게이트) 로 이전 h와 새로운  

$\tilde{h}_t$  
~  
ht​​  

를 섞어 최종 h_t를 만든다 → 

$\combi{h}_{t​}=(1−\combi{z}_t​)\combi{h}_{t−1}​+\combi{z}_t​\tilde{\combi{h}_{t​}}$ht​​=(1−zt​​)ht−1​​+zt​​  
~  
ht​​​  

          입력 x_t                     이전 h_{t-1}
             │                                │
             ├───────────┬────────────────────┘
             │           │
     (update gate z_t)   │
     (reset gate r_t)    │
             │           │
             ▼           ▼
        z_t = σ(...)   r_t = σ(...)
             │           │
             │           └───────────────┐
             │                           │
             ▼                           ▼
      -----------------        -------------------------
      |  후보 hidden   |  ←─  |  r_t ⊙ h_{t-1} (이전 h 필터링) |
      |   h̃_t = tanh(  |      -------------------------
      |   W x_t  +      |
      |   U (r_t ⊙ h) ) |
      ------------------
                 │
     (후보 h̃_t를 update gate로 조절)
                 │
                 ▼
       z_t ⊙ h̃_t    →→→→→→→→→→→→→→→→+
                                        |
(이전 h 유지량)   (1 - z_t) ⊙ h_{t-1} →→→→+
                                        ▼
                              최종 h_t (다음 단계로 전달)

Step 1. reset gate r_t 계산  

$r_t=\sigma (W_rx_t+U_rh_{t-1})$rt​=σ(Wr​xt​+Ur​ht−1​)​  

Step 2. update gate z_t 계산  

$z_t=\sigma (W_zx_t+U_zh_{t-1})$zt​=σ(Wz​xt​+Uz​ht−1​)​  

Step 3. 후보 hidden h̃_t 계산  

$\tilde{h}_t=\tanh (Wx_t+U(r_t\odot h_{t-1}))$  
~  
ht​=tanh(Wxt​+U(rt​⊙ht−1​))​  

Step 4. 최종 hidden h_t 계산  

$h_t=(1-z_t)h_{t-1}+z_t\tilde{h}_t$ht​=(1−zt​)ht−1​+zt​  
~  
ht​​  

---

## 4.Experiments Setting 

(내용 그대로)

---

## 5. Results and Analysis

Negative Log-Likelihood(NLL)은 모델이 예측을 얼마나 잘하는지를 나타내는 loss(손실) 값이고, 딥러닝에서 loss는 낮을수록 성능이 좋다.

그래프 상단의 “Per epoch” vs 하단 “Wall Clock Time”

(a) Per epoch  

단순히 학습 에폭 기준으로 비교  

같은 에폭 동안 LSTM/GRU가 tanh보다 훨씬 더 loss 낮아짐  

즉, 효율적으로 학습함  

(b) Wall Clock Time  

실제 시간 기준 비교  

LSTM/GRU는 한 번 업데이트에 시간이 조금 더 걸리지만  

loss가 훨씬 빠르게 떨어짐 → 실제 시간 기준으로도 tanh보다 훨씬 우수  

특히 Ubisoft Dataset B를 보면  

tanh는 거의 변화 없음  

GRU는 빠르게 내려가서 최종적으로 가장 좋음  

LSTM은 tanh보다는 좋지만 GRU보다 느림  

---

## 6. Conclusion

In this paper we empirically evaluated recurrent neural networks (RNN) with three widely used recurrent units; (1) a traditional tanh unit, (2) a long short-term memory (LSTM) unit and (3) a recently proposed gated recurrent unit (GRU). Our evaluation focused on the task of sequence modeling on a number of datasets including polyphonic music data and raw speech signal data. The evaluation clearly demonstrated the superiority of the gated units; both the LSTM unit and GRU, over the traditional tanh unit. This was more evident with the more challenging task of raw speech signal modeling. However, we could not make concrete conclusion on which of the two gating units was better. We consider the experiments in this paper as preliminary. In order to understand better how a gated unit helps learning and to separate out the contribution of each component, for instance gating units in the LSTM unit or the GRU, of the gating units, more thorough experiments will be required in the future. 

https://youtu.be/5Ar1aN9gceg?si=Fw6QCvJ1pMihpLaZ
