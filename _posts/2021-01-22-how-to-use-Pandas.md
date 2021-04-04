---
title: "파이썬 판다스 팁 (Python Pandas Tips and Methods)"
date: 2021-01-22 00:00:00 -0400
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
df.head()
df.tail()
df.info()  #cardinality, dtype 확인 (df.dtypes 로도 확인 가능)
df.describe()  #non-object type 변수들의 간단한 통계
df.describe(include = "O")  #object type 변수들의 간단한 통계
df.shape  #행, 열 개수
df.columns
```

### data 타입 변경

```python
df.astype('float') #df 전체 변경
df.astype({'columnA':'object'}) #특정 열만 변경
pd.to_numeric(df['colA'])
```

### 슬라이싱

```python
df.iloc[1, 1]  #index로 찾기
df.iloc[34:39, [0, 2, 5, 7]]  #범위로 선택하거나, []안에 넣어 여러개를 선택
df.loc['indexA', 'columnA']  #값으로 찾기
df.loc[:, ['columnA', 'columnB']]  #행, 열 전체를 선택할 때는 :

rows = list(range(5)) + [35, 36]
df.iloc[rows]

cols = df.columns[:3].to_list() + ["columnA", "columnB"]
df.loc[:, cols]
```

### 행,열 삭제하기

```python
#행 삭제
df.drop(df.index[1:2], axis=0, inplace=True)

#열 삭제
df.drop(['columnA', 'columnB'], axis=1, inplace=True)
del df['columnA']

#특정 조건으로 필터링해서 행 삭제
index = df[df['columnA'==0 | 'columnB' > 3]].index  # &나 |로 연결
df.drop(index, inplace=True)

#중복되는 행 삭제
df.drop_duplicates(inplace=True, ignore_index=True)  #ignore_index를 True로 하면 index를 0부터 리셋해줌
```

### 행,열 추가하기

```python
#열 추가
df['newColumn'] = 0
df.loc[:,'newColumn'] = ['val1','val2','val3', ...]

#행 추가
new_row = {'columnA':3, 'columnB':5}
df.append(new_row, ignore_index=True)

#행 순서 변경
df = df[['columnB', 'columnA']]  #A,B 순서였던 것을 B,A로 변경
```

### missing value 처리

```python
df.isnull().sum()  #missing value가 몇 개인지 알아보기

df.dropna(axis=0, inplace=True)  #행을 삭제
df.dropna(axis=1, inplace=True)  #열을 삭제
df.dropna(axis=1, how='all', inplace=True)  #열의 모든 값이 na일 때 그 열을 삭제

df.fillna(0, inplace=True, downcast='infer')  #downcast='infer'는 float를 int로 변경
df.fillna(method='ffill')  #value 없이 method=ffill이면 바로 윗행의 값을 가져옴
df.ffill()  #위와 같은 기능
```

### 행,열 이름 변경

```python
df.rename(columns = {'A':'B'}, index = {'C':'D'})  #A는 B로 바꾸고 C는 D로 바꿈
df.reset_index(drop = True, inplace=True)  #행의 index를 0,1,2,... 순서대로 다시 매김
df.set_index('columnA', inplace=True)  #행의 index를 columnA의 값으로 바꿈
```

### 행,열 합치기

```python
#데이터프레임 합치기 (위아래로)
pd.concat(list)
pd.concat(df1, df2)

#데이터프레임 합치기 (옆으로)
df.merge(df1, how = 'left', on = 'fullVisitorId')
```

### Lambda 함수로 열의 값을 합치거나 나누기

```python
#열 합치기
def mergeCols(a, b):
    return str(a) + "," + str(b)
df['AB'] = df.apply(lambda x: mergeCols(x['A'], x['B']), axis = 1)

#열 나누기
def splitCols(row):
    return row.split(',')
df = df.merge(df['AB'].apply(lambda x: pd.Series({'A':splitCols(x)[0], 'B':splitCols(x)[1]})), 
    left_index = True, right_index = True)  #join key로 양쪽의 index를 사용
del df['AB']  #원본 열은 삭제
```

### 특정 열을 기준으로 그룹화 하기
```python
df.groupby('colA').mean()
df.groupby(['colA', 'colB']).mean()

# colA의 값을 기준으로 colB의 값을 그룹화하여 list로 만듦
df.groupby('colA')['colB'].apply(list)

# colB, colC, colD의 값을 각각 다른 조건으로 그룹화
df.groupby('colA').agg({'colB':'sum' ,'colC':'count'}) 
df.groupby('colA').agg({'colB':'count', 'colC':lambda row: ', '.join(row)}) 
```

### 값 정렬하기

```python
df.sort_values(by = ['colA','colB'], axis = 0, ascending = True, inplace = True)
df.sort_index(axis = 0, ascending = True, inplace = True)
```
