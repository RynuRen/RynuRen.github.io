---
layout: jupyter
title: 자연어 처리 - GPT2와 한국어 언어 생성
published: true
date: 2023-01-19
description: GPT2GPT2와 한국어 언어 생성
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: GPT2와 한국어 언어 생성
use_math: true
---
---
# GPT2
Generative Pre-training

GTP2는 OpenAI에서 발표한 GPT1 모델을 여러 방법을 통해 성능을 향상시킨 모델로서 텍스트 생성에서 특히 좋은 성능을 보여주는 모델이다.

GPT1은 매우 큰 자연어 처리 데이터를 활용해 비지도 학습으로 사전 학습한 후 학습된 가중치를 활용해 우리가 풀고자 하는 문제에 미세 조정하는 방법론의 모델이다.  
기본 모델 구조는 트랜스포머 이며 BERT는 트랜스포머의 인코더 구조만 사용하고 GPT1은 트랜스포머의 디코더 구조만 사용한다.(순방향 마스크 어탠션) 

# GPT2를 활용한 한국어 언어 생성 모델

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
from google.colab import drive
drive.mount('/content/drive')
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>

{:.output_stream}

```
Mounted at /content/drive

```

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
!python --version
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>

{:.output_stream}

```
Python 3.8.10

```

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
%cd /content/drive/MyDrive/Colab Notebooks/gpt2_LM
!pip install -r requirements.txt
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
/content/drive/MyDrive/Colab Notebooks/gpt2_LM
Looking in indexes: https://pypi.org/simple, https://us-python.pkg.dev/colab-wheels/public/simple/
Collecting jupyter
  Downloading jupyter-1.0.0-py2.py3-none-any.whl (2.7 kB)

...

tensorflow-estimator-2.2.0 tokenizers-0.8.1rc1 transformers-3.0.2 wget-3.2

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
!pip install seqeval
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
Looking in indexes: https://pypi.org/simple, https://us-python.pkg.dev/colab-wheels/public/simple/
Requirement already satisfied: seqeval in /usr/local/lib/python3.8/dist-packages (1.2.2)
Requirement already satisfied: scikit-learn>=0.21.3 in /usr/local/lib/python3.8/dist-packages (from seqeval) (1.0.2)
Requirement already satisfied: numpy>=1.14.0 in /usr/local/lib/python3.8/dist-packages (from seqeval) (1.16.2)
Requirement already satisfied: threadpoolctl>=2.0.0 in /usr/local/lib/python3.8/dist-packages (from scikit-learn>=0.21.3->seqeval) (3.1.0)
Requirement already satisfied: joblib>=0.11 in /usr/local/lib/python3.8/dist-packages (from scikit-learn>=0.21.3->seqeval) (1.2.0)
Requirement already satisfied: scipy>=1.1.0 in /usr/local/lib/python3.8/dist-packages (from scikit-learn>=0.21.3->seqeval) (1.4.1)

```

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
import os

import numpy as np
import tensorflow as tf

import gluonnlp as nlp
from gluonnlp.data import SentencepieceTokenizer
from transformers import TFGPT2LMHeadModel

from tensorflow.keras.preprocessing.sequence import pad_sequences

from nltk.tokenize import sent_tokenize
```

</div>

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
tf.__version__
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>




{:.output_data_text}

```
'2.2.0'
```



<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
import wget
import zipfile

wget.download('https://github.com/NLP-kr/tensorflow-ml-nlp-tf2/releases/download/v1.0/gpt_ckpt.zip')

with zipfile.ZipFile('gpt_ckpt.zip') as z:
    z.extractall()
```

</div>

## 사전 학습된 모델 불러오기

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
class GPT2Model(tf.keras.Model):
    def __init__(self, dir_path):
        super(GPT2Model, self).__init__()
        self.gpt2 = TFGPT2LMHeadModel.from_pretrained(dir_path)
        
    def call(self, inputs):
        return self.gpt2(inputs)[0]
```

</div>

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
BASE_MODEL_PATH = './gpt_ckpt'
gpt_model = GPT2Model(BASE_MODEL_PATH)
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>

{:.output_stream}

```
WARNING:transformers.modeling_tf_utils:All model checkpoint weights were used when initializing TFGPT2LMHeadModel.

WARNING:transformers.modeling_tf_utils:All the weights of TFGPT2LMHeadModel were initialized from the model checkpoint at ./gpt_ckpt.
If your task is similar to the task the model of the ckeckpoint was trained on, you can already use TFGPT2LMHeadModel for predictions without further training.

```

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
BATCH_SIZE = 16
NUM_EPOCHS = 10
MAX_LEN = 30
TOKENIZER_PATH = './gpt_ckpt/gpt2_kor_tokenizer.spiece'

tokenizer = SentencepieceTokenizer(TOKENIZER_PATH)
vocab = nlp.vocab.BERTVocab.from_sentencepiece(TOKENIZER_PATH,
                                               mask_token=None,
                                               sep_token=None,
                                               cls_token=None,
                                               unknown_token='<unk>',
                                               padding_token='<pad>',
                                               bos_token='<s>',
                                               eos_token='</s>')
```

</div>

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
def tf_top_k_top_p_filtering(logits, top_k=0, top_p=0.0, filter_value=-99999):
    _logits = logits.numpy()
    top_k = min(top_k, logits.shape[-1])  
    if top_k > 0:
        indices_to_remove = logits < tf.math.top_k(logits, top_k)[0][..., -1, None]
        _logits[indices_to_remove] = filter_value

    if top_p > 0.0:
        sorted_logits = tf.sort(logits, direction='DESCENDING')
        sorted_indices = tf.argsort(logits, direction='DESCENDING')
        cumulative_probs = tf.math.cumsum(tf.nn.softmax(sorted_logits, axis=-1), axis=-1)

        sorted_indices_to_remove = cumulative_probs > top_p
        sorted_indices_to_remove = tf.concat([[False], sorted_indices_to_remove[..., :-1]], axis=0)
        indices_to_remove = sorted_indices[sorted_indices_to_remove].numpy().tolist()
        
        _logits[indices_to_remove] = filter_value
    return tf.constant([_logits])


def generate_sent(seed_word, model, max_step=100, greedy=False, top_k=0, top_p=0.):
    sent = seed_word
    toked = tokenizer(sent)
    
    for _ in range(max_step):
        input_ids = tf.constant([vocab[vocab.bos_token],]  + vocab[toked])[None, :] 
        outputs = model(input_ids)[:, -1, :]
        if greedy:
            gen = vocab.to_tokens(tf.argmax(outputs, axis=-1).numpy().tolist()[0])
        else:
            output_logit = tf_top_k_top_p_filtering(outputs[0], top_k=top_k, top_p=top_p)
            gen = vocab.to_tokens(tf.random.categorical(output_logit, 1).numpy().tolist()[0])[0]
        if gen == '</s>':
            break
        sent += gen.replace('▁', ' ')
        toked = tokenizer(sent)

    return sent
```

</div>

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
generate_sent('이때', gpt_model, greedy=True)
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
</div>




{:.output_data_text}

```
'이때문에 일부 전문가들은 “이번 사건은 ‘제2의 삼성’을 꿈꾸는 삼성의 내부 사정을 잘 보여주는 사례”라고 평가했다.'
```


greedy=True일 경우 가장 확률이 높은 단어만 선택하여 실행할 때마다 같은 결과를 보여준다.


<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
generate_sent('이때', gpt_model, top_k=0, top_p=0.95)
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
</div>




{:.output_data_text}

```
'이때문에 축구팬들은 축구 선수 중 ‘퍼펙트 이펙트’를 달성한 선수들을 헷갈려 할 수도 있다.'
```



<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
generate_sent('이때', gpt_model, top_k=0, top_p=0.95)
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
</div>




{:.output_data_text}

```
'이때 로버츠 스트림의 자화상이 달빛 에 노출된 채로 마주했는데, 경내에 전해지는 쟈화상은 그 모습이 매우 아름답게 그려졌다.'
```


greedy=False일 경우(default이므로 생략) 확률 또는 순위가 높은 단어만 선택해 무작위로 생성한다.


top_k 파라미터는 확률이 높은 순서대로 k번째까지 높은 단어에 대해 필터링하는 값이다.  
예측한 단어의 분포가 소수의 단어에만 높은 확률값으로 구성돼 있다면 자연스러운 문장을 생성하기 어렵다.


top_p 파라미터는 일정 확률값 이상인 단어에 대해 필터링 하는 값이다.  
top_k 샘플링보다 안정적으로 사람이 쓴 것과 같이 개연성 있게 문장 생성이 가능하다.


## 미세조정(finefunning)

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
DATA_IN_PATH = '/content/drive/MyDrive/Colab Notebooks/gpt2_LM/'
TRAIN_DATA_FILE = 'finetune_data.txt'

sents = [s[:-1] for s in open(DATA_IN_PATH + TRAIN_DATA_FILE, encoding='utf-8').readlines()]
```

</div>

### 소설 텍스트 전처리

<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
input_data = []
output_data = []

for s in sents:
    tokens = [vocab[vocab.bos_token],]  + vocab[tokenizer(s)] + [vocab[vocab.eos_token],]
    input_data.append(tokens[:-1])
    output_data.append(tokens[1:])

input_data = pad_sequences(input_data, MAX_LEN, value=vocab[vocab.padding_token])
output_data = pad_sequences(output_data, MAX_LEN, value=vocab[vocab.padding_token])

input_data = np.array(input_data, dtype=np.int64)
output_data = np.array(output_data, dtype=np.int64)
```

</div>

### 미세조정 모델 학습

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
loss_object = tf.keras.losses.SparseCategoricalCrossentropy(
    from_logits=True, reduction='none')

train_accuracy = tf.keras.metrics.SparseCategoricalAccuracy(name='accuracy')

def loss_function(real, pred):
    mask = tf.math.logical_not(tf.math.equal(real, vocab[vocab.padding_token]))
    loss_ = loss_object(real, pred)

    mask = tf.cast(mask, dtype=loss_.dtype)
    loss_ *= mask

    return tf.reduce_mean(loss_)

def accuracy_function(real, pred):
    mask = tf.math.logical_not(tf.math.equal(real, vocab[vocab.padding_token]))
    mask = tf.expand_dims(tf.cast(mask, dtype=pred.dtype), axis=-1)
    pred *= mask    
    acc = train_accuracy(real, pred)

    return tf.reduce_mean(acc)
```

</div>

<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
gpt_model.compile(loss=loss_function,
              optimizer=tf.keras.optimizers.Adam(1e-4),
              metrics=[accuracy_function])
```

</div>

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
history = gpt_model.fit(input_data, output_data, 
                    batch_size=BATCH_SIZE, epochs=NUM_EPOCHS,
                    validation_split=0.1)
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>

{:.output_stream}

```
Epoch 1/10

```

{:.output_stream}

```
/usr/local/lib/python3.8/dist-packages/tensorflow/python/framework/indexed_slices.py:433: UserWarning: Converting sparse IndexedSlices to a dense Tensor of unknown shape. This may consume a large amount of memory.
  warnings.warn(

```

{:.output_stream}

```
16/16 [==============================] - 117s 7s/step - loss: 3.0289 - accuracy_function: 0.0981 - val_loss: 2.4879 - val_accuracy_function: 0.1121
Epoch 2/10
16/16 [==============================] - 107s 7s/step - loss: 2.5223 - accuracy_function: 0.1222 - val_loss: 2.3971 - val_accuracy_function: 0.1299
Epoch 3/10
16/16 [==============================] - 104s 7s/step - loss: 2.2798 - accuracy_function: 0.1376 - val_loss: 2.3809 - val_accuracy_function: 0.1436
Epoch 4/10
16/16 [==============================] - 103s 6s/step - loss: 2.0652 - accuracy_function: 0.1505 - val_loss: 2.3908 - val_accuracy_function: 0.1563
Epoch 5/10
16/16 [==============================] - 103s 6s/step - loss: 1.8541 - accuracy_function: 0.1631 - val_loss: 2.4228 - val_accuracy_function: 0.1690
Epoch 6/10
16/16 [==============================] - 101s 6s/step - loss: 1.6672 - accuracy_function: 0.1757 - val_loss: 2.4758 - val_accuracy_function: 0.1808
Epoch 7/10
16/16 [==============================] - 101s 6s/step - loss: 1.4601 - accuracy_function: 0.1880 - val_loss: 2.5972 - val_accuracy_function: 0.1930
Epoch 8/10
16/16 [==============================] - 102s 6s/step - loss: 1.2582 - accuracy_function: 0.2006 - val_loss: 2.7063 - val_accuracy_function: 0.2050
Epoch 9/10
16/16 [==============================] - 104s 6s/step - loss: 1.0705 - accuracy_function: 0.2124 - val_loss: 2.8099 - val_accuracy_function: 0.2177
Epoch 10/10
16/16 [==============================] - 102s 6s/step - loss: 0.9147 - accuracy_function: 0.2250 - val_loss: 2.8990 - val_accuracy_function: 0.2300

```

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
DATA_OUT_PATH = '/content/drive/MyDrive/Colab Notebooks/gpt2_LM/data_out'
model_name = "tf2_gpt2_finetuned_model"

save_path = os.path.join(DATA_OUT_PATH, model_name)

if not os.path.exists(save_path):
    os.makedirs(save_path)

gpt_model.gpt2.save_pretrained(save_path)

loaded_gpt_model = GPT2Model(save_path)
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
</div>

{:.output_stream}

```
WARNING:transformers.modeling_tf_utils:All model checkpoint weights were used when initializing TFGPT2LMHeadModel.

WARNING:transformers.modeling_tf_utils:All the weights of TFGPT2LMHeadModel were initialized from the model checkpoint at /content/drive/MyDrive/Colab Notebooks/gpt2_LM/data_out/tf2_gpt2_finetuned_model.
If your task is similar to the task the model of the ckeckpoint was trained on, you can already use TFGPT2LMHeadModel for predictions without further training.

```

### 결과 확인

<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
generate_sent('이때', gpt_model, greedy=True)
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>




{:.output_data_text}

```
'이때에 마침 길가에 있던 행인이 길을 막 잡아끌어 버리려고 덤비는 게 아닌가.'
```



<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
generate_sent('이때', gpt_model, top_k=0, top_p=0.95)
```

</div>

<div class="output_prompt">
Out&nbsp;[22]:
</div>




{:.output_data_text}

```
'이때에 방에 있는 손님들은 그 지방의 어부 노릇을 하고 있는 듯하였다.'
```


