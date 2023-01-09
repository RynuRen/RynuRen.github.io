---
layout: jupyter
title: 자연어 처리 - 텍스트 유사도
published: true
date: 2023-01-09
description: 텍스트 유사도
comments: true
categories:
 - Need_modify
tags:
 - Need_modify
toc: true
toc_sticky: true
toc_label: 텍스트 유사도
use_math: true
---
---
# 텍스트 유사도

## 코사인 유사도(Cos)
가장 많이 사용되는 기법

* 백터 내적

$$\overrightarrow{a}\cdot{}\overrightarrow{b}=\vert\overrightarrow{a}\vert*\vert\overrightarrow{b}\vert*cos{θ}$$

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import numpy as np
from numpy import dot
from numpy.linalg import norm
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
# 코사인 세타를 구하는 함수
def cos_sim(A, B):
    return dot(A, B)/(norm(A)*norm(B)) # norm(A): 벡터 A의 길이
```

</div>

```
문서 1: 나는 치킨 좋아요
문서 2: 나는 피자 좋아요
문서 3: 나는 피자 좋아요 나는 피자 좋아요

          나는  치킨  피자  좋아요
문서 1:     1     1     0     1
문서 2:     1     0     1     1
문서 3:     2     0     2     2
```

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
doc1 = np.array([1,1,0,1])
doc2 = np.array([1,0,1,1])
doc3 = np.array([2,0,2,2])
```

</div>

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
print(cos_sim(doc1, doc2))
print(cos_sim(doc1, doc3))
print(cos_sim(doc2, doc3)) # 각도, 방향이 같으므로 두 문서의 유사도는 100%이다.
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
0.6666666666666667
0.6666666666666667
1.0000000000000002

```

### sklearn - cosine_similarity

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.metrics.pairwise import cosine_similarity
```

</div>

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
print(cosine_similarity([doc1], [doc2]))
print(cosine_similarity([doc1], [doc3]))
print(cosine_similarity([doc2], [doc3]))
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>

{:.output_stream}

```
[[0.66666667]]
[[0.66666667]]
[[1.]]

```

## 유클리드 거리
유클리디언 유사도(Euclidean Distance)

말의 어조, 억양에 대해서는 의미가 있으나 일반적으로 잘 사용하지 않는다.

* 두 점 사이의 거리 공식

$$d=\sqrt{(x_1-x_2)^2+(y_1-y_2)^2}$$

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
def eu_dist(A, B):
    return np.sqrt(np.sum((A-B)**2))
```

</div>

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
print(eu_dist(doc1, doc2))
print(eu_dist(doc1, doc3))
print(eu_dist(doc2, doc3)) # 각도가 아니라 거리이므로 강도의 차이가 있다.
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>

{:.output_stream}

```
1.4142135623730951
2.6457513110645907
1.7320508075688772

```

### sklearn - euclidean_distances

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.metrics.pairwise import euclidean_distances
```

</div>

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
print(euclidean_distances([doc1], [doc2]))
print(euclidean_distances([doc1], [doc3]))
print(euclidean_distances([doc2], [doc3]))
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>

{:.output_stream}

```
[[1.41421356]]
[[2.64575131]]
[[1.73205081]]

```

## 맨해튼 유사도
Manhattan Similarity

정수 처리가 필요한 경우 사용한다.

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.metrics.pairwise import manhattan_distances
```

</div>

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
print(manhattan_distances([doc1], [doc2]))
print(manhattan_distances([doc1], [doc3]))
print(manhattan_distances([doc2], [doc3]))
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
</div>

{:.output_stream}

```
[[2.]]
[[5.]]
[[3.]]

```

## 자카드 유사도
Jaccard Similarity

공통된 단어가 얼마나 있는지 나타낸다.

* $$J(A, B) = \frac{\vert{A∩B}\vert}{\vert{A∪B}\vert}$$

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
doc1 = '나는 치킨 좋아요'
doc2 = '나는 피자 좋아요'
doc3 = '나는 피자 좋아요 나는 피자 좋아요'

token_doc1 = doc1.split()
token_doc2 = doc2.split()
token_doc3 = doc3.split()

# 합집합
union = set(token_doc1).union(set(token_doc2))
# 교집합
intsec = set(token_doc1).intersection(set(token_doc2))
```

</div>

<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
union, intsec
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
</div>




{:.output_data_text}

```
({'나는', '좋아요', '치킨', '피자'}, {'나는', '좋아요'})
```



<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
len(intsec)/len(union)
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>




{:.output_data_text}

```
0.5
```


