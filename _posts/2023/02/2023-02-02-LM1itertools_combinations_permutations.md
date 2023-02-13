---
layout: jupyter
title: itertools - 조합, 순열
published: true
date: 2023-02-02
description: itertools - 조합, 순열
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: itertools - 조합, 순열
use_math: true
---
---
# itertools - 조합, 순열

조합(combinations)|순열(permutations)
:-:|:-:
순서x|순서o
$$_nC_r$$|$$_nP_r$$
$$\frac{n!}{(n-r)!r!}$$|$$\frac{n!}{(n-r)!}$$

## 조합
n개 중에서 r개를 뽑는 경우의 수

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
from itertools import combinations
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
fruits = ['apple', 'orange', 'banana']
```

</div>

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
for coms in combinations(fruits, 2):
    print(coms)
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
('apple', 'orange')
('apple', 'banana')
('orange', 'banana')

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
# combinations 구현
for i in range(len(fruits)):
    for j in range(i+1, len(fruits)):
        print((fruits[i], fruits[j]))
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
('apple', 'orange')
('apple', 'banana')
('orange', 'banana')

```

## 순열
n개 중에서 순서를 정해 r개를 뽑는 경우의 수

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
from itertools import permutations
```

</div>

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
fruits = ['apple', 'orange', 'banana']
```

</div>

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
for perms in permutations(fruits, 2):
    print(perms)
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>

{:.output_stream}

```
('apple', 'orange')
('apple', 'banana')
('orange', 'apple')
('orange', 'banana')
('banana', 'apple')
('banana', 'orange')

```

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
# permutations 구현
for i in range(len(fruits)):
    for j in range(len(fruits)):
        if i != j:
            print((fruits[i], fruits[j]))
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>

{:.output_stream}

```
('apple', 'orange')
('apple', 'banana')
('orange', 'apple')
('orange', 'banana')
('banana', 'apple')
('banana', 'orange')

```
