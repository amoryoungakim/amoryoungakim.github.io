---
title: "파이썬 정규표현식 (Python Regular Expressions)"
date: 2020-09-19 08:26:28 -0400
categories: Python
---

데이터를 다루면서 정규표현식(Regular Expressions)을 가장 유용하게 썼던 경험은 제품 모델명을 define할 때였다. 알파벳, 숫자, -혹은 _로 이루어진 모델명은 모델 종류에 따라 나름의 패턴이 있다. 예를 들어 여성복 TIME의 경우 아래와 같은 모델명이 있다.

> TM2A9WBL742W_BK 

TM은 TIME 브랜드 명이고 WBL은 여성 블라우스라는 뜻, 마지막 BK는 BLACK 색상을 의미한다.

> TM2A4WSC61WP3_BK

역시 TM으로 시작하고, WSC는 여성 스커트라는 뜻, 마지막 BK는 BLACK 색상이다. 첫 번째 예시보다 길이가 1칸 더 길다.

만약 TIME의 BLACK 색상에 해당하는 제품 명만 필터링 하고 싶다면 위 두 예시를 포괄하는 정규표현식(Regular Expressions)을 이렇게 작성할 수 있다.

> TM[a-zA-Z0-9]{10,11}_BK

TM 뒤에 알파벳 소문자 혹은 대문자 혹은 숫자가 10개 이상 11개 이하로 오고 _BK로 끝나는 패턴을 찾으라는 뜻이다.

이처럼 다양한 형태의 모델명을 필터링해서 원하는 정보만 가져오고자 할 때 정규표현식(Regular Expression)을 쓰면 쉽게 해결할 수 있다. 처음 정규표현식(Regular Expression)을 접하면 ~~암호 해독하는 것도 아니고 이게~~ 굉장히 막막할 수 있지만 한 번 배우고 나면 아주 신박하게 써먹을 수 있으니 꼭 익숙해지도록 하자.

실제 사용 예시는 다음과 같다.

```python
# re library를 호출한다.
import re  

# 제품명 목록
time_string = ['TM2A9WBL742W_BK', 'TM2A4WSC61WP3_BK', 'TM2A4WSC61WP33_BK', 'TM2A4WSC61WP3_OR', 'TM2A4WSC61WP3BK']

# 찾고자 하는 패턴을 정의해서 time_pattern 이라는 이름으로 저장한다.
time_pattern = re.compile('TM[a-zA-Z0-9]{10,11}_BK')

# time_string의 5개 모델명을 time_pattern에 대입해보고 일치하면 그 결과를, 아니면 None을 반환한다.
for s in time_string:
  print(time_pattern.match(s))
```
<re.Match object; span=(0, 15), match='TM2A9WBL742W_BK'>
<re.Match object; span=(0, 16), match='TM2A4WSC61WP3_BK'>
None
None
None

코드 실행 결과에서 첫 2개는 패턴과 일치했다.
3번째 제품명은 'TM'과 '_BK' 사이에 알파벳 혹은 숫자가 12개 오기 때문에 None이 리턴되었다.
4번째 제품명은 '_OR'로 끝나기 때문에 None이 리턴되었다.
마지막 5번째 제품명은 모델명에 '_'가 없어서 None이 리턴되었다.
