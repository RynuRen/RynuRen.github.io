---
layout: jupyter
title: s.split() vs. re.split(s)
published: true
date: 2023-01-05
description: s.split() vs. re.split(s)
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: s.split() vs. re.split(s)
---
---
# s.split() vs. re.split(s)

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import re, time, random 

def random_string(_len):
    letters = "ABC"
    return "".join([letters[random.randint(0,len(letters)-1)] for i in range(_len) ])

r = random_string(2000000)
pattern = re.compile(r"A") # f'문자열' fstring # r'문자열' raw string

start = time.time()
# pattern.split(r)
re.split(pattern, r)
print("re.split : ", time.time() - start)

start = time.time()
r.split("A")
print("내장 split : ", time.time() - start)
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>

{:.output_stream}

```
re.split :  0.042563438415527344
내장 split :  0.028233766555786133

```

re의 split은 정규표현식 상태를 불러오는데 조금 더 리소스를 사용하는데 반해 내장 split은 단순히 문자열에서 검색만 하므로 내장 split이 속도면에서는 빠르다.  
따라서 단순한 문자하나로 나눌 경우 re.split을 쓸 이유가 없다.

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
s = "One:two::t h r e e:::fourth field"
print(re.split(':+', s))
print(s.split(':'))

s = "One:two:2:t h r e e:3::fourth field"
print(re.split('[:\d]+',"One:two:2:t h r e e:3::fourth field"))
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>

{:.output_stream}

```
['One', 'two', 't h r e e', 'fourth field']
['One', 'two', '', 't h r e e', '', '', 'fourth field']
['One', 'two', 't h r e e', 'fourth field']

```

복잡한 문자열을 정규표현식으로 나누는데엔 re.split이 훨씬 유연하게 사용할 수 있다.

# Reference

* stackoverflow: [Python re.split() vs split()](https://stackoverflow.com/questions/7501609/python-re-split-vs-split) 
