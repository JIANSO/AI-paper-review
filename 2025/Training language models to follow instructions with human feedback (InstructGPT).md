L. Ouyang, J. Wu, X. Jiang, D. Almeida, C. L. Wainwright, P. Mishkin, C. Zhang, S. Agarwal, K. Slama, A. Ray, J. Schulman, J. Hilton, F. Kelton, L. Miller, M. Simens, A. Askell, P. Welinder, P. Christiano, J. Leike, and R. Lowe,  
"Training language models to follow instructions with human feedback,"  
arXiv preprint arXiv:2203.02155, 2022,  
doi: 10.48550/arXiv.2203.02155.

---

## 1.Background

GPT-3

In-context learning

모델이 task에 대한 정보를 참고해서 추론할 수 있도록 input에 예제(demonstrations)추가

one-shot, few-shot

GPT-3의 근본적인 문제점

misaligned

사실을 지어내거나, 편향적이거나, 유해한 텍스트를 생성하거나, 사용자의 지시를 따르지 않는 경우

본 논문의 목표 : Alignment

---

## 2. 제안 방법론 : InstructGPT

목표 : GPT-3 Fine-tuning

인간의 피드백을 통한 강화학습(Reinforcement Learning with Human Feedback, RLHF)으로 광범위한 지시사항에 따를 수 있도록 GPT-3를 fine-tuning

인간의 평가(선호도)를 reward로 활용

Fine-tuning을 위한 데이터 구축을 위해 40명의 labeler 고용

평가는 연구에 참여한 저자들과 labeler들로 한정됨

---

## 3.Evaluation

(내용 없음 – 원본 그대로)

---

## 4.ChatGPT

InstructGPT를 계승

PPO(Proximal Policy Optimization)

사람의 피드백으로 강화학습하는(RLHF, Reinforcement Learning from Human Feedback) 과정의 핵심 단계

기본 개념

기존의 강화학습 알고리즘을 업데이트할 때, 너무 큰 업데이트를 하면 학습이 불안정해짐. PPO는 이를 막기 위해 Clipping(잘라내기) 도입.

PPO의 목적함수

rt​(θ) : 새 정책과 옛 정책의 비율  
At : 어드밴티지(현재 행동이 평균보다 얼마나 좋은가)  
𝜖 : 클리핑 한계 (보통 0.1 ~ 0.3)  

즉 새 정책이 이전 정책보다 너무 멀리 벗어나면(Clipping), 업데이트를 제한해서 안정적으로 학습하게 함.

InstructGPT에서 PPO가 쓰이는 방식

Supervised Fine-Tuning(지도학습)

사람이 작성한 예시(QA Pairs)로 모델을 먼저 파인튜닝함

기본적인 문장 이해 & 응답 능력을 학습함

Reward Model 학습

여러개의 모델 응답 중 '어떤 것이 더 좋은지' 사람이 비교(labeling)

그 데이터를 학습해 보상 모델(Reward Model, RM)을 만듦

RM은 문장 하나에 사람이 선호할 확률을 예측함

PPO 강화학습

GPT가 답변을 생성하고 RM이 보상을 줌

PPO 알고리즘이 정책(모델 파라미터)를 업데이트 함

즉, RM이 사람이 좋아할 만한 답변을 강화하도록 모델을 훈련시킴.

---

https://youtu.be/vx3hxa9Bi5c?si=5SSOwAiRGLk275Ss
