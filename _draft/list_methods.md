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

L.sort()와 같은 정렬 방식은 List에만 쓸 수 있는 반면 sorted(L)는 모든 iterable (dictionary 등)에 사용 가능하다.
L.sort()은 원본 L도 정렬된 상태로 바꿔버리지만 sorted(L)은 원본 L을 그대로 유지한다.
숫자와 문자가 섞여 있는 경우에는 정렬이 불가능하다. (TypeError 발생)

```python
L = [3, 5, 11, 8, 4]
print(L.sort())
print(L)

# result
None
[3, 4, 5, 8, 11]

L = [3, 5, 11, 8, 4]
print(sorted(L))
print(L)

# result
[3, 4, 5, 8, 11]
[3, 5, 11, 8, 4]
```

sort()나 sorted() 메소드를 쓸 때 정렬 기준을 구체적으로 정할 수 있다. default는 오름차순이고 내림차순으로 정렬하려면 `L.sort(reverse = True)`와 같이 쓸 수 있다. `key=str.lower`과 같이 정렬 기준 값을 정해줄 수도 있다.

```python
# 대/소문자 구분 없이 정렬하기
sorted("This is a test string from Andrew Tate".split(), key=str.lower)

# result
['a', 'Andrew', 'from', 'is', 'string', 'Tate', 'test', 'This']

# tuple의 마지막 요소 기준으로 정렬하기
student_tuples = [
    ('john', 'A', 15),
    ('jane', 'B', 12),
    ('dave', 'B', 10),
]
sorted(student_tuples, key=lambda x: x[2])

# result
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

### 그 외 List를 다루는 Tip

```python
# 리스트에 특정 조건을 걸어 값을 filtering 하기
L = [1, 2, 3, 3, 2, 5, 2]
print([v for v in L if v != 2])

# result
[1, 3, 3, 5]

# 리스트의 요소들을 가공 하기
s = 'the quick  brown   fox jumped over     the lazy  dog'
print([len(x) for x in s.split()])

# result
[3, 5, 5, 3, 6, 4, 3, 4, 3]

# 리스트를 0.0 5개로 초기화 하기
L = [0.0] * 5

# result
[0.0, 0.0, 0.0, 0.0, 0.0]

# 특정 순서에 따라 정렬하기
index = [1, 3, 5, 4, 2]
companies = ['Netflix', 'Apple', 'Tesla', 'Facebook', 'Google']
print([x[1] for x in sorted(list(zip(index, value)))])

# result
['Netflix', 'Google', 'Apple', 'Facebook', 'Tesla']
```

Reference: https://docs.python.org/ko/3/howto/sorting.html#sortinghowto
