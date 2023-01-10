---
layout: jupyter
title: 자연어 처리 - 추천 시스템
published: true
date: 2023-01-10
description: 추천 시스템
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: 추천 시스템
use_math: true
---
---
# 추천 시스템

## 코사인 유사도

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import pandas as pd
from sklearn.metrics.pairwise import cosine_similarity
from sklearn.feature_extraction.text import TfidfVectorizer
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
data = pd.read_csv('datasets/movies_metadata.csv', low_memory=False)
data.head(2)
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>adult</th>
      <th>belongs_to_collection</th>
      <th>budget</th>
      <th>genres</th>
      <th>homepage</th>
      <th>id</th>
      <th>imdb_id</th>
      <th>original_language</th>
      <th>original_title</th>
      <th>overview</th>
      <th>...</th>
      <th>release_date</th>
      <th>revenue</th>
      <th>runtime</th>
      <th>spoken_languages</th>
      <th>status</th>
      <th>tagline</th>
      <th>title</th>
      <th>video</th>
      <th>vote_average</th>
      <th>vote_count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>{'id': 10194, 'name': 'Toy Story Collection', ...</td>
      <td>30000000</td>
      <td>[{'id': 16, 'name': 'Animation'}, {'id': 35, '...</td>
      <td>http://toystory.disney.com/toy-story</td>
      <td>862</td>
      <td>tt0114709</td>
      <td>en</td>
      <td>Toy Story</td>
      <td>Led by Woody, Andy's toys live happily in his ...</td>
      <td>...</td>
      <td>1995-10-30</td>
      <td>373554033.0</td>
      <td>81.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}]</td>
      <td>Released</td>
      <td>NaN</td>
      <td>Toy Story</td>
      <td>False</td>
      <td>7.7</td>
      <td>5415.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>NaN</td>
      <td>65000000</td>
      <td>[{'id': 12, 'name': 'Adventure'}, {'id': 14, '...</td>
      <td>NaN</td>
      <td>8844</td>
      <td>tt0113497</td>
      <td>en</td>
      <td>Jumanji</td>
      <td>When siblings Judy and Peter discover an encha...</td>
      <td>...</td>
      <td>1995-12-15</td>
      <td>262797249.0</td>
      <td>104.0</td>
      <td>[{'iso_639_1': 'en', 'name': 'English'}, {'iso...</td>
      <td>Released</td>
      <td>Roll the dice and unleash the excitement!</td>
      <td>Jumanji</td>
      <td>False</td>
      <td>6.9</td>
      <td>2413.0</td>
    </tr>
  </tbody>
</table>
<p>2 rows × 24 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 45466 entries, 0 to 45465
Data columns (total 24 columns):
 #   Column                 Non-Null Count  Dtype  
---  ------                 --------------  -----  
 0   adult                  45466 non-null  object 
 1   belongs_to_collection  4494 non-null   object 
 2   budget                 45466 non-null  object 
 3   genres                 45466 non-null  object 
 4   homepage               7782 non-null   object 
 5   id                     45466 non-null  object 
 6   imdb_id                45449 non-null  object 
 7   original_language      45455 non-null  object 
 8   original_title         45466 non-null  object 
 9   overview               44512 non-null  object 
 10  popularity             45461 non-null  object 
 11  poster_path            45080 non-null  object 
 12  production_companies   45463 non-null  object 
 13  production_countries   45463 non-null  object 
 14  release_date           45379 non-null  object 
 15  revenue                45460 non-null  float64
 16  runtime                45203 non-null  float64
 17  spoken_languages       45460 non-null  object 
 18  status                 45379 non-null  object 
 19  tagline                20412 non-null  object 
 20  title                  45460 non-null  object 
 21  video                  45460 non-null  object 
 22  vote_average           45460 non-null  float64
 23  vote_count             45460 non-null  float64
dtypes: float64(4), object(20)
memory usage: 8.3+ MB

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
data[['title', 'overview']]
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>title</th>
      <th>overview</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Toy Story</td>
      <td>Led by Woody, Andy's toys live happily in his ...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Jumanji</td>
      <td>When siblings Judy and Peter discover an encha...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Grumpier Old Men</td>
      <td>A family wedding reignites the ancient feud be...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Waiting to Exhale</td>
      <td>Cheated on, mistreated and stepped on, the wom...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Father of the Bride Part II</td>
      <td>Just when George Banks has recovered from his ...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>45461</th>
      <td>Subdue</td>
      <td>Rising and falling between a man and woman.</td>
    </tr>
    <tr>
      <th>45462</th>
      <td>Century of Birthing</td>
      <td>An artist struggles to finish his work while a...</td>
    </tr>
    <tr>
      <th>45463</th>
      <td>Betrayal</td>
      <td>When one of her hits goes wrong, a professiona...</td>
    </tr>
    <tr>
      <th>45464</th>
      <td>Satan Triumphant</td>
      <td>In a small town live two brothers, one a minis...</td>
    </tr>
    <tr>
      <th>45465</th>
      <td>Queerama</td>
      <td>50 years after decriminalisation of homosexual...</td>
    </tr>
  </tbody>
</table>
<p>45466 rows × 2 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
data['overview'].isnull().sum()
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>




{:.output_data_text}

```
954
```



<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
data = data[['title', 'overview']]
```

</div>

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 45466 entries, 0 to 45465
Data columns (total 2 columns):
 #   Column    Non-Null Count  Dtype 
---  ------    --------------  ----- 
 0   title     45460 non-null  object
 1   overview  44512 non-null  object
dtypes: object(2)
memory usage: 710.5+ KB

```

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
data.dropna(inplace=True)
data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 44506 entries, 0 to 45465
Data columns (total 2 columns):
 #   Column    Non-Null Count  Dtype 
---  ------    --------------  ----- 
 0   title     44506 non-null  object
 1   overview  44506 non-null  object
dtypes: object(2)
memory usage: 1.0+ MB

```

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
ftidf_vec = TfidfVectorizer(stop_words='english')
tfidf_dtm = ftidf_vec.fit_transform(data['overview'])
tfidf_dtm # 44506개의 리뷰에 대해 75827 단어의 tfidf 희소 행렬
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>




{:.output_data_text}

```
<44506x75827 sparse matrix of type '<class 'numpy.float64'>'
	with 1210839 stored elements in Compressed Sparse Row format>
```



<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
cos_sim_res = cosine_similarity(tfidf_dtm, tfidf_dtm)
```

</div>

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
# 타이틀을 정수화(index)
title_to_index = dict(zip(data['title'], data.index))
# 'Toy Story': 0, 'Jumanji': 1, 'Grumpier Old Men': 2...
```

</div>

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
def get_recommendation(title, n):
    idx = title_to_index[title]
    sim_scores = list(enumerate(cos_sim_res[idx])) # 해당 영화와 나머지 영화들의 유사도를 idx와 함께 list로
    sim_scores = sorted(sim_scores, key=lambda x: x[1], reverse=True) # 유사도를 기준(key)으로 내림차순 정렬
    sim_scores_n = sim_scores[1:n+1] # 유사도가 동일(title==title)한 첫번째를 제외하고 n번째까지
    movie_idx = [movie_dict[0] for movie_dict in sim_scores_n]
    return data['title'].iloc[movie_idx]
```

</div>

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
get_recommendation('Toy Story', 5)
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
</div>




{:.output_data_text}

```
15348                    Toy Story 3
2997                     Toy Story 2
10301         The 40 Year Old Virgin
24523                      Small Fry
23843    Andy Hardy's Blonde Trouble
Name: title, dtype: object
```



<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
get_recommendation('Batman', 5)
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
</div>




{:.output_data_text}

```
8681                 Scars of Dracula
18562    Johnny Cash at Folsom Prison
6600        The Prince and the Pauper
5355            Nosferatu the Vampyre
37915                          Vaesen
Name: title, dtype: object
```



<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
get_recommendation('The Dark Knight Rises', 5)
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>




{:.output_data_text}

```
31143             Deadly Daycare
19286            The One Percent
44918                  Once More
45106    Nicostratos the Pelican
33008       White Cannibal Queen
Name: title, dtype: object
```



## word2vec, 임베딩
딥러닝과 잘 어울림  
sklearn의 TfidfVectorizer는 머신러닝과 잘 어울림

<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
import pandas as pd
```

</div>

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
train_data = pd.read_table('datasets/ratings.txt')
train_data.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[17]:
</div>




<div markdown="0">
<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>document</th>
      <th>label</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>8112052</td>
      <td>어릴때보고 지금다시봐도 재밌어요ㅋㅋ</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>8132799</td>
      <td>디자인을 배우는 학생으로, 외국디자이너와 그들이 일군 전통을 통해 발전해가는 문화산...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4655635</td>
      <td>폴리스스토리 시리즈는 1부터 뉴까지 버릴께 하나도 없음.. 최고.</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>9251303</td>
      <td>와.. 연기가 진짜 개쩔구나.. 지루할거라고 생각했는데 몰입해서 봤다.. 그래 이런...</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10067386</td>
      <td>안개 자욱한 밤하늘에 떠 있는 초승달 같은 영화.</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
train_data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[18]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 200000 entries, 0 to 199999
Data columns (total 3 columns):
 #   Column    Non-Null Count   Dtype 
---  ------    --------------   ----- 
 0   id        200000 non-null  int64 
 1   document  199992 non-null  object
 2   label     200000 non-null  int64 
dtypes: int64(2), object(1)
memory usage: 4.6+ MB

```

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
train_data.dropna(inplace=True)
train_data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
Int64Index: 199992 entries, 0 to 199999
Data columns (total 3 columns):
 #   Column    Non-Null Count   Dtype 
---  ------    --------------   ----- 
 0   id        199992 non-null  int64 
 1   document  199992 non-null  object
 2   label     199992 non-null  int64 
dtypes: int64(2), object(1)
memory usage: 6.1+ MB

```

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
# 한글 이외 제거
train_data['document'] = train_data['document'].str.replace('[^ㄱ-ㅎㅏ-ㅣ가-힣 ]', '') # ㄱ-ㅎ,ㅏ-ㅣ,가-힣,공백 제외
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
</div>

{:.output_stream}

```
C:\Users\user\AppData\Local\Temp\ipykernel_14604\1359601085.py:2: FutureWarning: The default value of regex will change from True to False in a future version.
  train_data['document'] = train_data['document'].str.replace('[^ㄱ-ㅎㅏ-ㅣ가-힣 ]', '') # ㄱ-ㅎ,ㅏ-ㅣ,가-힣,공백 제외

```

<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
# ㄱ-ㅣ는 가능하지만 ㄱ-힣은 가능하지 않다.
print(ord('ㄱ'), ord('ㅎ'))
print(ord('ㅏ'), ord('ㅣ'))
print(ord('가'), ord('힣'))
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>

{:.output_stream}

```
12593 12622
12623 12643
44032 55203

```

<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
from konlpy.tag import Okt
okt = Okt()
```

</div>

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
stopwords = ["의", "가", "이", "은", "는",
             "들", "좀", "잘", "강", "과",
             "도", "를", "으로", "자", "에",
             "와", "한", "하다"]
```

</div>

<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
from tqdm import tqdm
toked_data=[]
for sen in tqdm(train_data['document']):
    toked_sen = okt.morphs(sen, stem=True)
    toked_sen_wo_stop = [word for word in toked_sen if not word in stopwords]
    toked_data.append(toked_sen_wo_stop)
```

</div>

<div class="output_prompt">
Out&nbsp;[24]:
</div>

{:.output_stream}

```
100%|█████████████████████████████████████████████████████████████████████████| 199992/199992 [09:49<00:00, 339.43it/s]

```

<div class="in_prompt">
In&nbsp;[25]:
</div>

<div class="input_area" markdown="1">

```python
# 리뷰 최대 길이
max(len(review) for review in toked_data)
```

</div>

<div class="output_prompt">
Out&nbsp;[25]:
</div>




{:.output_data_text}

```
72
```



<div class="in_prompt">
In&nbsp;[26]:
</div>

<div class="input_area" markdown="1">

```python
# 리뷰 최소 길이
min(len(review) for review in toked_data)
```

</div>

<div class="output_prompt">
Out&nbsp;[26]:
</div>




{:.output_data_text}

```
0
```



<div class="in_prompt">
In&nbsp;[27]:
</div>

<div class="input_area" markdown="1">

```python
# 리뷰 평균 길이
sum(len(review) for review in toked_data)/len(toked_data)
```

</div>

<div class="output_prompt">
Out&nbsp;[27]:
</div>




{:.output_data_text}

```
10.719648785951438
```



<div class="in_prompt">
In&nbsp;[28]:
</div>

<div class="input_area" markdown="1">

```python
import matplotlib.pyplot as plt
# 히스토그램 그려보기
plt.hist([len(review) for review in toked_data], bins=20)
plt.show()
```

</div>

<div class="output_prompt">
Out&nbsp;[28]:
</div>


    
![img]({{ '/assets/images/2023/01/10/img1.png' | relative_url }})
    


<div class="in_prompt">
In&nbsp;[29]:
</div>

<div class="input_area" markdown="1">

```python
from gensim.models import Word2Vec
model = Word2Vec(sentences=toked_data, vector_size=100, window=5, min_count=5, workers=4, sg=0)
model.wv.vectors.shape # 임베딩 행렬(단어수, 표현력) 단어당 100차원의 특성을 저장
```

</div>

<div class="output_prompt">
Out&nbsp;[29]:
</div>




{:.output_data_text}

```
(16477, 100)
```



<div class="in_prompt">
In&nbsp;[30]:
</div>

<div class="input_area" markdown="1">

```python
model.wv.vectors
```

</div>

<div class="output_prompt">
Out&nbsp;[30]:
</div>




{:.output_data_text}

```
array([[-2.8263861e-03,  9.8568387e-02, -9.4999635e-01, ...,
        -8.6086142e-01,  1.3070868e-01,  8.3628201e-01],
       [-1.3327576e+00,  5.4933226e-01, -4.0492460e-01, ...,
        -9.6057999e-01, -1.1362101e+00,  6.1550405e-02],
       [-1.5236272e+00,  2.4189599e-01, -1.3285520e+00, ...,
        -8.9836246e-01,  2.8833647e+00,  1.5803616e-01],
       ...,
       [-3.3974819e-02, -1.4981081e-02,  5.9175860e-02, ...,
        -8.6746484e-02,  4.4356149e-02, -6.0275033e-02],
       [-2.4699664e-02,  3.8863931e-02, -7.7239783e-03, ...,
        -1.0443001e-01, -2.6425140e-02, -9.8955519e-03],
       [-1.2799753e-02,  2.7629824e-02, -1.4718298e-03, ...,
        -4.9694967e-02,  1.4989703e-02,  3.4072924e-02]], dtype=float32)
```



<div class="in_prompt">
In&nbsp;[31]:
</div>

<div class="input_area" markdown="1">

```python
model.wv.most_similar('송혜교')
```

</div>

<div class="output_prompt">
Out&nbsp;[31]:
</div>




{:.output_data_text}

```
[('이종석', 0.892828643321991),
 ('남상미', 0.8924018740653992),
 ('김소연', 0.8526328206062317),
 ('이민정', 0.8493728041648865),
 ('신현준', 0.8422029614448547),
 ('주지훈', 0.8388743996620178),
 ('손예진', 0.8374811410903931),
 ('송윤아', 0.8355734944343567),
 ('서우', 0.8328922390937805),
 ('김동완', 0.8306312561035156)]
```



### 사전 학습된 모델

<div class="in_prompt">
In&nbsp;[32]:
</div>

<div class="input_area" markdown="1">

```python
import gensim
pre_trained_word2vec = gensim.models.KeyedVectors.load_word2vec_format('datasets/GoogleNews-vectors-negative300.bin', binary=True)
```

</div>

<div class="in_prompt">
In&nbsp;[33]:
</div>

<div class="input_area" markdown="1">

```python
pre_trained_word2vec.vectors.shape # 단어:3000000, 차원:300
```

</div>

<div class="output_prompt">
Out&nbsp;[33]:
</div>




{:.output_data_text}

```
(3000000, 300)
```



<div class="in_prompt">
In&nbsp;[34]:
</div>

<div class="input_area" markdown="1">

```python
pre_trained_word2vec.similarity('this', 'is')
```

</div>

<div class="output_prompt">
Out&nbsp;[34]:
</div>




{:.output_data_text}

```
0.40797037
```



<div class="in_prompt">
In&nbsp;[35]:
</div>

<div class="input_area" markdown="1">

```python
pre_trained_word2vec.similarity('food', 'book')
```

</div>

<div class="output_prompt">
Out&nbsp;[35]:
</div>




{:.output_data_text}

```
0.10171259
```



<div class="in_prompt">
In&nbsp;[36]:
</div>

<div class="input_area" markdown="1">

```python
pre_trained_word2vec.similarity('chicken', 'pizza')
```

</div>

<div class="output_prompt">
Out&nbsp;[36]:
</div>




{:.output_data_text}

```
0.37984928
```


