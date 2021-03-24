---
title: "PySpark 활용하기 (PySpark Examples)"
date: 2021-03-14 00:00:00 -0400
permalink: '/pyspark/'
categories: Python
---


### csv파일 읽기

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName('appName').getOrCreate()
df = spark.read.csv('파일경로/파일명.csv', header=True, inferSchema=True)
```

### Schema 지정 후 csv파일 읽기

```python
from pyspark.sql import SparkSession
from pyspark.sql.types import StructField,StringType,StructType

spark = SparkSession.builder.appName('appName').getOrCreate()
data_schema = [StructField("colA", StringType(), True),
               StructField("colB", StringType(), True)]
data_struc = StructType(fields=data_schema)
df = spark.read.csv('파일경로/파일명.csv', header=True, schema=data_struc)
```

### data 구조 살피기

```python
df.show()
df.printSchema()
df.head()
df.tail()
df.describe()
df.count()
df.columns
type(df['colA'])
```

### data 타입 변경

```python
PySpark Data Types(https://spark.apache.org/docs/latest/sql-ref-datatypes.html)

from pyspark.sql.types import IntegerType
df.withColumn("colA", df["colA"].cast(IntegerType()))
```

### 슬라이싱

```python
df.select("colA")
df.select(["colA", "colB"])
df.filter(df["colA"] > 2)
df.filter((df["colA"] > 2) & (df["colB"] > 10))
df.limit(10) # 상위 10개 row
```

### 행,열 삭제하기

```python
df.drop('colA', 'colB')
```

### 행,열 추가하기

```python
df.withColumn('new colA', df['colA']+5)
df.withColumn('new colA', lit(5))
```

### missing value 처리

```python
df.na.drop()
df.na.drop(subset=['colA'])
df.na.fill(value)
df.na.fill(value, subset=['colA'])
```

### 행,열 이름 변경

```python
from pyspark.sql.functions import col, sum

df.select(col('colA').alias('new colA'))
df.select(sum('colA').alias('sum colA'))
df.withColumnRenamed('colA','colB')
```

### 행,열 합치기

```python

```

### 값 정렬하기

```python
df.orderBy(df['colA'])
df.orderBy(df['colA'].desc())
```

### 전체열 집계

```python
from pyspark.sql.functions import sum

df.select(sum('colA'))  # colA의 전체 합을 리턴
df.select(countDistinct('colA'))  #colA의 unique한 값이 몇개인지 리턴
df.select('colA').distinct()  # colA의 unique한 값 리턴
```

### 그룹별 집계

```python
from pyspark.sql.functions import sum

df.groupBy('colA').sum()  # colA의 값으로 그룹화 하여 나머지 열의 sum을 리턴
df.groupBy('colA').agg({'colB':'sum'})
```

### 날짜 가공

```python
from pyspark.sql.functions import format_number,dayofmonth,hour,dayofyear,month,year,weekofyear,date_format

#colA가 timestamp 형식일 때
df.select(year(df['colA']))  # '년도'만 리턴
df.select(month(df['colA']))  # '월'만 리턴
df.select(dayofmonth(df['colA']))  # '일'만 리턴
df.select(hour(df['colA']))  # '시간'만 리턴
df.select(hour(df['colA']))  # '시간'만 리턴

date format (https://spark.apache.org/docs/latest/sql-ref-datetime-pattern.html)
```
