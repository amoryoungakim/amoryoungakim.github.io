---
title: "Python Tensorflow Tips and Examples(파이썬 텐서플로 활용 팁)"
date: 2021-10-11 00:00:00 -0400
permalink: '/tensorflow/'
categories: Python
---

### Create tensor and simple calculation

```python
import tensorflow as tf

a = tf.constant([[1, 2],
                 [3, 4]])
b = tf.constant([[1, 1],
                 [1, 1]])

tf.add(a, b)
a + b
tf.multiply(a, b)
a * b
tf.matmul(a, b)  #matrix multiply
a @ b
```

```python
x = tf.Variable(2.0, dtype=tf.float32, name='my_variable')
```

### Tensor Shapes

```python
d = tf.zeros([3, 2, 4, 5])

d.dtype     #float32
d.ndim      #4
d.shape     #(3,2,4,5)
d.shape[0]  #3
d.shape[-1] #5
tf.size(d).numpy()  #total number of elements 120
```

### Max, Min

```python
c = tf.constant([[4.0, 5.0], [10.0, 1.0]])

tf.reduce_max(c)  #returns maximum element
tf.reduce_min(c)  #returns minimum element
tf.argmax(c)      #returns index of maximum element
tf.argmin(c)      #returns index of minimum element
```

### Slicing

```python
e = tf.constant([0, 1, 1, 2, 3, 5, 8, 13, 21, 34])

print("First:", rank_1_tensor[0].numpy())
print("Second:", rank_1_tensor[1].numpy())
print("Last:", rank_1_tensor[-1].numpy())
print("Everything:", rank_1_tensor[:].numpy())
print("Before 4:", rank_1_tensor[:4].numpy())
print("From 4 to the end:", rank_1_tensor[4:].numpy())
print("From 2, before 7:", rank_1_tensor[2:7].numpy())
print("Every other item:", rank_1_tensor[::2].numpy())
print("Reversed:", rank_1_tensor[::-1].numpy())

f = tf.constant([[1, 2],[3, 4],[5, 6]])
                             
print(rank_2_tensor[1, 1].numpy())
print("Second row:", rank_2_tensor[1, :].numpy())
print("Second column:", rank_2_tensor[:, 1].numpy())
print("Last row:", rank_2_tensor[-1, :].numpy())
print("First item in last column:", rank_2_tensor[0, -1].numpy())
print("Skip the first row:")
print(rank_2_tensor[1:, :].numpy(), "\n")
```

### Others

```python
c = tf.constant([[4.0, 5.0], [10.0, 1.0]])

tf.nn.softmax(c)  #compute the softmax
```

Reference
https://github.com/GoogleCloudPlatform/training-data-analyst




