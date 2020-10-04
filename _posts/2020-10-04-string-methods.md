---
title: "파이썬 문자열 메소드 (Python String Methods)"
date: 2020-10-04 00:08:00 -0400
permalink: '/string_methods/'
categories: Python
---

### 문자열(string) 슬라이싱
```python
s[3] # index 3
s[3:6] # index 3~5
s[2:10:2] # index 2,4,6,8
s[10:2:-2] # index 10,8,6,4
s[2:10:-2] # 빈 문자열 '' 반환
```

역순으로 슬라이싱을 할 때는 start index 부터 거꾸로 end index까지 문자열을 가져오기 때문에 `s[2:10:-2]`와 같이 start index가 end index보다 작으면 빈 문자열 ''이 반환된다. 역방향으로 진행할 수가 없기 때문이다.

### 문자열(string) 형태 확인
```python
s.isalnum() # 문자열이 알파벳과 숫자로 이루어져 있나?
s.isalpha() # 문자열이 알파벳으로 이루어져 있나?
s.islower() # 문자열이 모두 소문자인가?
s.isupper() # 문자열이 모두 대문자인가?
s.istitle() # 단어의 시작만 대문자인가?
s.isdigit() # 문자열이 숫자로 이루어져 있나?
s.isnumeric() # isdigit()과 동일
s.isdecimal() # 문자열이 10진수인가?
s.isspace() # 문자열이 공백으로만 이루어져 있나?
s.startswidth('a') # 문자열이 'a'로 시작하나?
s.endswidth('a') # 문자열이 'a'로 끝나나?
```

```python
len(s) # 문자열의 길이
min(s) # 최소값
max(s) # 최대값
s.count('a') # 문자열 s 안에 'a'가 몇개 있나
```

### 문자열(string) 검색하기
```python
s.find(text, start, end) # 앞에서부터 찾은 위치를 리턴함, 못찾으면 -1
s.rfind() # 뒤에서부터 찾은 위치를 리턴함, 못찾으면 -1
s.index(text, start, end) # 앞에서부터 찾은 위치를 리턴함, 못찾으면 ValueError
s.rindex() # 뒤에서부터 찾은 위치를 리턴함, 못찾으면 ValueError
```

### 문자열(string) 나누기
```python
s.split() # 공백을 기준으로 나눔
s.split(',') # 콤마를 기준으로 나눔
s.splitlines() # \n을 기준으로 나눔
```

### 문자열(string) 공백 제거
```python
s.strip() # 문자열 앞/뒤의 공백 제거
s.strip('-') # 문자열 앞/뒤에서 '-' 제거
s.lstrip() # 왼쪽 공백 제거
s.rstrip() # 오른쪽 공백 제거
```

### 문자열(string) 바꾸기
```python
s.replace("A", "B") # 'A'를 'B'로 바꾸기
s.lower() # 모두 소문자로 바꾸기
s.upper() # 모두 대문자로 바꾸기
s.swapcase() # 소문자는 대문자로, 대문자는 소문자로 바꾸기
s.title() # 단어의 시작만 대문자로
s.capitalize() # 문자열의 시작만 대문자로, 나머지는 소문자로
```

### 문자열(string) 정렬하기
```python
s.center(21) # 총 길이가 21이 되도록 좌우에 공백을 추가해 가운데 정렬
s.zfill(21) # 총 길이가 21이 되도록 왼쪽에 0을 추가
s.ljust(21, 'A') # 총 길이가 21이 되도록 문자열은 왼쪽 정렬, 오른쪽은 'A'로 채움
s.rjust(21, 'A') # 총 길이가 21이 되도록 문자열은 오른쪽 정렬, 왼쪽은 'A'로 채움
```

Reference: https://rfriend.tistory.com/327 
