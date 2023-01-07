---
layout: jupyter
title: 자연어 처리 - 원-핫 인코딩
published: true
date: 2023-01-06
description: 원-핫 인코딩
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: 원-핫 인코딩
---
---
# 원-핫 인코딩

정형데이터 분석, 전처리시 유의

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
from konlpy.tag import Okt
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
okt = Okt()
tokens = okt.morphs("나는 자연어 처리를 배우는 거겠지")
tokens
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>




{:.output_data_text}

```
['나', '는', '자연어', '처리', '를', '배우는', '거', '겠지']
```



## one hot encoding vs label encoding

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
items = ['남자', '여자', '여자', '남자', '여자']
```

</div>

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.preprocessing import LabelEncoder
```

</div>

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
le = LabelEncoder()
le.fit_transform(items)
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>




{:.output_data_text}

```
array([0, 1, 1, 0, 1], dtype=int64)
```



레이블 인코딩은 숫자의 크기가 결과에 영항을 미친다라고 학습할 가능성이 있다. -> 잘 사용하지 않음

## 원-핫 인코딩

원-핫 인코딩의 한계
1. 수학적으로 처리해야할 차원이 늘어나 연산에 부담이 됨
2. 각 원소들의 유사도를 표현하지 못함

* pandas: get_dummies
* sklearn: OnHotEncoder
* tf.keras: to_categorical

### keras의 to_categorical

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
text = "나는 자연어 처리를 배우는 거겠지 자연어 처리는 어렵지 않기를 바래"
```

</div>

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
from keras.preprocessing.text import Tokenizer
from keras.utils import to_categorical
```

</div>

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
tok = Tokenizer()
tok.fit_on_texts([text])
print(tok.word_index)
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>

{:.output_stream}

```
{'자연어': 1, '나는': 2, '처리를': 3, '배우는': 4, '거겠지': 5, '처리는': 6, '어렵지': 7, '않기를': 8, '바래': 9}

```

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
sub_text = "나는 어렵지 않기를 바래"
enc1 = tok.texts_to_sequences([sub_text])[0]
enc1
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>




{:.output_data_text}

```
[2, 7, 8, 9]
```



<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
ohe1 = to_categorical(enc1)
ohe1
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>




{:.output_data_text}

```
array([[0., 0., 1., 0., 0., 0., 0., 0., 0., 0.],
       [0., 0., 0., 0., 0., 0., 0., 1., 0., 0.],
       [0., 0., 0., 0., 0., 0., 0., 0., 1., 0.],
       [0., 0., 0., 0., 0., 0., 0., 0., 0., 1.]], dtype=float32)
```



### pandas의 get_dummies

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
import pandas as pd
```

</div>

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
X = ['b', 'r', 'r', 'g', 'g']
y = [0, 1, 1, 0, 0]

df1 = pd.DataFrame({'X': X, 'y':y})
df1
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
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
      <th>X</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>r</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>r</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>g</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>g</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
pd.get_dummies(df1['X'])
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
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
      <th>b</th>
      <th>g</th>
      <th>r</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### sklearn의 OneHotEncoder

<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.preprocessing import OneHotEncoder
```

</div>

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
ohe = OneHotEncoder(sparse=False)
ohe.fit_transform(df1)
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>

{:.output_stream}

```
C:\Users\user\anaconda3\envs\sesac\lib\site-packages\sklearn\preprocessing\_encoders.py:808: FutureWarning: `sparse` was renamed to `sparse_output` in version 1.2 and will be removed in 1.4. `sparse_output` is ignored unless you leave `sparse` to its default value.
  warnings.warn(

```




{:.output_data_text}

```
array([[1., 0., 0., 1., 0.],
       [0., 0., 1., 0., 1.],
       [0., 0., 1., 0., 1.],
       [0., 1., 0., 1., 0.],
       [0., 1., 0., 1., 0.]])
```



<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
ohe.categories_
```

</div>

<div class="output_prompt">
Out&nbsp;[16]:
</div>




{:.output_data_text}

```
[array(['b', 'g', 'r'], dtype=object), array([0, 1], dtype=int64)]
```


