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
```

### data 타입 변경

```python
PySpark Data Types(https://spark.apache.org/docs/latest/sql-ref-datatypes.html)

from pyspark.sql.types import IntegerType
df.withColumn("colA", df["colA"].cast(IntegerType()))
```

### 슬라이싱

```python

```

### 행,열 삭제하기

```python

```

### 행,열 추가하기

```python

```

### missing value 처리

```python

```

### 행,열 이름 변경

```python

```

### 행,열 합치기

```python

```

### 값 정렬하기

```python

```
