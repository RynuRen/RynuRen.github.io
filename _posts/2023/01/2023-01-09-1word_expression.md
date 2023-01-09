---
layout: jupyter
title: 자연어 처리 - 단어 표현
published: true
date: 2023-01-09
description: 단어 표현
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: 단어 표현
---
---
# 단어 표현

단어 벡터(word vector), 워드 임베딩(word embedding)

## 원-핫 인코딩(one-hot encoding)

매우 간단하고 이해하기 쉬움

실제 자연어 처리를 할 경우 수십~수백만 개의 단어를 표현해야 하는데 각 단어 벡터의 크기가 너무 커져 공간을 많이 사용 -> 매우 비효율적  
단어의 의미나 특성 같은 것들이 전혀 표현되지 않음

## 카운트 기반

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
text = ['''
오늘 네가
보고싶다
널 다시 품에
안아보고 싶다
오늘 네가 난
생각난다
너랑 같이 산책하던
그곳에 서있다
너의 체온이
기억난다
따뜻하게 내 쉬던
숨소리 들려
오늘 네가
온 것 같아
우리 같이 잠들던
벤치에 기대니
시간이 흘러흘러
다시 만날 순 없지
그래도 보고싶다 널
그리운 내 사랑아
오늘 네가 정말
보고싶다
너를 다시 내 품에
안아보고 싶다
오늘 네가 난
생각난다
너를 쓰다듬던 손이
너를 기억한다
니가 보고싶다
''']
```

</div>

### sklearn - CountVectorizer

단어들의 카운트(출현 빈도;frequency)로 여러 문서들을 벡터화  
2글자 이상만 체크

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.feature_extraction.text import CountVectorizer
import pandas as pd
import numpy as np
```

</div>

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
cv = CountVectorizer()
countv = cv.fit_transform(text).toarray()
countv
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>




{:.output_data_text}

```
array([[1, 2, 1, 1, 1, 1, 1, 1, 1, 3, 1, 5, 1, 3, 1, 1, 1, 1, 4, 1, 1, 2,
        1, 1, 1, 1, 1, 2, 1, 2, 1, 5, 1, 1, 1, 1, 2, 1]], dtype=int64)
```



<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
print(cv.vocabulary_)
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
{'오늘': 31, '네가': 11, '보고싶다': 18, '다시': 13, '품에': 36, '안아보고': 29, '싶다': 27, '생각난다': 21, '너랑': 8, '같이': 1, '산책하던': 20, '그곳에': 2, '서있다': 22, '너의': 10, '체온이': 35, '기억난다': 6, '따뜻하게': 15, '쉬던': 25, '숨소리': 24, '들려': 14, '같아': 0, '우리': 32, '잠들던': 33, '벤치에': 17, '기대니': 5, '시간이': 26, '흘러흘러': 37, '만날': 16, '없지': 30, '그래도': 3, '그리운': 4, '사랑아': 19, '정말': 34, '너를': 9, '쓰다듬던': 28, '손이': 23, '기억한다': 7, '니가': 12}

```

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
pd.DataFrame({'vocab':sorted(list(cv.vocabulary_.keys())), 'CountVector':countv[0]})
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vocab</th>
      <th>CountVector</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>같아</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>같이</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>그곳에</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>그래도</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>그리운</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>기대니</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>기억난다</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>기억한다</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>너랑</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>너를</td>
      <td>3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>너의</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>네가</td>
      <td>5</td>
    </tr>
    <tr>
      <th>12</th>
      <td>니가</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>다시</td>
      <td>3</td>
    </tr>
    <tr>
      <th>14</th>
      <td>들려</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>따뜻하게</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>만날</td>
      <td>1</td>
    </tr>
    <tr>
      <th>17</th>
      <td>벤치에</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>보고싶다</td>
      <td>4</td>
    </tr>
    <tr>
      <th>19</th>
      <td>사랑아</td>
      <td>1</td>
    </tr>
    <tr>
      <th>20</th>
      <td>산책하던</td>
      <td>1</td>
    </tr>
    <tr>
      <th>21</th>
      <td>생각난다</td>
      <td>2</td>
    </tr>
    <tr>
      <th>22</th>
      <td>서있다</td>
      <td>1</td>
    </tr>
    <tr>
      <th>23</th>
      <td>손이</td>
      <td>1</td>
    </tr>
    <tr>
      <th>24</th>
      <td>숨소리</td>
      <td>1</td>
    </tr>
    <tr>
      <th>25</th>
      <td>쉬던</td>
      <td>1</td>
    </tr>
    <tr>
      <th>26</th>
      <td>시간이</td>
      <td>1</td>
    </tr>
    <tr>
      <th>27</th>
      <td>싶다</td>
      <td>2</td>
    </tr>
    <tr>
      <th>28</th>
      <td>쓰다듬던</td>
      <td>1</td>
    </tr>
    <tr>
      <th>29</th>
      <td>안아보고</td>
      <td>2</td>
    </tr>
    <tr>
      <th>30</th>
      <td>없지</td>
      <td>1</td>
    </tr>
    <tr>
      <th>31</th>
      <td>오늘</td>
      <td>5</td>
    </tr>
    <tr>
      <th>32</th>
      <td>우리</td>
      <td>1</td>
    </tr>
    <tr>
      <th>33</th>
      <td>잠들던</td>
      <td>1</td>
    </tr>
    <tr>
      <th>34</th>
      <td>정말</td>
      <td>1</td>
    </tr>
    <tr>
      <th>35</th>
      <td>체온이</td>
      <td>1</td>
    </tr>
    <tr>
      <th>36</th>
      <td>품에</td>
      <td>2</td>
    </tr>
    <tr>
      <th>37</th>
      <td>흘러흘러</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### sklearn - TfidfVectorizer

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.feature_extraction.text import TfidfVectorizer
```

</div>

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
ti = TfidfVectorizer()
tfidfv = ti.fit_transform(text).toarray()
tfidfv
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>




{:.output_data_text}

```
array([[0.08703883, 0.17407766, 0.08703883, 0.08703883, 0.08703883,
        0.08703883, 0.08703883, 0.08703883, 0.08703883, 0.26111648,
        0.08703883, 0.43519414, 0.08703883, 0.26111648, 0.08703883,
        0.08703883, 0.08703883, 0.08703883, 0.34815531, 0.08703883,
        0.08703883, 0.17407766, 0.08703883, 0.08703883, 0.08703883,
        0.08703883, 0.08703883, 0.17407766, 0.08703883, 0.17407766,
        0.08703883, 0.43519414, 0.08703883, 0.08703883, 0.08703883,
        0.08703883, 0.17407766, 0.08703883]])
```



<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
print(ti.vocabulary_)
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>

{:.output_stream}

```
{'오늘': 31, '네가': 11, '보고싶다': 18, '다시': 13, '품에': 36, '안아보고': 29, '싶다': 27, '생각난다': 21, '너랑': 8, '같이': 1, '산책하던': 20, '그곳에': 2, '서있다': 22, '너의': 10, '체온이': 35, '기억난다': 6, '따뜻하게': 15, '쉬던': 25, '숨소리': 24, '들려': 14, '같아': 0, '우리': 32, '잠들던': 33, '벤치에': 17, '기대니': 5, '시간이': 26, '흘러흘러': 37, '만날': 16, '없지': 30, '그래도': 3, '그리운': 4, '사랑아': 19, '정말': 34, '너를': 9, '쓰다듬던': 28, '손이': 23, '기억한다': 7, '니가': 12}

```

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
pd.DataFrame({'vocab':sorted(list(ti.vocabulary_.keys())), 'TfidfVector':tfidfv[0]})
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>vocab</th>
      <th>TfidfVector</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>같아</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>1</th>
      <td>같이</td>
      <td>0.174078</td>
    </tr>
    <tr>
      <th>2</th>
      <td>그곳에</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>3</th>
      <td>그래도</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>4</th>
      <td>그리운</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>5</th>
      <td>기대니</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>6</th>
      <td>기억난다</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>7</th>
      <td>기억한다</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>8</th>
      <td>너랑</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>9</th>
      <td>너를</td>
      <td>0.261116</td>
    </tr>
    <tr>
      <th>10</th>
      <td>너의</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>11</th>
      <td>네가</td>
      <td>0.435194</td>
    </tr>
    <tr>
      <th>12</th>
      <td>니가</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>13</th>
      <td>다시</td>
      <td>0.261116</td>
    </tr>
    <tr>
      <th>14</th>
      <td>들려</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>15</th>
      <td>따뜻하게</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>16</th>
      <td>만날</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>17</th>
      <td>벤치에</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>18</th>
      <td>보고싶다</td>
      <td>0.348155</td>
    </tr>
    <tr>
      <th>19</th>
      <td>사랑아</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>20</th>
      <td>산책하던</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>21</th>
      <td>생각난다</td>
      <td>0.174078</td>
    </tr>
    <tr>
      <th>22</th>
      <td>서있다</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>23</th>
      <td>손이</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>24</th>
      <td>숨소리</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>25</th>
      <td>쉬던</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>26</th>
      <td>시간이</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>27</th>
      <td>싶다</td>
      <td>0.174078</td>
    </tr>
    <tr>
      <th>28</th>
      <td>쓰다듬던</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>29</th>
      <td>안아보고</td>
      <td>0.174078</td>
    </tr>
    <tr>
      <th>30</th>
      <td>없지</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>31</th>
      <td>오늘</td>
      <td>0.435194</td>
    </tr>
    <tr>
      <th>32</th>
      <td>우리</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>33</th>
      <td>잠들던</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>34</th>
      <td>정말</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>35</th>
      <td>체온이</td>
      <td>0.087039</td>
    </tr>
    <tr>
      <th>36</th>
      <td>품에</td>
      <td>0.174078</td>
    </tr>
    <tr>
      <th>37</th>
      <td>흘러흘러</td>
      <td>0.087039</td>
    </tr>
  </tbody>
</table>
</div>
</div>



* Tfidf: Term Frequency-Inverse Document Frequency  
(TF: 단어의 반복)/(IDF: 문서에서 반복)  
모든 문서에 상투적으로 사용되는 단어(중요도가 낮은 단어)를 나눠서 제어해준다. - 무의미성

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
documents = [
    '먹고 싶은 치킨',
    '먹고 싶은 피자',
    '맛있지만 살이 찌는 피자 피자',
    '나는 야식이 좋아요'
]
```

</div>

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
vocab = list(set(word for document in documents for word in document.split()))
print(vocab)

vocab2 = []
for document in documents:
    for word in document.split():
        vocab2.append(word)
print(list(set(vocab2)))
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
</div>

{:.output_stream}

```
['나는', '살이', '치킨', '야식이', '피자', '찌는', '좋아요', '싶은', '먹고', '맛있지만']
['나는', '살이', '치킨', '야식이', '피자', '찌는', '좋아요', '싶은', '먹고', '맛있지만']

```

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
vocab.sort()
len(vocab), vocab
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
</div>




{:.output_data_text}

```
(10, ['나는', '맛있지만', '먹고', '살이', '싶은', '야식이', '좋아요', '찌는', '치킨', '피자'])
```



<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
# 문서의 길이
N = len(documents)
N
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
</div>




{:.output_data_text}

```
4
```



### TF 코드 구현
CounterVectorizer

<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
# TF 구하는 함수
def tf(text, d):
    return d.count(text)
```

</div>

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
tf('피자', '맛있지만 살이 찌는 피자 피자')
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>




{:.output_data_text}

```
2
```



<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
result = []
for i in range(N):
    result.append([])
    d = documents[i]
    for j in range(len(vocab)):
        t = vocab[j]
        result[-1].append(tf(t, d))
        
result # Bag of words: 분류 하기 위함, 특히 감성 평가
```

</div>

<div class="output_prompt">
Out&nbsp;[16]:
</div>




{:.output_data_text}

```
[[0, 0, 1, 0, 1, 0, 0, 0, 1, 0],
 [0, 0, 1, 0, 1, 0, 0, 0, 0, 1],
 [0, 1, 0, 1, 0, 0, 0, 1, 0, 2],
 [1, 0, 0, 0, 0, 1, 1, 0, 0, 0]]
```



<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
# CountVectorizer와 비교
cv = CountVectorizer()
countv = cv.fit_transform(documents).toarray()
countv
```

</div>

<div class="output_prompt">
Out&nbsp;[17]:
</div>




{:.output_data_text}

```
array([[0, 0, 1, 0, 1, 0, 0, 0, 1, 0],
       [0, 0, 1, 0, 1, 0, 0, 0, 0, 1],
       [0, 1, 0, 1, 0, 0, 0, 1, 0, 2],
       [1, 0, 0, 0, 0, 1, 1, 0, 0, 0]], dtype=int64)
```



<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
tf_res = pd.DataFrame(result, columns=vocab)
tf_res
```

</div>

<div class="output_prompt">
Out&nbsp;[18]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>나는</th>
      <th>맛있지만</th>
      <th>먹고</th>
      <th>살이</th>
      <th>싶은</th>
      <th>야식이</th>
      <th>좋아요</th>
      <th>찌는</th>
      <th>치킨</th>
      <th>피자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
</div>



'먹고 싶은 치킨',  
'먹고 싶은 피자',  
'맛있지만 살이 찌는 피자 피자',  
'나는 야식이 좋아요'

### IDF 코드 구현
T
문서마다 많이 나온 단어는 값이 작음(inverse)

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
# IDF 구하는 함수
from math import log

def idf(text):
    df = 0
    for doc in documents:
        df += t in doc # 문서마다 반복 수 카운트
    return log(N/(df+1)) # N:문서 수, df+1:zero division 방지, log:큰 값을 제한
```

</div>

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
result = []
for j in range(len(vocab)):
    t = vocab[j]
    result.append(idf(t))
np.array(result)
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
</div>




{:.output_data_text}

```
array([0.69314718, 0.69314718, 0.28768207, 0.69314718, 0.28768207,
       0.69314718, 0.69314718, 0.69314718, 0.69314718, 0.28768207])
```



<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
idf_result = pd.DataFrame(result, columns=['idf'], index=vocab)
idf_result
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idf</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>나는</th>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>맛있지만</th>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>먹고</th>
      <td>0.287682</td>
    </tr>
    <tr>
      <th>살이</th>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>싶은</th>
      <td>0.287682</td>
    </tr>
    <tr>
      <th>야식이</th>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>좋아요</th>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>찌는</th>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>치킨</th>
      <td>0.693147</td>
    </tr>
    <tr>
      <th>피자</th>
      <td>0.287682</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### TF-IDF 코드 구현
TfidfVectorizer

<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
# TF-IDF 구하는 함수
def tfidf(t, d):
    return tf(t, d) * idf(t)
```

</div>

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
result = []
for i in range(N):
    result.append([])
    d = documents[i]
    for j in range(len(vocab)):
        t = vocab[j]
        result[-1].append(tfidf(t, d))
np.array(result)
```

</div>

<div class="output_prompt">
Out&nbsp;[23]:
</div>




{:.output_data_text}

```
array([[0.        , 0.        , 0.28768207, 0.        , 0.28768207,
        0.        , 0.        , 0.        , 0.69314718, 0.        ],
       [0.        , 0.        , 0.28768207, 0.        , 0.28768207,
        0.        , 0.        , 0.        , 0.        , 0.28768207],
       [0.        , 0.69314718, 0.        , 0.69314718, 0.        ,
        0.        , 0.        , 0.69314718, 0.        , 0.57536414],
       [0.69314718, 0.        , 0.        , 0.        , 0.        ,
        0.69314718, 0.69314718, 0.        , 0.        , 0.        ]])
```



#### sklearn의 TfidfVectorizer 구현

<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
# TF 구하는 함수
def tf(text, d):
    return d.count(text)
```

</div>

<div class="in_prompt">
In&nbsp;[25]:
</div>

<div class="input_area" markdown="1">

```python
count_vec = []
for i in range(N):
    count_vec.append([])
    d = documents[i]
    for j in range(len(vocab)):
        t = vocab[j]
        count_vec[-1].append(tf(t, d))
count_vec
```

</div>

<div class="output_prompt">
Out&nbsp;[25]:
</div>




{:.output_data_text}

```
[[0, 0, 1, 0, 1, 0, 0, 0, 1, 0],
 [0, 0, 1, 0, 1, 0, 0, 0, 0, 1],
 [0, 1, 0, 1, 0, 0, 0, 1, 0, 2],
 [1, 0, 0, 0, 0, 1, 1, 0, 0, 0]]
```



보편적인 idf를 구하는 식에서 로그항의 분자에 1을 더해주고 로그항에 1을 더함

<div class="in_prompt">
In&nbsp;[26]:
</div>

<div class="input_area" markdown="1">

```python
from math import log

def s_idf(text):
    df = 0
    for doc in documents:
        df += t in doc
    return log((N+1)/(df+1))+1
```

</div>

<div class="in_prompt">
In&nbsp;[27]:
</div>

<div class="input_area" markdown="1">

```python
s_idf_rst = []
for j in range(len(vocab)):
    t = vocab[j]
    s_idf_rst.append(s_idf(t))
np.array(s_idf_rst)
```

</div>

<div class="output_prompt">
Out&nbsp;[27]:
</div>




{:.output_data_text}

```
array([1.91629073, 1.91629073, 1.51082562, 1.91629073, 1.51082562,
       1.91629073, 1.91629073, 1.91629073, 1.91629073, 1.51082562])
```



Tfidfvectorizer의 idf와 비교해 본다.

<div class="in_prompt">
In&nbsp;[28]:
</div>

<div class="input_area" markdown="1">

```python
ti = TfidfVectorizer()
ti.fit(documents)
ti.idf_
```

</div>

<div class="output_prompt">
Out&nbsp;[28]:
</div>




{:.output_data_text}

```
array([1.91629073, 1.91629073, 1.51082562, 1.91629073, 1.51082562,
       1.91629073, 1.91629073, 1.91629073, 1.91629073, 1.51082562])
```



구한 TF 벡터와 IDF 벡터를 곱해준다. 

<div class="in_prompt">
In&nbsp;[29]:
</div>

<div class="input_area" markdown="1">

```python
import numpy as np
np.multiply(np.array(count_vec), np.array(s_idf_rst))
```

</div>

<div class="output_prompt">
Out&nbsp;[29]:
</div>




{:.output_data_text}

```
array([[0.        , 0.        , 1.51082562, 0.        , 1.51082562,
        0.        , 0.        , 0.        , 1.91629073, 0.        ],
       [0.        , 0.        , 1.51082562, 0.        , 1.51082562,
        0.        , 0.        , 0.        , 0.        , 1.51082562],
       [0.        , 1.91629073, 0.        , 1.91629073, 0.        ,
        0.        , 0.        , 1.91629073, 0.        , 3.02165125],
       [1.91629073, 0.        , 0.        , 0.        , 0.        ,
        1.91629073, 1.91629073, 0.        , 0.        , 0.        ]])
```



L2 정규화를 한다.

<div class="in_prompt">
In&nbsp;[30]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.preprocessing import normalize
```

</div>

<div class="in_prompt">
In&nbsp;[31]:
</div>

<div class="input_area" markdown="1">

```python
tfidf_bfor_l2 = np.multiply(np.array(count_vec), np.array(s_idf_rst))
tfidf_afer_l2 = normalize(tfidf_bfor_l2, norm='l2')
```

</div>

<div class="in_prompt">
In&nbsp;[32]:
</div>

<div class="input_area" markdown="1">

```python
tfidf_afer_l2
```

</div>

<div class="output_prompt">
Out&nbsp;[32]:
</div>




{:.output_data_text}

```
array([[0.        , 0.        , 0.52640543, 0.        , 0.52640543,
        0.        , 0.        , 0.        , 0.66767854, 0.        ],
       [0.        , 0.        , 0.57735027, 0.        , 0.57735027,
        0.        , 0.        , 0.        , 0.        , 0.57735027],
       [0.        , 0.42693074, 0.        , 0.42693074, 0.        ,
        0.        , 0.        , 0.42693074, 0.        , 0.6731942 ],
       [0.57735027, 0.        , 0.        , 0.        , 0.        ,
        0.57735027, 0.57735027, 0.        , 0.        , 0.        ]])
```



TfidfVectorizer 최종 결과와 비교해본다.

<div class="in_prompt">
In&nbsp;[33]:
</div>

<div class="input_area" markdown="1">

```python
ti = TfidfVectorizer()
tfidfv = ti.fit_transform(documents).toarray()
tfidfv
```

</div>

<div class="output_prompt">
Out&nbsp;[33]:
</div>




{:.output_data_text}

```
array([[0.        , 0.        , 0.52640543, 0.        , 0.52640543,
        0.        , 0.        , 0.        , 0.66767854, 0.        ],
       [0.        , 0.        , 0.57735027, 0.        , 0.57735027,
        0.        , 0.        , 0.        , 0.        , 0.57735027],
       [0.        , 0.42693074, 0.        , 0.42693074, 0.        ,
        0.        , 0.        , 0.42693074, 0.        , 0.6731942 ],
       [0.57735027, 0.        , 0.        , 0.        , 0.        ,
        0.57735027, 0.57735027, 0.        , 0.        , 0.        ]])
```



<div class="in_prompt">
In&nbsp;[34]:
</div>

<div class="input_area" markdown="1">

```python
tfidf_res = pd.DataFrame(tfidf_afer_l2, columns=vocab)
tfidf_res
```

</div>

<div class="output_prompt">
Out&nbsp;[34]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>나는</th>
      <th>맛있지만</th>
      <th>먹고</th>
      <th>살이</th>
      <th>싶은</th>
      <th>야식이</th>
      <th>좋아요</th>
      <th>찌는</th>
      <th>치킨</th>
      <th>피자</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.526405</td>
      <td>0.000000</td>
      <td>0.526405</td>
      <td>0.00000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.667679</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.577350</td>
      <td>0.000000</td>
      <td>0.577350</td>
      <td>0.00000</td>
      <td>0.00000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.577350</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.00000</td>
      <td>0.426931</td>
      <td>0.000000</td>
      <td>0.426931</td>
      <td>0.000000</td>
      <td>0.00000</td>
      <td>0.00000</td>
      <td>0.426931</td>
      <td>0.000000</td>
      <td>0.673194</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.57735</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.57735</td>
      <td>0.57735</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 네이버 영화 리뷰 분석

<div class="in_prompt">
In&nbsp;[35]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.feature_extraction.text import TfidfVectorizer
import pandas as pd
```

</div>

<div class="in_prompt">
In&nbsp;[36]:
</div>

<div class="input_area" markdown="1">

```python
df_train = pd.read_csv('datasets/ratings_train.txt', delimiter='\t')
df_train.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[36]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>document</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>9976970</td>
      <td>아 더빙.. 진짜 짜증나네요 목소리</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>3819312</td>
      <td>흠...포스터보고 초딩영화줄....오버연기조차 가볍지 않구나</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10265843</td>
      <td>너무재밓었다그래서보는것을추천한다</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9045019</td>
      <td>교도소 이야기구먼 ..솔직히 재미는 없다..평점 조정</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6483659</td>
      <td>사이몬페그의 익살스런 연기가 돋보였던 영화!스파이더맨에서 늙어보이기만 했던 커스틴 ...</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[37]:
</div>

<div class="input_area" markdown="1">

```python
# df_train = df_train[:10000]
```

</div>

<div class="in_prompt">
In&nbsp;[38]:
</div>

<div class="input_area" markdown="1">

```python
df_train.dropna(inplace=True)
df_train.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[38]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 149995 entries, 0 to 149999
Data columns (total 3 columns):
 #   Column    Non-Null Count   Dtype 
---  ------    --------------   ----- 
 0   id        149995 non-null  int64 
 1   document  149995 non-null  object
 2   label     149995 non-null  int64 
dtypes: int64(2), object(1)
memory usage: 4.6+ MB

```

<div class="in_prompt">
In&nbsp;[39]:
</div>

<div class="input_area" markdown="1">

```python
df_test = pd.read_csv('datasets/ratings_test.txt', delimiter='\t')
df_test.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[39]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>document</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>6270596</td>
      <td>굳 ㅋ</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>9274899</td>
      <td>GDNTOPCLASSINTHECLUB</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>8544678</td>
      <td>뭐야 이 평점들은.... 나쁘진 않지만 10점 짜리는 더더욱 아니잖아</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6825595</td>
      <td>지루하지는 않은데 완전 막장임... 돈주고 보기에는....</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>6723715</td>
      <td>3D만 아니었어도 별 다섯 개 줬을텐데.. 왜 3D로 나와서 제 심기를 불편하게 하죠??</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[40]:
</div>

<div class="input_area" markdown="1">

```python
df_test.dropna(inplace=True)
df_test.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[40]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 49997 entries, 0 to 49999
Data columns (total 3 columns):
 #   Column    Non-Null Count  Dtype 
---  ------    --------------  ----- 
 0   id        49997 non-null  int64 
 1   document  49997 non-null  object
 2   label     49997 non-null  int64 
dtypes: int64(2), object(1)
memory usage: 1.5+ MB

```

<div class="in_prompt">
In&nbsp;[41]:
</div>

<div class="input_area" markdown="1">

```python
X_train = df_train['document']
y_train = df_train['label']

X_test = df_test['document']
y_test = df_test['label']
```

</div>

>출력 초과 에러
{:.filename}
{% highlight dos %}
{% raw %}
IOPub data rate exceeded.
The Jupyter server will temporarily stop sending output
to the client in order to avoid crashing it.
To change this limit, set the config variable
`--ServerApp.iopub_data_rate_limit`.

Current values:
ServerApp.iopub_data_rate_limit=1000000.0 (bytes/sec)
ServerApp.rate_limit_window=3.0 (secs)
{% endraw %}
{% endhighlight dos %}

>CMD
{:.filename}
{% highlight dos %}
jupyter lab --ServerApp.iopub_data_rate_limit=1.0e10
{% endhighlight dos %}

<div class="in_prompt">
In&nbsp;[42]:
</div>

<div class="input_area" markdown="1">

```python
naver_re_tv = TfidfVectorizer()
naver_re_tv.fit(X_train)
print(naver_re_tv.vocabulary_)
```

</div>

<div class="output_prompt">
Out&nbsp;[42]:
</div>

{:.output_stream}

```
{'더빙': 71119, '진짜': 246232, '짜증나네요': 248358, '목소리': 99567, '포스터보고': 273335, '초딩영화줄': 255126, '오버연기조차': 190112, '가볍지': 16352, '않구나': 167602, '너무재밓었다그래서보는것을추천한다': 57394, '교도소': 33783, '이야기구먼': 208071, '솔직히': 145795, '재미는': 222295, '없다': 177352, '평점': 271982, '조정': 234711, '사이몬페그의': 133947,

...

'고마움': 28958, '1점도아깝다진짜': 2615, '척편이여서': 253296, '불가50가지': 126113, '찎었냐': 250252, '220215679580': 3645, '크리쳐개그물임ㅋㅋ': 263752, '흥겹다ㅋ': 291905, '높아서ㅋㅋ': 60705, 'carl': 8822, '세이건으로': 143023, '디케이드': 80079, '오즈인데': 190555, '더블은': 71115, '뭔죄인가': 105744, '거들먹거리고': 24486, '혼혈은': 287413, '수간하는': 146244}

```

<div class="in_prompt">
In&nbsp;[43]:
</div>

<div class="input_area" markdown="1">

```python
X_train_tfidf = naver_re_tv.transform(X_train)
X_train_tfidf # 희소행렬 생성 (293366개의 단어에 대해 Bag of word)
```

</div>

<div class="output_prompt">
Out&nbsp;[43]:
</div>




{:.output_data_text}

```
<149995x293366 sparse matrix of type '<class 'numpy.float64'>'
	with 1074805 stored elements in Compressed Sparse Row format>
```



<div class="in_prompt">
In&nbsp;[44]:
</div>

<div class="input_area" markdown="1">

```python
X_test_tfidf = naver_re_tv.transform(X_test)
X_test_tfidf
```

</div>

<div class="output_prompt">
Out&nbsp;[44]:
</div>




{:.output_data_text}

```
<49997x293366 sparse matrix of type '<class 'numpy.float64'>'
	with 284188 stored elements in Compressed Sparse Row format>
```



#### LogisticRegression 머신러닝

<div class="in_prompt">
In&nbsp;[45]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.linear_model import LogisticRegression
```

</div>

<div class="in_prompt">
In&nbsp;[46]:
</div>

<div class="input_area" markdown="1">

```python
lr_model = LogisticRegression(max_iter=400)
lr_model.fit(X_train_tfidf, y_train)
lr_model.score(X_test_tfidf, y_test)
```

</div>

<div class="output_prompt">
Out&nbsp;[46]:
</div>




{:.output_data_text}

```
0.8125087505250315
```



## 예측 기반

### Word2vec

* CBOW: 입력값으로 여러 개의 단어를 사용, 학습을 위해 하나의 단어와 비교
* Skip-Gram: 입력값이 하나의 단어를 사용, 학습을 위해 주변의 여러 단어와 비교

단어 간의 유사도를 잘 측정한다.

[word2vec](https://word2vec.kr/search/)

한국-서울+도쿄 => 일본
: 단어 한국과 단어 서울의 거리는 단어 '일본'과 단어 도쿄의 거리와 같다

한국-서울+베이징 => 중국  
맥주-치킨+피자 => 음료수  
남자-아빠+엄마 => 여자

---
# Reference
* 은공지능 공작소: [본격 TF-IDF 개념 해부](https://chan-lab.tistory.com/24)