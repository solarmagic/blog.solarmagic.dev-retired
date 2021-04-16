---
title: 두 사람의 동작 유사도를 계산하기
date: 2021-04-16 00:00:00
categories:
- ML
tags:
- Pose Estimation
---

> 이글은 2020년 8월 4일에 작성한 글 을 옮긴 글입니다. 

## 배경

### Pose Estimaton 설명

![AlphaPose](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbpt20m%2FbtquaPjneyA%2FwbNlbM9jVIHqfb3TgEtdfK%2Fimg.gif) ![alt](https://github.com/MVIG-SJTU/AlphaPose/raw/master/docs/posetrack2.gif)

머신 러닝, 딥 러닝을 이용하여 이미지나 영상의 사람의 포즈를 분석하는 기술을 Pose Estimation(포즈 추정)이라고 한다. 이미 많이 연구가 된 분야이고 통제된 데이터에서 잘 추출해내고 있다. 사람의 포즈를 추정하는 방식은 주요 관절 위치를 추정하는 것이다. 코, 어꺠 팔꿈치, 손목, 발목, 엉덩이, 무릎 등의 위치를 종합적으로 추정한다. 이런 주요 관절들을 **Key Points**라고 한다.

### 쉽게 사용가능한 Pose Estimation

현재 다양한 상용 Pose Estimation [API](https://en.wikipedia.org/wiki/Application_programming_interface)가 나와 있다. 이를 통해서 응용 프로그램 개발자는 큰 ML 지식 없이도 ML을 활용한 프로그램을 만들 수 있게 되었다.

- [구글 Vision AI](https://cloud.google.com/vision?hl=ko)
- [Kakao Developers](https://developers.kakao.com/docs/latest/ko/pose/common#intro)
- [네이버 클라우드 플랫폼](https://www.ncloud.com/product/aiService/poseEstimation)
- [마음 AI](https://maum.ai/?lang=kr)

주로 Pose Estimation을 하면 17개 정도의 키포인트와 함께 그 키포인트가 정말로 그 위치에 있을지 확신하는 정도인 Confidence Score를 같이 주곤 한다.

### 다양한 Pose Estimation 응용 분야

![alt](https://developers.kakao.com/docs/latest/ko/assets/style/images/pose/pose_use_case.png)

- 요가, 필라테스 자세 분석 & 교정
- 스포츠 자세 분석 & 교정
- 자신의 포즈를 그대로 따라하는 캐릭터
- 이상 행동 탐지

특히 이를 활용해서 스포츠에 활용하려는 앱, 회사들이 많이 생기고 있고 나도 이에 관한 프로젝트를 진행중이다.

## 문제 정의

어떤 사람의 포즈를 교정하려면 어떻게 해야할까? 먼저 잘 했는지 판단을 해야한다. 기준이 되는 포즈가 있고 어떤 사람의 포즈가 있어서 그 사람이 기준이 되는 포즈를 얼마나 잘 따라 했는지 수치화해서 보여줄 수 있을 것이다. 어떤 두 사람의 순간 포즈를 비교할 수 있다면 이를 활용해서 두 영상이 얼마나 비슷한지도 계산 할 수 있을 것이다.

현재 나는 TensorFlow.js(PoseNet)를 활용해서 브라우저에서 홈 트레이닝을 할 수 있는 웹앱을 만드는 중이다.  모델(가이드) 포즈는 프레임마다 포즈를 추출 할 수 있다. 그러나 유저의 포즈는 하드웨어 자원을 활용하기 힘들고, Javascript라는 언어의 한계 때문에 초당 포즈 추정프레임이 그리 높지 않다.

이런 불균형한 데이터가 있을때 어떻게 보정해서 점수를 매길지 생각해 본다. 이 문제에 대해 정리를 잘 정리하면 나중에도 쓰일 날이 있을 것으로 판단 된다.

## 문제 해결

이 문제를 해결하기 위해서 먼저 문제를 분해해서 각각의 문제를 푸는 방식을 생각해봤다.

1. 단일 이미지에서 포즈 비교를 어떻게 할 것인가?
2. 측정될때의 노이즈를 어떻게 제거할 것인가?
3. 속도 문제로 인해 부족한 프레임을 어떻게 채울 것인가?
4. 네트워크 지연, 유저의 반응속도 등으로 인한 가격차이를 어떻게 해소 할 것인가?
5. 유저는 다양한 각도로 촬영, 좌우 반전 촬영될 수 있는데 이를 처리하는 방법은 무엇인가?

### 1. 단일 이미지에서 비교하는 법

단일 이미지에서 두 사람이 얼마나 비슷한지 찾는 것은 구글의 [Move Mirror](https://experiments.withgoogle.com/collection/ai/move-mirror/view)에서 그 방법을 찾아볼 수 있다.

![alt](https://lh3.googleusercontent.com/8lXXHoS-ibHI8hXPnU8mbqnckhXY2Gj8aHv4mE9HOxQWZzKQsGETiSao2BGsgvEBgVAFWfzYcalKA2ZHE8WS14Sw1JjwJw=s850)

[Move Mirror를 설명한 글](https://medium.com/tensorflow/move-mirror-an-ai-experiment-with-pose-estimation-in-the-browser-using-tensorflow-js-2f7b769f9b23)을 참조해보면 2가지의 거리 함수를 정의하여 빠르게 유사한 이미지를 매칭 시킨다. 그 두 가지 방법을 소개해보곘다.

#### Cosine Distance

17개의 키포인트(일반적으로 키포인트는 이정도 개수이다.)를 벡터로 변환하면 34차원 벡터가 나올 것이다. (x, y 좌표) ~~[5차원 구사과 초콜릿](https://www.acmicpc.net/problem/13727)이나 11차원 [하이퍼 토마토](https://www.acmicpc.net/problem/17114) 보다 고차원이다.~~ 이런 고차원 벡터 간의 가장 가까운 벡터를 찾아주는게 바로 코사인 거리이다.

![alt](https://miro.medium.com/max/700/0*n0USCt5M-yN6wkvy)
코사인 유사도는 두 벡터의 유사도를 측정하는데 각도가 완벽히 반대면 -1, 완벽히 같으면 1이 된다. 여기서 중요한건 벡터의 방향만 고려하고, 크기는 고려하지 않는다는 것이다. 이런 코사인 유사도는 꼭 그래프에 그려진 선위에서만 쓰일수 있는 것이 아니라 어떠한 [문장의 유사도를 구하는데](https://engineering.continuity.net/cosine-similarity/)에도 유용하게 쓸 수 있다.

![alt](https://i.imgur.com/C6VnrI4.png)

그런데 바로 코사인 유사도를 적용하기 전에 해야할 일이 2가지 있다. 어떤 사진에서 사람이 나올때 그 사람의 크기는 다 다르다. 이를 다 맞춰 줄 것이다.

1. Resize and Scale: 각 사람을 둘러싸는 박스(bbox, bounding box) 좌표를 기준으로 이미지를 자르고 scale을 다 상수로 맞춰준다.
2. L2 Normalization: 키포인트의 좌표를 L2 Normaalize를 한다.

여기서 [L2 Normalization](https://mathworld.wolfram.com/L2-Norm.html)을 한다는 것은 모든 벡터가 단위 norm을 갖게 하겠다는 얘기이다. 좀 더 쉽게 말해서 L2 Norm된 벡터의 원소를 제곱해서 다 더하면 1이 되도록 하겠다는 거다. 아래 그림을 통해서 L2 Normzalize를 한 것과 안 한 것의 차이를 볼 수 있다.

![alt](https://i.imgur.com/DmNxtKb.png)

![alt](https://i.imgur.com/OWQKAys.png)

정규화된 키포인트 좌표를 통해 위 공식대로 코사인 유사도를 계산한다.

![alt](https://i.imgur.com/9ifWsXa.png)

그리고 이런 공식에 대입하면 코사인 거리를 얻을 수 있다.

이 떄 주목할건 F와 G의 17개의 x,y 좌표만 썼을 뿐 각 좌표의 Confidence Score는 사용하지 않았다는 것이다.

#### Weighted Distance

코사인 거리는 매우 훌륭하고 좋은 결과 값을 주긴 하지만 아직 큰 결함이 있다. Confidence Score는 Pose를 추정할 때 이 좌표에는 어떤 키포인트가 있을지 얼마나 확신하는지 알려주는 정도이다. 이 정보를 무시한다면 우리는 매우 중요한 정보를 버리는 것이다.

구글 연구진들은 이 공식을 통해 더 정확하게 거리를 추정했다.

![alt](https://i.imgur.com/SzAtmQD.png)

여기서 F_c가 Confidence Score이고 모두 L2 Norm이후이다. 아쉽게도 원 글에서 정확히 이 공식이 어떤 의미를 가지는지 설명을 하지는 않았지만 식을 대충 해석해봤을떄 17개 좌표중에서 Confidence Score가 높은 것은 거리를 계산할떄 영향을 많이 준다고 해석 된다.

#### 구현

[유사도 구현 깃헙](https://github.com/freshsomebody/posenet-similarity) 내가 처음부터 짤 필요 없이 이 설명대로 이미 구현이 되어 있는 라이브러리가 이미 존재하고 npm으로도 쉽게 설치할 수 있다. 이를 통해서 두 사진의 pose가 얼마나 비슷한지 측정하는건 할 수 있다.

#### 뱀발

이 글에서는 이 이후에 VP Tree라는 자료구조를 이용하여 어떻게 80,000개의 사진에서 빠르게 가장 비슷한 포즈 사진을 찾는지를 설명했다. 다음 기회에는 아마 이 VP Tree라는 자료구조에 대한 포스팅을 올릴 생각이다.

### 2. 노이즈를 제거하는 방법

사실 노이즈가 이 데이터에 있는지는 아직 잘 모르겠는데 멘토님께서 이걸 써보는게 어떻냐 해서 도입하게 되었다.

칼만 필터는 데이터에 포함된 노이즈를 필터링 하는 알고리즘이다.

![alt](http://image.yes24.com/goods/73621194/800x0)

칼만 필터는 어렵지 않다고 한다.

칼만 필터는 컴퓨터비전, 로켓, 로봇, 위성, 미사일, 제어 분야에 많이 이용되는 알고리즘이다. 원래는 신호 처리 쪽에서 많이 쓰이는데 OpenPose의 Object Tracking을 위해서도 쓰였다고 알려진다. 그러므로 우리의 데이터를 다룰 때도 사용해도 괜찮을 것으로 판단 된다.

![alt](https://i.imgur.com/n3vBCWi.png)

칼만 필터는 선형 시스템을 기반으로 설계된 필터로써 정상 상태 기준의 모델을 수행하였으므로 선형 시스템이라고 가정할 수 있다. 칼만 필터의 알고리즘은 예측과 업데이트를 반복적으로 계산하여 시스템 출력 값의 잡음 영향을 최소화하고 출력 값을 예측한다. 예측은 현재 상태의 예측을 말하고, 업데이트는 현재 상태에서 관측된 추정까지 포함한 값을 통해서 더 정확한 예측을 할 수 있는 것을 말한다.

![alt](https://i.imgur.com/u8CStNQ.png)

0. 초기값
1. 추정과 오차 공분산 예측
2. 칼만 이득 계산
3. 추정값 계산
4. 오차 공분산 계산

과정 2의 칼만 이득 계산 시에서 행렬 H와 치환 행렬 H^T는 계산을 위한 변환 행렬이므로 단순화 시키면 다음과 같다.

![alt](https://i.imgur.com/eBOvQyl.png)

전개한 수식을 통해 칼만 이득은 측정 값 오류 R 대비 추정값 오류 P_bar(k)의 비율임을 알 수 있다. 따라서 측정값 오류가 추정값 오류와 비교하면 상대적으로 크면 칼만 이득은 작아지고, 측정값 오류가 추정값 오류보다 상대적으로 작으면 칼만 이득은 커지게 된다.

과정 3의 추정갑 계산 식을 전개하면 다음과 같다.

![alt](https://i.imgur.com/YFhddz1.png)

칼만 이득 K_k 은 0에서 1값을 가지며, 예측값 hat(x)_bar(k) 과 측정값 Z_k에 곱해지는 가중치를 의미한다. 즉, 예측값과 추정값 중 상대적으로 신뢰할 수 있는 값에 큰 가중치를 두고 계산하여 단위시간의 추정 값을 계산한다. 칼만 필터는 순환적으로 반복되어 수행되므로, 필터가 잘 작동되어 예측값 계산의 신뢰성이 높아질수록 칼만 이득은 점점 작아지며, 측정값 대비 상대적으로 높은 가중치가 예측값에 부여되게 된다.

과정 1에서 예측 값 계산을 위해 예측시스템 오류 분산 값이 사용되며, 또한 과정 2 칼만 이득 계산 식에서 예측 시스템 오류 분산 값과 측정값 오류 분산 값이 이용된다. 과정 4에서 계산된 칼만 이득으로 예측시스템 오류 분산 값을 최적화한다.

### 구현체

[kalmanjs](https://github.com/wouterbulten/kalmanjs) 현재 프로젝트에 사용중이다. 아직 성능은 측정해보지 못하였다.

[poseTrack](https://github.com/windsor718/poseTrack)

### 더 읽어보기

- [Real-Time Multi-Person Pose Tracking using Data Assimilation
](https://openaccess.thecvf.com/content_WACV_2020/papers/Buizza_Real-Time_Multi-Person_Pose_Tracking_using_Data_Assimilation_WACV_2020_paper.pdf)
- [EKFPnP: Extended Kalman Filter for Camera Pose Estimation in
a Sequence of Images]( https://arxiv.org/pdf/1906.10324v2.pdf)

아무래도 선형으로 안 맞다보니 실제로는 EKF를 많이 쓴다고 한다.

### 3. 프레임 부족함을 해결하는 방법

이 분야는 motion interpolation 혹은 pose interpolation 이라고 할 수 있다.

가이드가 될 영상은 preprocessing으로 프레임별 pose data를 미리 구해놓을 수 있지만 유저의 영상은 실시간으로 처리하기 때문에 프레임이 가이드 보다 부족하다. 예를 들어 현재 데모에서는 초당 10프레임 정도의 계산이 가능하다. 30fps 기준으로 사라진 20fps를 복구시켜서 같은 사이즈에서 비교하는 것이 좋아보인다.

기존의 motion interpolation 은 대표적으로 다음과 같은 기술이 있다.

- Time remapping 은 단순히 똑같은 프레임을 한장 한장 더 늘려서 프레임만 증가시키는 기술.
- Frame blending 은 프레임들 사이에 앞 뒤 프레임을 겹친 프레임을 추가로 만든 기술.
- Frame interpolation 은 플래시의 Motion Shape 처럼 프레임들 사이에 움직임을 감지하고 바뀐 모양을 표현해주는 기술이다.

가장 간단하게 현재도 쓸수 있는 방법 Time remapping이고 사실 이걸로 계산을 해도 크게 차이는 안 나긴한다. (사람이 0.x초만에 그렇게 빨리 움직이기 않기 떄문이다.) 그래도 정확한 frame interpolation을 해주는 것이 좋아보인다

![alt](https://i.imgur.com/s7loIc8.png)

간단하면서도 효과적일것으로 예상이 되는 것은 linear interpolcation을 통해서 이전 프레임과 다음 프레임 사이를 구하는 것이다. 사람의 어떤 키포인트가 x1에서 x2로 이동했다면 아마 그 사이를 지나갔을것이라고 추측하는 것이 합리적이다. 혹은 cubic 을 쓰는 것도 좋아보이는데 이는 테스를 통해서 알아 봐야 할 것 같다.

![alt](https://i.imgur.com/mQ7ALxu.png)

pose interpolation 동영상

<iframe width="560" height="315" src="https://www.youtube.com/embed/32m8rQ-lbuo" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### 4. 싱크/딜레이에 관한 문제 해결법

유저들이 모든 프레임에 정확히 그 동작을 따라하는 것은 매우 힘들것이다. 그래서 실제 동작을 조금씩 느리게 하거나 조금씩 빠르게 하는 것에 대한 보정이 들어가야 할 것으로 보인다.

이 문제는 [Sequence alignment](https://en.wikipedia.org/wiki/Sequence_alignment) 문제와 비슷하다.

![alt](https://i.imgur.com/JXQYJ2s.png)

이중에서도 [Needleman–Wunsch algorithm](https://en.wikipedia.org/wiki/Needleman%E2%80%93Wunsch_algorithm)을 변형해서 사용한다면 포즈 데이터를 잘 끼워 맞출 수 있을 것으로 예상 된다. 어차피 시간족잡도가 O(nm) 이라서 사실 다른 아무거나 써도 괜찮을 것으로 예상 된다. (Method of Four Russians 을 사용한다면 좀 더 단축할 수 있는 것으로 알려져 있다.)

간단하면서도 효과적인 알고리즘으로 대각선 비교 알고리즘(가제)이 있다. n*m matrix를 다 채운다음 대각선 similarity / distance의 mean 값의 maximum을 통해서 점수를 계산하는 것이다. 단 프레임 수가 너무 작을 경우 극단 적이 값이 나오기 쉬우므로 일정 비율 이상일 때만 점수를 계산하는 것이다. 현재 그냥 생각나는대로 이런식으로 짜져있는데 좀더 생각해서 바꿔야 겠다.

### 5. 다양한 각도 / 반전 문제 해결법

유저들은 가이드 화면과 똑같은 방향으로 서서만 운동을 해야한다면 너무 불편할 것이다. 어디에서 어떤 각도로 촬영을 해도 비슷한 동작을 하고 있는지 아닌지 잘 판단을 해야한다.

먼저 3D Pose Estimation을 이용해서 키포인트에 대한 (x,y,z) 값을 알고 있다면 이 문제를 좀더 쉽게 해결 할수 있을 것이다. 그러나 아직은 3D pose estimation을 활용할 생각은 없다.

![alt](https://i.imgur.com/I2IUQ6s.png)

이러한 문제를 Procrustes는 그리스 신화에 나오는 도적인데 피해자들을 자신의 침대에 눕히고 잡아 늘이거나 침대 밖으로 나온 신체 부위를 잘라버려서 침대에 딱 맞추는 무서운 짓을 했다고 한다. Procrustes Analysis는 이와 같이 여러 geometrical shape들이 있을 때 크기와 회전에 대해서 모두 같게 만들기 때문에 신화의 인물의 이름을 차용해 왔다.

[Procrustes analysis | 위키 피디아](https://en.wikipedia.org/wiki/Procrustes_analysis)

![alt](https://i.imgur.com/WmDIbfs.png)

1. 각각의 Geometrical shape에서 평균 shape을 구해 빼주어 (0,0) 에 중심이 위치하도록 한다.
2. 모든 geometrical shape 들의 평균을 구한다.
3. 각각의 shape i 마다 size, rotation 그리고 translation 에 대한 parameter를 구해서 평균 geometric shape 과 차이가 최소가 되도록 한다. 자세한 과정은 다음 수식과 같으며 에러가 수렴할 때 까지 2~3번 과정을 반복한다.

## 결론

- 어렵다
  - 두 영상의 포즈를 스코어링하는 것은 어려운 일이다.
  - 그러나 위에서 제시한 몇가지에 대한 해답을 정교하게 만들면 불가능한 일만은 아닌 것으로 보인다.
- 여러분들의 도움이 필요하다
  - 이 글을 쓰는 사람은 아직 학부 졸업도 안한 사람이다 보니, insight가 부족하고 guru에 비하면 배경지식도 떨어진다.
  - 좋은 솔루션이 있다면 댓글이나 연락처로 의견 주시면 정말 감사하게 더 생각해 볼 수 있을 것 같다.
- 비전이 좋다
  - 만약 이걸 정말 잘 만든다면 슈퍼스타 축구 선수, 야구 선수, 농구 선수와 나의 포즈를 비교해서 코치를 받을 수도 있다.
  - 요가나 필라테스, 홈 트레이닝
  - [작렬 정신통일](https://youtu.be/LKcbQHVB3xM) 과 같은 게임 분야

이 글은 계속해서 업데이트 해 갈 예정이고 나중에 데모 GIF 등을 추가하고 더 낳은 알고리즘을 적용해서 더 나아지는 것을 보일 예정이다.

## References

- [Kakao Developers Pose](https://developers.kakao.com/docs/latest/ko/pose/common)
- [Pose Estimation | TensorFlow](https://www.tensorflow.org/lite/models/pose_estimation/overview)
- [Move Mirror: An AI Experiment with Pose Estimation in the Browser using TensorFlow.js](https://medium.com/tensorflow/move-mirror-an-ai-experiment-with-pose-estimation-in-the-browser-using-tensorflow-js-2f7b769f9b23)
- [Real-time Human Pose Estimation in the Browser with TensorFlow.js](https://medium.com/tensorflow/real-time-human-pose-estimation-in-the-browser-with-tensorflow-js-7dd0bc881cd5)
- [Human Pose Estimation: Pose Similarity](https://medium.com/@cavaldovinos/human-pose-estimation-pose-similarity-dc8bf9f78556)
- [칼만필터 kalmanfilter](http://blog.naver.com/msnayana/80106682874)
- [딥 러닝 및 칼만 필터를 이용한 객체 추적 방법](http://www.kibme.org/resources/journal/20190613143212078.pdf)
- [Procrustes Analysis](https://trip2ee.tistory.com/105#:~:text=Procrustes%EB%8A%94%20%EA%B7%B8%EB%A6%AC%EC%8A%A4%20%EC%8B%A0%ED%99%94%EC%97%90,%EB%AC%B4%EC%84%9C%EC%9A%B4%20%EC%A7%93%EC%9D%84%20%ED%96%88%EB%8B%A4%EA%B3%A0%20%ED%95%9C%EB%8B%A4.)
- [motion interpolation란 뭘까?](https://xiyo.tistory.com/6)
- [Alphapose](https://github.com/MVIG-SJTU/AlphaPose)
- [2019 가을호 / 기획특집 / 미디어 콘텐츠 시대](https://admission.postech.ac.kr/bbsLink.do?f=sub6-1_read&boardId=GSKO_POS_YEAR&BOARDSEQ=566&iNowPage=2)
- [칼만 필터는 어렵지 않아](http://www.yes24.com/Product/Goods/73621194)