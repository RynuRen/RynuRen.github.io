---
layout: jupyter
title: 자연어 처리 - RNN 스팸 메일 분류
published: true
date: 2023-01-13
description: RNN 스팸 메일 분류
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: RNN 스팸 메일 분류
use_math: true
---
---
# 스팸 메일 분류 - RNN

## 전처리

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import pandas as pd
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
data = pd.read_csv('datasets/spam.csv', encoding='latin1')
data = data[['v1', 'v2']]
data['v1'] = data['v1'].replace(['ham', 'spam'], [0, 1])
# data['v1'] = data['v1'].replace({'ham':0, 'spam':1})
data
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
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
      <th>v1</th>
      <th>v2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Go until jurong point, crazy.. Available only ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>Ok lar... Joking wif u oni...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>Free entry in 2 a wkly comp to win FA Cup fina...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>U dun say so early hor... U c already then say...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>Nah I don't think he goes to usf, he lives aro...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>5567</th>
      <td>1</td>
      <td>This is the 2nd time we have tried 2 contact u...</td>
    </tr>
    <tr>
      <th>5568</th>
      <td>0</td>
      <td>Will Ì_ b going to esplanade fr home?</td>
    </tr>
    <tr>
      <th>5569</th>
      <td>0</td>
      <td>Pity, * was in mood for that. So...any other s...</td>
    </tr>
    <tr>
      <th>5570</th>
      <td>0</td>
      <td>The guy did some bitching but I acted like i'd...</td>
    </tr>
    <tr>
      <th>5571</th>
      <td>0</td>
      <td>Rofl. Its true to its name</td>
    </tr>
  </tbody>
</table>
<p>5572 rows × 2 columns</p>
</div>
</div>



* 데이터 중복 확인

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5572 entries, 0 to 5571
Data columns (total 2 columns):
 #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   v1      5572 non-null   int64 
 1   v2      5572 non-null   object
dtypes: int64(1), object(1)
memory usage: 87.2+ KB

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
data['v2'].nunique()
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>




{:.output_data_text}

```
5169
```



<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
data = data.drop_duplicates(subset=['v2'])
data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 5169 entries, 0 to 5571
Data columns (total 2 columns):
 #   Column  Non-Null Count  Dtype 
---  ------  --------------  ----- 
 0   v1      5169 non-null   int64 
 1   v2      5169 non-null   object
dtypes: int64(1), object(1)
memory usage: 121.1+ KB

```

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
data['v2'].nunique()
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>




{:.output_data_text}

```
5169
```



<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
data['v1'].value_counts()
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>




{:.output_data_text}

```
0    4516
1     653
Name: v1, dtype: int64
```



<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
X_data = data['v2']
y_data = data['v1']
```

</div>

* train, test 나누기

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
from sklearn.model_selection import train_test_split
```

</div>

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
X_train, X_test, y_train, y_test = train_test_split(X_data, y_data, test_size=0.2, random_state=0, stratify=y_data)
```

</div>

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
X_train.shape, X_test.shape, y_train.shape, y_test.shape
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
</div>




{:.output_data_text}

```
((4135,), (1034,), (4135,), (1034,))
```



* 토큰화

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
from keras.preprocessing.text import Tokenizer
```

</div>

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
tok = Tokenizer()
tok.fit_on_texts(X_train)
X_train_encoded = tok.texts_to_sequences(X_train)
X_train_encoded[:2]
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
</div>




{:.output_data_text}

```
[[102, 1, 210, 230, 3, 17, 39], [1, 59, 8, 427, 17, 5, 137, 2, 2326]]
```



<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
print(tok.word_index)
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
</div>

{:.output_stream}

```
{'i': 1, 'to': 2, 'you': 3, 'a': 4, 'the': 5, 'u': 6, 'and': 7, 'in': 8, 'is': 9, 'me': 10, 'my': 11, 'for': 12, 'your': 13, 'it': 14, 'of': 15, 'have': 16, 'on': 17, 'call': 18, 'that': 19, 'are': 20, '2': 21, 'now': 22, 'so': 23, 'but': 24, 'not': 25, 'can': 26, 'or': 27, "i'm": 28, 'get': 29, 'at': 30, 'do': 31, 'if': 32, 'be': 33, 'will': 34, 'just': 35, 'with': 36, 'we': 37, 'no': 38, 'this': 39, 'ur': 40, 

...

 'khelate': 7804, 'kintu': 7805, 'opponenter': 7806, 'dhorte': 7807, 'lage': 7808, "tm'ing": 7809, 'laughs': 7810, 'adding': 7811, 'zeros': 7812, 'savings': 7813, 'hos': 7814, 'heroes': 7815, 'tips': 7816, 'genes': 7817, 'begun': 7818, 'registration': 7819, 'permanent': 7820, 'residency': 7821}

```

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
total_cnt = len(tok.word_index)
vocab_size = len(tok.word_index) + 1 # zero padding
vocab_size
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>




{:.output_data_text}

```
7822
```



* 메일 제목 길이 분포 확인

<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
import matplotlib.pyplot as plt
```

</div>

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
plt.hist([len(sample) for sample in X_data], bins=100)
plt.show()
```

</div>

<div class="output_prompt">
Out&nbsp;[17]:
</div>


    
![img]({{ '/assets/images/2023/01/13/img01.png' | relative_url }})
    


<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
from keras.utils import pad_sequences
```

</div>

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
max_len = 200 # 너무 긴 제목은 200 단어가 넘어가면 잘라냄, 모자르면 0으로 채움
X_train_padded = pad_sequences(X_train_encoded, maxlen=max_len)
X_train_padded.shape
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>




{:.output_data_text}

```
(4135, 200)
```



## 딥러닝

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
from keras.models import Sequential
from keras.layers import Embedding, SimpleRNN, Dense
```

</div>

<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
model = Sequential([
    Embedding(vocab_size, 100),
    SimpleRNN(32),
    Dense(1, activation='sigmoid')
])
model.summary()
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>

{:.output_stream}

```
Model: "sequential"
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 embedding (Embedding)       (None, None, 100)         782200    
                                                                 
 simple_rnn (SimpleRNN)      (None, 32)                4256      
                                                                 
 dense (Dense)               (None, 1)                 33        
                                                                 
=================================================================
Total params: 786,489
Trainable params: 786,489
Non-trainable params: 0
_________________________________________________________________

```

<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
model.compile(optimizer='rmsprop', loss='binary_crossentropy', metrics=['acc'])
history = model.fit(X_train_padded, y_train, epochs=5, batch_size=64, validation_split=0.2)
```

</div>

<div class="output_prompt">
Out&nbsp;[22]:
</div>

{:.output_stream}

```
Epoch 1/5

```

{:.output_stream}

```
C:\Users\user\AppData\Roaming\Python\Python38\site-packages\keras\engine\data_adapter.py:1508: FutureWarning: The behavior of `series[i:j]` with an integer-dtype index is deprecated. In a future version, this will be treated as *label-based* indexing, consistent with e.g. `series[i]` lookups. To retain the old behavior, use `series.iloc[i:j]`. To get the future behavior, use `series.loc[i:j]`.
  return t[start:end]

```

{:.output_stream}

```
52/52 [==============================] - 3s 36ms/step - loss: 0.2688 - acc: 0.9144 - val_loss: 0.1273 - val_acc: 0.9613
Epoch 2/5
52/52 [==============================] - 2s 31ms/step - loss: 0.0851 - acc: 0.9794 - val_loss: 0.0962 - val_acc: 0.9698
Epoch 3/5
52/52 [==============================] - 2s 32ms/step - loss: 0.0425 - acc: 0.9885 - val_loss: 0.0716 - val_acc: 0.9794
Epoch 4/5
52/52 [==============================] - 2s 31ms/step - loss: 0.0244 - acc: 0.9927 - val_loss: 0.0623 - val_acc: 0.9843
Epoch 5/5
52/52 [==============================] - 2s 33ms/step - loss: 0.0135 - acc: 0.9958 - val_loss: 0.0615 - val_acc: 0.9831

```

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
X_test_encoded = tok.texts_to_sequences(X_test)
X_test_padded = pad_sequences(X_test_encoded, maxlen=max_len)
model.evaluate(X_test_padded, y_test)
```

</div>

<div class="output_prompt">
Out&nbsp;[23]:
</div>

{:.output_stream}

```
33/33 [==============================] - 0s 7ms/step - loss: 0.0889 - acc: 0.9768

```




{:.output_data_text}

```
[0.08886431902647018, 0.9767891764640808]
```



딥러닝에서는 일반 머신러닝과 다르게 제목의 문자 조합 즉, 문맥으로 구분한다.  
하지만 진짜 의미를 아는게(XAI) 아니다.
