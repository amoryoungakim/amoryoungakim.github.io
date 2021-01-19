---
title: "Recommendation"
date: 2021-01-19 08:26:28 -0400
categories: recommendation
tags: recommendation
---

1. 협업 필터링 (Collaborative Filtering)
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


1. 내용 기반 필터링(Contents-Based filtering)
like A then like B 방식
A를 좋아한 사용자에게 비슷한 B를 추천
텍스트가 많은 제품 추천시 유용 (신문, 책)
제품에서 키워드를 추출해 비교해보고 비슷한 키워드가 많은 제품을 화면 상단에 올려 추천하면 클릭률이 높을 것임

지식 기반 필터링(Knolwedge-Based filtering)
전문가의 도움을 받아 전체적인 지식 구조를 만들어 활용
키워드 추출이 불가한 방식일 때 비슷한 제품을 찾기 위해 체계도(ontology)를 만들어 씀
와인, 커피, 컴퓨터 등
이건 약간 휴리스틱하네



```r
# 기본 값 1로 설정해 준 다음
Carseats[, “Bad_Shelf”] <- 1
Carseats$Bad_Shelf <- ifelse(Carseats$ShelveLoc == 'Bad', 1, 0) 

Carseats[, “Good_Shelf”] <- 1
Carseats$Good_Shelf <- ifelse(Carseats$ShelveLoc == 'Good', 1, 0) 

