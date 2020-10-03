---
title: "파이썬 리스트 메소드 (Python List Methods)"
date: 2020-09-27 00:00:00 -0400
permalink: '/list_methods/'
categories: Python
---

### 요약

```python
L.sort(reverse = False) # 정렬
sorted(L, reverse = False) # 정렬
L.reverse() # 역순으로 배열 (역순 정렬이 아님)
L[::-1] # L.reverse()와 동일

L.append(21) # 끝에 21 추가
L.extend(M) # 끝에 List M 추가
L + M # 끝에 List M 추가
L.insert(0, 5) # 0번 index에 숫자 5 추가
L *= 3 # L의 요소들이 세 번 반복됨

L.remove(26) # 26 하나 삭제 (앞에서부터)
L.pop() # 맨 끝에 하나 삭제
L.pop(-3) # 뒤에서 세 번째 삭제
del L[-5:] # 범위에 해당하는 요소 삭제

L.index(23) # 23이라는 요소의 index (앞에서부터)
L.count(15) # 15라는 요소가 몇개 들어 있나
L.copy() # L과 동일한 List 생성
L.clear() # 비어있는 List로 만들기

len(L) # List의 전체 길이
min(L) # 최소값
max(L) # 최대값
sum(L) # 요소 전체 더함

from statistics import mean, variance, stdev
mean(L) # 요소 전체의 평균
variance(L) # 요소 전체의 분산
stdev(L) # 요소 전체의 표준편차
```

### Sort vs. Sorted

L.sort()와 같은 정렬 방식은 List에만 쓸 수 있는 반면 Sorted는 모든 iterable (dictionary 등)에 사용 가능하다.
숫자와 문자가 섞여 있는 경우에는 정렬이 불가능하다. (TypeError 발생)

```python
sorted("This is a test string from Andrew".split(), key=str.lower)
['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']

student_tuples = [
    ('john', 'A', 15),
    ('jane', 'B', 12),
    ('dave', 'B', 10),
]
sorted(student_tuples, key=lambda student: student[2])   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

### 

```python
리스트에서 특정 조건을 걸어 값을 filtering 할 때
L = [1, 2, 3, 3, 2, 5, 2]
Return [v for v in L if v != 2]

리스트의 값을 가공한 다음 return 할 때
s = 'the quick  brown   fox jumped over     the lazy  dog'
Return [len(x) for x in s.split()] 

리스트 초기화
L = [0.0] * n

index = [1, 3, 5, 4, 2]
value = ['Netflix', 'Apple', 'Tesla', 'Facebook', 'Google']
['Netflix', 'Google', 'Apple', 'Facebook', 'Tesla']

```

Reference: https://docs.python.org/ko/3/howto/sorting.html#sortinghowto
