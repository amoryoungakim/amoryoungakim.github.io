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

|기호| 의미                                                   |
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

<br>
**반복 회수**

|  기호  |       의미       | 예시        | 비고                 |
|-------|------------------|------------|----------------------|
| ?     | 0 or 1회         | go?gle     | ggle, gogle 과 일치  |
| *     | 0 or 1회 이상    | go\*gle    | ggle, gogle, google, gooogle 등과 일치  |
| +     | 1회 이상          | go+gle    | gogle, google, gooogle 등과 일치  |
| {m}   | m회              | go{2}gle | google 과 일치  |
| {m,} | m회 이상          | go{2,}gle | google, gooogle, goooogle 과 일치  |
| {m,n} | m회 이상 n회 이하 | go{2,3}gle | google, gooogle 과 일치  |

<br>
**기타**

|기호| 의미                                                   |
|----|--------------------------------------------------------|
| ^  | \[]안에서 쓰이면 \[]안의 문자를 제외한 나머지라는 뜻<br>\'\'안에서 쓰이면 패턴의 시작을 나타냄      |
| $  | \'\'안에서 쓰이면 패턴의 끝을 나타냄                     |
| \| | \[]안에서 쓰이면 or의 의미                              | 
| () | 패턴을 그룹화할 때 사용                                 |

<br>
**예시**

```python
import re

# Phone number
s = '832-38-1847'
digit_pattern = re.match('\d{3}-*\d{2}-*\d{3}', s)
print(digit_pattern)

# result
<re.Match object; span=(0, 10), match='832-38-184'>
```

설명 :<br>
`\d{3}` 숫자 3개가 와야 함, `-*` 하이픈은 있어도 되고 없어도 됨

```python
# Name
s = 'Rich Salamander Vuduc'
name_pattern = re.match('^[a-zA-Z]+\s+([a-zA-Z]+\s)?[a-zA-Z]+$', s)
print(name_pattern)

# result
<re.Match object; span=(0, 21), match='Rich Salamander Vuduc'>
```

설명 :<br>
`^` string이 바로 뒤에 이어지는 패턴으로 시작해야 함 `[a-zA-Z]+` First name은 알파벳이 1개 이상(+) `\s+` First name 뒤에 공백이 1개 이상(+) 올 수 있음 `([a-zA-Z]+\s)` Middle name은 알파벳 1개 이상(+) 그리고 공백이 1개 올 수 있음 `?` Middle name은 있어도 되고 없어도 됨 `[a-zA-Z]+` Last name은 알파벳이 1개 이상(+) 임 `$` string이 바로 앞의 패턴으로 끝나야 함

```python
# Email
s = 'test_email@gmail.com'
email_pattern = re.match('^[a-zA-Z][a-zA-Z0-9.\-_+]*@[a-zA-Z0-9.\-_]+[a-zA-z]$', s)
print(email_pattern)

# result
<re.Match object; span=(0, 20), match='test_email@gmail.com'>
```

설명 :<br>
`^[a-zA-Z]` 알파벳으로 시작해야 함 `[a-zA-Z0-9.\-_]*` 알파벳, 숫자, 마침표, -, _ 에 해당하는 문자가 0 또는 1개 이상(\*) 올 수 있음 `@` @ 문자가 와야 함 `[a-zA-Z0-9.\-_]+` 알파벳, 숫자, 마침표, -, _ 에 해당하는 문자가 1개 이상(+) 와야 함 `[a-zA-z]$` 알파벳으로 끝나야 함

```python
# Example
s = 'new_ep_f014'
example_pattern = re.match('new_?[a-z]+_[f|m]\d{2,4}', s)
print(example_pattern)

# result
<re.Match object; span=(0, 11), match='new_ep_f014'>
```

설명 :<br>
`new` new로 시작 `_?` 언더바는 있어도 되고 없어도 됨 `[a-z]+` 알파벳 소문자가 1개 이상(+) 옴 `[f|m]` f 혹은 m 문자가 와야 함 `\d{2,4}` 숫자가 2개 이상 4개 이하 와야 함


### 정규표현식(Regular Expression) 활용 메소드

정규표현식 활용 메소드에는 `match, search, findall, finditer, sub` 등이 있다.

#### 1. match

위에서는 `digit_pattern = re.match('\d{3}-*\d{2}-*\d{3}', s)`와 같이 코드 한 줄로 패턴을 찾는 방법을 썼지만, 정규표현식(Regular Expression)을 여러번 반복해서 쓸 경우 아래 예시와 같이 compile을 활용하는 것이 좋다.

```python
import re  

# 제품명 목록
model_name = ['TM2A4WSC61WP3_BK', 'TM2A4WSC61WP33_BK', 'TM2A4WSC61WP3_OR', 'TM2A4WSC61WP3BK']

# 찾고자 하는 패턴을 정의해서 time_pattern 이라는 이름으로 저장한다.
model_pattern = re.compile('TM[a-zA-Z0-9]{10,11}_BK')

# time_string의 4개 모델명을 time_pattern에 대입해보고 일치하면 그 결과를, 아니면 None을 반환한다.
for s in model_name:
  print(model_pattern.match(s))
  
# result
<re.Match object; span=(0, 16), match='TM2A4WSC61WP3_BK'>
None
None
None
```

1번째 제품명은 패턴과 일치했다. 2번째 제품명은 'TM'과 '\_BK' 사이에 알파벳 혹은 숫자가 12개 오기 때문에 None이 리턴되었다. 3번째 제품명은 '\_OR'로 끝나기 때문에 None이 리턴되었다. 마지막 5번째 제품명은 모델명에 '\_'가 없어서 None이 리턴되었다.

모델명 `'TM2A4WSC61WP3_BK'`에서 품목(WSC)과 색상(BK)만 추출하고 싶다면 괄호를 사용해 그룹화 할 수 있다.

```python
# 괄호로 그룹화 하는 예
s = 'TM2A4WSC61WP3_BK'
model_pattern = re.match('\w{5}([A-Z]{3})\w+_([A-Z]{2})', s)
print(model_pattern.group())
print(model_pattern.group(0))
print(model_pattern.group(1))
print(model_pattern.group(2))

# result
TM2A4WSC61WP3_BK
TM2A4WSC61WP3_BK
WSC
BK
```

`group()`과 `group(0)`는 패턴과 일치한 전체 문자열을 반환하고 `group(1)`은 첫번째 괄호에 해당하는 문자열, `group(2)`는 두번째 괄호에 해당하는 문자열을 반환한다.

괄호 안에 `?P<name>`을 써서 그룹에 이름을 지정해 줄 수도 있다.

```python
# ?P<name>으로 그룹에 이름 부여하기
s = 'TM2A4WSC61WP3_BK'
model_pattern = re.match('\w{5}(?P<category>[A-Z]{3})\w+_(?P<color>[A-Z]{2})', s)
print(model_pattern.group('category'))
print(model_pattern.group('color'))

# result
WSC
BK
```

`group()` 외에도 `start(), end(), span()`을 써서 일치하는 패턴의 위치를 파악할 수 있다.

```python
s = 'TM2A4WSC61WP3_BK'
model_pattern = re.match('\w{5}(?P<category>[A-Z]{3})\w+_(?P<color>[A-Z]{2})', s)

# 첫번째 패턴 WSC에 해당하는 start, end, span 위치 찾기
print(model_pattern.start(1))
print(model_pattern.end(1))
print(model_pattern.span(1))

# result
5
8
(5, 8)
```

#### 2. Search

문자열의 처음과 끝이 모두 정규표현식(Regular Expression)과 일치해야만 하는 `match`와는 달리 `search`는 전체 문자열에서 원하는 패턴과 일치하는 부분을 골라준다.

```python
s = 'With Deal:	$5.94 + No Import Fees Deposit & $7.46 Shipping to Korea, Republic of'
price_pattern = re.search('\$\d[.]\d{2}', s)
print(price_pattern)
print(price_pattern.group())
print(price_pattern.start())
print(price_pattern.end())
print(price_pattern.span())

# result
<re.Match object; span=(11, 16), match='$5.94'>
$5.94
11
16
(11, 16)
```

#### 3. findall

정규표현식(Regular Expression)과 일치하는 모든 문자열을 찾아 list로 반환한다.

```python
s = 'With Deal:	$5.94 + No Import Fees Deposit & $7.46 Shipping to Korea, Republic of'
price_pattern = re.findall('\$\d[.]\d{2}', s)
print(price_pattern)

# result
['$5.94', '$7.46']
```

#### 4. finditer

만약 정규표현식(Regular Expression)과 일치하는 모든 문자열을 찾아, `group(), start(), end(), span()` 메소드로 얻을 수 있는 정보를 추출하고 싶다면 `finditer`를 써야 한다.

```python
s = 'With Deal:	$5.94 + No Import Fees Deposit & $7.46 Shipping to Korea, Republic of'
price_pattern = re.finditer('\$\d[.]\d{2}', s)

for m in price_pattern:
    print("---------------")
    print(m)
    print(m.group())
    print(m.start())
    print(m.end())
    print(m.span())
    
# result
---------------
<re.Match object; span=(11, 16), match='$5.94'>
$5.94
11
16
(11, 16)
---------------
<re.Match object; span=(44, 49), match='$7.46'>
$7.46
44
49
(44, 49)
```

#### 5. Sub

특정 패턴을 갖는 문자열을 찾은 다음, 다른 문자로 바꾸고 싶다면 `sub`을 쓰면 된다.

```python
s = 'With Deal:	$5.94 + No Import Fees Deposit & $7.46 Shipping to Korea, Republic of'
price_pattern = re.sub('\$\d[.]\d{2}','$TBD', s)
print(price_pattern)

# result
With Deal:	$TBD + No Import Fees Deposit & $TBD Shipping to Korea, Republic of
```
