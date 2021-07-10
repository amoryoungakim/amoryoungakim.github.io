---
title: "파이썬 넘파이 활용 팁 (Python Numpy Tips and Examples)"
date: 2021-04-18 00:00:00 -0400
permalink: '/numpy/'
categories: Python
---

numpy는 list와 생김새도 유사하고 오퍼레이션도 유사한 측면이 많은데, numpy를 사용하는 이유는 list에 비해 연산 속도가 월등히 빠르기 때문이다. numpy는 배열(array)를 다룬다.

### numpy로 배열(array) 생성하기

```python
import numpy as np

# 1차원 배열 생성
A = np.array([1, 2, 3, 4])
A = np.arange(4)  # [0,1,2,3]
A = np.arange(1,10,2)  # [1,3,5,7,9]

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
np.zeros_lik(A)    # A와 같은 크기의 0으로 채워진 array
```

### Numpy 배열 슬라이싱

```python
A[0]  # 1st row
A[0, 1]  # 1st row의 2nd 요소
A[:, 0]  # 1st column

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

### 차원 변경하기
```python
np.reshape(a, b)  # 길이가 12인 1차원 배열을 3x4로 바꾸려면 (3,4)로 지정하거나 (3,-1)로 지정
```

### 난수 생성하기

```python
# 일정 범위 내의 정수 배열 생성
np.random.randint(0, 20, 15)           # 0과 20 사이의 수 중 15개를 랜덤하게 생성
np.random.randint(0, 20, size=(4,3))   # 0과 20 사이의 수 중 12개를 랜덤하게 생성해 4x3 배열로

# 0,1 사이의 배열 생성
np.random.rand(n)  # n개의 요소 랜덤하게 생성
np.random.rand(n, m)  # n x m 배열 생성

```

### 간단한 통계 내기

```python
np.min()
np.max()
np.sum()  # 모든 요소를 다 더한 결과
np.mean()
np.var()
np.std()
np.abs()
np.absolute()
np.maximum(A, B)  # A,B의 각 요소중 큰것만 뽑아줌
np.sum(A, axis=0)  # A가 [m, n, k] 행렬일 때 axis=0 요소를 없애는 방향으로 더함, 즉, [n, k]가 리턴되도록 더함
np.unique(A)  # unique한 값을 리턴
np.bincount(A)  # 오름차순으로 정렬 후 빈도를 계산해서 반환
np.argmax(A, axis=0)  # 각 차원별 최대값의 index를 반환 axis=0이면 row끼리 비교, axis=1이면 column끼리 
np.argmin(A, axis=0)  # 각 차원별 최소값의 index를 반환
```

### 값 비교하기

```python
A > 0  # 조건에 대한 결과를 True, False로 반환
np.sum(A > 0)  # 조건을 만족하는 True의 개수
np.any(A > 0)  # 조건을 만족하는 값이 하나라도 있으면 True, 없으면 False
np.all(A > 0)  # 배열의 모든 요소가 조건을 만족해야 True, 아니면 False
A > B  # 배열의 각 원소를 비교해 True, False로 반환
A = B  # 두 배열이 같으면 True, 다르면 False
np.where(A > 0)  # 조건을 만족하는 요소만 반환
np.where(A > 0, 1, -1)  # 요소가 0보다 크면 1로 반환, 작으면 -1로 반환
```

### array 연산

```python
A + 3    # 각 요소에 3을 더함
A + B
A - B
A * B
A / B
A**2
```

### array 정렬

```python
A = np.array([1, 3, 4, 2])
s = A.argsort()  # array A의 값이 작은 순서대로 index를 기록 [0, 3, 1, 2]
s1 = A.argsort()[::-1]  # 값이 큰 순서대로 index 기록
A[s]  # s에 따라 정렬됨
```

### array를 index로 지정해서 array 슬라이싱 하기

```python
inds = np.array([3, 7, 8, 12])
print(x[inds])
```

### Linear Algebra

```python
import numpy as n
from numpy import linalg

A = [[4, 6, 2],
     [3, 4, 1],
     [2, 8, 13]]
s = [9, 7, 2]

Ainv = np.linalg.inv(A)
# Ar = s 일 때 r 구하기
r = np.linalg.solve(A, s)

A.T  # transpose
transpose(A)  # transpose
np.dot(A, B)  # dot product
A @ B  # matrix-matrix product, matrix-vector product
linalg.det(A)  # determinant 구함
linalg.norm(A)  # 벡터의 크기 구함
eigenvalues, eigenvectors = linalg.eig(A)
gaBasis(A)  # matrix of orthonormal column vectors 구함
```

### 딕셔너리를 생성하는 여러가지 방법

```python
num_dict = {}

for i in [1,2,2,3,3,3,4,4,4,4]:
  try:
    num_dict[i] += 1
  except:
    num_dict[i] = 1
  
```

Reference
https://docs.scipy.org/doc/numpy/index.html
https://docs.scipy.org/doc/numpy/reference/routines.html 





