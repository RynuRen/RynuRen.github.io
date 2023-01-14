---
layout: jupyter
title: 자연어 처리 - 시계열 데이터 예측 
published: true
date: 2023-01-12
description: 시계열 데이터 예측
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: 시계열 데이터 예측
use_math: true
---
---
# 시계열 데이터 예측

텍스트 문장 분석, 주식, 기온변화 등 흐름에 따라 값이 오르락 내리락하는 데이터

## 시계열 데이터(삼각 함수) 준비

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import numpy as np
import matplotlib.pyplot as plt
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
steps_per_cycle = 20
number_of_cycles = 176
noise_factor = 0.5
plot_num_cycles = 20

seq_len = steps_per_cycle * number_of_cycles
t = np.arange(seq_len)
sin_t_noisy = np.sin(2*np.pi/steps_per_cycle*t + noise_factor*np.random.uniform(-1.0, 1.0, seq_len))
sin_t_clean = np.sin(2*np.pi/steps_per_cycle*t) # 2π

upto = plot_num_cycles * steps_per_cycle
fig = plt.figure(figsize=(15, 4))
plt.plot(t[:upto], sin_t_noisy[:upto], label='noisy')
plt.plot(t[:upto], sin_t_clean[:upto], label='clean')
plt.legend()
plt.show()
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>


    
![img]({{ '/assets/images/2023/01/12/img02.png' | relative_url }})
    


<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
def pack_truncated_data(data, num_prev=100):
    X, y = [], []
    # num_prev만큼의 데이터를 주고 그 다음 올 값 1개를 학습
    for i in range(len(data)-num_prev):
        X.append(data[i:i+num_prev])
        y.append(data[i+num_prev])
    X = np.array(X)[:, :, np.newaxis] # 딥러닝에 맞게 3차원으로 reshape
    y = np.array(y)[:, np.newaxis] # 딥러닝에 맞게 2차원으로 reshape
    return X, y
```

</div>

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
trunceted_seq_len = 10
data = sin_t_noisy # 또는 data = sin_t_clean
data_len = data.shape[0]
test_split = 0.25 # test 비율
num_train = int(data_len * (1 - test_split))

train_data = data[:num_train] # 시계열 데이터에선 주로 옛날 패턴
test_data = data[num_train:] # 시계열 데이터에선 주로 최근 패턴

X_train, y_train = pack_truncated_data(train_data, trunceted_seq_len)
X_test, y_test = pack_truncated_data(test_data, trunceted_seq_len)
```

</div>

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
data_len, X_train.shape, X_test.shape, y_train.shape, y_test.shape
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>




{:.output_data_text}

```
(3520, (2630, 10, 1), (870, 10, 1), (2630, 1), (870, 1))
```



## RNN 모델 정의, 학습

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
from keras.models import Sequential
from keras.layers import SimpleRNN, Dense
from keras.optimizers import Adam
```

</div>

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
def define_model(truncated_seq_len):
    input_dimension = 1 # truncated_seq_len(10)개 묶음 1개가 들어옴
    output_dimension = 1 # 예측값
    hidden_dimension = 1 # RNN 중간 층 갯수
    model = Sequential()
    model.add(SimpleRNN(input_shape=(truncated_seq_len, input_dimension),
                        units=hidden_dimension,
                        return_sequences=False, name='hidden_layer'))
    model.add(Dense(output_dimension, name='output_layer'))
    model.compile(loss='mse', 
                optimizer=Adam(learning_rate=1e-3))
    return model
```

</div>

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
model = define_model(X_train.shape[1]) # X의 길이가 10
model.summary()
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>

{:.output_stream}

```
Model: "sequential"
_________________________________________________________________
 Layer (type)                Output Shape              Param #   
=================================================================
 hidden_layer (SimpleRNN)    (None, 1)                 3         
                                                                 
 output_layer (Dense)        (None, 1)                 2         
                                                                 
=================================================================
Total params: 5
Trainable params: 5
Non-trainable params: 0
_________________________________________________________________

```

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
history = model.fit(X_train, y_train, batch_size=600, epochs=1000, verbose=1, validation_split=0.5)
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>

{:.output_stream}

```
Epoch 1/1000
3/3 [==============================] - 1s 83ms/step - loss: 0.5931 - val_loss: 0.5973
Epoch 2/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5881 - val_loss: 0.5923
Epoch 3/1000
3/3 [==============================] - 0s 12ms/step - loss: 0.5833 - val_loss: 0.5874
Epoch 4/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5785 - val_loss: 0.5825
Epoch 5/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.5736 - val_loss: 0.5776
Epoch 6/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.5690 - val_loss: 0.5728
Epoch 7/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.5642 - val_loss: 0.5681
Epoch 8/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.5596 - val_loss: 0.5633
Epoch 9/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5549 - val_loss: 0.5587
Epoch 10/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5503 - val_loss: 0.5541

...

Epoch 996/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1221 - val_loss: 0.1221
Epoch 997/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1221 - val_loss: 0.1221
Epoch 998/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1221 - val_loss: 0.1221
Epoch 999/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1221 - val_loss: 0.1221
Epoch 1000/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1221 - val_loss: 0.1221

```

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
plt.plot(history.history['loss'], label='loss')
plt.plot(history.history['val_loss'], label='val_loss')
plt.legend()
plt.show()
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>


    
![img]({{ '/assets/images/2023/01/12/img03.png' | relative_url }})
    


## 예측

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
y_pred = model.predict(X_test[:100])
plt.figure(figsize=(20, 4))
plt.plot(y_test[:100], label='y_label')
plt.plot(y_pred, label='y_pred')
plt.legend()
plt.show()
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
</div>

{:.output_stream}

```
4/4 [==============================] - 0s 3ms/step

```


    
![img]({{ '/assets/images/2023/01/12/img04.png' | relative_url }})
    


* layer 가중치

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
for layer in model.layers:
    print(f'model name: {layer.name}, lapyer_weight: {layer.get_weights()}')
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
</div>

{:.output_stream}

```
model name: hidden_layer, lapyer_weight: [array([[-0.9871975]], dtype=float32), array([[-0.03002893]], dtype=float32), array([-0.01226329], dtype=float32)]
model name: output_layer, lapyer_weight: [array([[-1.0779022]], dtype=float32), array([-0.00863029], dtype=float32)]

```
hidden_layer(SimpleRNN): $$h_t = W_x*X_t + W_h*h_{t-1} + b$$ => weights: W_x, W_h, b

output_layer(Dense): $$y= W*x + b$$ => weights: W, b
