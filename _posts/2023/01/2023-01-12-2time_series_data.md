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
Epoch 11/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5458 - val_loss: 0.5495
Epoch 12/1000
3/3 [==============================] - 0s 10ms/step - loss: 0.5412 - val_loss: 0.5449
Epoch 13/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.5367 - val_loss: 0.5404
Epoch 14/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.5323 - val_loss: 0.5359
Epoch 15/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5279 - val_loss: 0.5315
Epoch 16/1000
3/3 [==============================] - 0s 10ms/step - loss: 0.5234 - val_loss: 0.5271
Epoch 17/1000
3/3 [==============================] - 0s 10ms/step - loss: 0.5191 - val_loss: 0.5227
Epoch 18/1000
3/3 [==============================] - 0s 10ms/step - loss: 0.5147 - val_loss: 0.5183
Epoch 19/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5105 - val_loss: 0.5140
Epoch 20/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5062 - val_loss: 0.5097
Epoch 21/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.5020 - val_loss: 0.5055
Epoch 22/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.4978 - val_loss: 0.5013
Epoch 23/1000
3/3 [==============================] - 0s 11ms/step - loss: 0.4937 - val_loss: 0.4972
Epoch 24/1000
3/3 [==============================] - 0s 12ms/step - loss: 0.4896 - val_loss: 0.4932
Epoch 25/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.4856 - val_loss: 0.4891
Epoch 26/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.4815 - val_loss: 0.4851
Epoch 27/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.4776 - val_loss: 0.4811
Epoch 28/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.4736 - val_loss: 0.4771
Epoch 29/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.4696 - val_loss: 0.4731
Epoch 30/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.4657 - val_loss: 0.4691
Epoch 31/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.4618 - val_loss: 0.4652
Epoch 32/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.4580 - val_loss: 0.4614
Epoch 33/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.4542 - val_loss: 0.4576
Epoch 34/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.4505 - val_loss: 0.4537
Epoch 35/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.4467 - val_loss: 0.4500
Epoch 36/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.4430 - val_loss: 0.4462
Epoch 37/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.4393 - val_loss: 0.4425
Epoch 38/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.4356 - val_loss: 0.4388
Epoch 39/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.4320 - val_loss: 0.4351
Epoch 40/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.4284 - val_loss: 0.4315
Epoch 41/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.4249 - val_loss: 0.4279
Epoch 42/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.4214 - val_loss: 0.4244
Epoch 43/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.4179 - val_loss: 0.4209
Epoch 44/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.4144 - val_loss: 0.4174
Epoch 45/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.4110 - val_loss: 0.4139
Epoch 46/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.4075 - val_loss: 0.4104
Epoch 47/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.4041 - val_loss: 0.4069
Epoch 48/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.4008 - val_loss: 0.4035
Epoch 49/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.3974 - val_loss: 0.4001
Epoch 50/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.3941 - val_loss: 0.3968
Epoch 51/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.3909 - val_loss: 0.3935
Epoch 52/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.3877 - val_loss: 0.3903
Epoch 53/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.3845 - val_loss: 0.3872
Epoch 54/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.3814 - val_loss: 0.3840
Epoch 55/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.3783 - val_loss: 0.3809
Epoch 56/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.3752 - val_loss: 0.3778
Epoch 57/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.3721 - val_loss: 0.3747
Epoch 58/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.3691 - val_loss: 0.3717
Epoch 59/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.3661 - val_loss: 0.3688
Epoch 60/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.3631 - val_loss: 0.3658
Epoch 61/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.3602 - val_loss: 0.3628
Epoch 62/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.3573 - val_loss: 0.3598
Epoch 63/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.3544 - val_loss: 0.3569
Epoch 64/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.3515 - val_loss: 0.3540
Epoch 65/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.3487 - val_loss: 0.3511
Epoch 66/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.3458 - val_loss: 0.3483
Epoch 67/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.3431 - val_loss: 0.3455
Epoch 68/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.3403 - val_loss: 0.3428
Epoch 69/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.3376 - val_loss: 0.3401
Epoch 70/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.3350 - val_loss: 0.3374
Epoch 71/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.3323 - val_loss: 0.3348
Epoch 72/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.3297 - val_loss: 0.3321
Epoch 73/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.3271 - val_loss: 0.3295
Epoch 74/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.3246 - val_loss: 0.3269
Epoch 75/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.3220 - val_loss: 0.3244
Epoch 76/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.3195 - val_loss: 0.3218
Epoch 77/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.3170 - val_loss: 0.3193
Epoch 78/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.3146 - val_loss: 0.3168
Epoch 79/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.3121 - val_loss: 0.3144
Epoch 80/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.3097 - val_loss: 0.3120
Epoch 81/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.3073 - val_loss: 0.3096
Epoch 82/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.3049 - val_loss: 0.3072
Epoch 83/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.3026 - val_loss: 0.3048
Epoch 84/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.3003 - val_loss: 0.3025
Epoch 85/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.2980 - val_loss: 0.3002
Epoch 86/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2958 - val_loss: 0.2979
Epoch 87/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.2935 - val_loss: 0.2957
Epoch 88/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2913 - val_loss: 0.2935
Epoch 89/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2891 - val_loss: 0.2913
Epoch 90/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.2870 - val_loss: 0.2891
Epoch 91/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2849 - val_loss: 0.2870
Epoch 92/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.2828 - val_loss: 0.2849
Epoch 93/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.2807 - val_loss: 0.2828
Epoch 94/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.2787 - val_loss: 0.2808
Epoch 95/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.2767 - val_loss: 0.2788
Epoch 96/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.2747 - val_loss: 0.2768
Epoch 97/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.2727 - val_loss: 0.2748
Epoch 98/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.2708 - val_loss: 0.2728
Epoch 99/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.2689 - val_loss: 0.2709
Epoch 100/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.2670 - val_loss: 0.2691
Epoch 101/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.2652 - val_loss: 0.2672
Epoch 102/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2634 - val_loss: 0.2654
Epoch 103/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2617 - val_loss: 0.2636
Epoch 104/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.2599 - val_loss: 0.2619
Epoch 105/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.2582 - val_loss: 0.2601
Epoch 106/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.2565 - val_loss: 0.2584
Epoch 107/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2548 - val_loss: 0.2567
Epoch 108/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2531 - val_loss: 0.2550
Epoch 109/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.2515 - val_loss: 0.2534
Epoch 110/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.2499 - val_loss: 0.2517
Epoch 111/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.2482 - val_loss: 0.2501
Epoch 112/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.2467 - val_loss: 0.2486
Epoch 113/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2451 - val_loss: 0.2470
Epoch 114/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.2436 - val_loss: 0.2455
Epoch 115/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.2421 - val_loss: 0.2439
Epoch 116/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.2406 - val_loss: 0.2425
Epoch 117/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.2391 - val_loss: 0.2410
Epoch 118/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.2377 - val_loss: 0.2396
Epoch 119/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2363 - val_loss: 0.2382
Epoch 120/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.2349 - val_loss: 0.2368
Epoch 121/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.2336 - val_loss: 0.2354
Epoch 122/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.2322 - val_loss: 0.2341
Epoch 123/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2309 - val_loss: 0.2328
Epoch 124/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.2296 - val_loss: 0.2314
Epoch 125/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.2283 - val_loss: 0.2302
Epoch 126/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2270 - val_loss: 0.2289
Epoch 127/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.2257 - val_loss: 0.2276
Epoch 128/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.2246 - val_loss: 0.2264
Epoch 129/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.2233 - val_loss: 0.2252
Epoch 130/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.2222 - val_loss: 0.2240
Epoch 131/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.2210 - val_loss: 0.2228
Epoch 132/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2199 - val_loss: 0.2217
Epoch 133/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.2188 - val_loss: 0.2206
Epoch 134/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2177 - val_loss: 0.2195
Epoch 135/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.2166 - val_loss: 0.2184
Epoch 136/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.2155 - val_loss: 0.2173
Epoch 137/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2144 - val_loss: 0.2163
Epoch 138/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.2134 - val_loss: 0.2152
Epoch 139/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.2124 - val_loss: 0.2142
Epoch 140/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.2114 - val_loss: 0.2132
Epoch 141/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.2104 - val_loss: 0.2122
Epoch 142/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2095 - val_loss: 0.2113
Epoch 143/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.2085 - val_loss: 0.2103
Epoch 144/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.2076 - val_loss: 0.2094
Epoch 145/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2067 - val_loss: 0.2085
Epoch 146/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.2058 - val_loss: 0.2075
Epoch 147/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.2049 - val_loss: 0.2066
Epoch 148/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.2040 - val_loss: 0.2058
Epoch 149/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.2031 - val_loss: 0.2049
Epoch 150/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.2023 - val_loss: 0.2040
Epoch 151/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.2015 - val_loss: 0.2032
Epoch 152/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.2007 - val_loss: 0.2024
Epoch 153/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1999 - val_loss: 0.2017
Epoch 154/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1991 - val_loss: 0.2009
Epoch 155/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1984 - val_loss: 0.2001
Epoch 156/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1976 - val_loss: 0.1994
Epoch 157/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1969 - val_loss: 0.1986
Epoch 158/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1961 - val_loss: 0.1979
Epoch 159/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1954 - val_loss: 0.1972
Epoch 160/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1947 - val_loss: 0.1965
Epoch 161/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1940 - val_loss: 0.1958
Epoch 162/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1933 - val_loss: 0.1951
Epoch 163/1000
3/3 [==============================] - 0s 12ms/step - loss: 0.1927 - val_loss: 0.1944
Epoch 164/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1920 - val_loss: 0.1938
Epoch 165/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1914 - val_loss: 0.1931
Epoch 166/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1908 - val_loss: 0.1925
Epoch 167/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1902 - val_loss: 0.1919
Epoch 168/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1896 - val_loss: 0.1913
Epoch 169/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1890 - val_loss: 0.1907
Epoch 170/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1884 - val_loss: 0.1902
Epoch 171/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1879 - val_loss: 0.1896
Epoch 172/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1873 - val_loss: 0.1890
Epoch 173/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1868 - val_loss: 0.1885
Epoch 174/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1862 - val_loss: 0.1880
Epoch 175/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1857 - val_loss: 0.1874
Epoch 176/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1852 - val_loss: 0.1869
Epoch 177/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1846 - val_loss: 0.1864
Epoch 178/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1841 - val_loss: 0.1858
Epoch 179/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1836 - val_loss: 0.1853
Epoch 180/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1831 - val_loss: 0.1848
Epoch 181/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1826 - val_loss: 0.1844
Epoch 182/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1821 - val_loss: 0.1839
Epoch 183/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1817 - val_loss: 0.1834
Epoch 184/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1812 - val_loss: 0.1829
Epoch 185/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1807 - val_loss: 0.1824
Epoch 186/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1803 - val_loss: 0.1820
Epoch 187/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1798 - val_loss: 0.1815
Epoch 188/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1793 - val_loss: 0.1811
Epoch 189/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1789 - val_loss: 0.1806
Epoch 190/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1785 - val_loss: 0.1802
Epoch 191/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1780 - val_loss: 0.1797
Epoch 192/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1776 - val_loss: 0.1793
Epoch 193/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1772 - val_loss: 0.1789
Epoch 194/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1768 - val_loss: 0.1784
Epoch 195/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1763 - val_loss: 0.1780
Epoch 196/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1759 - val_loss: 0.1776
Epoch 197/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1755 - val_loss: 0.1772
Epoch 198/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1751 - val_loss: 0.1768
Epoch 199/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1747 - val_loss: 0.1764
Epoch 200/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1743 - val_loss: 0.1760
Epoch 201/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1739 - val_loss: 0.1756
Epoch 202/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1736 - val_loss: 0.1752
Epoch 203/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1732 - val_loss: 0.1748
Epoch 204/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1728 - val_loss: 0.1745
Epoch 205/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1724 - val_loss: 0.1741
Epoch 206/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1721 - val_loss: 0.1737
Epoch 207/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1717 - val_loss: 0.1734
Epoch 208/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1714 - val_loss: 0.1730
Epoch 209/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1710 - val_loss: 0.1726
Epoch 210/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1707 - val_loss: 0.1723
Epoch 211/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1703 - val_loss: 0.1719
Epoch 212/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1700 - val_loss: 0.1716
Epoch 213/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1697 - val_loss: 0.1712
Epoch 214/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1693 - val_loss: 0.1709
Epoch 215/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1690 - val_loss: 0.1706
Epoch 216/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1686 - val_loss: 0.1702
Epoch 217/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1683 - val_loss: 0.1699
Epoch 218/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1680 - val_loss: 0.1696
Epoch 219/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1677 - val_loss: 0.1692
Epoch 220/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1673 - val_loss: 0.1689
Epoch 221/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1670 - val_loss: 0.1686
Epoch 222/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1667 - val_loss: 0.1683
Epoch 223/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1664 - val_loss: 0.1680
Epoch 224/1000
3/3 [==============================] - 0s 30ms/step - loss: 0.1661 - val_loss: 0.1677
Epoch 225/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1658 - val_loss: 0.1674
Epoch 226/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1655 - val_loss: 0.1671
Epoch 227/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1652 - val_loss: 0.1668
Epoch 228/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1649 - val_loss: 0.1665
Epoch 229/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1646 - val_loss: 0.1662
Epoch 230/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1643 - val_loss: 0.1659
Epoch 231/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1640 - val_loss: 0.1656
Epoch 232/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1638 - val_loss: 0.1653
Epoch 233/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1635 - val_loss: 0.1650
Epoch 234/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1632 - val_loss: 0.1647
Epoch 235/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1629 - val_loss: 0.1645
Epoch 236/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1626 - val_loss: 0.1642
Epoch 237/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1624 - val_loss: 0.1639
Epoch 238/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1621 - val_loss: 0.1636
Epoch 239/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1618 - val_loss: 0.1634
Epoch 240/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1616 - val_loss: 0.1631
Epoch 241/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1613 - val_loss: 0.1628
Epoch 242/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1611 - val_loss: 0.1626
Epoch 243/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1608 - val_loss: 0.1623
Epoch 244/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1605 - val_loss: 0.1621
Epoch 245/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1603 - val_loss: 0.1618
Epoch 246/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1600 - val_loss: 0.1616
Epoch 247/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1598 - val_loss: 0.1613
Epoch 248/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1596 - val_loss: 0.1611
Epoch 249/1000
3/3 [==============================] - 0s 29ms/step - loss: 0.1593 - val_loss: 0.1609
Epoch 250/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1591 - val_loss: 0.1606
Epoch 251/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1589 - val_loss: 0.1604
Epoch 252/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1587 - val_loss: 0.1602
Epoch 253/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1584 - val_loss: 0.1599
Epoch 254/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1582 - val_loss: 0.1597
Epoch 255/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1579 - val_loss: 0.1594
Epoch 256/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1577 - val_loss: 0.1592
Epoch 257/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1574 - val_loss: 0.1589
Epoch 258/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1572 - val_loss: 0.1587
Epoch 259/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1570 - val_loss: 0.1585
Epoch 260/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1568 - val_loss: 0.1582
Epoch 261/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1565 - val_loss: 0.1580
Epoch 262/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1563 - val_loss: 0.1578
Epoch 263/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1561 - val_loss: 0.1575
Epoch 264/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1559 - val_loss: 0.1573
Epoch 265/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1557 - val_loss: 0.1571
Epoch 266/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1554 - val_loss: 0.1569
Epoch 267/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1552 - val_loss: 0.1567
Epoch 268/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1550 - val_loss: 0.1564
Epoch 269/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1548 - val_loss: 0.1562
Epoch 270/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1546 - val_loss: 0.1560
Epoch 271/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1544 - val_loss: 0.1558
Epoch 272/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1542 - val_loss: 0.1556
Epoch 273/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1540 - val_loss: 0.1554
Epoch 274/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1538 - val_loss: 0.1552
Epoch 275/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1536 - val_loss: 0.1550
Epoch 276/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1534 - val_loss: 0.1548
Epoch 277/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1532 - val_loss: 0.1546
Epoch 278/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1530 - val_loss: 0.1544
Epoch 279/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1528 - val_loss: 0.1542
Epoch 280/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1526 - val_loss: 0.1540
Epoch 281/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1525 - val_loss: 0.1538
Epoch 282/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1523 - val_loss: 0.1536
Epoch 283/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1521 - val_loss: 0.1534
Epoch 284/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1519 - val_loss: 0.1533
Epoch 285/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1517 - val_loss: 0.1531
Epoch 286/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1515 - val_loss: 0.1529
Epoch 287/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1513 - val_loss: 0.1527
Epoch 288/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1512 - val_loss: 0.1525
Epoch 289/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1510 - val_loss: 0.1523
Epoch 290/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1508 - val_loss: 0.1521
Epoch 291/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1506 - val_loss: 0.1520
Epoch 292/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1504 - val_loss: 0.1518
Epoch 293/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1503 - val_loss: 0.1516
Epoch 294/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1501 - val_loss: 0.1515
Epoch 295/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1499 - val_loss: 0.1513
Epoch 296/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1498 - val_loss: 0.1511
Epoch 297/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1496 - val_loss: 0.1509
Epoch 298/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1494 - val_loss: 0.1508
Epoch 299/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1493 - val_loss: 0.1506
Epoch 300/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1491 - val_loss: 0.1504
Epoch 301/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1490 - val_loss: 0.1503
Epoch 302/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1488 - val_loss: 0.1501
Epoch 303/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1486 - val_loss: 0.1499
Epoch 304/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1485 - val_loss: 0.1498
Epoch 305/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1483 - val_loss: 0.1496
Epoch 306/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1482 - val_loss: 0.1495
Epoch 307/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1480 - val_loss: 0.1493
Epoch 308/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1479 - val_loss: 0.1492
Epoch 309/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1477 - val_loss: 0.1490
Epoch 310/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1476 - val_loss: 0.1488
Epoch 311/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1474 - val_loss: 0.1487
Epoch 312/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1472 - val_loss: 0.1485
Epoch 313/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1471 - val_loss: 0.1484
Epoch 314/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1470 - val_loss: 0.1483
Epoch 315/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1468 - val_loss: 0.1481
Epoch 316/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1467 - val_loss: 0.1480
Epoch 317/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1465 - val_loss: 0.1478
Epoch 318/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1464 - val_loss: 0.1477
Epoch 319/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1463 - val_loss: 0.1475
Epoch 320/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1461 - val_loss: 0.1474
Epoch 321/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1460 - val_loss: 0.1472
Epoch 322/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1458 - val_loss: 0.1471
Epoch 323/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1457 - val_loss: 0.1469
Epoch 324/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1455 - val_loss: 0.1468
Epoch 325/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1454 - val_loss: 0.1466
Epoch 326/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1453 - val_loss: 0.1465
Epoch 327/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1451 - val_loss: 0.1464
Epoch 328/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1450 - val_loss: 0.1462
Epoch 329/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1449 - val_loss: 0.1461
Epoch 330/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1448 - val_loss: 0.1460
Epoch 331/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1446 - val_loss: 0.1458
Epoch 332/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1445 - val_loss: 0.1457
Epoch 333/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1443 - val_loss: 0.1455
Epoch 334/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1442 - val_loss: 0.1454
Epoch 335/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1441 - val_loss: 0.1453
Epoch 336/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1439 - val_loss: 0.1451
Epoch 337/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1438 - val_loss: 0.1450
Epoch 338/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1437 - val_loss: 0.1449
Epoch 339/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1436 - val_loss: 0.1447
Epoch 340/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1434 - val_loss: 0.1446
Epoch 341/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1433 - val_loss: 0.1445
Epoch 342/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1432 - val_loss: 0.1444
Epoch 343/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1430 - val_loss: 0.1442
Epoch 344/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1429 - val_loss: 0.1441
Epoch 345/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1428 - val_loss: 0.1440
Epoch 346/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1427 - val_loss: 0.1439
Epoch 347/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1426 - val_loss: 0.1437
Epoch 348/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1425 - val_loss: 0.1436
Epoch 349/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1423 - val_loss: 0.1435
Epoch 350/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1422 - val_loss: 0.1434
Epoch 351/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1421 - val_loss: 0.1433
Epoch 352/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1420 - val_loss: 0.1432
Epoch 353/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1419 - val_loss: 0.1430
Epoch 354/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1418 - val_loss: 0.1429
Epoch 355/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1416 - val_loss: 0.1428
Epoch 356/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1415 - val_loss: 0.1427
Epoch 357/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1414 - val_loss: 0.1426
Epoch 358/1000
3/3 [==============================] - 0s 34ms/step - loss: 0.1413 - val_loss: 0.1424
Epoch 359/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1412 - val_loss: 0.1423
Epoch 360/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1411 - val_loss: 0.1422
Epoch 361/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1410 - val_loss: 0.1421
Epoch 362/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1409 - val_loss: 0.1420
Epoch 363/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1408 - val_loss: 0.1419
Epoch 364/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1407 - val_loss: 0.1418
Epoch 365/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1406 - val_loss: 0.1417
Epoch 366/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1404 - val_loss: 0.1416
Epoch 367/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1403 - val_loss: 0.1415
Epoch 368/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1402 - val_loss: 0.1414
Epoch 369/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1401 - val_loss: 0.1413
Epoch 370/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1400 - val_loss: 0.1412
Epoch 371/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1399 - val_loss: 0.1411
Epoch 372/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1399 - val_loss: 0.1410
Epoch 373/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1398 - val_loss: 0.1408
Epoch 374/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1396 - val_loss: 0.1407
Epoch 375/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1396 - val_loss: 0.1406
Epoch 376/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1394 - val_loss: 0.1405
Epoch 377/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1393 - val_loss: 0.1404
Epoch 378/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1393 - val_loss: 0.1403
Epoch 379/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1391 - val_loss: 0.1402
Epoch 380/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1391 - val_loss: 0.1401
Epoch 381/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1390 - val_loss: 0.1400
Epoch 382/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1389 - val_loss: 0.1399
Epoch 383/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1388 - val_loss: 0.1398
Epoch 384/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1387 - val_loss: 0.1397
Epoch 385/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1386 - val_loss: 0.1396
Epoch 386/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1385 - val_loss: 0.1395
Epoch 387/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1384 - val_loss: 0.1394
Epoch 388/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1383 - val_loss: 0.1394
Epoch 389/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1382 - val_loss: 0.1393
Epoch 390/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1381 - val_loss: 0.1392
Epoch 391/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1381 - val_loss: 0.1391
Epoch 392/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1379 - val_loss: 0.1390
Epoch 393/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1378 - val_loss: 0.1389
Epoch 394/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1377 - val_loss: 0.1388
Epoch 395/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1377 - val_loss: 0.1387
Epoch 396/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1375 - val_loss: 0.1386
Epoch 397/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1375 - val_loss: 0.1385
Epoch 398/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1374 - val_loss: 0.1384
Epoch 399/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1373 - val_loss: 0.1383
Epoch 400/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1372 - val_loss: 0.1382
Epoch 401/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1371 - val_loss: 0.1381
Epoch 402/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1370 - val_loss: 0.1380
Epoch 403/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1369 - val_loss: 0.1379
Epoch 404/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1368 - val_loss: 0.1378
Epoch 405/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1368 - val_loss: 0.1377
Epoch 406/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1367 - val_loss: 0.1377
Epoch 407/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1366 - val_loss: 0.1376
Epoch 408/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1365 - val_loss: 0.1375
Epoch 409/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1364 - val_loss: 0.1374
Epoch 410/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1363 - val_loss: 0.1373
Epoch 411/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1362 - val_loss: 0.1372
Epoch 412/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1362 - val_loss: 0.1371
Epoch 413/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1361 - val_loss: 0.1371
Epoch 414/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1360 - val_loss: 0.1370
Epoch 415/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1359 - val_loss: 0.1369
Epoch 416/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1359 - val_loss: 0.1368
Epoch 417/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1358 - val_loss: 0.1368
Epoch 418/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1357 - val_loss: 0.1367
Epoch 419/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1356 - val_loss: 0.1366
Epoch 420/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1356 - val_loss: 0.1365
Epoch 421/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1355 - val_loss: 0.1364
Epoch 422/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1354 - val_loss: 0.1363
Epoch 423/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1353 - val_loss: 0.1362
Epoch 424/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1352 - val_loss: 0.1362
Epoch 425/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1351 - val_loss: 0.1361
Epoch 426/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1351 - val_loss: 0.1360
Epoch 427/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1350 - val_loss: 0.1359
Epoch 428/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1349 - val_loss: 0.1359
Epoch 429/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1349 - val_loss: 0.1358
Epoch 430/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1348 - val_loss: 0.1357
Epoch 431/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1347 - val_loss: 0.1356
Epoch 432/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1346 - val_loss: 0.1356
Epoch 433/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1346 - val_loss: 0.1355
Epoch 434/1000
3/3 [==============================] - 0s 29ms/step - loss: 0.1345 - val_loss: 0.1354
Epoch 435/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1344 - val_loss: 0.1354
Epoch 436/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1344 - val_loss: 0.1353
Epoch 437/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1343 - val_loss: 0.1352
Epoch 438/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1342 - val_loss: 0.1351
Epoch 439/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1342 - val_loss: 0.1351
Epoch 440/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1341 - val_loss: 0.1350
Epoch 441/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1340 - val_loss: 0.1349
Epoch 442/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1340 - val_loss: 0.1348
Epoch 443/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1339 - val_loss: 0.1348
Epoch 444/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1338 - val_loss: 0.1347
Epoch 445/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1338 - val_loss: 0.1346
Epoch 446/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1337 - val_loss: 0.1346
Epoch 447/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1336 - val_loss: 0.1345
Epoch 448/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1336 - val_loss: 0.1344
Epoch 449/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1335 - val_loss: 0.1344
Epoch 450/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1334 - val_loss: 0.1343
Epoch 451/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1334 - val_loss: 0.1342
Epoch 452/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1333 - val_loss: 0.1342
Epoch 453/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1332 - val_loss: 0.1341
Epoch 454/1000
3/3 [==============================] - 0s 30ms/step - loss: 0.1332 - val_loss: 0.1340
Epoch 455/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1331 - val_loss: 0.1340
Epoch 456/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1330 - val_loss: 0.1339
Epoch 457/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1330 - val_loss: 0.1338
Epoch 458/1000
3/3 [==============================] - 0s 33ms/step - loss: 0.1329 - val_loss: 0.1337
Epoch 459/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1328 - val_loss: 0.1337
Epoch 460/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1328 - val_loss: 0.1336
Epoch 461/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1327 - val_loss: 0.1336
Epoch 462/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1327 - val_loss: 0.1335
Epoch 463/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1326 - val_loss: 0.1334
Epoch 464/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1326 - val_loss: 0.1334
Epoch 465/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1325 - val_loss: 0.1333
Epoch 466/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1324 - val_loss: 0.1333
Epoch 467/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1324 - val_loss: 0.1332
Epoch 468/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1323 - val_loss: 0.1331
Epoch 469/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1323 - val_loss: 0.1331
Epoch 470/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1322 - val_loss: 0.1330
Epoch 471/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1322 - val_loss: 0.1330
Epoch 472/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1321 - val_loss: 0.1329
Epoch 473/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1321 - val_loss: 0.1328
Epoch 474/1000
3/3 [==============================] - 0s 30ms/step - loss: 0.1320 - val_loss: 0.1328
Epoch 475/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1319 - val_loss: 0.1327
Epoch 476/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1319 - val_loss: 0.1327
Epoch 477/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1318 - val_loss: 0.1326
Epoch 478/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1317 - val_loss: 0.1325
Epoch 479/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1317 - val_loss: 0.1325
Epoch 480/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1316 - val_loss: 0.1324
Epoch 481/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1316 - val_loss: 0.1324
Epoch 482/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1315 - val_loss: 0.1323
Epoch 483/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1315 - val_loss: 0.1323
Epoch 484/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1314 - val_loss: 0.1322
Epoch 485/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1314 - val_loss: 0.1321
Epoch 486/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1313 - val_loss: 0.1321
Epoch 487/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1313 - val_loss: 0.1320
Epoch 488/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1312 - val_loss: 0.1320
Epoch 489/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1312 - val_loss: 0.1319
Epoch 490/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1311 - val_loss: 0.1319
Epoch 491/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1311 - val_loss: 0.1318
Epoch 492/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1310 - val_loss: 0.1318
Epoch 493/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1309 - val_loss: 0.1317
Epoch 494/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1309 - val_loss: 0.1316
Epoch 495/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1308 - val_loss: 0.1316
Epoch 496/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1308 - val_loss: 0.1315
Epoch 497/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1307 - val_loss: 0.1315
Epoch 498/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1307 - val_loss: 0.1314
Epoch 499/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1306 - val_loss: 0.1314
Epoch 500/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1306 - val_loss: 0.1313
Epoch 501/1000
3/3 [==============================] - 0s 12ms/step - loss: 0.1305 - val_loss: 0.1313
Epoch 502/1000
3/3 [==============================] - 0s 12ms/step - loss: 0.1305 - val_loss: 0.1312
Epoch 503/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1304 - val_loss: 0.1312
Epoch 504/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1304 - val_loss: 0.1311
Epoch 505/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1304 - val_loss: 0.1311
Epoch 506/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1303 - val_loss: 0.1310
Epoch 507/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1303 - val_loss: 0.1310
Epoch 508/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1302 - val_loss: 0.1309
Epoch 509/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1301 - val_loss: 0.1309
Epoch 510/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1301 - val_loss: 0.1308
Epoch 511/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1300 - val_loss: 0.1308
Epoch 512/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1300 - val_loss: 0.1307
Epoch 513/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1299 - val_loss: 0.1306
Epoch 514/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1299 - val_loss: 0.1306
Epoch 515/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1298 - val_loss: 0.1305
Epoch 516/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1298 - val_loss: 0.1305
Epoch 517/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1297 - val_loss: 0.1304
Epoch 518/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1297 - val_loss: 0.1304
Epoch 519/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1297 - val_loss: 0.1303
Epoch 520/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1296 - val_loss: 0.1303
Epoch 521/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1296 - val_loss: 0.1302
Epoch 522/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1295 - val_loss: 0.1302
Epoch 523/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1295 - val_loss: 0.1302
Epoch 524/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1294 - val_loss: 0.1301
Epoch 525/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1294 - val_loss: 0.1301
Epoch 526/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1293 - val_loss: 0.1300
Epoch 527/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1293 - val_loss: 0.1300
Epoch 528/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1292 - val_loss: 0.1299
Epoch 529/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1292 - val_loss: 0.1299
Epoch 530/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1292 - val_loss: 0.1298
Epoch 531/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1291 - val_loss: 0.1298
Epoch 532/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1291 - val_loss: 0.1298
Epoch 533/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1291 - val_loss: 0.1297
Epoch 534/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1290 - val_loss: 0.1297
Epoch 535/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1290 - val_loss: 0.1296
Epoch 536/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1289 - val_loss: 0.1296
Epoch 537/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1289 - val_loss: 0.1295
Epoch 538/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1288 - val_loss: 0.1295
Epoch 539/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1288 - val_loss: 0.1294
Epoch 540/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1287 - val_loss: 0.1294
Epoch 541/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1287 - val_loss: 0.1293
Epoch 542/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1287 - val_loss: 0.1293
Epoch 543/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1286 - val_loss: 0.1293
Epoch 544/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1286 - val_loss: 0.1292
Epoch 545/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1286 - val_loss: 0.1292
Epoch 546/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1285 - val_loss: 0.1291
Epoch 547/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1285 - val_loss: 0.1291
Epoch 548/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1284 - val_loss: 0.1291
Epoch 549/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1284 - val_loss: 0.1290
Epoch 550/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1284 - val_loss: 0.1290
Epoch 551/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1283 - val_loss: 0.1289
Epoch 552/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1283 - val_loss: 0.1289
Epoch 553/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1282 - val_loss: 0.1289
Epoch 554/1000
3/3 [==============================] - 0s 29ms/step - loss: 0.1282 - val_loss: 0.1288
Epoch 555/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1282 - val_loss: 0.1288
Epoch 556/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1281 - val_loss: 0.1287
Epoch 557/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1281 - val_loss: 0.1287
Epoch 558/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1281 - val_loss: 0.1287
Epoch 559/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1280 - val_loss: 0.1286
Epoch 560/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1280 - val_loss: 0.1286
Epoch 561/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1280 - val_loss: 0.1285
Epoch 562/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1279 - val_loss: 0.1285
Epoch 563/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1279 - val_loss: 0.1285
Epoch 564/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1278 - val_loss: 0.1284
Epoch 565/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1278 - val_loss: 0.1284
Epoch 566/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1278 - val_loss: 0.1283
Epoch 567/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1277 - val_loss: 0.1283
Epoch 568/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1277 - val_loss: 0.1283
Epoch 569/1000
3/3 [==============================] - 0s 40ms/step - loss: 0.1277 - val_loss: 0.1282
Epoch 570/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1276 - val_loss: 0.1282
Epoch 571/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1276 - val_loss: 0.1282
Epoch 572/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1275 - val_loss: 0.1281
Epoch 573/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1275 - val_loss: 0.1281
Epoch 574/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1275 - val_loss: 0.1280
Epoch 575/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1274 - val_loss: 0.1280
Epoch 576/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1274 - val_loss: 0.1280
Epoch 577/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1274 - val_loss: 0.1279
Epoch 578/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1273 - val_loss: 0.1279
Epoch 579/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1273 - val_loss: 0.1279
Epoch 580/1000
3/3 [==============================] - 0s 12ms/step - loss: 0.1273 - val_loss: 0.1278
Epoch 581/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1272 - val_loss: 0.1278
Epoch 582/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1272 - val_loss: 0.1278
Epoch 583/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1272 - val_loss: 0.1277
Epoch 584/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1272 - val_loss: 0.1277
Epoch 585/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1271 - val_loss: 0.1277
Epoch 586/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1271 - val_loss: 0.1276
Epoch 587/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1271 - val_loss: 0.1276
Epoch 588/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1271 - val_loss: 0.1276
Epoch 589/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1270 - val_loss: 0.1276
Epoch 590/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1270 - val_loss: 0.1276
Epoch 591/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1270 - val_loss: 0.1275
Epoch 592/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1270 - val_loss: 0.1275
Epoch 593/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1269 - val_loss: 0.1275
Epoch 594/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1269 - val_loss: 0.1274
Epoch 595/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1269 - val_loss: 0.1274
Epoch 596/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1268 - val_loss: 0.1274
Epoch 597/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1268 - val_loss: 0.1273
Epoch 598/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1268 - val_loss: 0.1273
Epoch 599/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1267 - val_loss: 0.1273
Epoch 600/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1267 - val_loss: 0.1272
Epoch 601/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1267 - val_loss: 0.1272
Epoch 602/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1267 - val_loss: 0.1272
Epoch 603/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1266 - val_loss: 0.1271
Epoch 604/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1266 - val_loss: 0.1271
Epoch 605/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1265 - val_loss: 0.1270
Epoch 606/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1265 - val_loss: 0.1270
Epoch 607/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1265 - val_loss: 0.1270
Epoch 608/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1264 - val_loss: 0.1269
Epoch 609/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1264 - val_loss: 0.1269
Epoch 610/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1264 - val_loss: 0.1269
Epoch 611/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1264 - val_loss: 0.1269
Epoch 612/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1263 - val_loss: 0.1269
Epoch 613/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1263 - val_loss: 0.1268
Epoch 614/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1263 - val_loss: 0.1268
Epoch 615/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1263 - val_loss: 0.1268
Epoch 616/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1262 - val_loss: 0.1267
Epoch 617/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1262 - val_loss: 0.1267
Epoch 618/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1262 - val_loss: 0.1267
Epoch 619/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1262 - val_loss: 0.1266
Epoch 620/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1261 - val_loss: 0.1266
Epoch 621/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1261 - val_loss: 0.1266
Epoch 622/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1261 - val_loss: 0.1265
Epoch 623/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1260 - val_loss: 0.1265
Epoch 624/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1260 - val_loss: 0.1265
Epoch 625/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1260 - val_loss: 0.1264
Epoch 626/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1259 - val_loss: 0.1264
Epoch 627/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1259 - val_loss: 0.1264
Epoch 628/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1259 - val_loss: 0.1263
Epoch 629/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1259 - val_loss: 0.1263
Epoch 630/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1258 - val_loss: 0.1263
Epoch 631/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1258 - val_loss: 0.1263
Epoch 632/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1258 - val_loss: 0.1262
Epoch 633/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1258 - val_loss: 0.1262
Epoch 634/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1257 - val_loss: 0.1262
Epoch 635/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1257 - val_loss: 0.1262
Epoch 636/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1257 - val_loss: 0.1261
Epoch 637/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1256 - val_loss: 0.1261
Epoch 638/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1256 - val_loss: 0.1261
Epoch 639/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1256 - val_loss: 0.1260
Epoch 640/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1256 - val_loss: 0.1260
Epoch 641/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1256 - val_loss: 0.1260
Epoch 642/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1255 - val_loss: 0.1260
Epoch 643/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1255 - val_loss: 0.1259
Epoch 644/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1255 - val_loss: 0.1259
Epoch 645/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1255 - val_loss: 0.1259
Epoch 646/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1254 - val_loss: 0.1259
Epoch 647/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1254 - val_loss: 0.1258
Epoch 648/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1254 - val_loss: 0.1258
Epoch 649/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1254 - val_loss: 0.1258
Epoch 650/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1253 - val_loss: 0.1258
Epoch 651/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1253 - val_loss: 0.1257
Epoch 652/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1253 - val_loss: 0.1257
Epoch 653/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1253 - val_loss: 0.1257
Epoch 654/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1253 - val_loss: 0.1257
Epoch 655/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1253 - val_loss: 0.1257
Epoch 656/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1252 - val_loss: 0.1257
Epoch 657/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1252 - val_loss: 0.1256
Epoch 658/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1252 - val_loss: 0.1256
Epoch 659/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1252 - val_loss: 0.1256
Epoch 660/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1251 - val_loss: 0.1255
Epoch 661/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1251 - val_loss: 0.1255
Epoch 662/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1251 - val_loss: 0.1255
Epoch 663/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1251 - val_loss: 0.1255
Epoch 664/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1251 - val_loss: 0.1255
Epoch 665/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1250 - val_loss: 0.1254
Epoch 666/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1250 - val_loss: 0.1254
Epoch 667/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1250 - val_loss: 0.1254
Epoch 668/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1250 - val_loss: 0.1254
Epoch 669/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1249 - val_loss: 0.1253
Epoch 670/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1249 - val_loss: 0.1253
Epoch 671/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1249 - val_loss: 0.1253
Epoch 672/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1249 - val_loss: 0.1253
Epoch 673/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1249 - val_loss: 0.1253
Epoch 674/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1249 - val_loss: 0.1252
Epoch 675/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1248 - val_loss: 0.1252
Epoch 676/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1248 - val_loss: 0.1252
Epoch 677/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1248 - val_loss: 0.1252
Epoch 678/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1248 - val_loss: 0.1252
Epoch 679/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1248 - val_loss: 0.1251
Epoch 680/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1248 - val_loss: 0.1251
Epoch 681/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1247 - val_loss: 0.1251
Epoch 682/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1247 - val_loss: 0.1251
Epoch 683/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1247 - val_loss: 0.1250
Epoch 684/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1247 - val_loss: 0.1250
Epoch 685/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1247 - val_loss: 0.1250
Epoch 686/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1246 - val_loss: 0.1250
Epoch 687/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1246 - val_loss: 0.1250
Epoch 688/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1246 - val_loss: 0.1250
Epoch 689/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1246 - val_loss: 0.1249
Epoch 690/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1246 - val_loss: 0.1249
Epoch 691/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1246 - val_loss: 0.1249
Epoch 692/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1246 - val_loss: 0.1249
Epoch 693/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1245 - val_loss: 0.1249
Epoch 694/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1245 - val_loss: 0.1248
Epoch 695/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1245 - val_loss: 0.1248
Epoch 696/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1245 - val_loss: 0.1248
Epoch 697/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1244 - val_loss: 0.1248
Epoch 698/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1244 - val_loss: 0.1247
Epoch 699/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1244 - val_loss: 0.1247
Epoch 700/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1244 - val_loss: 0.1247
Epoch 701/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1244 - val_loss: 0.1247
Epoch 702/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1243 - val_loss: 0.1247
Epoch 703/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1243 - val_loss: 0.1247
Epoch 704/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1243 - val_loss: 0.1247
Epoch 705/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1243 - val_loss: 0.1246
Epoch 706/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1243 - val_loss: 0.1246
Epoch 707/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1243 - val_loss: 0.1246
Epoch 708/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1243 - val_loss: 0.1246
Epoch 709/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1242 - val_loss: 0.1246
Epoch 710/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1242 - val_loss: 0.1245
Epoch 711/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1242 - val_loss: 0.1245
Epoch 712/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1242 - val_loss: 0.1245
Epoch 713/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1242 - val_loss: 0.1245
Epoch 714/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1242 - val_loss: 0.1245
Epoch 715/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1242 - val_loss: 0.1245
Epoch 716/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1241 - val_loss: 0.1245
Epoch 717/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1241 - val_loss: 0.1244
Epoch 718/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1241 - val_loss: 0.1244
Epoch 719/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1241 - val_loss: 0.1244
Epoch 720/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1241 - val_loss: 0.1244
Epoch 721/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1241 - val_loss: 0.1244
Epoch 722/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1241 - val_loss: 0.1244
Epoch 723/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1240 - val_loss: 0.1243
Epoch 724/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1240 - val_loss: 0.1243
Epoch 725/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1240 - val_loss: 0.1243
Epoch 726/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1240 - val_loss: 0.1243
Epoch 727/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1240 - val_loss: 0.1243
Epoch 728/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1240 - val_loss: 0.1243
Epoch 729/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1239 - val_loss: 0.1243
Epoch 730/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1239 - val_loss: 0.1242
Epoch 731/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1239 - val_loss: 0.1242
Epoch 732/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1239 - val_loss: 0.1242
Epoch 733/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1239 - val_loss: 0.1242
Epoch 734/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1239 - val_loss: 0.1242
Epoch 735/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1239 - val_loss: 0.1241
Epoch 736/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1239 - val_loss: 0.1241
Epoch 737/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1238 - val_loss: 0.1241
Epoch 738/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1238 - val_loss: 0.1241
Epoch 739/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1238 - val_loss: 0.1241
Epoch 740/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1238 - val_loss: 0.1241
Epoch 741/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1238 - val_loss: 0.1241
Epoch 742/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1238 - val_loss: 0.1240
Epoch 743/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1238 - val_loss: 0.1240
Epoch 744/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1237 - val_loss: 0.1240
Epoch 745/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1237 - val_loss: 0.1240
Epoch 746/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1237 - val_loss: 0.1240
Epoch 747/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1237 - val_loss: 0.1240
Epoch 748/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1237 - val_loss: 0.1240
Epoch 749/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1237 - val_loss: 0.1240
Epoch 750/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1237 - val_loss: 0.1239
Epoch 751/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1237 - val_loss: 0.1239
Epoch 752/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1237 - val_loss: 0.1239
Epoch 753/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1236 - val_loss: 0.1239
Epoch 754/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1236 - val_loss: 0.1239
Epoch 755/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1236 - val_loss: 0.1239
Epoch 756/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1236 - val_loss: 0.1238
Epoch 757/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1236 - val_loss: 0.1238
Epoch 758/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1236 - val_loss: 0.1238
Epoch 759/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1236 - val_loss: 0.1238
Epoch 760/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1235 - val_loss: 0.1238
Epoch 761/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1235 - val_loss: 0.1238
Epoch 762/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1235 - val_loss: 0.1238
Epoch 763/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1235 - val_loss: 0.1238
Epoch 764/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1235 - val_loss: 0.1237
Epoch 765/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1235 - val_loss: 0.1237
Epoch 766/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1235 - val_loss: 0.1237
Epoch 767/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1235 - val_loss: 0.1237
Epoch 768/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1235 - val_loss: 0.1237
Epoch 769/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1234 - val_loss: 0.1237
Epoch 770/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1234 - val_loss: 0.1237
Epoch 771/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1234 - val_loss: 0.1237
Epoch 772/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1234 - val_loss: 0.1236
Epoch 773/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1234 - val_loss: 0.1236
Epoch 774/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1234 - val_loss: 0.1236
Epoch 775/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1234 - val_loss: 0.1236
Epoch 776/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1234 - val_loss: 0.1236
Epoch 777/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1234 - val_loss: 0.1236
Epoch 778/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1234 - val_loss: 0.1236
Epoch 779/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1233 - val_loss: 0.1236
Epoch 780/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1233 - val_loss: 0.1236
Epoch 781/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 782/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 783/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 784/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 785/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 786/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 787/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 788/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1233 - val_loss: 0.1235
Epoch 789/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 790/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 791/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 792/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 793/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 794/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 795/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 796/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 797/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1232 - val_loss: 0.1234
Epoch 798/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1232 - val_loss: 0.1233
Epoch 799/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 800/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 801/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 802/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 803/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 804/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 805/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 806/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 807/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 808/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 809/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1231 - val_loss: 0.1232
Epoch 810/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1230 - val_loss: 0.1232
Epoch 811/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 812/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1231 - val_loss: 0.1233
Epoch 813/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1231 - val_loss: 0.1232
Epoch 814/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1231 - val_loss: 0.1232
Epoch 815/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1230 - val_loss: 0.1232
Epoch 816/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1230 - val_loss: 0.1232
Epoch 817/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1230 - val_loss: 0.1232
Epoch 818/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1230 - val_loss: 0.1232
Epoch 819/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1230 - val_loss: 0.1231
Epoch 820/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1230 - val_loss: 0.1231
Epoch 821/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1230 - val_loss: 0.1231
Epoch 822/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 823/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 824/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 825/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 826/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 827/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 828/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 829/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 830/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 831/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 832/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 833/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1229 - val_loss: 0.1231
Epoch 834/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1229 - val_loss: 0.1230
Epoch 835/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1229 - val_loss: 0.1230
Epoch 836/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1229 - val_loss: 0.1230
Epoch 837/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1229 - val_loss: 0.1230
Epoch 838/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1229 - val_loss: 0.1230
Epoch 839/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1229 - val_loss: 0.1230
Epoch 840/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1228 - val_loss: 0.1230
Epoch 841/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1228 - val_loss: 0.1230
Epoch 842/1000
3/3 [==============================] - 0s 27ms/step - loss: 0.1228 - val_loss: 0.1230
Epoch 843/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1228 - val_loss: 0.1229
Epoch 844/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1228 - val_loss: 0.1229
Epoch 845/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1228 - val_loss: 0.1229
Epoch 846/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1228 - val_loss: 0.1229
Epoch 847/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1228 - val_loss: 0.1229
Epoch 848/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1228 - val_loss: 0.1229
Epoch 849/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1227 - val_loss: 0.1229
Epoch 850/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1227 - val_loss: 0.1229
Epoch 851/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1227 - val_loss: 0.1229
Epoch 852/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1227 - val_loss: 0.1229
Epoch 853/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1227 - val_loss: 0.1229
Epoch 854/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 855/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 856/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 857/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 858/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 859/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 860/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 861/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 862/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 863/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 864/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1227 - val_loss: 0.1228
Epoch 865/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1226 - val_loss: 0.1228
Epoch 866/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1226 - val_loss: 0.1228
Epoch 867/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1226 - val_loss: 0.1228
Epoch 868/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 869/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 870/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 871/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 872/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 873/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 874/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 875/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 876/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 877/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 878/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 879/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 880/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 881/1000
3/3 [==============================] - 0s 24ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 882/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1226 - val_loss: 0.1227
Epoch 883/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1225 - val_loss: 0.1227
Epoch 884/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1225 - val_loss: 0.1227
Epoch 885/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 886/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 887/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 888/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 889/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 890/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 891/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 892/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 893/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 894/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 895/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 896/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 897/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 898/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 899/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 900/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 901/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1225 - val_loss: 0.1226
Epoch 902/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 903/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 904/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 905/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 906/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 907/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 908/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 909/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 910/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 911/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 912/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 913/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 914/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 915/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 916/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 917/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 918/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 919/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 920/1000
3/3 [==============================] - 0s 31ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 921/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 922/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1224 - val_loss: 0.1225
Epoch 923/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 924/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 925/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 926/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 927/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 928/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 929/1000
3/3 [==============================] - 0s 26ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 930/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1224 - val_loss: 0.1224
Epoch 931/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 932/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 933/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 934/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 935/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 936/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 937/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 938/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 939/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1223 - val_loss: 0.1224
Epoch 940/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 941/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 942/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 943/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 944/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 945/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 946/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 947/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 948/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 949/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 950/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 951/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1223 - val_loss: 0.1223
Epoch 952/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 953/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 954/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 955/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 956/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 957/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 958/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 959/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 960/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 961/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 962/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 963/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 964/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 965/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1222 - val_loss: 0.1223
Epoch 966/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 967/1000
3/3 [==============================] - 0s 16ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 968/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 969/1000
3/3 [==============================] - 0s 18ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 970/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 971/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 972/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 973/1000
3/3 [==============================] - 0s 25ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 974/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 975/1000
3/3 [==============================] - 0s 23ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 976/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 977/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 978/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 979/1000
3/3 [==============================] - 0s 13ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 980/1000
3/3 [==============================] - 0s 17ms/step - loss: 0.1222 - val_loss: 0.1222
Epoch 981/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 982/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 983/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 984/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 985/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 986/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 987/1000
3/3 [==============================] - 0s 14ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 988/1000
3/3 [==============================] - 0s 15ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 989/1000
3/3 [==============================] - 0s 28ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 990/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 991/1000
3/3 [==============================] - 0s 21ms/step - loss: 0.1221 - val_loss: 0.1222
Epoch 992/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1221 - val_loss: 0.1221
Epoch 993/1000
3/3 [==============================] - 0s 20ms/step - loss: 0.1221 - val_loss: 0.1221
Epoch 994/1000
3/3 [==============================] - 0s 22ms/step - loss: 0.1221 - val_loss: 0.1221
Epoch 995/1000
3/3 [==============================] - 0s 19ms/step - loss: 0.1221 - val_loss: 0.1221
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

* RNN(Recurrent Neural Network)

![img](https://images.ctfassets.net/3viuren4us1n/6lAT51BtXZ3zD5HCxe09m2/2b3b959d4c2a24aaa243c4b7771e9fd6/RNN2.jpg?fm=webp&w=828)

현재 대상은 과거 대상 출력의 중첩.. 다시(re) 현재(current) => Dense 학습 => 미래 예측  
현재입력의 가중치 + 과거출력의 가중치 = 현재출력  
미래입력의 가중치 + 현재출력의 가중치 = 미래예측

라인(시간, 흐름)이 길어질수록 과거의 입력값이 미치는 영향이 희석된다!

과거의 값을 일부는 그대로, 일부는 현재와 혼합해 과거의 값이 미치는 영향을 보존해 RNN을 보강 => LSTM  
LSTM(Long Short Term Memory) - RNN만 사용할 경우 과거의 영향이 줄어들 경우 사용한다.  
LSTM은 연산과정이 오래 걸리므로 Long Term이 필요없을 경우 RNN만 사용하여도 충분하다.

![img](http://i.imgur.com/jKodJ1u.png)

# Reference

* TELUS International: [What’s the difference between CNN and RNN?](https://www.telusinternational.com/insights/ai-data/article/difference-between-cnn-and-rnn)
