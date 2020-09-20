---
title: "파이썬 정규표현식 (Python Regular Expression)"
date: 2020-09-19 08:26:28 -0400
permalink: '/regex_part1/'
categories: Python
---

데이터를 다루면서 정규표현식(Regular Expression)을 가장 유용하게 썼던 경험은 제품 모델명을 필터링 할 때였다. 알파벳, 숫자, -혹은 \_로 이루어진 모델명은 모델 종류에 따라 나름의 패턴이 있다. 예를 들어 여성복 TIME의 경우 아래와 같은 모델명이 있다.

''' **TM**2A9**WBL**742W_**BK** '''

TM은 TIME 브랜드 명, WBL은 여성 블라우스라는 뜻, 마지막 BK는 BLACK 색상을 의미한다.

> **TM**2A4**WSC**61WP3_**BK**

TM은 TIME 브랜드 명, WSC는 여성 스커트라는 뜻, 마지막 BK는 BLACK 색상이다. 첫 번째 예시보다 길이가 1칸 더 길다.

만약 TIME의 BLACK 색상에 해당하는 제품 명만 필터링 하고 싶다면 위 두 예시를 포괄하는 정규표현식(Regular Expression)을 이렇게 작성할 수 있다.

> TM[a-zA-Z0-9]{10,11}\_BK

TM 뒤에 알파벳 소문자 혹은 대문자 혹은 숫자가 10개 이상 11개 이하로 오고 \_BK로 끝나는 패턴을 찾으라는 뜻이다.

이처럼 다양한 형태의 모델명을 필터링해서 원하는 정보만 가져오고자 할 때 정규표현식(Regular Expression)을 쓰면 쉽게 해결할 수 있다. 처음 정규표현식(Regular Expression)을 접하면 ~~암호 해독하는 것도 아니고 이게 뭐람~~ 막막하게 느껴질 수 있는데 한 번 배우고 나면 쓸모가 많으니 꼭 익숙해지도록 하자.

이 포스트에서는 먼저 정규표현식(Regular Expression)의 활용 메소드에 match, search, findall, sub에 대해 살펴보고 이어서 ~~암호를 작성하는~~ 정규표현식(Regular Expression)을 작성하는 법을 다룬다.

### 정규표현식(Regular Expression) 활용 메소드

#### match

```python
# re library를 호출한다.
import re  

# 제품명 목록
time_string = ['TM2A4WSC61WP3_BK', 'TM2A4WSC61WP33_BK', 'TM2A4WSC61WP3_OR', 'TM2A4WSC61WP3BK']

# 찾고자 하는 패턴을 정의해서 time_pattern 이라는 이름으로 저장한다.
time_pattern = re.compile('TM[a-zA-Z0-9]{10,11}_BK')

# time_string의 4개 모델명을 time_pattern에 대입해보고 일치하면 그 결과를, 아니면 None을 반환한다.
for s in time_string:
  print(time_pattern.match(s))
```

결과는 아래와 같다.

``` 
<re.Match object; span=(0, 16), match='TM2A4WSC61WP3_BK'>
None <br>
None <br>
None <br>
```

1번째 제품명은 패턴과 일치했다. 2번째 제품명은 'TM'과 '\_BK' 사이에 알파벳 혹은 숫자가 12개 오기 때문에 None이 리턴되었다. 3번째 제품명은 '\_OR'로 끝나기 때문에 None이 리턴되었다. 마지막 5번째 제품명은 모델명에 '\_'가 없어서 None이 리턴되었다.

### 정규표현식(Regular Expression) 작성하는 법

string을 나타내는 작은따옴표 안에 문자의 타입과 반복 회수를 설정해주면 된다.

#### 문자 타입

알파벳인가, 숫자인가, 다른 기호인가

#### 반복 설정

반복을 설정하는 기호로는 \*, +, {m, n}, ?가 있다.
- \* 0번 혹은 여러번 반복
- + 1번 혹은 여러번 반복
- {m, n} m번이상 n번 이하 반복
- ? 0번 혹은 1번 (있어도 되고 없어도 될 때 씀)

#### 고급 설정

시작을 타나태는 ^ 끝을 나타내는 $
