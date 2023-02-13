---
layout: jupyter
title: collections - Counter
published: true
date: 2023-02-02
description: collections - Counter
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: collections - Counter
use_math: true
---
---
# collections - Counter

파이썬의 기본 자료구조인 사전(dictionary)를 확장하고 있기 때문에, 사전에서 제공하는 API를 그대로 다 시용할 수가 있다.

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
from collections import Counter
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
word = 'hello world'
```

</div>

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
Counter(word)
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>




{:.output_data_text}

```
Counter({'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1})
```



<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
# Counter 구현
cnt = {}
for w in word:
    if w not in cnt:
        cnt[w] = 0
    cnt[w] += 1
cnt
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>




{:.output_data_text}

```
{'h': 1, 'e': 1, 'l': 3, 'o': 2, ' ': 1, 'w': 1, 'r': 1, 'd': 1}
```


