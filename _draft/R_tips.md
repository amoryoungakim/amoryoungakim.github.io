---
title: "R Tips"
date: 2020-09-19 08:26:28 -0400
categories: R
tags: R
---

열 추가하기
```r
# 기본 값 1로 설정해 준 다음
Carseats[, “Bad_Shelf”] <- 1
Carseats$Bad_Shelf <- ifelse(Carseats$ShelveLoc == 'Bad', 1, 0) 

Carseats[, “Good_Shelf”] <- 1
Carseats$Good_Shelf <- ifelse(Carseats$ShelveLoc == 'Good', 1, 0) 

