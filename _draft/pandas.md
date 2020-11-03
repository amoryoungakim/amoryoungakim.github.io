---
title: "파이썬 판다스 활용하기 (Python Pandas Examples)"
date: 2020-10-17 00:00:00 -0400
permalink: '/pandas/'
categories: Python
---

실무에서 정말 자주 사용하는 pandas 기능들. 

### csv파일 읽기

```python
import pandas as pd

df = pd.read_csv('파일경로/파일명.csv')
```

### data 구조 살피기

```python
import pandas as pd

df.head()
df.tail()
df.info()  #cardinality, dtype 확인
df.describe()  #non-object type 변수들의 간단한 통계
df.describe(include = "O")  #object type 변수들의 간단한 통계
df.shape  #행, 열 개수
df.columns
```

### 슬라이싱

```python
import pandas as pd

df.iloc[1, 1]
df.iloc[34:39, [0, 2, 5, 7]]  #범위로 선택하거나, []안에 넣어 여러개를 선택
df.loc['indexA', 'columnA']
df.loc[:, ['columnA', 'columnB']]  #행, 열 전체를 선택할 때는 :

rows = list(range(5)) + [35, 36]
df.iloc[rows]

cols = df.columns[:3].to_list() + ["columnA", "columnB"]
df.loc[:, cols]
```

### 행, 열 삭제

```python
import pandas as pd

df.iloc[1, 1]
df.iloc[34:39, [0, 2, 5, 7]]  #범위로 선택하거나, []안에 넣어 여러개를 선택
df.loc['indexA', 'columnA']
df.loc[:, ['columnA', 'columnB']]  #행, 열 전체를 선택할 때는 :

rows = list(range(5)) + [35, 36]
df.iloc[rows]

cols = df.columns[:3].to_list() + ["columnA", "columnB"]
df.loc[:, cols]
```



df_path2 = df_path2.drop(df_path2.index[500:1146])

### 행, 열 이름 변경

```python
import pandas as pd


df.rename
pd.concat(list)
df.fillna
df.drop_duplicates
df.sort_values

apply 쓰는법 복습
```
