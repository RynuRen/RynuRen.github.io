---
layout: jupyter
title: iterable 객체의 출력
published: true
date: 2023-01-10
description: iterable 객체의 출력
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: iterable 객체의 출력
use_math: true
---
---
# iterable 객체의 출력

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
string = '문자열 입니다.'
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
list(string)
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>




{:.output_data_text}

```
['문', '자', '열', ' ', '입', '니', '다', '.']
```



<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
[*string]
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>




{:.output_data_text}

```
['문', '자', '열', ' ', '입', '니', '다', '.']
```



<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
test = [10 ,20, 30, 40, 50]
print(*test)
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
10 20 30 40 50

```

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
print(*test, sep='\n')
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>

{:.output_stream}

```
10
20
30
40
50

```

iterable 객체(list, dict, set, str, bytes, tuple, range) 앞에 \*를 붙이면 객체 안의 값을 차례대로 꺼내준다.

## 실제 사용

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
# yolo 데이터 처리 중
import os
data = ['/content/data/train/images/A/1.jpg', '/content/data/train/images/A/2.jpg', '/content/data/train/images/B/1.jpg']
for j in range(len(data)):
    print('/'+os.path.join(*data[j].split('/')[:-2])+'/labels/'+data[j].split('/')[-1].split('.jpg')[0]+'.txt')
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>

{:.output_stream}

```
/content\data\train\images/labels/1.txt
/content\data\train\images/labels/2.txt
/content\data\train\images/labels/1.txt

```

* '\\'와 '/'  
Windows OS는 역슬레쉬 '\\': jupyter  
Linux계열 OS는 슬레쉬 '/': colab

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
# OpenCV 영상 코덱 설정 중
import cv2
fourcc = cv2.VideoWriter_fourcc('H','2','6','4')
print(fourcc)
fourcc = cv2.VideoWriter_fourcc(*'H264')
print(fourcc)
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>

{:.output_stream}

```
875967048
875967048

```
