U-Net 논문 리뷰는 의료 영상 분할(Biomedical Image Segmentation) 분야에서 혁신을 가져온 u-net 아키텍처의 핵심 원리와 실용적 적용 방안을 깊이 있게 해설합니다. 이 콘텐츠를 통해 독자는 u-net이 기존 cnn의 한계(슬라이딩 윈도우 방식의 비효율성)를 어떻게 극복하고, 대칭적인 인코더-디코더 구조(Contraction Path & Expansive Path)를 통해 정확한 로컬라이제이션을 달성하는지 명확히 이해할 수 있습니다. 특히, 적은 데이터로도 높은 성능을 내기 위한 미러링 기법을 활용한 오버랩 타일 전략과 Elastic Deformation 기반의 데이터 증강(Data Augmentation) 기법 등, 실제 바이오메디컬 데이터셋에 적용 가능한 구체적인 최적화 전략을 배울 수 있습니다.

💡U-Net은 생체 의학 이미지 분할을 위해 어떤 방식으로 작동하는가?

u-net은 대칭적인 인코더-디코더 구조를 가지며, 컨볼루션과 풀링을 통해 특징을 추출하고, 업샘플링과 스킵 커넥션을 통해 정확한 분할을 수행합니다.

💡 U-Net이 기존 슬라이딩 윈도우 방식보다 효율적인 이유는?

u-net은 오버랩 타일 전략을 사용하여 이미 검증된 영역을 다시 검증하지 않고, 패딩 없이 컨볼루션을 수행하여 시간과 연산 측면에서 우위를 가집니다.

---

# Table of Contents

1. Review of CNN and Sliding Window Technique  
2. Architecture of U-net : Contraction Path  
3. Architecture of U-net : Expansive Path  
4. Data Augmentation Method  
5. Training Modeling & Experiments  

---

## 1. Review of Convolutional Neural Networks(CNN)

Convolutions Layer + Subsampling(Pooling) Layer : 이미지 특징을 추출 및 축약

Fully Connected Layer : 추출된 특징을 사용하여 입력에 속하는 범주 분류

Review of Convolution Layer

U-Net의 경우 Fully Connected Layer가 없기 때문에 Convolution Layer에 대해만 짚고 넘어간다.

Review of Pooling Layer(2x2 filter)

풀링층은 피처맵을 다운샘플링하여 특성 맵의 크기를 줄여주는 풀링 연산을 한다.

주로 max pooling과 average pooling을 한다.

Review of Sliding Window Technique

Sliding Window : 사진을 윈도 사이즈에 맞춰 나눈 다음, 매 윈도우로 잘린 이미지를 입력 값으로 모델을 통과해서 결과를 얻는 방법. 이미지 하나하나가 CNN에 입력으로 들어감.

---

## 2. Architecture of U-net : Contraction Path + Expansive Path

Contractin Path

이미지의 feature 맵에서 컨텍스트를 포착하여 도출하는 역할을 수행한다.

Expansive Path

포착된 피처 맵들을 다시 업샘플링하여 좀 더 정확한 Localization을 수행할 수 있도록 돕는 역할을 한다.

아래 그림에 빨간색 대신 초록색의 upconvolution이라고 불리는 블럭들이 들어감.

Contraction Path에서 줄어든 사이즈를 업샘플링하여 컨볼루션을 통과하게 함.

아래 그림의 회색 블록은, 좌측 Contraction Path의 피처를 카피해서 우측에 보내는 것이다. 중앙 부분을 알맞은 사이즈로 cropping해서 보낸다. 가장자리 손실에 대해 보정하는 것이다.

---

## 3. U-net : Improved Sliding Window Search Method

U-net은 윈도우 슬라이딩 방식을 사용하지 않는다. 검증이 끝난 부분은 다시 검증을 하지 않아 속도와 연산 측면에서 우위를 가질 수 있다.

---

## 4. U-net : Overlap tile Method(Strategy)

패딩을 수행하지 않고 컨볼루션을 수행하기 때문에, 출력 크기가 입력 크기보다 항상 작다.

오버랩 타일 전략 채택

이미지를 자르면 경계선 부근의 픽셀은 컨볼루션 receptive field가 작아서 정보 손실이 생김. 즉, 자른 조각의 가장자리 부분은 정확한 세그멘테이션이 어려움.

이를 해결하기 위해 타일 간에 겹치는 영역(Overlap)을 두고 추론

- 경계부 세그멘테이션 품질 향상  
- 패딩(padding)이나 경계 왜곡 문제 완화  

미러링 기법을 통한 Missing Value 처리

예를 들어, 256×256 타일을 뽑을 때 끝 부분이 이미지 밖으로 넘어가면 그 바깥쪽 픽셀의 값이 “비어 있는(missing)” 상태가 됨

이때 이미지의 경계를 거울처럼 반사(reflect) 하여 결측 부분을 채움.

- 인위적인 0-padding보다 자연스러운 경계 처리  
- 경계 근처 세그멘테이션 정확도 향상  
- Missing Value 문제 해결  

---

## 5. Data Augmentation Method

Basic

Elastic Deformation

픽셀별로 이미지가 다른 방향으로 뒤틀리게 됨. 자연스럽고 현실세계에 있을 법한 변화여서 의료 데이터에 적합하다고 볼 수 있음.

---

## 6. Train

Stochastic Gradient Descent(SGD)을 이용하여 학습

Pixel-wise Loss Function

이미지의 각 픽셀 단위로 예측값과 정답을 비교하여 손실을 계산하는 방법

아래는 segmentation을 위해 사용되는 식

---

## 7. Experiments

(원문 그대로 — 상세 없음)

---

https://youtu.be/O_7mR4H9WLk?si=NtQ7CgE12E9DVG5k
