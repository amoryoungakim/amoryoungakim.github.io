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

### df 생성하기

```python
df = pd.DataFrame([[1,2,3], [4,5,6], [7,8,9]],
                columns = ['a', 'b', 'c'],
                index = ['A', 'B'])
```

### data 구조 살피기

```python
df.head()
df.tail()
df.info()  #cardinality, dtype 확인 (df.dtypes 로도 확인 가능)
df.describe()  #non-object type 변수들의 간단한 통계
df.describe(include = "O")  #object type 변수들의 간단한 통계
df.shape  #행, 열 개수
df.size  #데이터 개수 5x2 이면 10
df.columns
```

### 간단한 통계 내기

```python
len(df)  #행 개수
df.count()  #null 제외한 행 개수
df.sum()
df.mean()
df.std()
df.min()
df.max()
len(df.columns)  #열 개수
pd.unique(df['colA'])  #colA를 unique한 값만 가져오기
len(pd.unique(df['colA']))  #colA의 unique한 값의 개수 세기
df['colA'].value_counts(normalize=False, sort=True, ascending=False, dropna=True, bins=None)  #colA를 구성하는 값들이 총 몇 번 출현하는지 빈도 세기
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

### 필터링

```python
df.loc[df['colA'] == 'A']  #colA 값이 A인 행만 보기
df.loc[(df['colA'] == 'A') & (df['colB'] == 'B')]  #colA 값이 A이고 colB 값이 B인 행만 보기 (or 조건일 때는 | 사용)
df.loc[df['colA'].str.contains('apple')]  #colA 값이 apple을 포함하는 행만 보기
df.loc[df['colA'].str.startswith('A')]  #colA 값이 A로 시작하는 행만 보기
df.loc[df['colA'].str.isnumber()]  #colA 값을 구성하는 string이 모두 숫자인 행만 보기, 예. "093482"
df.loc[df['colA'].isin(['A', 'B', 'C']]  #colA 값이 A~C인 행만 보기
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
df[df['colA'].isnull()]  #colA에 missing value가 있는 행만 가져오기

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
pd.concat([df1, df2])

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

### 값 정렬하기

```python
df.sort_values(by = ['colA','colB'], axis = 0, ascending = True, inplace = True)
df.sort_index(axis = 0, ascending = True, inplace = True)
```

### 값 변경하기

```python
df.loc[df['colA'] == 'A'] = 'AB'  #colA의 값 중 A를 AB로 변경
df['colA'] = df['colA'].apply(lambda x: 1 if x=='A' else 0)  #colA의 값이 A면 1로 바꾸고 아니면 0으로 바꿈
df['colA'] = df['colA'].apply(lambda x: 'alpha' if x=='A' else 'beta' if x=='B' else x)  #colA의 값이 A면 alpha로 바꾸고 B면 beta로 그외는 그냥 유지
```

### Index 변경

```python
df.set_index('colA')  #index를 colA로 변경
df.reset_index()  #index를 0부터 순서대로 다시 매겨줌 (일부 행을 drop시킨 뒤 종종 사용)
```

### 기타 세팅값

```python
pd.set_option('display.max_row', 100)  #행을 최대 100개까지 표시
pd.set_option('display.max_columns', 100)  #열을 최대 100개까지 
```
