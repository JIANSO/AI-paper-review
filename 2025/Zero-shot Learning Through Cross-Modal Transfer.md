R. Socher, M. Ganjoo, H. Sridhar, O. Bastani, C. D. Manning, and A. Y. Ng,

“Zero-Shot Learning Through Cross-Modal Transfer,”

Proc. Advances in Neural Information Processing Systems (NIPS),

vol. 26, pp. 935–943, 2013.

​

Abstract 

This work introduces a model that can recognize objects in images even if no training data is available for the objects. The only necessary knowledge about the unseen categories comes from unsupervised large text corpora. In our zero-shot framework distributional information in language can be seen as spanning a semantic basis for understanding what objects look like. Most previous zero-shot learning models can only differentiate between unseen classes. In contrast, our model can both obtain state of the art performance on classes that have thousands of training images and obtain reasonable performance on unseen classes. This is achieved by first using outlier detection in the semantic space and then two separate recognition models. Furthermore, our model does not require any manually defined semantic features for either words or images.

1. Introduction


세상에는 라벨이 없거나 새롭게 등장하는 시각적 객체가 너무 많기 때문에, 본 적 없는 클래스(unseen class)를 분류하는 zero-shot learning이 필요하다.이 논문은 인간이 “설명만 읽고” 새로운 물체의 모습을 대략 상상해 인식할 수 있는 것처럼, 자연어가 담고 있는 세계 지식을 활용해 unseen 이미지를 분류하는 방법을 제안한다.

핵심은 두 가지이다.

첫째, 대규모 텍스트로 비지도학습된 단어 벡터(semantic word vectors) 가 단어 간 의미적 유사성을 잘 표현한다는 점에 착안하여, 이미지를 이 단어 의미 공간으로 투영하는 매핑(θ) 을 학습한다. 이렇게 하면 이미지와 단어가 동일한 의미 공간에서 비교될 수 있고, 이미 본 적 없는 클래스도 그 단어의 의미적 위치만으로 유추할 수 있게 된다.

둘째, 분류기가 원래는 본 적 있는 클래스(seen) 로만 예측하려는 경향이 있기 때문에, 새로운 이미지가 seen인지 unseen인지를 먼저 판단하는

outlier detection(이상치 탐지) 절차를 도입한다. 이미지가 seen 클래스들이 만드는 의미적 공간(semantic manifold) 안에 있으면 seen으로,

벗어나 있으면 unseen으로 분류하며, seen이면 일반 softmax 분류기, unseen이면 단어 벡터 기반 확률 모델로 분류한다.

이 두 절차는 하나의 베이지안 확률 모델로 통합된다. 이 접근은 기존 zero-shot 연구들이 “zero-shot 클래스들끼리 구분”만 가능하거나 “사람이 만든 속성(attribute)”에 의존했던 한계를 넘어, seen과 unseen을 동시에, 속성 없이, 자연어만으로 분류할 수 있는 모델을 제시한다. 실험에서는 CIFAR-10에서 이러한 방식이 실제로 잘 작동함을 보여준다.

2. Related Work

​

(1) Zero-Shot Learning

Palatucci: 의미 공간으로 매핑하지만 seen+unseen 동시 분류는 불가능

이 논문은 outlier detection으로 그것을 가능하게 만듦

​

(2) One-Shot Learning

적은 데이터로 학습

우리 모델은 데이터 0개인 unseen도 가능​

deep learning + probabilistic transfer 사용

​

(3) Knowledge / Attribute Transfer

다른 연구들은 사람 손으로 만든 속성(attribute)을 사용

우리는 pure word embeddings(비지도 텍스트) 만 사용​

​

(4) Domain Adaptation

도메인은 다르지만 각 클래스 데이터는 존재함​

우리의 setting과는 다름

​

(5) Multimodal Embeddings

이미지 + 텍스트 결합 연구들

우리는 이것을 확장해 zero-shot 분류까지 가능하게 함​

3. Word and Image Representations

​

*단어(word) 표현은 어떻게 만드나?

큰 텍스트(Wikipedia)를 가지고 어떤 단어가 어떤 단어와 자주 같이 나오나(co-occurrence)를 학습함.

이렇게 만들어진 분포 기반(distributional) 단어 벡터를 사용함. 

Huang et al.(2012)의 사전 학습된 50차원 word embedding을 그대로 가져옴.

이 임베딩은 문맥 정보를 반영해서 문법적 특징 + 의미적 특징을 모두 담고 있음. 

즉: 단어는 50차원 의미 벡터로 표현한다.

​

*이미지(image) 표현은 어떻게 만드나?

Coates & Ng의 비지도 알고리즘으로 원본 픽셀에서 특징을 자동으로 추출함.

그래서 각 이미지는 F 차원 특징 벡터 x로 표현됨.

즉: 이미지는 F차원(예: 12,800차원) 특징 벡터로 표현한다.

​

* 이 두 가지를 합쳐서?

단어 → 50차원 의미 공간

이미지 → F차원 특징 벡터

​

→ 이미지(F차원) → 단어 공간(50차원)으로 보내는 매핑 θ를 학습해서 이미지와 단어를 같은 의미 공간에서 비교할 수 있게 한다.

​

* 한 줄 총정리

단어는 50차원 의미 벡터로, 이미지는 F차원 특징 벡터로 표현한 뒤, 이미지를 단어 의미 공간에 투영해서 서로 비교할 수 있게 만드는 단계다.

4. Projecting Images into Semantic Word Spaces

​


​

이미지들 사이의 의미적 관계나 어떤 클래스에 속하는지를 학습하기 위해, 우리는 이미지 특징 벡터를 50차원 단어 벡터 공간(semantic word space)으로 투영한다. 훈련 및 테스트 과정에서는 클래스들의 집합 Y를 고려한다. 이 클래스들 중 일부 y 는 훈련 이미지가 존재하고, 또 다른 일부는 아예 훈련 이미지가 없는 zero-shot 클래스이다. 훈련 이미지가 있는 클래스들을 seen 클래스 Y_s 라고 하고, 훈련 이미지가 없는 클래스들을 unseen 클래스 Y_u 라고 한다. 또한 단어 벡터 집합 

$W=W_s\cup W_u$W=Ws​∪Wu​​
각 클래스 이름에 해당하는 단어 벡터(word vectors) 의 집합인데, 이는 모두 대규모 텍스트에서 비지도학습으로 얻은 의미 정보를 담고 있다. 

​

seen 클래스에 속한 모든 훈련 이미지는 해당 클래스 이름의 단어 벡터에 가까워지도록 매핑해야 한다.

$Seenclass\ y\in Y_s의이미지x^{(i)}들은\ 이미\ 아는\ 단어\ 벡터\ w_yw로\ 매핑되도록\ 학습.$Seenclass y∈Ys​의이미지x(i)들은 이미 아는 단어 벡터 wy​w로 매핑되도록 학습.​

여기서 θ는 이미지 특징(F차원) →  단어 공간(50차원)으로 보내는 50×F 크기의 행렬이다. 즉, 이미지 벡터에 θ를 곱하면 그 이미지가 속한 클래스의 단어 벡터 위치로 이동하도록 θ를 학습하는 것이다. 이렇게 이미지들을 단어 벡터 공간에 투영하면, 단어 벡터의 의미가 시각적 정보로 보강(grounding) 된다. 예를 들면, 단어 “dog”의 위치 주변에 개 이미지들이 모이므로 우리는 그 근처를 탐색함으로써 이 단어가 가리키는 전형적 이미지(시각적 프로토타입), 이 단어와 관련된 색깔 같은 속성을 유추할 수 있게 된다.

​

Fig. 2는 단어 벡터(50차원)와 이미지 투영 결과를 t-SNE로 2차원에 시각화한 그림이다. unseen 클래스는 cat, truck, seen 클래스들은 대부분 자기 단어 벡터 주변에 촘촘하게 클러스터를 형성한다. unseen 클래스(cat, truck)의 이미지는 해당 단어벡터 바로 근처에 있진 않지만, 의미적으로 가까운 seen 클래스 주변에 위치한다. 

예:

cat 이미지 → dog, horse 근처에 위치

truck 이미지 → car 근처에 위치

이는 매우 중요하다. 왜냐하면 훈련 데이터가 없는 클래스라도, 비슷한 의미를 가진 seen 클래스 근처에 자동으로 매핑된다는 것을 보여주기 때문이다. 이 관찰이 논문의 핵심 아이디어로 이어졌다: 먼저 outlier를 찾아서 seen인지 unseen인지 판단한 다음, unseen이면 그에 해당하는 단어 벡터 주변으로 분류.

이제 단어 표현과 이미지 표현, 그리고 이미지→단어 공간 매핑까지 설명했으므로, 다음 섹션에서는 zero-shot 분류와 일반 분류를 하나의 확률 모델로 결합한 구조를 설명한다.

5. Zero-Shot Learning Model

​

이 절에서는 먼저 전체 모델의 구조를 개괄적으로 설명하고, 이후 각 구성 요소를 자세히 설명한다.

일반적으로 우리의 목표는, 입력 이미지 x가 주어졌을 때 seen 클래스와 unseen 클래스 모두에 대해

$p(y∣x)$p(y∣x)​
를 예측하는 것이다. 여기서 클래스 y 는 Ys ∪ Yu에 속한다.

​

일반적인 분류기들은 훈련 예제가 없는 클래스를 절대 예측하지 않기 때문에, 우리는 이미지가 seen 클래스인지 unseen 클래스인지를 나타내는 이진 변수V∈{s,u} 를 도입한다.

또한 X_s는 모든 seen 클래스에 대한 훈련 이미지 특징 벡터들의 집합이다. 새로운 입력 이미지 x 의 클래스를 예측하는 식은 다음과 같다:


​

다음으로 식 (2)에 등장하는 각 항목을 설명한다.

​

*Unseen일 확률 P(V = u | x)


는 이미지가 unseen 클래스일 가능성이다. 이 값은 outlier detection score(이상치 점수) 에 threshold를 적용하여 계산된다.

이 점수는, 이미 semantic word space로 매핑된 seen 클래스 이미지들이 이루는 'manifold(데이터 공간)' 위에서 계산된다.

​

우리는 각 점의 marginal probability(주변 확률)를  혼합 가우시안 모델(Gaussian Mixture) 로 추정하여 사용한다.

seen 클래스 이미지로부터 다음의 주변 확률을 얻는다:


여기서 각 클래스 y 의 Gaussian 분포는 

평균(mean): 그 클래스의 단어 벡터 w_y 와 

공분산(covariance): 그 클래스의 매핑된 이미지들로부터 추정한 Σy 

로 정의된다.

과적합을 피하기 위해 Σy 등방성(isometric) 으로 제한한다.

​

*Outlier 판단

새로운 이미지 x 에 대해 outlier detector는 다음처럼 정의된다 :


즉, marginal 확률이 threshold T보다 작으면 → unseen으로 판단한다. 

논문 후반부에서는 여러 threshold T 에 관한 실험 결과를 제시한다.

​

* Seen으로 판단된 경우

만약 V = s 즉, 해당 포인트가 seen 클래스라고 판단되면, 


은 어떤 분류기든 사용할 수 있다.

이 논문에서는 원래의 F차원 이미지 특징에 대해 softmax classifier를 사용한다.

​

*Unseen으로 판단된 경우

zero-shot 상황인 V=u 의 경우, 우리는 각각의 zero-shot 단어 벡터 주변에 등방성 가우시안 분포가 존재한다고 가정한다.

​

* 대안 방법

다른 방법으로는, Kriegel et al. [16]이 제안한 outlier 확률 계산법 을 사용할 수도 있다.

이 방법은 각 테스트 포인트에 대해 outlier probability 를 계산한 뒤, seen 클래스용 분류기와 unseen 클래스용 분류기의 출력을 확률 가중(weighted combination) 으로 결합한다.

6. Experiments

​

우리는 대부분의 실험을 CIFAR-10 데이터셋에서 수행한다. 이 데이터셋은 10개의 클래스로 구성되어 있으며, 각 클래스는 5000개의 32×32×3 RGB 이미지를 포함한다. 우리는 Coates와 Ng [6]의 비지도 특징 추출 방법을 사용하여, 각 이미지에 대해 12,800차원의 특징 벡터를 얻는다.

이후의 실험들에서는, 제로샷 분석을 위해 10개 클래스 중 2개 클래스의 훈련 이미지를 제외한다.

​

6.1 Zero-Shot Classes Only

두 unseen 클래스만 분류할 때 실험.

unseen 클래스와 semantically 유사한 seen 클래스가 있어야 ZSL 성능이 좋다.

예: cat & truck을 제거 → cat은 dog와 horse가 seen class라서 transfer가 잘됨. truck은 car가 seen class라서 잘됨.

반대로 cat & dog 둘 다 제거하면 성능 매우 낮음.

unseen 클래스끼리만 분류 시 정확도 ~80% 이상 가능.

​

6.2 Zero-Shot and Seen Classes 

Fig. 3에서 우리는, 테스트 시점에 이미지를 seen 클래스와 unseen 클래스로 구분하는 threshold 값에 따라, 훈련된(seen) 클래스들에 대해 약 80%의 정확도를 얻을 수 있음을 관찰할 수 있다.

또한 70% 정확도 수준에서, unseen 클래스들은 약 30%에서 15% 사이의 정확도로 분류될 수 있다. 

무작위 추측(random chance)은 10%이다.


7. Conclusion 

​

We introduced a novel model for joint standard and zero-shot classification based on deep learned word and image representations. The two key ideas are that (i) using semantic word vector representations can help to transfer knowledge between categories even when these representations are learned in an unsupervised way and (ii) that our Bayesian framework that first differentiates outliers from points on the projected semantic manifold can help to combine both zero-shot and seen classification into one framework. If the task was only to differentiate between various zero-shot classes we could obtain accuracies of up to 90% with a fully unsupervised model. 

https://youtu.be/3LJz44iDuXs?si=vSqs2fKxSyxfEVZg


이미지를 단어 의미 공간으로 보내서, seen 영역 안/밖을 판단하고, 밖이면 단어 의미로 zero-shot 분류하는 모델.

​

1. 단어 의미 공간 (Semantic Word Space)

Wikipedia 텍스트로 학습한 50차원 단어 임베딩

단어 = 의미 정보가 담긴 벡터

​

2. 이미지 특징 (Image Feature Vector)

Coates & Ng 비지도 특징 추출

이미지 = F차원(12,800차원) 특징 벡터

​

3. 이미지 → 단어공간 매핑 (θ)

θ는 50×F 선형 변환 행렬

목표: θx가 해당 클래스 단어 벡터 w_y 근처로 가도록 학습

Loss: ‖w_y – θx‖² 최소화

즉, 이미지를 단어의 의미 공간으로 위치시킴.

​

4. Seen / Unseen 클래스 개념

Seen: 학습 이미지 있음

Unseen: 학습 이미지 0개

이 논문의 모든 문제는 unseen 분류를 어떻게 할 것인가.

​

5. Semantic Manifold (의미 공간에 쌓인 seen 클래스의 형태)

θx로 투영한 seen 이미지들이 모여 만든 공간

이 영역 안에 있으면 seen, 밖으로 튀면 unseen 후보

​

6. Outlier Detection (이상치 탐지)

θx가 seen manifold 밖이면 → unseen 가능성 높음

혼합 가우시안(mixture of Gaussians) marginal로 판단

Threshold T로 seen/unseen 구분

즉, 먼저 이미지가 seen인가 unseen인가 확률적으로 판단함.

​

7. Seen 분류기 = Softmax

seen이라고 판단되면 그냥 softmax로 분류

일반 이미지 분류기 역할

​

8. Unseen 분류기 = 단어 벡터 주변 Gaussian

unseen이라고 판단되면 θx와 각 unseen 단어 벡터 w_y 사이의 Gaussian likelihood 비교

가장 가까운 unseen 클래스를 선택

즉, 단어 의미만으로 분류.

​

9. Joint Model (하나의 확률 모델로 통합)

전체 확률:

$p(y∣x)=P(y∣seen)P(seen∣x)+P(y∣unseen)P(unseen∣x)$p(y∣x)=P(y∣seen)P(seen∣x)+P(y∣unseen)P(unseen∣x)​
즉, 

seen 판단 → seen classifier

unseen 판단 → zero-shot classifier

를 하나의 Bayesian 모델에서 결합함.

​

10. 실험 결과 핵심

zero-shot 클래스끼리만 분류하면 정확도 80~90%

seen/unseen 함께 분류할 때도 seen 약 80%, unseen 약 15~30%

이 논문의 메시지: “텍스트 의미 + 이미지 투영 + outlier 기반 선택 = 단 한 번도 본 적 없는 클래스를 분류할 수 있다.”