---
layout: jupyter
title: 반복문~else 문
published: true
date: 2023-01-05
description: 반복문~else 문
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: 반복문~else 문
---
---
# 반복문~else 문

## while~else 문

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
n = int(input())
ans = 0
while n > 0:
    if n % 2 == 0:
        ans = '짝수'
        break
    n = 0
else:
    ans = -1
print(ans)
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>

{:.output_stream}

```
 31

```

{:.output_stream}

```
-1

```

while절의 조건을 만족하지 못하고 while절이 종료될 경우 else절로 가고  
while절 안에서 break에 의해 while절이 종료될 경우 else절을 넘어간다.

## for~else 문을 이용한 이중 for문 탈출

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
for i in range(5) :
    for j in range(5, 11):
        print(i, j)
        if j > 9:
            print("over")
            break
    else:
        continue
    break
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>

{:.output_stream}

```
0 5
0 6
0 7
0 8
0 9
0 10
over

```

안의 for절을 끝까지 완료하고 for절이 완료되면 else절로 향하고 continue에 의해 뒤에 올 break는 만나지 못한채 바깥 for문의 다음 조건을 수행한다.  
안의 for절에서 조건문으로 break를 만나게 되면 else절은 넘어가고 break를 만나 바깥 for문도 종료하게 된다.
