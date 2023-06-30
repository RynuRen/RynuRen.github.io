---
layout: jupyter
title: python round 함수의 문제
published: true
date: 2023-06-30
description: python round 함수의 문제
comments: true
categories:
 - python
tags:
 - python
 - coding tip
toc: true
toc_sticky: true
toc_label: python round 함수의 문제
use_math: true
---
---
# python round 함수의 문제

## 사사오입과 오사오입

* 사사오입: 일반적으로 사용하는 반올림 방법이다. 4 이하의 숫자는 내림, 5 이상의 숫자는 올린다.

* 오사오입(round-to-nearest-even): 파이썬에서 기본적으로 적용되어 있는 방법이다. 5 미만의 숫자는 내림, 5 초과의 숫자는 올린다.  
5일 경우에는 해당 숫자의 앞자리가 짝수면 내림, 홀수면 올린다.  
이 방법이 오차가 적은 반올림 방법이라고 할 수 있다.

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
print(round(0.5))
print(round(1.5))
print(round(2.5))
print(round(3.5))
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>

{:.output_stream}

```
0
2
2
4

```

## 사사오입 반올림 함수

실생활에서 자주 사용되는 사사오입 반올림을 적용하기 위해 함수를 작성했다.

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
def roundTraditional(val, digits=1):
    return round(val+10**(-len(str(val))-1), digits)
```

</div>

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
print(roundTraditional(0.5))
print(roundTraditional(1.5))
print(roundTraditional(2.5))
print(roundTraditional(3.5))
print(roundTraditional(1.499999999999999, 0))
print(roundTraditional(1.4999999999999999, 0))
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
0.5
1.5
2.5
3.5
1.0
2.0

```

하지만 5번째와 6번째는 둘다 1이 출력되지 않았다. 이는 파이썬에서 실수 자료형 float을 다룰 때 발생하는 부동 소수점 문제이다.

## 부동소수점 반올림

아래의 반올림들은 오사오입으로도 설명할 수 없다.

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
print(round(0.05, 1))
print(round(0.15, 1))
print(round(0.25, 1))
print(round(0.36, 1))
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
0.1
0.1
0.2
0.4

```

해당 문제가 발생하는 원인은 '파이썬 부동소수점 오차'로 찾아보자.

## decimal 모듈 사용

decimal 모듈에는 회계 응용 프로그램 및 고정밀 응용 프로그램에 적합한 십진 산술이 구현되어 있다.  
매개변수로 문자열을 입력해야 한다.

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
import decimal

context = decimal.getcontext() # 산술 연산을 위한 환경설정
context.rounding = decimal.ROUND_HALF_UP # 사사오입으로 변경
                # decimal.ROUND_HALF_EVEN: 기본 오사오입
```

</div>

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
print(round(decimal.Decimal("1.499999999999999")))
print(round(decimal.Decimal("1.4999999999999999")))
print(round(decimal.Decimal("1.5")))
print(round(decimal.Decimal("0.15"), 1))
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>

{:.output_stream}

```
1
1
2
0.2

```

# Reference

* 위키백과: [반올림](https://ko.wikipedia.org/wiki/%EB%B0%98%EC%98%AC%EB%A6%BC)
* 아무튼 워라밸: [파이썬에서 사사오입으로 반올림 처리하기 (엑셀 반올림 스타일)](https://hleecaster.com/python-round/)
* Python 3.11.4 documentation: [Built-in Types 부동 소수점 산술: 문제점 및 한계](https://docs.python.org/ko/3/tutorial/floatingpoint.html)
* Python 3.11.4 documentation: [decimal — 십진 고정 소수점 및 부동 소수점 산술](https://docs.python.org/ko/3/library/decimal.html)
