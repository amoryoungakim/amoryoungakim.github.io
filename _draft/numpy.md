---
title: "파이썬 넘파이 활용하기 (Python Numpy Examples)"
date: 2020-10-15 00:00:00 -0400
permalink: '/numpy/'
categories: Python
---

### numpy로 배열(array) 생성하기

numpy는 list와 생김새도 유사하고 오퍼레이션도 유사한 측면이 많은데, numpy를 사용하는 이유는 list에 비해 연산 속도가 월등히 빠르기 때문이다. numpy는 배열(array)를 다룬다.

```python
import numpy as np

# 1차원 배열 생성
A = np.array([1, 2, 3, 4])

print(A.ndim)  # 몇 차원인가?
print(A.shape) # 차원 별 요소의 개수
print(A.size)  # 총 몇 개의 요소를 가지고 있나?
len(A)         # 가장 높은 차원의 요소 개수

# result
1
(4,)
4
4
```

```python
# 2차원 배열 생성
B = np.array([[0, 1, 2, 3],
              [4, 5, 6, 7],
              [8, 9, 10, 11]])
              
print(B.ndim)  # 몇 차원인가?
print(B.shape) # 차원 별 요소의 개수
print(B.size)  # 총 몇 개의 요소를 가지고 있나?
len(B)         # 가장 높은 차원의 요소 개수

# result
2
(3, 4)
12
3
```

```python
# 3차원 배열 생성
C = np.array([[[0, 1, 2, 3],
              [4, 5, 6, 7],
              [8, 9, 10, 11]],

             [[12, 13, 14, 15],
              [16, 17, 18, 19],
              [20, 21, 22, 23]]])

print(C.ndim)  # 몇 차원인가?
print(C.shape) # 차원 별 요소의 개수
print(C.size)  # 총 몇 개의 요소를 가지고 있나?
len(C)         # 가장 높은 차원의 요소 개수

# result
3
(2, 3, 4)
24
2
```

### 특수한 배열 생성 하기

```python
np.zeros((3, 4))              # 값이 0(float)으로 채워진 3x4 array
np.zeros((3, 4), dtype=int)   # 값이 0(int)으로 채워진 3x4 array
np.ones((3, 4))               # 값이 1(float)로 채워진 3x4 array
np.eye(3)                     # 대각선 요소만 1(float)로 채워진 3x3 array
np.diag([1, 2, 3])            # 대각선 요소만 1, 2, 3(int)으로 채워진 3x3 array
np.empty((3, 4))              # 무의미한 값으로 채운 3x4 array
np.full((3, 4), 1)            # 값이 1(int)로 채워진 3x4 array
```

### Numpy 배열 슬라이싱

```python
C = np.array([[[0, 1, 2, 3],
              [4, 5, 6, 7],
              [8, 9, 10, 11]],

             [[12, 13, 14, 15],
              [16, 17, 18, 19],
              [20, 21, 22, 23]]])

prin(C[1, 1:3, 0:2])
print(C[0, 0::2, 3::-2])

# result
[[16 17]
 [20 21]]
 
[[ 3  1]
 [11  9]]
```

### 난수 생성하기

```python
x = np.random.randint(0, 20, 15) # 0과 20 사이의 수 중 15개의 수를 랜덤하게 생성
print(x)

# result
[10  3  8  0 19 10 11  9 10  6  0 12  7 14 17]
```

### array를 index로 지정해서 array 슬라이싱 하기

```python
inds = np.array([3, 7, 8, 12])
print(x[inds])
```



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
