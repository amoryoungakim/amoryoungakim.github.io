---
title: "Recommendation"
date: 2021-01-19 08:26:28 -0400
categories: recommendation
tags: recommendation
---

1. 협업 필터링 (User-Based Collaborative Filtering)
A like then B like 방식
A, B가 동일 세그먼트일 때 A가 높은 점수를 준 것을 B에게도 추천해줌
점수가 없을 경우 한계
현업에서는 '점수' 대신 '구매', '검색', '재구매', '방문', '카트', '시청', '머문시간' 을 지표로 쓸 수도 있음
취향이 뚜렷이 구분되는 제품 추천시 정확함 (영화, 음악 패션)
예) 아마존 제품 추천, 넷플릭스 영화 추천

1) B와 평점 correlation(혹은 코사인 유사도)이 높은 사람들을 찾아 Group으로 만든다
2) Group이 본 영화 중 B가 아직 안 본거를 찾는다
3) 아직 안 본거 중 평점이 제일 높은 것을 추천한다

유사도 지표 (similarity index)
데이터가 연속형일 때 구할 수 있음 (바이너리는 안됨)
1) correlation -1(완전 불일치) 1(완전 일치)
2) cosine similarity 평가값이 좌표, 벡터가 나옴. 두 벡터사이의 코사인 값을 구함
유사할 수록 각도가 작고 코사인 값이 큼
1 완전 일치 -1 완전 불일치
바이너리일 때
1) 타니모토 계수(Tanimoto coefficient)
2) 자카드 계수(Jaccard coefficient)


2. 내용 기반 필터링(Contents/Item-Based Collaborative filtering)
like A then like B 방식
A를 좋아한 사용자에게 비슷한 B를 추천
텍스트가 많은 제품 추천시 유용 (신문, 책)
제품에서 키워드를 추출해 비교해보고 비슷한 키워드가 많은 제품을 화면 상단에 올려 추천하면 클릭률이 높을 것임


UB와 CB차이

UB
사용자별 맞춤형 추천
데이터가 풍부한 경우 정확한 추천 가능 
정확할 땐 정확한데 터무니없는 추천 할 때 있음 (User의 성향이 독특할 수 있으므로)
업데이트 자주
데이터 크기가 적고 사용자에 대한 충분한 정보 있는 경우
CB(IB)
정확도는 떨어지지만 계산이 빠름
대체로 정확성 높음
업데이트 자주 안함
데이터 크기 크고 각 사용자에 대한 충분한 정보 없는 경우 (아마존 같은데가 IB 기반)

cosine similarity를 user 기반으로 하느냐 item 기반으로 하느냐의 차이

UB
A에게 stranger things를 추천한다면
stranger things를 본 사람의 평점에 A와의 cosine similarity를 가중평균해서 예상 평점을 구함 

CB
intern을 재밌게 본 사람에게 stranger things를 추천한다면
stranger things를 본 사람의 평점에 intern과의 cosign similarity를 가중평균해서 에상 평점을 구함

CF 정확도 높이는 법
- 비슷한 성향을 가진 그룹을 KNN으로 구해서 (n은 여러번 시뮬해서 구해야 함) 그 그룹 내의 평점만 활용한다
- 높게 평가하는 성향 / 낮게 평가하는 성향이 있으므로 개인의 평균 평점대비 그 영화가 얼마나 높은 평점을 받았나 본다
- 모델링에 신뢰도가 높은 사용자(여러개 평가한 사람) / 아이템(여러사람으로부터 평가받은 아이템)만 포함시킨다

성과지표
train set의 모델을 이용해 test set 아이템의 에상 평점을 구한다
실제 평점과 예상 평점 차이를 계산해 정확도를 측정한다
MSE나 RMSE 계산 - 작을 수록 좋다
바이너리값인 경우는 (클릭 했음/안했음) accuracy, precision, recall, F1 score 등을 씀
정밀도(precision) 추천한 것중 몇개나 클릭 했나
재현율(recall) 사용자가 클릭한 것중 내가 몇개나 추천했나 (추천을 왕창 하면 재현율이 100%가 됨, 정밀도는 낮아짐)
둘은 tradeoff 관계에 있어서 조화 평균을 쓰기도 F1 score

현실에서는 TN이 매우 크다. 아이템이 많으므로 추천 안하고 클릭 안하는 경우가 대다수
PPM에서도 TN이 엄청 큼 (안살꺼라고 예측 했고 안삼)
TN이 포함되는 지표는 신뢰도가 낮음 (accuracy같은거)
그래서 정밀도와 재현율로 이루어진 F1 Score가 젤 조음...

모델기반 / 메모리 기반
메모리 기반은 모델에 필요한 모든 데이터를 메모리에 들고 있어서 느리고(CF), 모델 기반은 원래 데이터는 쓰지 않고 모델만 들고 있어서 대응이 빠름(Matrix Factorization, 신경망)

3. Matrix Factorization
사용자의 성향, 아이템의 성향을 K개의 요인(feature)로 정의하고 그 성향이 비슷한 경우 높은 점수를 줄 것이라 예측...?

실제 행렬 R=PQ
예측 행렬 R=PXQT
오차를 구해보고 오차를 줄이는 방향(stochastic gradient)으로 PQ를 수정하는 것이 핵심
에러의 제곱을 편미분한 것에 학습률을 곱해주고 이거를 원래 값에서 빼면 새로운 예측
오버피팅을 막기 위해 에러의 제곱에 regularization항을 추가한 다음 편미분하고.. 등등 할 수도 있음

sklearn Surprise 패키지를 이용하면 위 알고리즘 구현되어 있음

활용
제품 추천
검색 결과 정확도 높이기
광고 추천 (클릭 가능성 예측)

간접 평가 데이터 (indirect evaluation data)
쇼핑몰에서 몇번 방문 했고 얼마나 시간을 보냈나 - 선호도 측정에 효과적

ppm score를 rating 대신 쓸 수도 있을 거 같다

```python
