---
title: "파이썬 정규표현식 (Python Regular Expression)"
date: 2020-09-19 08:26:28 -0400
permalink: '/regex_part1/'
categories: Python
---

데이터를 다루면서 정규표현식(Regular Expression)을 가장 유용하게 썼던 경험은 제품 모델명을 필터링 할 때였다. 알파벳, 숫자, -혹은 \_로 이루어진 모델명은 모델 종류에 따라 나름의 패턴이 있다. 예를 들어 여성복 TIME의 경우 아래와 같은 모델명이 있다.

> **TM**2A9**WBL**742W_**BK**

TM은 TIME 브랜드 명, WBL은 여성 블라우스라는 뜻, 마지막 BK는 BLACK 색상을 의미한다.

> **TM**2A4**WSC**61WP3_**BK**

TM은 TIME 브랜드 명, WSC는 여성 스커트라는 뜻, 마지막 BK는 BLACK 색상이다. 첫 번째 예시보다 길이가 더 길다.

만약 TIME의 BLACK 색상에 해당하는 제품 명만 필터링 하고 싶다면 위 두 예시를 포괄하는 정규표현식(Regular Expression)을 이렇게 작성할 수 있다.

> TM[a-zA-Z0-9]{10,11}\_BK

TM 뒤에 알파벳 소문자 혹은 대문자 혹은 숫자가 10개 이상 11개 이하로 오고 \_BK로 끝나는 패턴을 찾으라는 뜻이다.

이처럼 다양한 형태의 모델명을 필터링해서 원하는 정보만 가져오고자 할 때 정규표현식(Regular Expression)을 쓰면 쉽게 해결할 수 있다. 이 포스트에서는 먼저 정규표현식(Regular Expression) 작성하는 법을 살펴보고 이어서 정규표현식(Regular Expression) 활용 메소드를 다룬다.

### 정규표현식(Regular Expression) 작성하는 법

Python 문자열(string)을 작성할 때 작은 따옴표로 감싸듯이 정규표현식(Regular Expression) 역시 `'google'` 과 같은 식으로 작은 따옴표 안에 작성하면 된다. 단, 반복되는 패턴이 있는 부분은 `'go{2,4}gle'` (알파벳 o를 2~4번 반복하라는 뜻) 과 같이 문자열 뒤에 반복 회수를 지정해준다.

**문자**

|   | 의미                                                   |
|---|--------------------------------------------------------|
| . | \n을 제외한 모든 문자                                   |
| \[.] | 마침표                                              |
| \[a-z] | 알파벳 소문자 중 하나                              |
| \[A-Z] | 알파벳 대문자 중 하나                              | 
| \[a-zA-Z] | 알파벳 전체 중 하나                             |
| \[0-9] | 숫자 중 하나                                       | 
| \[a-zA-Z0-9] | 알파벳 전체, 숫자 전체 중 하나                | 
| \\d     | 숫자 (\[0-9]와 같은 표현)                         | 
| \\w | 알파벳 전체, 숫자 전체, 언더바 (\[a-zA-Z0-9_]와 같은 표현)| 
| \\s | 공백                                                 |

**반복 회수**

|       |       의미       | 예시        | 비고                 |
|-------|------------------|------------|----------------------|
| ?     | 0 or 1회         | go?gle     | ggle, gogle 과 일치  |
| *     | 0 or 1회 이상    | go\*gle    | ggle, gogle, google, gooogle 등과 일치  |
| +     | 1회 이상          | go+gle    | gogle, google, gooogle 등과 일치  |
| {m}   | m회              | go{2}gle | google 과 일치  |
| {m,} | m회 이상          | go{2,}gle | google, gooogle, goooogle 과 일치  |
| {m,n} | m회 이상 n회 이하 | go{2,3}gle | google, gooogle 과 일치  |

**기타**

|    | 의미                                                   |
|----|--------------------------------------------------------|
| ^  | \[]안에서 쓰이면 \[]안의 문자를 제외한 나머지라는 뜻<br>\'\'안에서 쓰이면 패턴의 시작을 나타냄      |
| $  | \'\'안에서 쓰이면 패턴의 끝을 나타냄                     |
| \| | \[]안에서 쓰이면 or의 의미                              | 
| () | 패턴을 그룹화할 때 사용                                 |

**예시**

```python
# re Library import
import re

# Phone number
s = '832-38-1847'
digit_pattern = re.match('\d{3}-*\d{2}-*\d{3}', s)
```

* \\d{3} : 숫자 3개가 와야 함
* -* : 하이픈은 있어도 되고 없어도 됨

```python
# Name
s = 'Rich Salamander Vuduc'
name_pattern = re.match('^[a-zA-Z]+\s+([a-zA-Z]+\s)?[a-zA-Z]+$', s)
```

* ^ : string이 바로 뒤에 이어지는 패턴으로 시작해야 함
* \[a-zA-Z]+ : First name은 알파벳이 1개 이상(+) 임
* \\s+ : First name 뒤에 공백이 1개 이상(+) 올 수 있음
* (\[a-zA-Z]+\\s) : Middle name은 알파벳 1개 이상(+) 그리고 공백이 1개 올 수 있음
* ? : Middle name은 있어도 되고 없어도 됨
* \[a-zA-Z]+ : Last name은 알파벳이 1개 이상(+) 임
* $ : string이 바로 앞의 패턴으로 끝나야 함

```python
# Email
s = 'test_email@gmail.com'
email_pattern = re.match('^[a-zA-Z][a-zA-Z0-9.\-_+]*@[a-zA-Z0-9.\-_]+[a-zA-z]$', s)
```

* ^\[a-zA-Z] : 알파벳으로 시작해야 함
* \[a-zA-Z0-9.\\-\_]* : 알파벳, 숫자, 마침표, -, _ 에 해당하는 문자가 0 또는 1개 이상(\*) 올 수 있음
* @ : @ 문자가 와야 함
* \[a-zA-Z0-9.\\-\_]+ : 알파벳, 숫자, 마침표, -, _ 에 해당하는 문자가 1개 이상(+) 와야 함
* \[a-zA-z]$ : 알파벳으로 끝나야 함


```python
# Example
s = 'new_ep_f014'
xample_pattern = re.match('new_?[a-z]+_[f|m]\d{2,4}', s)
```

* new : new로 시작
* \_? : 언더바는 있어도 되고 없어도 됨
* \[a-z]+ : 알파벳 소문자가 1개 이상(+) 옴
* \[f\|m] : f 혹은 m 문자가 와야 함
* \\d{2,4} : 숫자가 2개 이상 4개 이하 와야 함


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

결과는
``` 
<re.Match object; span=(0, 16), match='TM2A4WSC61WP3_BK'>
None
None
None
```

1번째 제품명은 패턴과 일치했다. 2번째 제품명은 'TM'과 '\_BK' 사이에 알파벳 혹은 숫자가 12개 오기 때문에 None이 리턴되었다. 3번째 제품명은 '\_OR'로 끝나기 때문에 None이 리턴되었다. 마지막 5번째 제품명은 모델명에 '\_'가 없어서 None이 리턴되었다.

시작을 타나태는 ^ 끝을 나타내는 $
