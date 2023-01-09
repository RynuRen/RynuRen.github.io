---
layout: jupyter
title: 자연어 처리 - 정규 표현식
published: true
date: 2023-01-04
description: 정규 표현식
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: 정규 표현식
---
---
# 정규 표현식
re: 정규 표현식, regular expression, 텍스트 처리

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import re 
```

</div>

## 기호 .
 .의 갯수만큼의 문자

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("a.c")
```

</div>

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("qwerty"))
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
# r = re.compile("a.c")
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 3), match='abc'>

```

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("axc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 3), match='axc'>

```

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
r2 = re.compile("a..c")
r2.search("abbc")
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>




{:.output_data_text}

```
<re.Match object; span=(0, 4), match='abbc'>
```



## 기호 ?
기호 앞의 문자가 0개 이거나 1개 이거나

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("ab?c") # a와 c사이에 b가 와야함
```

</div>

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 3), match='abc'>

```

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("axc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("ac"))
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 2), match='ac'>

```

## 기호 *
기호 앞의 문자가 1개 이상

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("ab*c")
```

</div>

<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("a"))
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("ac"))
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 2), match='ac'>

```

<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[16]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 3), match='abc'>

```

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[17]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 4), match='abbc'>

```

## 기호 +
기호 앞의 문자가 1개 이상

<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("ab+c")
```

</div>

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("ac"))
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 3), match='abc'>

```

<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 4), match='abbc'>

```

## 기호 ^
기호 뒤의 문자로 시작

<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("^ab")
```

</div>

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("ab"))
```

</div>

<div class="output_prompt">
Out&nbsp;[23]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 2), match='ab'>

```

<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("aab"))
```

</div>

<div class="output_prompt">
Out&nbsp;[24]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[25]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[25]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 2), match='ab'>

```

## 기호 {숫자}
기호 앞의 문자가 숫자 만큼 반복

<div class="in_prompt">
In&nbsp;[26]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("ab{2}c")
```

</div>

<div class="in_prompt">
In&nbsp;[27]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[27]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[28]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[28]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 4), match='abbc'>

```

## 기호 {숫자1, 숫자2}
기호 앞의 문자가 숫자1 ~ 숫자2까지 반복

<div class="in_prompt">
In&nbsp;[29]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("ab{2,4}c")
```

</div>

<div class="in_prompt">
In&nbsp;[30]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[30]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[31]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[31]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 4), match='abbc'>

```

<div class="in_prompt">
In&nbsp;[32]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[32]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 5), match='abbbc'>

```

<div class="in_prompt">
In&nbsp;[33]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbbbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[33]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 6), match='abbbbc'>

```

<div class="in_prompt">
In&nbsp;[34]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbbbbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[34]:
</div>

{:.output_stream}

```
None

```

## 기호 {숫자, }
기호 앞의 문자가 숫자이상 반복

<div class="in_prompt">
In&nbsp;[35]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("ab{2,}c")
```

</div>

<div class="in_prompt">
In&nbsp;[36]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[36]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[37]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[37]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 4), match='abbc'>

```

<div class="in_prompt">
In&nbsp;[38]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[38]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 5), match='abbbc'>

```

<div class="in_prompt">
In&nbsp;[39]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbbbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[39]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 6), match='abbbbc'>

```

<div class="in_prompt">
In&nbsp;[40]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abbbbbc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[40]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 7), match='abbbbbc'>

```

## 기호 [ - ]
문자의 범위 앞-뒤 까지 순서대로 하나 찾음

<div class="in_prompt">
In&nbsp;[41]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("[b-e]")
```

</div>

<div class="in_prompt">
In&nbsp;[42]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abcdefghi"))
```

</div>

<div class="output_prompt">
Out&nbsp;[42]:
</div>

{:.output_stream}

```
<re.Match object; span=(1, 2), match='b'>

```

<div class="in_prompt">
In&nbsp;[43]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("defghijklmn"))
```

</div>

<div class="output_prompt">
Out&nbsp;[43]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 1), match='d'>

```

<div class="in_prompt">
In&nbsp;[44]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("efghijklmn"))
```

</div>

<div class="output_prompt">
Out&nbsp;[44]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 1), match='e'>

```

<div class="in_prompt">
In&nbsp;[45]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("fghijklmn"))
```

</div>

<div class="output_prompt">
Out&nbsp;[45]:
</div>

{:.output_stream}

```
None

```

## 기호 [^ ]
뒤의 문자를 제외하고 하나 찾음

<div class="in_prompt">
In&nbsp;[46]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("[^abcd]")
```

</div>

<div class="in_prompt">
In&nbsp;[47]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("a"))
```

</div>

<div class="output_prompt">
Out&nbsp;[47]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[48]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abc"))
```

</div>

<div class="output_prompt">
Out&nbsp;[48]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[49]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("bcd"))
```

</div>

<div class="output_prompt">
Out&nbsp;[49]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[50]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("abcde"))
```

</div>

<div class="output_prompt">
Out&nbsp;[50]:
</div>

{:.output_stream}

```
<re.Match object; span=(4, 5), match='e'>

```

<div class="in_prompt">
In&nbsp;[51]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("efg"))
```

</div>

<div class="output_prompt">
Out&nbsp;[51]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 1), match='e'>

```

## re.match vs re.search

<div class="in_prompt">
In&nbsp;[52]:
</div>

<div class="input_area" markdown="1">

```python
r = re.compile("ab.")
```

</div>

<div class="in_prompt">
In&nbsp;[53]:
</div>

<div class="input_area" markdown="1">

```python
print(r.match("tttabc")) # match: 처음부터 조사
```

</div>

<div class="output_prompt">
Out&nbsp;[53]:
</div>

{:.output_stream}

```
None

```

<div class="in_prompt">
In&nbsp;[54]:
</div>

<div class="input_area" markdown="1">

```python
print(r.match("abcttt"))
```

</div>

<div class="output_prompt">
Out&nbsp;[54]:
</div>

{:.output_stream}

```
<re.Match object; span=(0, 3), match='abc'>

```

<div class="in_prompt">
In&nbsp;[55]:
</div>

<div class="input_area" markdown="1">

```python
print(r.search("tttabc"))# search: 텍스트 전체를 보고 정규식(compile) 조사
```

</div>

<div class="output_prompt">
Out&nbsp;[55]:
</div>

{:.output_stream}

```
<re.Match object; span=(3, 6), match='abc'>

```

## 텍스트 분해

<div class="in_prompt">
In&nbsp;[56]:
</div>

<div class="input_area" markdown="1">

```python
text = "자, 이제 match 메서드와 search 메서드를 수행한 결과로 리턴된 match 객체에 대해 알아보자. 앞에서 정규식을 사용한 문자열 검색을 수행하면서 아마도 다음과 같은 궁금증이 생겼을 것이다."
```

</div>

<div class="in_prompt">
In&nbsp;[57]:
</div>

<div class="input_area" markdown="1">

```python
# 파이썬 내장 함수 split
text.split()
```

</div>

<div class="output_prompt">
Out&nbsp;[57]:
</div>




{:.output_data_text}

```
['자,',
 '이제',
 'match',
 '메서드와',
 'search',
 '메서드를',
 '수행한',
 '결과로',
 '리턴된',
 'match',
 '객체에',
 '대해',
 '알아보자.',
 '앞에서',
 '정규식을',
 '사용한',
 '문자열',
 '검색을',
 '수행하면서',
 '아마도',
 '다음과',
 '같은',
 '궁금증이',
 '생겼을',
 '것이다.']
```



<div class="in_prompt">
In&nbsp;[58]:
</div>

<div class="input_area" markdown="1">

```python
# re split 함수: 간단한 상수 문자열 일치로는 충분하지 않은 경우를 위해 설계
re.split(" ", text)
```

</div>

<div class="output_prompt">
Out&nbsp;[58]:
</div>




{:.output_data_text}

```
['자,',
 '이제',
 'match',
 '메서드와',
 'search',
 '메서드를',
 '수행한',
 '결과로',
 '리턴된',
 'match',
 '객체에',
 '대해',
 '알아보자.',
 '앞에서',
 '정규식을',
 '사용한',
 '문자열',
 '검색을',
 '수행하면서',
 '아마도',
 '다음과',
 '같은',
 '궁금증이',
 '생겼을',
 '것이다.']
```



<div class="in_prompt">
In&nbsp;[59]:
</div>

<div class="input_area" markdown="1">

```python
text2 = "안녕 나는+파이썬이야"
re.split("\+", text2)
```

</div>

<div class="output_prompt">
Out&nbsp;[59]:
</div>




{:.output_data_text}

```
['안녕 나는', '파이썬이야']
```



<div class="in_prompt">
In&nbsp;[60]:
</div>

<div class="input_area" markdown="1">

```python
text2.split("+")
```

</div>

<div class="output_prompt">
Out&nbsp;[60]:
</div>




{:.output_data_text}

```
['안녕 나는', '파이썬이야']
```



<div class="in_prompt">
In&nbsp;[61]:
</div>

<div class="input_area" markdown="1">

```python
text3 = '''
이름:Luke Skywarker
전화번호: 02-123-4567
이메일:luke@daum.net
성별:남자
'''
re.findall("\d+", text3) # \d: 숫자
```

</div>

<div class="output_prompt">
Out&nbsp;[61]:
</div>




{:.output_data_text}

```
['02', '123', '4567']
```



<div class="in_prompt">
In&nbsp;[62]:
</div>

<div class="input_area" markdown="1">

```python
text4 = '''“이온빔을 HfO2 기반 강유전체에 조사해 산소 공공을 형성했다”며 “기존의 복잡한 공정과 후처리 과정 없이 이온빔 조사밀도 조절만으로 강유전성을 200% 이상 강화했다”'''
re_res = re.sub("[^a-zA-Z]", ' ', text4) # 대소문자를 제외하고 ' '(공백)으로 치환
re_res
```

</div>

<div class="output_prompt">
Out&nbsp;[62]:
</div>




{:.output_data_text}

```
'      HfO                                                                                      '
```



<div class="in_prompt">
In&nbsp;[63]:
</div>

<div class="input_area" markdown="1">

```python
re_res.split()
```

</div>

<div class="output_prompt">
Out&nbsp;[63]:
</div>




{:.output_data_text}

```
['HfO']
```



<div class="in_prompt">
In&nbsp;[64]:
</div>

<div class="input_area" markdown="1">

```python
text5 = "ㅋㅋㅋ so 이따가 시간 돼?ㅠㅠ"
re.compile("[ 가-힣]+").sub("", text5) # 가-힝: 완성형 한글 전체 범위
```

</div>

<div class="output_prompt">
Out&nbsp;[64]:
</div>




{:.output_data_text}

```
'ㅋㅋㅋso?ㅠㅠ'
```



<div class="in_prompt">
In&nbsp;[65]:
</div>

<div class="input_area" markdown="1">

```python
re.compile("[가-힣]+").findall(text5)
```

</div>

<div class="output_prompt">
Out&nbsp;[65]:
</div>




{:.output_data_text}

```
['이따가', '시간', '돼']
```



<div class="in_prompt">
In&nbsp;[66]:
</div>

<div class="input_area" markdown="1">

```python
re.compile("[가-힣]+").sub("", text5).split()
```

</div>

<div class="output_prompt">
Out&nbsp;[66]:
</div>




{:.output_data_text}

```
['ㅋㅋㅋ', 'so', '?ㅠㅠ']
```



### s.split() vs. re.split(s)

<div class="in_prompt">
In&nbsp;[67]:
</div>

<div class="input_area" markdown="1">

```python
import re, time, random 

def random_string(_len):
    letters = "ABC"
    return "".join([letters[random.randint(0,len(letters)-1)] for i in range(_len) ])

r = random_string(2000000)
pattern = re.compile(r"A")

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
Out&nbsp;[67]:
</div>

{:.output_stream}

```
re.split :  0.06091761589050293
내장 split :  0.03753256797790527

```

<div class="in_prompt">
In&nbsp;[68]:
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
Out&nbsp;[68]:
</div>

{:.output_stream}

```
['One', 'two', 't h r e e', 'fourth field']
['One', 'two', '', 't h r e e', '', '', 'fourth field']
['One', 'two', 't h r e e', 'fourth field']

```

내장 split은 추가로 작업이 더 필요하다

## ex

<div class="in_prompt">
In&nbsp;[69]:
</div>

<div class="input_area" markdown="1">

```python
search_target = '''Luke Skywarker 021234567 luke@daum.net
다스베이더 070-9999-9999 darth_vader@gmail.com
princess leia 010 2454 3457 leia@gmail.com'''
```

</div>

<div class="in_prompt">
In&nbsp;[70]:
</div>

<div class="input_area" markdown="1">

```python
regex = r'0\d{1,2}[ -]?\d{3,4}[ -]?\d{3,4}' 
```

</div>

<div class="in_prompt">
In&nbsp;[71]:
</div>

<div class="input_area" markdown="1">

```python
result = re.findall(regex, search_target)
print("\n".join(result))
```

</div>

<div class="output_prompt">
Out&nbsp;[71]:
</div>

{:.output_stream}

```
021234567
070-9999-9999
010 2454 3457

```

# Reference

* stackoverflow: [Python re.split() vs split()](https://stackoverflow.com/questions/7501609/python-re-split-vs-split)
* 프로그래머스: [정규표현식 사용해 보기](https://school.programmers.co.kr/learn/courses/11/lessons/132#fnref1)
