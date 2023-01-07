---
layout: jupyter
title: 자연어 처리 - 정수 인코딩
published: true
date: 2023-01-06
description: 정수 인코딩
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: 정수 인코딩
---
---
# 정수 인코딩

Neural Network에 사용하기 위한 데이터 전처리  
텍스트를 숫자로 바꾸는 기법

## dictionary 이용

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
from konlpy.tag import Okt
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
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
In&nbsp;[3]:
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
Out&nbsp;[3]:
</div>

{:.output_stream}

```
['오늘', '다시', '안아', '보고', '싶다', '오늘', '같이', '산책', '하던', '서있다', '체온', '쉬던', '숨소리', '들려', '오늘', '같아', '우리', '같이', '잠들던', '벤치', '기대니', '시간', '흘러', '흘러', '다시', '만날', '없지', '그래도', '그리운', '사랑', '오늘', '정말', '다시', '안아', '보고', '싶다', '오늘', '기억']

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
# 총 단어 갯수, 중복제거 단어 갯수
len(text2), len(set(text2))
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>




{:.output_data_text}

```
(38, 27)
```



<div class="in_prompt">
In&nbsp;[5]:
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
Out&nbsp;[5]:
</div>

{:.output_stream}

```
{'오늘': 5, '다시': 3, '안아': 2, '보고': 2, '싶다': 2, '같이': 2, '산책': 1, '하던': 1, '서있다': 1, '체온': 1, '쉬던': 1, '숨소리': 1, '들려': 1, '같아': 1, '우리': 1, '잠들던': 1, '벤치': 1, '기대니': 1, '시간': 1, '흘러': 2, '만날': 1, '없지': 1, '그래도': 1, '그리운': 1, '사랑': 1, '정말': 1, '기억': 1}

```

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
vocab_sorted = sorted(vocab.items(), key=lambda x:x[1], reverse=True)
print(vocab_sorted)
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>

{:.output_stream}

```
[('오늘', 5), ('다시', 3), ('안아', 2), ('보고', 2), ('싶다', 2), ('같이', 2), ('흘러', 2), ('산책', 1), ('하던', 1), ('서있다', 1), ('체온', 1), ('쉬던', 1), ('숨소리', 1), ('들려', 1), ('같아', 1), ('우리', 1), ('잠들던', 1), ('벤치', 1), ('기대니', 1), ('시간', 1), ('만날', 1), ('없지', 1), ('그래도', 1), ('그리운', 1), ('사랑', 1), ('정말', 1), ('기억', 1)]

```

* 말뭉치: Corpus

<div class="in_prompt">
In&nbsp;[7]:
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
Out&nbsp;[7]:
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

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
# vocab_size를 벗어나는 단어 제거
vocab_size = 5

word_freq_cut = [word for word, index in word_to_index.items() if index >= vocab_size + 1]
word_freq_cut
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>




{:.output_data_text}

```
['같이', '흘러']
```



<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
print(word_to_index)
for w in word_freq_cut:
    del word_to_index[w]
print(word_to_index)
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>

{:.output_stream}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5, '같이': 6, '흘러': 7}
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5}

```

단어를 숫자로 치환할 때 word_to_index에 없는 단어를 치환하기 위해 OOV(Out-Of-Vocabulary;단어 집합에 없는 단어)를 지정한다.

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
word_to_index["OOV"] = len(word_to_index) + 1
print(word_to_index)
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>

{:.output_stream}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5, 'OOV': 6}

```

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
# 전처리된 문자열(자연어)을 숫자(정수)로 치환
encoded_sentences = []
for word in text2:
    try:
        encoded_sentences.append(word_to_index[word])
    except:
        encoded_sentences.append(word_to_index['OOV'])
print(encoded_sentences)
print(text2)
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
</div>

{:.output_stream}

```
[1, 2, 3, 4, 5, 1, 6, 6, 6, 6, 6, 6, 6, 6, 1, 6, 6, 6, 6, 6, 6, 6, 6, 6, 2, 6, 6, 6, 6, 6, 1, 6, 2, 3, 4, 5, 1, 6]
['오늘', '다시', '안아', '보고', '싶다', '오늘', '같이', '산책', '하던', '서있다', '체온', '쉬던', '숨소리', '들려', '오늘', '같아', '우리', '같이', '잠들던', '벤치', '기대니', '시간', '흘러', '흘러', '다시', '만날', '없지', '그래도', '그리운', '사랑', '오늘', '정말', '다시', '안아', '보고', '싶다', '오늘', '기억']

```

### 문장단위 처리

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
text = '''
오늘 네가 보고싶다
널 다시 품에 안아보고 싶다
오늘 네가 난 생각난다
너랑 같이 산책하던 그곳에 서있다
너의 체온이 기억난다
따뜻하게 내 쉬던 숨소리 들려
오늘 네가 온 것 같아
우리 같이 잠들던 벤치에 기대니
시간이 흘러흘러 다시 만날 순 없지
그래도 보고싶다 널 그리운 내 사랑아
오늘 네가 정말 보고싶다
너를 다시 내 품에 안아보고 싶다
오늘 네가 난 생각난다
너를 쓰다듬던 손이 너를 기억한다
니가 보고싶다
'''
```

</div>

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
import kss
```

</div>

<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
text = text.replace('\n', ' ')
text = kss.split_sentences(text)

okt = Okt()

vocab = {}
text2 = []
for sentence in text:
    token = okt.morphs(sentence)
    rst = []
    for word in token:
        if 4 > len(word) > 1:
            rst.append(word)
            if word not in vocab:
                vocab[word] = 1
            else:
                vocab[word] += 1
    text2.append(rst)
print(text2)
print(vocab)
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
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

{:.output_stream}

```
[['오늘'], ['다시', '안아', '보고', '싶다'], ['오늘', '같이', '산책', '하던', '서있다'], ['체온'], ['쉬던', '숨소리', '들려', '오늘', '같아', '우리', '같이', '잠들던', '벤치', '기대니', '시간', '흘러', '흘러', '다시', '만날', '없지', '그래도'], ['그리운', '사랑', '오늘', '정말'], ['다시', '안아', '보고', '싶다'], ['오늘', '기억'], []]
{'오늘': 5, '다시': 3, '안아': 2, '보고': 2, '싶다': 2, '같이': 2, '산책': 1, '하던': 1, '서있다': 1, '체온': 1, '쉬던': 1, '숨소리': 1, '들려': 1, '같아': 1, '우리': 1, '잠들던': 1, '벤치': 1, '기대니': 1, '시간': 1, '흘러': 2, '만날': 1, '없지': 1, '그래도': 1, '그리운': 1, '사랑': 1, '정말': 1, '기억': 1}

```

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
vocab_sorted = sorted(vocab.items(), key=lambda x:x[1], reverse=True)

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
Out&nbsp;[15]:
</div>




{:.output_data_text}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5, '같이': 6, '흘러': 7}
```



<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
# vocab_size를 벗어나는 단어 제거
vocab_size = 5

word_freq_cut = [word for word, index in word_to_index.items() if index >= vocab_size + 1]
word_freq_cut

print(word_to_index)
for w in word_freq_cut:
    del word_to_index[w]
print(word_to_index)
```

</div>

<div class="output_prompt">
Out&nbsp;[16]:
</div>

{:.output_stream}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5, '같이': 6, '흘러': 7}
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5}

```

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
word_to_index["OOV"] = len(word_to_index) + 1
print(word_to_index)
```

</div>

<div class="output_prompt">
Out&nbsp;[17]:
</div>

{:.output_stream}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5, 'OOV': 6}

```

<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
# 전처리된 문자열(자연어)을 숫자(정수)로 치환
encoded_sentences = []
for sentence in text2:
    encoded_sentence = []
    for word in sentence:
        try:
            encoded_sentence.append(word_to_index[word])
        except:
            encoded_sentence.append(word_to_index['OOV'])
    encoded_sentences.append(encoded_sentence)
print(encoded_sentences)
print(text2)
```

</div>

<div class="output_prompt">
Out&nbsp;[18]:
</div>

{:.output_stream}

```
[[1], [2, 3, 4, 5], [1, 6, 6, 6, 6], [6], [6, 6, 6, 1, 6, 6, 6, 6, 6, 6, 6, 6, 6, 2, 6, 6, 6], [6, 6, 1, 6], [2, 3, 4, 5], [1, 6], []]
[['오늘'], ['다시', '안아', '보고', '싶다'], ['오늘', '같이', '산책', '하던', '서있다'], ['체온'], ['쉬던', '숨소리', '들려', '오늘', '같아', '우리', '같이', '잠들던', '벤치', '기대니', '시간', '흘러', '흘러', '다시', '만날', '없지', '그래도'], ['그리운', '사랑', '오늘', '정말'], ['다시', '안아', '보고', '싶다'], ['오늘', '기억'], []]

```

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
encoded_sentences
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>




{:.output_data_text}

```
[[1],
 [2, 3, 4, 5],
 [1, 6, 6, 6, 6],
 [6],
 [6, 6, 6, 1, 6, 6, 6, 6, 6, 6, 6, 6, 6, 2, 6, 6, 6],
 [6, 6, 1, 6],
 [2, 3, 4, 5],
 [1, 6],
 []]
```



## keras의 텍스트 전처리

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
from keras.preprocessing.text import Tokenizer
```

</div>

<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
text2
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>




{:.output_data_text}

```
[['오늘'],
 ['다시', '안아', '보고', '싶다'],
 ['오늘', '같이', '산책', '하던', '서있다'],
 ['체온'],
 ['쉬던',
  '숨소리',
  '들려',
  '오늘',
  '같아',
  '우리',
  '같이',
  '잠들던',
  '벤치',
  '기대니',
  '시간',
  '흘러',
  '흘러',
  '다시',
  '만날',
  '없지',
  '그래도'],
 ['그리운', '사랑', '오늘', '정말'],
 ['다시', '안아', '보고', '싶다'],
 ['오늘', '기억'],
 []]
```



<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
tokenizer = Tokenizer()
```

</div>

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
tokenizer.fit_on_texts(text2)
```

</div>

<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
print(tokenizer.word_index)
```

</div>

<div class="output_prompt">
Out&nbsp;[24]:
</div>

{:.output_stream}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5, '같이': 6, '흘러': 7, '산책': 8, '하던': 9, '서있다': 10, '체온': 11, '쉬던': 12, '숨소리': 13, '들려': 14, '같아': 15, '우리': 16, '잠들던': 17, '벤치': 18, '기대니': 19, '시간': 20, '만날': 21, '없지': 22, '그래도': 23, '그리운': 24, '사랑': 25, '정말': 26, '기억': 27}

```

<div class="in_prompt">
In&nbsp;[25]:
</div>

<div class="input_area" markdown="1">

```python
print(tokenizer.word_counts)
```

</div>

<div class="output_prompt">
Out&nbsp;[25]:
</div>

{:.output_stream}

```
OrderedDict([('오늘', 5), ('다시', 3), ('안아', 2), ('보고', 2), ('싶다', 2), ('같이', 2), ('산책', 1), ('하던', 1), ('서있다', 1), ('체온', 1), ('쉬던', 1), ('숨소리', 1), ('들려', 1), ('같아', 1), ('우리', 1), ('잠들던', 1), ('벤치', 1), ('기대니', 1), ('시간', 1), ('흘러', 2), ('만날', 1), ('없지', 1), ('그래도', 1), ('그리운', 1), ('사랑', 1), ('정말', 1), ('기억', 1)])

```

<div class="in_prompt">
In&nbsp;[26]:
</div>

<div class="input_area" markdown="1">

```python
tokenizer.texts_to_sequences(text2)
```

</div>

<div class="output_prompt">
Out&nbsp;[26]:
</div>




{:.output_data_text}

```
[[1],
 [2, 3, 4, 5],
 [1, 6, 8, 9, 10],
 [11],
 [12, 13, 14, 1, 15, 16, 6, 17, 18, 19, 20, 7, 7, 2, 21, 22, 23],
 [24, 25, 1, 26],
 [2, 3, 4, 5],
 [1, 27],
 []]
```



### OOV 적용

<div class="in_prompt">
In&nbsp;[27]:
</div>

<div class="input_area" markdown="1">

```python
vocab_size = 5
```

</div>

<div class="in_prompt">
In&nbsp;[28]:
</div>

<div class="input_area" markdown="1">

```python
# num_words: 파라미터 -1 만큼의 단어로 단어집합의 크기를 지정
tok_oov = Tokenizer(num_words = vocab_size + 2, oov_token='OOV') # 0과 OOV
tok_oov.fit_on_texts(text2)
```

</div>

<div class="in_prompt">
In&nbsp;[29]:
</div>

<div class="input_area" markdown="1">

```python
print(tok_oov.word_index)
print(tok_oov.word_counts)
tok_oov.texts_to_sequences(text2)
```

</div>

<div class="output_prompt">
Out&nbsp;[29]:
</div>

{:.output_stream}

```
{'OOV': 1, '오늘': 2, '다시': 3, '안아': 4, '보고': 5, '싶다': 6, '같이': 7, '흘러': 8, '산책': 9, '하던': 10, '서있다': 11, '체온': 12, '쉬던': 13, '숨소리': 14, '들려': 15, '같아': 16, '우리': 17, '잠들던': 18, '벤치': 19, '기대니': 20, '시간': 21, '만날': 22, '없지': 23, '그래도': 24, '그리운': 25, '사랑': 26, '정말': 27, '기억': 28}
OrderedDict([('오늘', 5), ('다시', 3), ('안아', 2), ('보고', 2), ('싶다', 2), ('같이', 2), ('산책', 1), ('하던', 1), ('서있다', 1), ('체온', 1), ('쉬던', 1), ('숨소리', 1), ('들려', 1), ('같아', 1), ('우리', 1), ('잠들던', 1), ('벤치', 1), ('기대니', 1), ('시간', 1), ('흘러', 2), ('만날', 1), ('없지', 1), ('그래도', 1), ('그리운', 1), ('사랑', 1), ('정말', 1), ('기억', 1)])

```




{:.output_data_text}

```
[[2],
 [3, 4, 5, 6],
 [2, 1, 1, 1, 1],
 [1],
 [1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1],
 [1, 1, 2, 1],
 [3, 4, 5, 6],
 [2, 1],
 []]
```



<div class="in_prompt">
In&nbsp;[30]:
</div>

<div class="input_area" markdown="1">

```python
encoded_text = tok_oov.texts_to_sequences(text2)
```

</div>

### OOV 적용 - word_index,count에도 적용

<div class="in_prompt">
In&nbsp;[31]:
</div>

<div class="input_area" markdown="1">

```python
tok = Tokenizer(num_words = vocab_size + 1) # oov_token을 적용하지 않으면 단어집합에 없는 단어는 삭제됨
```

</div>

<div class="in_prompt">
In&nbsp;[32]:
</div>

<div class="input_area" markdown="1">

```python
tok.fit_on_texts(text2)
```

</div>

<div class="in_prompt">
In&nbsp;[33]:
</div>

<div class="input_area" markdown="1">

```python
word_freq_cut = [word for word, index in tok.word_index.items() if index >= vocab_size + 1]
print(word_freq_cut)
```

</div>

<div class="output_prompt">
Out&nbsp;[33]:
</div>

{:.output_stream}

```
['같이', '흘러', '산책', '하던', '서있다', '체온', '쉬던', '숨소리', '들려', '같아', '우리', '잠들던', '벤치', '기대니', '시간', '만날', '없지', '그래도', '그리운', '사랑', '정말', '기억']

```

<div class="in_prompt">
In&nbsp;[34]:
</div>

<div class="input_area" markdown="1">

```python
for w in word_freq_cut:
    del tok.word_index[w]
    del tok.word_counts[w]
print(tok.word_index)
print(tok.word_counts)
tok.texts_to_sequences(text2)
```

</div>

<div class="output_prompt">
Out&nbsp;[34]:
</div>

{:.output_stream}

```
{'오늘': 1, '다시': 2, '안아': 3, '보고': 4, '싶다': 5}
OrderedDict([('오늘', 5), ('다시', 3), ('안아', 2), ('보고', 2), ('싶다', 2)])

```




{:.output_data_text}

```
[[1], [2, 3, 4, 5], [1], [], [1, 2], [1], [2, 3, 4, 5], [1], []]
```



# 패딩(Padding)

<div class="in_prompt">
In&nbsp;[35]:
</div>

<div class="input_area" markdown="1">

```python
max(len(item) for item in encoded_text)
```

</div>

<div class="output_prompt">
Out&nbsp;[35]:
</div>




{:.output_data_text}

```
17
```



<div class="in_prompt">
In&nbsp;[36]:
</div>

<div class="input_area" markdown="1">

```python
MAX_LEN = 17
```

</div>

<div class="in_prompt">
In&nbsp;[37]:
</div>

<div class="input_area" markdown="1">

```python
for sentence in encoded_text:
    while len(sentence) < MAX_LEN:
        sentence.append(0)
encoded_text
```

</div>

<div class="output_prompt">
Out&nbsp;[37]:
</div>




{:.output_data_text}

```
[[2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [3, 4, 5, 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [2, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1],
 [1, 1, 2, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [3, 4, 5, 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [2, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
 [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]]
```



### keras의 패딩 처리

<div class="in_prompt">
In&nbsp;[38]:
</div>

<div class="input_area" markdown="1">

```python
from tensorflow.keras.preprocessing.sequence import pad_sequences
```

</div>

<div class="in_prompt">
In&nbsp;[39]:
</div>

<div class="input_area" markdown="1">

```python
tokenizer = Tokenizer(num_words = vocab_size + 2, oov_token='OOV')
tokenizer.fit_on_texts(text2)
enc = tokenizer.texts_to_sequences(text2)
enc
```

</div>

<div class="output_prompt">
Out&nbsp;[39]:
</div>




{:.output_data_text}

```
[[2],
 [3, 4, 5, 6],
 [2, 1, 1, 1, 1],
 [1],
 [1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1],
 [1, 1, 2, 1],
 [3, 4, 5, 6],
 [2, 1],
 []]
```



<div class="in_prompt">
In&nbsp;[40]:
</div>

<div class="input_area" markdown="1">

```python
padded = pad_sequences(enc)
padded
```

</div>

<div class="output_prompt">
Out&nbsp;[40]:
</div>




{:.output_data_text}

```
array([[0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 3, 4, 5, 6],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1, 1, 1, 1],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1],
       [1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, 2, 1],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 3, 4, 5, 6],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 2, 1],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]])
```



Sequence정보는 최근(순서상 마지막)정보가 중요하므로 0을 앞으로 채워 데이터는 뒤쪽으로 밀어둬야한다!

<div class="in_prompt">
In&nbsp;[41]:
</div>

<div class="input_area" markdown="1">

```python
# 뒤로 0을 채우는 경우도 가능하다
padded2 = pad_sequences(enc, padding='post')
padded2
```

</div>

<div class="output_prompt">
Out&nbsp;[41]:
</div>




{:.output_data_text}

```
array([[2, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
       [3, 4, 5, 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
       [2, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
       [1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
       [1, 1, 1, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1],
       [1, 1, 2, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
       [3, 4, 5, 6, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
       [2, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
       [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]])
```


