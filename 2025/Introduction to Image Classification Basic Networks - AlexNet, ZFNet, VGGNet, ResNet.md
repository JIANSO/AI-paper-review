AlexNet (2012)

“현대 CNN의 시작점”

​

핵심 특징

ReLU: Sigmoid/Softmax보다 gradient vanishing 해결

Dropout: 과적합 크게 감소

Data Augmentation 사용

큰 Convolution 필터 (11×11, 5×5)

CNN에서 “11×11”, “5×5”, “3×3” 같은 필터 크기(kernel size)는 단순한 숫자가 아니라, 네트워크가 이미지를 어느 ‘영역 단위’로 바라보는지를 결정하는 핵심 요소다. 각 숫자는 필터(커널)의 가로/세로 픽셀 크기를 의미한다.

​

두 개의 GPU로 병렬 학습

​

왜 중요한가?

→ 딥러닝이 “실제로 대규모 이미지 분류에서 기존 SVM/전통기법을 박살낸 첫 사례”

ZFNet (2013)

“CNN 내부를 본 최초의 연구 / 구조 개선은 약간”

​

핵심 기여

CNN 필터가 무엇을 학습하는지 Deconvolution(역컨볼루션)으로 시각화

AlexNet의 첫 Conv를

11×11 → 7×7

stride 4 → stride 2

로 바꿔 성능 개선

​

왜 중요한가?

→ “CNN 설계는 직관이 아니라 시각화 기반으로 해야 한다”는 것을 처음으로 증명

VGGNet (2014)

“아키텍처 디자인의 표준을 제시”

​

핵심 아이디어

3×3 Convolution만 반복

(심플·일관적)

네트워크를 깊게 만들수록 성능이 좋아짐

VGG-16, VGG-19(레이어 수)

​

왜 중요한가?

→ “CNN 구조는 간단할수록 좋고, 깊게 쌓는 것이 핵심”이라는 철학 확립

→ 이후 거의 모든 모델이 VGG 구조 철학을 따름

ResNet (2015)

“초심층 네트워크 학습의 혁명”

​

문제

깊게 만들수록 성능이 떨어지는 Degradation Problem이 발생함

(Overfitting이 아니라 Gradient 흐름이 무너짐)

​

해결책: Residual Block

F(x) + x

입력 x를 출력에 그대로 더해줌(skip connection)

gradient가 역전파 시 직접 흘러가는 통로를 열어줌

​

왜 중요한가?

34, 50, 101, 152층까지 학습 성공

ResNet은 지금도 backbone으로 가장 널리 사용됨

AlexNet → ZFNet → VGG → ResNet
   |        |        |       |
   |        |        |       +-- Skip Connection (깊은 네트워크의 시대)
   |        |        +---------- Simple & Deep CNN 구조 확립
   |        +-------------------- CNN 내부 시각화
   +----------------------------- 딥러닝 부흥의 시작
History of CNN architectures for image recognition


​

MLP의 문제점을 해결하는 CNN 등장


​

AlexNet

2개의 GPU 병렬연산

Max Pooling : Strid 2를 주어 윈도우가 겹치도록 풀링 수행

ReLu 함수

Dropout

Augmentation


ZFNet

이미 존재하는 CNN을 가시화하여 개선방향을 파악하고자 하는 모델

Unpooling시 스위치 활용

AlexNet에서 11x11 필터를 ZFNet에서 7x7 필터로 변경하여 사용



​

VGGNet

CNN 학습 능력 확장을 입증함.

더 깊은 레이어


ResNet

56 -layer 네트워크가 20-layer 네트워크보다 왜 성능이 나쁠까?



Residual Shortcut 추가


며칠 걸쳐서 보는 중-

​

[1] 발표자: DSBA 연구실 석사과정 정의석 

[2] 발표 논문 : 본 발표는 Image Classification Task의 기반이 되는 논문 4개를 소개합니다. 

AlexNet : ImageNet Classification with Deep Convolutional Neural Networks 

(https://papers.nips.cc/paper/2012/fil...) 

ZFNet : Visualizing and Understanding Convolutional Networks 

(https://arxiv.org/abs/1311.2901) 

VGGNet : Very Deep Convolutional Networks for Large Scale Image Recognition 

(https://arxiv.org/abs/1409.1556) 

ResNet : Deep Residual Learning for Image Recognition 

(https://arxiv.org/abs/1512.03385)

​

https://youtu.be/vRtM4K8e_Q4?si=n8DsG_nJjQI1LRSC


ZFNet 설명에 대한 자료

https://medium.com/apache-mxnet/transposed-convolutions-explained-with-ms-excel-52d13030c7e8