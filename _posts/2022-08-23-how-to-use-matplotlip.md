---
title: "matplotlip 예시"
date: 2022-11-23 00:00:00 -0400
permalink: '/matplotlip/'
categories: Python
---

### line graph 그리기

```python
import matplotlib.pyplot as plt

df['High'].plot()
plt.show()
```

### 두 개의 line graph 그리기

```python
df[['Close', 'Adj Close']].plot()
plt.show()
```
