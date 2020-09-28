# Feature Engineering 방법
출처 : http://hero4earth.com/blog/learning/2018/01/29/Feature_Engineering_Basic/

## 1. 관점에 따른 분류
Business driven features : 해결하려는 문제가 있는 현장인 비즈니스 관점에서 데이터를 분석하여 특징을 만들어내는 관점입니다.
Data driven features : 비즈니스 관점이 없어도 주어진 데이터를 다루는 과정에서 특징을 만들어내는 관점입니다.
위의 두 가지를 설명을 위해 구분하였지만 실제로는 분리되어 있지 않고 서로 조합되어 좋은 Feature를 만들게 됩니다.

## 2. 방법에 따른 분류
### 지표 변수(Indicator Variables)
<br>지표 변수를 만드는 것으로 예를 들어 나이 feature로 부터 21세 이상일 경우 성인으로 구분하는 feature를 만들 수 있습니다. 그리고, 부동산 정보의 경우 침실과 화장실의 갯수를 통해 부동산 가치를 판단하는 지표 변수를 만들 수 있습니다.
<br>
### 중복 특징(Interaction Features)
<br>두 개의 특징을 결합하여 새로운 특징을 만드는 방법입니다. 예를 들어 클릭 수와 접속 수를 결합하여 클릭 당 방문자수와 같은 특징을 만들 수 있습니다. 주의해야할 점은 특징이 늘어나기 때문에 자동으로 이러한 작업을 할 경우 특징이 너무 많아질 수가 있습니다.(feature explosion이라고 합니다.)
<br>
### 대표 특징(Feature Representation)
<br>특징들로부터 대표성을 갖는 새로운 특징을 만드는 작업입니다. 예를 들어, 미국의 12학년 제도로 표시되는 데이터가 있을 때 이를 기반으로 초등학교, 중학교, 고등학교와 같이 대표성을 가는 특징을 만들 수 있습니다.
<br>
### 외부 데이터(External Data)
<br>모델 성능을 높이기 위해 기존의 주어진 데이터 외의 다른 데이터를 활용하는 방법입니다.
<br>
### 에러 분석(Error Analysis - Post-Modeling)
<br>모델을 통해 나온 결과를 바탕으로 특징을 만드는 방법입니다. 일반적으로 데이터 사이언스의 프로세스가 반복을 기반으로 모델의 성능을 높이기 때문에 당연하다고 생각하실 수도 있을 것 같습니다. 그래서 아래에 보다 구체적으로 구분한 에러 분석을 통해 특징을 만드는 방법을 소개해드리겠습니다.
<br>
#### Start with larger errors
<br>모델을 통해 나온 모든 값을 확인하기 보다 ‘에러(Error)’값이 큰 feature부터 확인하는 방법입니다.
<br>
#### Segment By classes
<br>평균 에러(Error)값을 기준으로 Segment를 나누어 비교하면서 분석하는 방법입니다.
<br>
#### Unsupervised clustering
<br>패턴을 발견하는데 어려움이 있을 경우 Unsupervised(비지도) 학습인 clustering 알고리즘을 사용하여 분류되지 않은 값들을 확인하는 방법입니다. 여기서 clustering을 클래스로 분류하는 것이 아니라 에러(Error)의 원인을 찾는 방법으로 사용해야 한다는 점을 주의해야 합니다.
<br>
#### Ask colleagues or domain experts
<br>데이터를 통해서 발견할 수 없다면 도메인(분야) 전문가의 도움을 통해 에러(Error)의 원인을 찾아낼 수도 있습니다.
