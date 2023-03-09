---
layout: jupyter
title: List append_vs_extend
published: true
date: 2023-01-17
description: List append_vs_extend
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: list append_vs_extend
use_math: true
---
---
# append vs extend

* append는 대상을 하나의 요소로 리스트에 추가한다.

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
a = ['apple', 'banana', 'grape']
b = ['orange', 'lemon']
a.append(b)
print(a)
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>

{:.output_stream}

```
['apple', 'banana', 'grape', ['orange', 'lemon']]

```

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
a = ['apple', 'banana', 'grape']
b = 'orange'
a.append(b)
print(a)
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>

{:.output_stream}

```
['apple', 'banana', 'grape', 'orange']

```

* extend는 대상의 요소를 하나하나 뽑아 각각을 리스트에 추가한다.

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
a = ['apple', 'banana', 'grape']
b = ['orange', 'lemon']
a.extend(b)
print(a)
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
['apple', 'banana', 'grape', 'orange', 'lemon']

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
a = ['apple', 'banana', 'grape']
b = 'orange'
a.extend(b)
print(a)
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
['apple', 'banana', 'grape', 'o', 'r', 'a', 'n', 'g', 'e']

```
