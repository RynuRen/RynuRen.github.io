---
layout: jupyter
title: 자연어 처리 - 형태소 분석
published: true
date: 2023-01-05
description: 형태소 분석
comments: true
categories:
 - python
tags:
 - python
toc: true
toc_sticky: true
toc_label: 형태소 분석
---
---
# 형태소 분석

* konlpy: korean natural language processing python (pip install konlpy)
* JPype: java 라이브러리를 python에서 사용 (pip install JPype1-1.1.2-cp38-cp38-win_amd64.whl) python3.8용

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import konlpy
konlpy.__version__
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>




{:.output_data_text}

```
'0.6.0'
```



<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
from konlpy.tag import Okt
```

</div>

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
okt = Okt()
tokens = okt.morphs("나는 자연어 처리를 배우고 있어요, 너무 신기해요")
tokens
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>




{:.output_data_text}

```
['나', '는', '자연어', '처리', '를', '배우고', '있어요', ',', '너무', '신기해요']
```



<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
"나는 자연어 처리를 배우고 있어요, 너무 신기해요".split()
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>




{:.output_data_text}

```
['나는', '자연어', '처리를', '배우고', '있어요,', '너무', '신기해요']
```



split()으로는 처리를, 처리가, 처리는 모두 다르게 인식하여 '처리'라는 단어를 구분하지 못한다.

형태소 단위로 나눔 - 조사들을 찾아서 때어냄 -> 시간이 오래 걸림

## 영어 형태소 분석기

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
import nltk # 영어에서 많이 사용되는 형태소 분석기
# nltk.download('punkt')
```

</div>

### word_tokenize

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
from nltk.tokenize import word_tokenize
```

</div>

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
text = '''"Wagner soldiers openly advance under fire towards us even if they're littering the land with their bodies, even if out of 60 people in their platoon only 20 are left.
It's very difficult to hold against such an invasion. We weren't prepared for that, and we're learning now," Commander Skala says.'''
```

</div>

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
print(word_tokenize(text))
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>

{:.output_stream}

```
['``', 'Wagner', 'soldiers', 'openly', 'advance', 'under', 'fire', 'towards', 'us', 'even', 'if', 'they', "'re", 'littering', 'the', 'land', 'with', 'their', 'bodies', ',', 'even', 'if', 'out', 'of', '60', 'people', 'in', 'their', 'platoon', 'only', '20', 'are', 'left', '.', 'It', "'s", 'very', 'difficult', 'to', 'hold', 'against', 'such', 'an', 'invasion', '.', 'We', 'were', "n't", 'prepared', 'for', 'that', ',', 'and', 'we', "'re", 'learning', 'now', ',', "''", 'Commander', 'Skala', 'says', '.']

```

### WordPunctTokenizer

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
from nltk.tokenize import WordPunctTokenizer
from keras.preprocessing.text import text_to_word_sequence
```

</div>

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
print(WordPunctTokenizer().tokenize(text)) # 위와 달리 '를 분해.. 다양한 tokenize가 존재
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>

{:.output_stream}

```
['"', 'Wagner', 'soldiers', 'openly', 'advance', 'under', 'fire', 'towards', 'us', 'even', 'if', 'they', "'", 're', 'littering', 'the', 'land', 'with', 'their', 'bodies', ',', 'even', 'if', 'out', 'of', '60', 'people', 'in', 'their', 'platoon', 'only', '20', 'are', 'left', '.', 'It', "'", 's', 'very', 'difficult', 'to', 'hold', 'against', 'such', 'an', 'invasion', '.', 'We', 'weren', "'", 't', 'prepared', 'for', 'that', ',', 'and', 'we', "'", 're', 'learning', 'now', ',"', 'Commander', 'Skala', 'says', '.']

```

### text_to_word_sequence

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
print(text_to_word_sequence(text))
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
</div>

{:.output_stream}

```
['wagner', 'soldiers', 'openly', 'advance', 'under', 'fire', 'towards', 'us', 'even', 'if', "they're", 'littering', 'the', 'land', 'with', 'their', 'bodies', 'even', 'if', 'out', 'of', '60', 'people', 'in', 'their', 'platoon', 'only', '20', 'are', 'left', "it's", 'very', 'difficult', 'to', 'hold', 'against', 'such', 'an', 'invasion', 'we', "weren't", 'prepared', 'for', 'that', 'and', "we're", 'learning', 'now', 'commander', 'skala', 'says']

```

### sent_tokenize

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
from nltk.tokenize import sent_tokenize
```

</div>

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
sent_tokenize(text) # 문장 단위 분리 -> 챗봇 등에 사용
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
</div>




{:.output_data_text}

```
['"Wagner soldiers openly advance under fire towards us even if they\'re littering the land with their bodies, even if out of 60 people in their platoon only 20 are left.',
 "It's very difficult to hold against such an invasion.",
 'We weren\'t prepared for that, and we\'re learning now," Commander Skala says.']
```



### kss

<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
import kss
```

</div>

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
text = "딥러닝 자연어 처리는 흥미롭습니다. 그런데 재미는 없을 수도 있습니다. 특히 일상 언어는 너무 복잡합니다."
kss.split_sentences(text)
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>

{:.output_stream}

```
[Kss]: Because there's no supported C++ morpheme analyzer, Kss will take pecab as a backend. :D
For your information, Kss also supports mecab backend.
We recommend you to install mecab or konlpy.tag.Mecab for faster execution of Kss.
Please refer to following web sites for details:
- mecab: https://cleancode-ws.tistory.com/97
- konlpy.tag.Mecab: https://uwgdqo.tistory.com/363


```




{:.output_data_text}

```
['딥러닝 자연어 처리는 흥미롭습니다.', '그런데 재미는 없을 수도 있습니다.', '특히 일상 언어는 너무 복잡합니다.']
```



* tokenizer 선택 기준: 원하는 파싱(Parsing) 결과가 나오는가? 처리 속도는 적절한가?

## 한국어 형태소 분석기

<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
from konlpy.tag import Okt, Kkma, Komoran
```

</div>

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
okt = Okt()
kkma = Kkma()
kom = Komoran()
```

</div>

<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
text = '열심히 코딩한 당신, 잠도 잘 자고 일하세요'
```

</div>

### Okt

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
print('---- OKT ----') # 활용형에 취약 - 동사, 형용사에서 주로... 명사를 다루는데 문제 없음
print('형태소 분석: ', okt.morphs(text))
print('품사 태깅: ', okt.pos(text))
print('명사 분석: ', okt.nouns(text))
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>

{:.output_stream}

```
---- OKT ----
형태소 분석:  ['열심히', '코딩', '한', '당신', ',', '잠도', '잘', '자고', '일', '하세요']
품사 태깅:  [('열심히', 'Adverb'), ('코딩', 'Noun'), ('한', 'Josa'), ('당신', 'Noun'), (',', 'Punctuation'), ('잠도', 'Noun'), ('잘', 'Verb'), ('자고', 'Noun'), ('일', 'Noun'), ('하세요', 'Verb')]
명사 분석:  ['코딩', '당신', '잠도', '자고', '일']

```

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
print('---- OKT ----') # norm: 단어들을 정규화 # stem: 오타도 수정
print('형태소 분석: ', okt.morphs(text, norm=True, stem=True))
print('품사 태깅: ', okt.pos(text, norm=True, stem=True))
print('명사 분석: ', okt.nouns(text))
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
</div>

{:.output_stream}

```
---- OKT ----
형태소 분석:  ['열심히', '코딩', '한', '당신', ',', '잠도', '자다', '자고', '일', '하다']
품사 태깅:  [('열심히', 'Adverb'), ('코딩', 'Noun'), ('한', 'Josa'), ('당신', 'Noun'), (',', 'Punctuation'), ('잠도', 'Noun'), ('자다', 'Verb'), ('자고', 'Noun'), ('일', 'Noun'), ('하다', 'Verb')]
명사 분석:  ['코딩', '당신', '잠도', '자고', '일']

```

### Kkma

<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
print('---- KKMA ----')
print('형태소 분석: ', kkma.morphs(text))
print('품사 태깅: ', kkma.pos(text))
print('명사 분석: ', kkma.nouns(text))
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>

{:.output_stream}

```
---- KKMA ----
형태소 분석:  ['열심히', '코딩', '하', 'ㄴ', '당신', ',', '잠', '도', '잘', '자', '고', '일하', '세요']
품사 태깅:  [('열심히', 'MAG'), ('코딩', 'NNG'), ('하', 'XSV'), ('ㄴ', 'ETD'), ('당신', 'NP'), (',', 'SP'), ('잠', 'NNG'), ('도', 'JX'), ('잘', 'MAG'), ('자', 'VV'), ('고', 'ECE'), ('일하', 'VV'), ('세요', 'EFN')]
명사 분석:  ['코딩', '당신', '잠']

```

### Komoran

<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
print('---- Komoran ----')
print('형태소 분석: ', kom.morphs(text))
print('품사 태깅: ', kom.pos(text))
print('명사 분석: ', kom.nouns(text))
```

</div>

<div class="output_prompt">
Out&nbsp;[22]:
</div>

{:.output_stream}

```
---- Komoran ----
형태소 분석:  ['열심히', '코', '딩', '하', 'ㄴ', '당신', ',', '잠도', '잘', '자', '고', '일', '하', '시', '어요']
품사 태깅:  [('열심히', 'MAG'), ('코', 'NNG'), ('딩', 'MAG'), ('하', 'XSV'), ('ㄴ', 'ETM'), ('당신', 'NNP'), (',', 'SP'), ('잠도', 'NNP'), ('잘', 'MAG'), ('자', 'VV'), ('고', 'EC'), ('일', 'NNG'), ('하', 'XSV'), ('시', 'EP'), ('어요', 'EC')]
명사 분석:  ['코', '당신', '잠도', '일']

```

## 한국어 전처리 패키지

### 자동 띄어쓰기

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
# !pip install git+https://github.com/haven-jeon/PyKospacing --user
# tensorflow 2.9.3 설치됨
```

</div>

<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
from pykospacing import Spacing
```

</div>

<div class="in_prompt">
In&nbsp;[25]:
</div>

<div class="input_area" markdown="1">

```python
text = '''대한(大寒)보다 더 춥다는 소한(小寒)이자 금요일인 6일 오후부터 토요일인 7일까지 눈이 내리겠다. 강원권에서는 최대 10㎝, 수도권에는 최대 8㎝가 쌓이겠다. 서해안과 남해안에는 비가 함께 내릴 수 있다. 눈은 7일 낮 대부분 그칠 전망이다.'''
```

</div>

<div class="in_prompt">
In&nbsp;[26]:
</div>

<div class="input_area" markdown="1">

```python
new_text = text.replace(' ', '')
new_text
```

</div>

<div class="output_prompt">
Out&nbsp;[26]:
</div>




{:.output_data_text}

```
'대한(大寒)보다더춥다는소한(小寒)이자금요일인6일오후부터토요일인7일까지눈이내리겠다.강원권에서는최대10㎝,수도권에는최대8㎝가쌓이겠다.서해안과남해안에는비가함께내릴수있다.눈은7일낮대부분그칠전망이다.'
```



<div class="in_prompt">
In&nbsp;[27]:
</div>

<div class="input_area" markdown="1">

```python
s = Spacing()
recon_text = s(new_text)
print(text)
print(recon_text)
```

</div>

<div class="output_prompt">
Out&nbsp;[27]:
</div>

{:.output_stream}

```
1/1 [==============================] - 1s 821ms/step
대한(大寒)보다 더 춥다는 소한(小寒)이자 금요일인 6일 오후부터 토요일인 7일까지 눈이 내리겠다. 강원권에서는 최대 10㎝, 수도권에는 최대 8㎝가 쌓이겠다. 서해안과 남해안에는 비가 함께 내릴 수 있다. 눈은 7일 낮 대부분 그칠 전망이다.
대한(大寒)보다 더 춥다는 소한(小寒)이 자금요일인 6일 오후부터 토요일인 7일까지 눈이 내리겠다. 강원권에서는 최대 10㎝, 수도권에는 최대 8㎝가 쌓이겠다. 서해안과 남해안에는 비가 함께 내릴 수 있다. 눈은 7일 낮 대부분 그칠 전망이다.

```

<div class="in_prompt">
In&nbsp;[28]:
</div>

<div class="input_area" markdown="1">

```python
import tensorflow as tf
tf.__version__
```

</div>

<div class="output_prompt">
Out&nbsp;[28]:
</div>




{:.output_data_text}

```
'2.9.3'
```



### 맞춤법 검사

<div class="in_prompt">
In&nbsp;[29]:
</div>

<div class="input_area" markdown="1">

```python
# !pip install git+https://github.com/ssut/py-hanspell.git
# 또는 !pip install py-hanspell
```

</div>

<div class="in_prompt">
In&nbsp;[30]:
</div>

<div class="input_area" markdown="1">

```python
from hanspell import spell_checker
```

</div>

<div class="in_prompt">
In&nbsp;[31]:
</div>

<div class="input_area" markdown="1">

```python
text_kr = "마춤법 틀리면 외 않되? 내맘대로 쓰면돼지! 이따가 시간되?"
text_ok = spell_checker.check(text_kr)
text_ok.checked
```

</div>

<div class="output_prompt">
Out&nbsp;[31]:
</div>




{:.output_data_text}

```
'맞춤법 틀리면 왜 안돼? 내 맘대로 쓰면 되지! 이따가 시간 돼?'
```



### 이모티콘 축약
반복문자 정제

<div class="in_prompt">
In&nbsp;[32]:
</div>

<div class="input_area" markdown="1">

```python
# !pip install soynlp
```

</div>

<div class="in_prompt">
In&nbsp;[33]:
</div>

<div class="input_area" markdown="1">

```python
from soynlp.normalizer import *
```

</div>

<div class="in_prompt">
In&nbsp;[34]:
</div>

<div class="input_area" markdown="1">

```python
text = '앜ㅋㅋㅋㅋㅋㅋㅋㅋ이김밥존맛탱ㅋㅋㅋㅋ쿠쿠쿠ㅜㅜㅋㅋ쿠쿠루삥뽕'
emoticon_normalize(text)
```

</div>

<div class="output_prompt">
Out&nbsp;[34]:
</div>




{:.output_data_text}

```
'아ㅋㅋ김밥존맛ㅋㅋㅜ쿠쿠ㅜㅜㅋㅋㅋㅜ쿠루삥뽕'
```



<div class="in_prompt">
In&nbsp;[35]:
</div>

<div class="input_area" markdown="1">

```python
emoticon_normalize(text, num_repeats=3)
```

</div>

<div class="output_prompt">
Out&nbsp;[35]:
</div>




{:.output_data_text}

```
'아ㅋㅋㅋ김밥존맛ㅋㅋㅋㅜ쿠쿠ㅜㅜㅋㅋㅋㅜ쿠루삥뽕'
```



## 영어 불용어(Stopwords)

<div class="in_prompt">
In&nbsp;[36]:
</div>

<div class="input_area" markdown="1">

```python
import nltk
# nltk.download("stopwords")
# nltk.download("punkt")
```

</div>

<div class="in_prompt">
In&nbsp;[37]:
</div>

<div class="input_area" markdown="1">

```python
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from konlpy.tag import Okt
```

</div>

<div class="in_prompt">
In&nbsp;[38]:
</div>

<div class="input_area" markdown="1">

```python
# 영어에서 불용어 처리되는 단어들
stop_words_list = stopwords.words("english")
len(stop_words_list)
```

</div>

<div class="output_prompt">
Out&nbsp;[38]:
</div>




{:.output_data_text}

```
179
```



<div class="in_prompt">
In&nbsp;[39]:
</div>

<div class="input_area" markdown="1">

```python
stop_words_list[:10]
```

</div>

<div class="output_prompt">
Out&nbsp;[39]:
</div>




{:.output_data_text}

```
['i', 'me', 'my', 'myself', 'we', 'our', 'ours', 'ourselves', 'you', "you're"]
```



<div class="in_prompt">
In&nbsp;[40]:
</div>

<div class="input_area" markdown="1">

```python
test = "Tottenham head coach Antonio Conte says he is happy that Harry Kane & Son Heung-min both scored in his side's 4-0 victory over Crystal Palace in the Premier League."
test = test.lower()
token_1 = word_tokenize(test)
print(token_1)
```

</div>

<div class="output_prompt">
Out&nbsp;[40]:
</div>

{:.output_stream}

```
['tottenham', 'head', 'coach', 'antonio', 'conte', 'says', 'he', 'is', 'happy', 'that', 'harry', 'kane', '&', 'son', 'heung-min', 'both', 'scored', 'in', 'his', 'side', "'s", '4-0', 'victory', 'over', 'crystal', 'palace', 'in', 'the', 'premier', 'league', '.']

```

<div class="in_prompt">
In&nbsp;[41]:
</div>

<div class="input_area" markdown="1">

```python
# 불용어 제거
token_2 = []
for word in token_1:
    if word not in stop_words_list:
        token_2.append(word)
print(token_2)
```

</div>

<div class="output_prompt">
Out&nbsp;[41]:
</div>

{:.output_stream}

```
['tottenham', 'head', 'coach', 'antonio', 'conte', 'says', 'happy', 'harry', 'kane', '&', 'son', 'heung-min', 'scored', 'side', "'s", '4-0', 'victory', 'crystal', 'palace', 'premier', 'league', '.']

```

<div class="in_prompt">
In&nbsp;[42]:
</div>

<div class="input_area" markdown="1">

```python
my_stop_words = ['antonio', 'conte']
```

</div>

<div class="in_prompt">
In&nbsp;[43]:
</div>

<div class="input_area" markdown="1">

```python
# 커스텀 불용어 제거
token_3 = []
for word in token_2:
    if word not in my_stop_words:
        token_3.append(word)
print(token_3)
```

</div>

<div class="output_prompt">
Out&nbsp;[43]:
</div>

{:.output_stream}

```
['tottenham', 'head', 'coach', 'says', 'happy', 'harry', 'kane', '&', 'son', 'heung-min', 'scored', 'side', "'s", '4-0', 'victory', 'crystal', 'palace', 'premier', 'league', '.']

```

* 욕설 불용어 처리

<div class="in_prompt">
In&nbsp;[44]:
</div>

<div class="input_area" markdown="1">

```python
test = "Drop your weapon. Fuck you ass hole. go the hell mother fucker"
test = test.lower()
token_1 = word_tokenize(test)
print(token_1)

token_2 = []
for word in token_1:
    if word not in stop_words_list:
        token_2.append(word)
print(token_2)

my_stop_words = ['fuck', 'ass', 'hole', 'fucker']

token_3 = []
for word in token_2:
    if word not in my_stop_words:
        token_3.append(word)
print(token_3)
```

</div>

<div class="output_prompt">
Out&nbsp;[44]:
</div>

{:.output_stream}

```
['drop', 'your', 'weapon', '.', 'fuck', 'you', 'ass', 'hole', '.', 'go', 'the', 'hell', 'mother', 'fucker']
['drop', 'weapon', '.', 'fuck', 'ass', 'hole', '.', 'go', 'hell', 'mother', 'fucker']
['drop', 'weapon', '.', '.', 'go', 'hell', 'mother']

```

## 한국어 불용어

<div class="in_prompt">
In&nbsp;[45]:
</div>

<div class="input_area" markdown="1">

```python
okt = Okt()
text = '이 따위 물건을 팔고도 돈을 처먹냐 그냥 줘도 아깝다. 하자 있는 물건을 어떻게 쓰냐. 병신아'
word_token = okt.morphs(text)
print(word_token)
```

</div>

<div class="output_prompt">
Out&nbsp;[45]:
</div>

{:.output_stream}

```
['이', '따위', '물건', '을', '팔고', '도', '돈', '을', '처', '먹냐', '그냥', '줘도', '아깝다', '.', '하자', '있는', '물건', '을', '어떻게', '쓰냐', '.', '병신', '아']

```

<div class="in_prompt">
In&nbsp;[46]:
</div>

<div class="input_area" markdown="1">

```python
len('이'), len('물건')
```

</div>

<div class="output_prompt">
Out&nbsp;[46]:
</div>




{:.output_data_text}

```
(1, 2)
```



<div class="in_prompt">
In&nbsp;[47]:
</div>

<div class="input_area" markdown="1">

```python
token_ko_2 = []
for word in word_token:
    if len(word) > 1:
        token_ko_2.append(word)
print(token_ko_2)
```

</div>

<div class="output_prompt">
Out&nbsp;[47]:
</div>

{:.output_stream}

```
['따위', '물건', '팔고', '먹냐', '그냥', '줘도', '아깝다', '하자', '있는', '물건', '어떻게', '쓰냐', '병신']

```

<div class="in_prompt">
In&nbsp;[48]:
</div>

<div class="input_area" markdown="1">

```python
word_token_nouns = okt.nouns(text)
print(word_token_nouns)
```

</div>

<div class="output_prompt">
Out&nbsp;[48]:
</div>

{:.output_stream}

```
['이', '따위', '물건', '팔고', '돈', '처', '그냥', '하자', '물건', '병신']

```

<div class="in_prompt">
In&nbsp;[49]:
</div>

<div class="input_area" markdown="1">

```python
my_stop_words_ko = ['병신']
```

</div>

<div class="in_prompt">
In&nbsp;[50]:
</div>

<div class="input_area" markdown="1">

```python
token_ko_3 = []
for word in token_ko_2:
    if word not in my_stop_words_ko:
        token_ko_3.append(word)
print(token_ko_3)
```

</div>

<div class="output_prompt">
Out&nbsp;[50]:
</div>

{:.output_stream}

```
['따위', '물건', '팔고', '먹냐', '그냥', '줘도', '아깝다', '하자', '있는', '물건', '어떻게', '쓰냐']

```

<div class="in_prompt">
In&nbsp;[51]:
</div>

<div class="input_area" markdown="1">

```python
word_token_nouns_2 = []
for word in word_token_nouns:
    if word not in my_stop_words_ko:
        word_token_nouns_2.append(word)
print(word_token_nouns_2)
```

</div>

<div class="output_prompt">
Out&nbsp;[51]:
</div>

{:.output_stream}

```
['이', '따위', '물건', '팔고', '돈', '처', '그냥', '하자', '물건']

```

# 정수 인코딩

텍스트를 숫자로 바꾸는 기법

<div class="in_prompt">
In&nbsp;[52]:
</div>

<div class="input_area" markdown="1">

```python
text = '''
오늘 네가
보고싶다
널 다시 품에
안아보고 싶다
오늘 네가 난
생각난다
너랑 같이 산책하던
그곳에 서있다
너의 체온이
기억난다
따뜻하게 내 쉬던
숨소리 들려
오늘 네가
온 것 같아
우리 같이 잠들던
벤치에 기대니
시간이 흘러흘러
다시 만날 순 없지
그래도 보고싶다 널
그리운 내 사랑아
오늘 네가 정말
보고싶다
너를 다시 내 품에
안아보고 싶다
오늘 네가 난
생각난다
너를 쓰다듬던 손이
너를 기억한다
니가 보고싶다
'''
```

</div>

<div class="in_prompt">
In&nbsp;[53]:
</div>

<div class="input_area" markdown="1">

```python
okt = Okt()
text = text.replace("\n", " ")
text1 = okt.morphs(text)

text2 = []
for word in text1:
    if 4 > len(word) > 1:
        text2.append(word)
print(text2)
```

</div>

<div class="output_prompt">
Out&nbsp;[53]:
</div>

{:.output_stream}

```
['오늘', '다시', '안아', '보고', '싶다', '오늘', '같이', '산책', '하던', '서있다', '체온', '쉬던', '숨소리', '들려', '오늘', '같아', '우리', '같이', '잠들던', '벤치', '기대니', '시간', '흘러', '흘러', '다시', '만날', '없지', '그래도', '그리운', '사랑', '오늘', '정말', '다시', '안아', '보고', '싶다', '오늘', '기억']

```

<div class="in_prompt">
In&nbsp;[54]:
</div>

<div class="input_area" markdown="1">

```python
# 총 단어 갯수, 중복제거 단어 갯수
len(text2), len(set(text2))
```

</div>

<div class="output_prompt">
Out&nbsp;[54]:
</div>




{:.output_data_text}

```
(38, 27)
```



<div class="in_prompt">
In&nbsp;[55]:
</div>

<div class="input_area" markdown="1">

```python
vocab = {}
for word in text2:
    if word not in vocab:
        vocab[word] = 1
    else:
        vocab[word] += 1
print(vocab)
```

</div>

<div class="output_prompt">
Out&nbsp;[55]:
</div>

{:.output_stream}

```
{'오늘': 5, '다시': 3, '안아': 2, '보고': 2, '싶다': 2, '같이': 2, '산책': 1, '하던': 1, '서있다': 1, '체온': 1, '쉬던': 1, '숨소리': 1, '들려': 1, '같아': 1, '우리': 1, '잠들던': 1, '벤치': 1, '기대니': 1, '시간': 1, '흘러': 2, '만날': 1, '없지': 1, '그래도': 1, '그리운': 1, '사랑': 1, '정말': 1, '기억': 1}

```

<div class="in_prompt">
In&nbsp;[56]:
</div>

<div class="input_area" markdown="1">

```python
vocab_sorted = sorted(vocab.items(), key=lambda x:x[1], reverse=True)
print(vocab_sorted)
```

</div>

<div class="output_prompt">
Out&nbsp;[56]:
</div>

{:.output_stream}

```
[('오늘', 5), ('다시', 3), ('안아', 2), ('보고', 2), ('싶다', 2), ('같이', 2), ('흘러', 2), ('산책', 1), ('하던', 1), ('서있다', 1), ('체온', 1), ('쉬던', 1), ('숨소리', 1), ('들려', 1), ('같아', 1), ('우리', 1), ('잠들던', 1), ('벤치', 1), ('기대니', 1), ('시간', 1), ('만날', 1), ('없지', 1), ('그래도', 1), ('그리운', 1), ('사랑', 1), ('정말', 1), ('기억', 1)]

```

## 말뭉치: Corpus

<div class="in_prompt">
In&nbsp;[57]:
</div>

<div class="input_area" markdown="1">

```python
# 말뭉치: 빈도 목록
# 2번이상 반복된 단어만 뽑아 반복된 횟수 순서대로 나열
word_to_index = {}
i = 0
for (word, freq) in vocab_sorted:
    if freq > 1:
        i += 1
        word_to_index[word] = i
word_to_index
```

</div>

<div class="output_prompt">
Out&nbsp;[57]:
</div>




{:.output_data_text}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5, '같이': 6, '흘러': 7}
```



i = 0은 padding에 사용된다.

말뭉치  
 ('오늘', 5),  
 ('다시', 3),  
 ('안아', 2),  
 ('보고', 2),  
 ('싶다', 2),  
 ('같이', 2),  
 ('흘러', 2),  

# Reference

* DACON [Lv1 전처리 8/14 python 파이썬 형태소 분석기 - (2)](https://dacon.io/competitions/open/235698/talkboard/404707?page=1&dtype=recent)
