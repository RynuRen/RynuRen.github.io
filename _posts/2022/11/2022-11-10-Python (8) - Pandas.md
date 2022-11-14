---
layout: jupyter
title: Python (8) - Pandas
published: true
date: 2022-11-10
description: Pandas의 기초
comments: true
categories:
 - python
tags:
 - python
 - numpy
 - pandas
toc: true
toc_sticky: true
toc_label: Pandas의 기초
---
---
# Pandas란?

- pandas는 <b>"python data analysis"</b>의 약자
> pandas는 정형 데이터 처리에 특화되어 있다.

- pandas 역시 다양한 머신러닝 라이브러리들에 의존성을 가지고 있다.
> scikit-learn, scipy, statsmodel, tensorflow, pytorch, ...

- 간단하게 생각하면, **python에서 excel의 기능을 사용**할 수 있게 된다.
> pandas = python + excel // pandas & excel // pandas VS MS Excel

- 하지만, pandas는 numpy array를 베이스로 지원하며 파이썬과 함께 강력한 시너지를 내기 때문에, 엑셀 그 이상의 퍼포먼스를 낸다.
> pandas가 Excel에 비해 고성능 데이터처리에 적합하다.

![img]({{ '/assets/images/2022/11/10/img6.png' | relative_url }}){: .left-image }

- Pandas 라이브러리에서 기본적으로 데이터를 다루는 단위는 DataFrame이다. 흔히 알고있는 spreadsheet와 같은 개념

- 이러한 형태의 데이터는 Structured Data 또는 Panel Data 또는 Tabular Data라고 부른다.

- pandas를 공부한다는 것은 결국 dataframe의 사용법을 익히고 활용하는 방법을 배운다는 것과 같다.

- pandas를 잘 활용하면 대부분의 structured data를 자유자재로 다룰 수 있게 된다.

![img]({{ '/assets/images/2022/11/10/img7.png' | relative_url }}){: .left-image }

# Pandas Basic

## Pandas의 기본 자료구조(Series, DataFrame)

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
# pandas 라이브러리를 불러온다. pd를 약칭으로 사용한다.
import pandas as pd
import numpy as np
print(pd.__version__) # pandas version 확인
```

</div>

<div class="output_prompt">
Out&nbsp;[1]:
</div>

{:.output_stream}

```
1.5.1

```

- DataFrame은 2차원 테이블이고, 테이블의 한 줄(행/열)을 Series라고 한다.

- Series의 모임이 곧, DataFrame이 된다.

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
# s는 1, 3, 5, np.nan, 6, 8을 원소로 가지는 pandas.Series
s = pd.Series([1, 3, 5, np.nan, 6, 8])
s
```

</div>

<div class="output_prompt">
Out&nbsp;[2]:
</div>




{:.output_data_text}

```
0    1.0
1    3.0
2    5.0
3    NaN
4    6.0
5    8.0
dtype: float64
```



- pandas는 date_range라는 함수를 통해, 날짜정보를 쉽게 생성해주는 객체도 제공한다.

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
# 20210101부터 6일간의 날짜 범위를 생성하는 pandas.date_range
dates = pd.date_range('20210101', periods=6)
dates
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>




{:.output_data_text}

```
DatetimeIndex(['2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04',
               '2021-01-05', '2021-01-06'],
              dtype='datetime64[ns]', freq='D')
```



<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
# 6x4 행렬에 -1에서 1 사이의 랜덤한 숫자를 가지는 원소를 가지고, index열은 dates, 
# 나머지 coulmns은 순서대로 A, B, C, D로 하는 DataFrame 생성
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=['A', 'B', 'C', 'D'])
df
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>-2.460511</td>
      <td>-1.200258</td>
      <td>-0.783842</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>-1.125980</td>
      <td>1.665172</td>
      <td>0.789345</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.657120</td>
      <td>0.388580</td>
      <td>0.909311</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>0.506280</td>
      <td>-0.699825</td>
      <td>-1.805485</td>
      <td>-0.639235</td>
    </tr>
  </tbody>
</table>
</div>
</div>



## Dataframe 기초 method

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe의 맨 위 다섯줄을 보여주는 head()
df.head() # default=5
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>-2.460511</td>
      <td>-1.200258</td>
      <td>-0.783842</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>-1.125980</td>
      <td>1.665172</td>
      <td>0.789345</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.657120</td>
      <td>0.388580</td>
      <td>0.909311</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
# 3줄
df.head(3)
# df.tail(3)
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>-2.460511</td>
      <td>-1.200258</td>
      <td>-0.783842</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe index
df.index
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>




{:.output_data_text}

```
DatetimeIndex(['2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04',
               '2021-01-05', '2021-01-06'],
              dtype='datetime64[ns]', freq='D')
```



<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe columns
df.columns
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
</div>




{:.output_data_text}

```
Index(['A', 'B', 'C', 'D'], dtype='object')
```



<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe values
df.values
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>




{:.output_data_text}

```
array([[ 0.99572683, -2.46051099, -1.20025819, -0.78384235],
       [ 1.03639362,  0.4734429 , -1.17827902,  1.73439309],
       [-0.34024606,  0.003744  ,  0.70652361,  0.22062538],
       [ 0.00399443, -1.12598039,  1.6651719 ,  0.78934461],
       [ 0.16732115,  0.65711984,  0.38858045,  0.9093105 ],
       [ 0.50628041, -0.69982494, -1.80548532, -0.63923458]])
```



<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe에 대한 전체적인 요약정보 (index, columns, null/not-null/dtype/memory usage)
df.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
DatetimeIndex: 6 entries, 2021-01-01 to 2021-01-06
Freq: D
Data columns (total 4 columns):
 #   Column  Non-Null Count  Dtype  
---  ------  --------------  -----  
 0   A       6 non-null      float64
 1   B       6 non-null      float64
 2   C       6 non-null      float64
 3   D       6 non-null      float64
dtypes: float64(4)
memory usage: 240.0 bytes

```

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe에 대한 전체적인 통계정보
df.describe()
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
      <td>6.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>0.394912</td>
      <td>-0.525335</td>
      <td>-0.237291</td>
      <td>0.371766</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.553164</td>
      <td>1.167203</td>
      <td>1.354538</td>
      <td>0.969585</td>
    </tr>
    <tr>
      <th>min</th>
      <td>-0.340246</td>
      <td>-2.460511</td>
      <td>-1.805485</td>
      <td>-0.783842</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.044826</td>
      <td>-1.019442</td>
      <td>-1.194763</td>
      <td>-0.424270</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.336801</td>
      <td>-0.348040</td>
      <td>-0.394849</td>
      <td>0.504985</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.873365</td>
      <td>0.356018</td>
      <td>0.627038</td>
      <td>0.879319</td>
    </tr>
    <tr>
      <th>max</th>
      <td>1.036394</td>
      <td>0.657120</td>
      <td>1.665172</td>
      <td>1.734393</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
# column B를 기준으로 내림차순 정렬
df.sort_values(by='B', ascending=False).head(3)
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.657120</td>
      <td>0.388580</td>
      <td>0.909311</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
  </tbody>
</table>
</div>
</div>



## DataFrame Indexing

> Indexing : 데이터에서 어떤 특정 조건을 만족하는 원소를 찾는 방법.

> 전체 DataFrame에서 조건에 만족하는 데이터를 쉽게 찾아서 조작할 때 유용하게 사용할 수 있다.

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
# column 이름을 이용하여 기본적인 Indexing이 가능
df["A"]
# dataframe에 바로 indexing을 사용하면, column을 찾는다. == dictionary의 indexing과 같다.
# == "key"를 indexing. == "key" == "column"

# df["2021-01-01"]
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
</div>




{:.output_data_text}

```
2021-01-01    0.995727
2021-01-02    1.036394
2021-01-03   -0.340246
2021-01-04    0.003994
2021-01-05    0.167321
2021-01-06    0.506280
Freq: D, Name: A, dtype: float64
```



<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
# 인덱스명으로 indexing (Serise를 하나 추출) loc: location
df.loc['2021-01-01']
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
</div>




{:.output_data_text}

```
A    0.995727
B   -2.460511
C   -1.200258
D   -0.783842
Name: 2021-01-01 00:00:00, dtype: float64
```



<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
# 특정 위치를 통한 indexing (인덱스번호로 추출) iloc: integer location
df.iloc[2]
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>




{:.output_data_text}

```
A   -0.340246
B    0.003744
C    0.706524
D    0.220625
Name: 2021-01-03 00:00:00, dtype: float64
```



<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe에서 slicing을 이용하면 row 단위로
# 앞에서 3줄을 slicing
df[:3] # 숫자를 그냥 사용하게되면, index(양의 정수)를 이용한 slicing
```

</div>

<div class="output_prompt">
Out&nbsp;[16]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>-2.460511</td>
      <td>-1.200258</td>
      <td>-0.783842</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
# df에서 index value를 기준으로 indexing (row 단위)
# 20210102부터 20210104까지
df['2021-01-02':'2021-01-04'] # index를 이용한 slicing
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>-1.125980</td>
      <td>1.665172</td>
      <td>0.789345</td>
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
# df.loc는 특정값을 기준으로 indexing (key - value)
df.loc[dates[0]] # 굳이 dates 변수를 가져오므로 좋지 못함...(리스트 형태)
```

</div>

<div class="output_prompt">
Out&nbsp;[18]:
</div>




{:.output_data_text}

```
A    0.995727
B   -2.460511
C   -1.200258
D   -0.783842
Name: 2021-01-01 00:00:00, dtype: float64
```



<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
# df.loc에 2차원 indexing도 가능
# [:, ["A", "B"]]의 의미는 모든 row에 대해서 columns는 A, B만 가져오라는 의미
df.loc[:, ["A", "C"]] # dataframe에서 2차원 indexing을 할 때, column들은 리스트로 넘겨줄 수 있다.
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
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
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>-1.200258</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>-1.178279</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.706524</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>1.665172</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.388580</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>0.506280</td>
      <td>-1.805485</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
# slicing을 통해 특정 row중에서 columns는 A, B
df.loc['2021-01-03':'2021-01-05', ['A', 'C']]
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
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
      <th>A</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.706524</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>1.665172</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.388580</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
# 특정 row를 index값을 통한 indexing
df.loc['2021-01-02', ['A', 'B']]
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>




{:.output_data_text}

```
A    1.036394
B    0.473443
Name: 2021-01-02 00:00:00, dtype: float64
```



<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
# 2차원 리스트 indexing과 같은 원리
df.loc['2021-01-01', 'C'] # 특정 row(index)에 특정 column값.
```

</div>

<div class="output_prompt">
Out&nbsp;[22]:
</div>




{:.output_data_text}

```
-1.2002581918334518
```



<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
# df.iloc는 정수를 이용한 indexing과 같다.(row 기준)
df.iloc[3]
```

</div>

<div class="output_prompt">
Out&nbsp;[23]:
</div>




{:.output_data_text}

```
A    0.003994
B   -1.125980
C    1.665172
D    0.789345
Name: 2021-01-04 00:00:00, dtype: float64
```



<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
df.iloc[3:5, 0:2] # df.iloc의 indexing은 numpy array의 2차원 index과 동일해진다.
```

</div>

<div class="output_prompt">
Out&nbsp;[24]:
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
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>-1.12598</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.65712</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[25]:
</div>

<div class="input_area" markdown="1">

```python
# slicing이 아닌 직접 리스트 형태로 기재하는 indexing
df.iloc[[1, 2, 4], [0, 3]] # filtering.
```

</div>

<div class="output_prompt">
Out&nbsp;[25]:
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
      <th>A</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.220625</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.909311</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[26]:
</div>

<div class="input_area" markdown="1">

```python
df.iloc[1:3, :]
```

</div>

<div class="output_prompt">
Out&nbsp;[26]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[27]:
</div>

<div class="input_area" markdown="1">

```python
df.iloc[:, 1:3] # numpy array의 2차원 indexing과 같다.
```

</div>

<div class="output_prompt">
Out&nbsp;[27]:
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
      <th>B</th>
      <th>C</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>-2.460511</td>
      <td>-1.200258</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>0.473443</td>
      <td>-1.178279</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>0.003744</td>
      <td>0.706524</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>-1.125980</td>
      <td>1.665172</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.657120</td>
      <td>0.388580</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>-0.699825</td>
      <td>-1.805485</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[28]:
</div>

<div class="input_area" markdown="1">

```python
df
```

</div>

<div class="output_prompt">
Out&nbsp;[28]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>-2.460511</td>
      <td>-1.200258</td>
      <td>-0.783842</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>-1.178279</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>-0.340246</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>-1.125980</td>
      <td>1.665172</td>
      <td>0.789345</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.657120</td>
      <td>0.388580</td>
      <td>0.909311</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>0.506280</td>
      <td>-0.699825</td>
      <td>-1.805485</td>
      <td>-0.639235</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[29]:
</div>

<div class="input_area" markdown="1">

```python
# pandas는 fancy indexing을 지원한다. (사실 numpy에서 지원하기 때문에 pandas도 지원.)
# fancy indexing이란 조건문을 통해 indexing을 할 수 있는 방법으로
#  True와 False를 원소로 하는 리스트를 통해 masking하는 원리로 동작한다.
# column A에 있는 원소들중에 0보다 큰 데이터를 가져온다.
df['A'] > 0
```

</div>

<div class="output_prompt">
Out&nbsp;[29]:
</div>




{:.output_data_text}

```
2021-01-01     True
2021-01-02     True
2021-01-03    False
2021-01-04     True
2021-01-05     True
2021-01-06     True
Freq: D, Name: A, dtype: bool
```



<div class="in_prompt">
In&nbsp;[30]:
</div>

<div class="input_area" markdown="1">

```python
df.loc[:, 'A'] > 0
```

</div>

<div class="output_prompt">
Out&nbsp;[30]:
</div>




{:.output_data_text}

```
2021-01-01     True
2021-01-02     True
2021-01-03    False
2021-01-04     True
2021-01-05     True
2021-01-06     True
Freq: D, Name: A, dtype: bool
```



<div class="in_prompt">
In&nbsp;[31]:
</div>

<div class="input_area" markdown="1">

```python
df[df['A'] > 0]['B'] # A가 0이상인 값 중에 B값만 출력
```

</div>

<div class="output_prompt">
Out&nbsp;[31]:
</div>




{:.output_data_text}

```
2021-01-01   -2.460511
2021-01-02    0.473443
2021-01-04   -1.125980
2021-01-05    0.657120
2021-01-06   -0.699825
Name: B, dtype: float64
```



<div class="in_prompt">
In&nbsp;[32]:
</div>

<div class="input_area" markdown="1">

```python
# fancy indexing
df['A'][df["A"] > 0] # chain indexing : indexing이 앞에서부터 뒤로 쭉 순서대로 적용
```

</div>

<div class="output_prompt">
Out&nbsp;[32]:
</div>




{:.output_data_text}

```
2021-01-01    0.995727
2021-01-02    1.036394
2021-01-04    0.003994
2021-01-05    0.167321
2021-01-06    0.506280
Name: A, dtype: float64
```



<div class="in_prompt">
In&nbsp;[33]:
</div>

<div class="input_area" markdown="1">

```python
df[df < 0] = 0
df
```

</div>

<div class="output_prompt">
Out&nbsp;[33]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>0.000000</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>0.000000</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>0.000000</td>
      <td>1.665172</td>
      <td>0.789345</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.657120</td>
      <td>0.388580</td>
      <td>0.909311</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>0.506280</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[34]:
</div>

<div class="input_area" markdown="1">

```python
df[df > 0]
```

</div>

<div class="output_prompt">
Out&nbsp;[34]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>0.995727</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>1.036394</td>
      <td>0.473443</td>
      <td>NaN</td>
      <td>1.734393</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>NaN</td>
      <td>0.003744</td>
      <td>0.706524</td>
      <td>0.220625</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>0.003994</td>
      <td>NaN</td>
      <td>1.665172</td>
      <td>0.789345</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>0.167321</td>
      <td>0.657120</td>
      <td>0.388580</td>
      <td>0.909311</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>0.506280</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[35]:
</div>

<div class="input_area" markdown="1">

```python
dates = pd.date_range('20210101', periods=6)
dates
```

</div>

<div class="output_prompt">
Out&nbsp;[35]:
</div>




{:.output_data_text}

```
DatetimeIndex(['2021-01-01', '2021-01-02', '2021-01-03', '2021-01-04',
               '2021-01-05', '2021-01-06'],
              dtype='datetime64[ns]', freq='D')
```



<div class="in_prompt">
In&nbsp;[36]:
</div>

<div class="input_area" markdown="1">

```python
df = pd.DataFrame(np.random.randn(6, 4), index=dates,
                  columns=['A', 'B', 'C', 'D'])
df
```

</div>

<div class="output_prompt">
Out&nbsp;[36]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>-2.043028</td>
      <td>0.185646</td>
      <td>-2.136323</td>
      <td>-1.423348</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>-0.367051</td>
      <td>0.685330</td>
      <td>-1.289581</td>
      <td>0.697019</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>0.554580</td>
      <td>0.839659</td>
      <td>2.055305</td>
      <td>-1.228955</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>2.711900</td>
      <td>0.242401</td>
      <td>-0.590835</td>
      <td>0.813524</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>1.220737</td>
      <td>0.362214</td>
      <td>2.217728</td>
      <td>0.350189</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>-1.570875</td>
      <td>-1.106490</td>
      <td>-0.866496</td>
      <td>1.076941</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[37]:
</div>

<div class="input_area" markdown="1">

```python
df2 = df.copy() # dataframe 하나를 복사
```

</div>

<div class="in_prompt">
In&nbsp;[38]:
</div>

<div class="input_area" markdown="1">

```python
# dataframe은 dictionary와 비슷한 방식으로 assignment가 가능하다.
# df에 ['one', 'one','two','three','four','three'] 리스트를 column의 value로 하는 column E를 추가한다.
df2['E'] = ['one', 'one','two','three','four','three'] # 만약 이미 column E가 존재한다면 update
df2
```

</div>

<div class="output_prompt">
Out&nbsp;[38]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-01</th>
      <td>-2.043028</td>
      <td>0.185646</td>
      <td>-2.136323</td>
      <td>-1.423348</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2021-01-02</th>
      <td>-0.367051</td>
      <td>0.685330</td>
      <td>-1.289581</td>
      <td>0.697019</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2021-01-03</th>
      <td>0.554580</td>
      <td>0.839659</td>
      <td>2.055305</td>
      <td>-1.228955</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2021-01-04</th>
      <td>2.711900</td>
      <td>0.242401</td>
      <td>-0.590835</td>
      <td>0.813524</td>
      <td>three</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>1.220737</td>
      <td>0.362214</td>
      <td>2.217728</td>
      <td>0.350189</td>
      <td>four</td>
    </tr>
    <tr>
      <th>2021-01-06</th>
      <td>-1.570875</td>
      <td>-1.106490</td>
      <td>-0.866496</td>
      <td>1.076941</td>
      <td>three</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[39]:
</div>

<div class="input_area" markdown="1">

```python
# df.isin은 해당 value들이 들어있는 row에 대해선 True를 가지는 Series를 리턴한다.
df2['E'].isin(['two','four'])
```

</div>

<div class="output_prompt">
Out&nbsp;[39]:
</div>




{:.output_data_text}

```
2021-01-01    False
2021-01-02    False
2021-01-03     True
2021-01-04    False
2021-01-05     True
2021-01-06    False
Freq: D, Name: E, dtype: bool
```



<div class="in_prompt">
In&nbsp;[40]:
</div>

<div class="input_area" markdown="1">

```python
df2[df2['E'].isin(['two','four'])]
```

</div>

<div class="output_prompt">
Out&nbsp;[40]:
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
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
      <th>E</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2021-01-03</th>
      <td>0.554580</td>
      <td>0.839659</td>
      <td>2.055305</td>
      <td>-1.228955</td>
      <td>two</td>
    </tr>
    <tr>
      <th>2021-01-05</th>
      <td>1.220737</td>
      <td>0.362214</td>
      <td>2.217728</td>
      <td>0.350189</td>
      <td>four</td>
    </tr>
  </tbody>
</table>
</div>
</div>



# 외부 데이터 읽고 쓰기

<div class="in_prompt">
In&nbsp;[41]:
</div>

<div class="input_area" markdown="1">

```python
# data 폴더에 있는 iris.csv를 불러오자.
import pandas as pd
data = pd.read_csv("data/Iris.csv")
data
```

</div>

<div class="output_prompt">
Out&nbsp;[41]:
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
      <th>Id</th>
      <th>SepalLengthCm</th>
      <th>SepalWidthCm</th>
      <th>PetalLengthCm</th>
      <th>PetalWidthCm</th>
      <th>Species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>Iris-setosa</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>146</td>
      <td>6.7</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>146</th>
      <td>147</td>
      <td>6.3</td>
      <td>2.5</td>
      <td>5.0</td>
      <td>1.9</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>147</th>
      <td>148</td>
      <td>6.5</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.0</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>148</th>
      <td>149</td>
      <td>6.2</td>
      <td>3.4</td>
      <td>5.4</td>
      <td>2.3</td>
      <td>Iris-virginica</td>
    </tr>
    <tr>
      <th>149</th>
      <td>150</td>
      <td>5.9</td>
      <td>3.0</td>
      <td>5.1</td>
      <td>1.8</td>
      <td>Iris-virginica</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 6 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[42]:
</div>

<div class="input_area" markdown="1">

```python
# Species column을 숫자로 바꿔보자.
set(data["Species"])
```

</div>

<div class="output_prompt">
Out&nbsp;[42]:
</div>




{:.output_data_text}

```
{'Iris-setosa', 'Iris-versicolor', 'Iris-virginica'}
```



<div class="in_prompt">
In&nbsp;[43]:
</div>

<div class="input_area" markdown="1">

```python
data.loc[data["Species"] == "Iris-setosa", "Species"] = 0
data.loc[data["Species"] == "Iris-versicolor", "Species"] = 1
data.loc[data["Species"] == "Iris-virginica", "Species"] = 2
data
```

</div>

<div class="output_prompt">
Out&nbsp;[43]:
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
      <th>Id</th>
      <th>SepalLengthCm</th>
      <th>SepalWidthCm</th>
      <th>PetalLengthCm</th>
      <th>PetalWidthCm</th>
      <th>Species</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>5.1</td>
      <td>3.5</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>4.9</td>
      <td>3.0</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>4.7</td>
      <td>3.2</td>
      <td>1.3</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>4.6</td>
      <td>3.1</td>
      <td>1.5</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>5.0</td>
      <td>3.6</td>
      <td>1.4</td>
      <td>0.2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>145</th>
      <td>146</td>
      <td>6.7</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>146</th>
      <td>147</td>
      <td>6.3</td>
      <td>2.5</td>
      <td>5.0</td>
      <td>1.9</td>
      <td>2</td>
    </tr>
    <tr>
      <th>147</th>
      <td>148</td>
      <td>6.5</td>
      <td>3.0</td>
      <td>5.2</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>148</th>
      <td>149</td>
      <td>6.2</td>
      <td>3.4</td>
      <td>5.4</td>
      <td>2.3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>149</th>
      <td>150</td>
      <td>5.9</td>
      <td>3.0</td>
      <td>5.1</td>
      <td>1.8</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
<p>150 rows × 6 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[44]:
</div>

<div class="input_area" markdown="1">

```python
# 바꾼 Dataframe을 Iris_edited.csv 로 저장하자.
data.to_csv("data/Iris_edited.csv")
```

</div>

<div class="in_prompt">
In&nbsp;[45]:
</div>

<div class="input_area" markdown="1">

```python
# 다른 파일도 불러오자.
import pandas as pd
data2 = pd.read_csv("data/kaggle_survey_2020_responses.csv", low_memory=False)
```

</div>

<div class="in_prompt">
In&nbsp;[46]:
</div>

<div class="input_area" markdown="1">

```python
# 박사 학위 소지자들만 골라보자.
phds = data2[data2.iloc[:, 4] == "Doctoral degree"]
phds
```

</div>

<div class="output_prompt">
Out&nbsp;[46]:
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
      <th>Time from Start to Finish (seconds)</th>
      <th>Q1</th>
      <th>Q2</th>
      <th>Q3</th>
      <th>Q4</th>
      <th>Q5</th>
      <th>Q6</th>
      <th>Q7_Part_1</th>
      <th>Q7_Part_2</th>
      <th>Q7_Part_3</th>
      <th>...</th>
      <th>Q35_B_Part_2</th>
      <th>Q35_B_Part_3</th>
      <th>Q35_B_Part_4</th>
      <th>Q35_B_Part_5</th>
      <th>Q35_B_Part_6</th>
      <th>Q35_B_Part_7</th>
      <th>Q35_B_Part_8</th>
      <th>Q35_B_Part_9</th>
      <th>Q35_B_Part_10</th>
      <th>Q35_B_OTHER</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1838</td>
      <td>35-39</td>
      <td>Man</td>
      <td>Colombia</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>R</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9</th>
      <td>762</td>
      <td>35-39</td>
      <td>Man</td>
      <td>Germany</td>
      <td>Doctoral degree</td>
      <td>Data Scientist</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12</th>
      <td>742</td>
      <td>35-39</td>
      <td>Man</td>
      <td>United States of America</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>1-2 years</td>
      <td>NaN</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>21</th>
      <td>3313</td>
      <td>22-24</td>
      <td>Woman</td>
      <td>India</td>
      <td>Doctoral degree</td>
      <td>Statistician</td>
      <td>3-5 years</td>
      <td>NaN</td>
      <td>R</td>
      <td>SQL</td>
      <td>...</td>
      <td>Weights &amp; Biases</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>33</th>
      <td>459</td>
      <td>30-34</td>
      <td>Man</td>
      <td>Other</td>
      <td>Doctoral degree</td>
      <td>Machine Learning Engineer</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>20003</th>
      <td>917</td>
      <td>30-34</td>
      <td>Man</td>
      <td>Colombia</td>
      <td>Doctoral degree</td>
      <td>Software Engineer</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>SQL</td>
      <td>...</td>
      <td>Weights &amp; Biases</td>
      <td>Comet.ml</td>
      <td>Sacred + Omniboard</td>
      <td>TensorBoard</td>
      <td>Guild.ai</td>
      <td>Polyaxon</td>
      <td>Trains</td>
      <td>Domino Model Monitor</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20005</th>
      <td>406</td>
      <td>30-34</td>
      <td>Man</td>
      <td>Italy</td>
      <td>Doctoral degree</td>
      <td>Data Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20007</th>
      <td>487</td>
      <td>45-49</td>
      <td>Man</td>
      <td>United States of America</td>
      <td>Doctoral degree</td>
      <td>Software Engineer</td>
      <td>20+ years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20011</th>
      <td>375</td>
      <td>40-44</td>
      <td>Man</td>
      <td>United Kingdom of Great Britain and Northern I...</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>20020</th>
      <td>611</td>
      <td>25-29</td>
      <td>Prefer not to say</td>
      <td>Germany</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>2302 rows × 355 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[47]:
</div>

<div class="input_area" markdown="1">

```python
# 박사 학위 소지자들에 대한 정보만 kaggle_survey_2020_phd.csv로 다시 저장하자.
phds.to_csv("data/kaggle_survey_2020_phd.csv")
```

</div>

<div class="in_prompt">
In&nbsp;[48]:
</div>

<div class="input_area" markdown="1">

```python
# (OPTIONAL) 박사 학위 소지자이면서, 대한민국 국적을 가진 사람들을 뽑아보자.
data2[(data2["Q4"] == "Doctoral degree") & (data2["Q3"].isin(["Republic of Korea", "South Korea"]))]
```

</div>

<div class="output_prompt">
Out&nbsp;[48]:
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
      <th>Time from Start to Finish (seconds)</th>
      <th>Q1</th>
      <th>Q2</th>
      <th>Q3</th>
      <th>Q4</th>
      <th>Q5</th>
      <th>Q6</th>
      <th>Q7_Part_1</th>
      <th>Q7_Part_2</th>
      <th>Q7_Part_3</th>
      <th>...</th>
      <th>Q35_B_Part_2</th>
      <th>Q35_B_Part_3</th>
      <th>Q35_B_Part_4</th>
      <th>Q35_B_Part_5</th>
      <th>Q35_B_Part_6</th>
      <th>Q35_B_Part_7</th>
      <th>Q35_B_Part_8</th>
      <th>Q35_B_Part_9</th>
      <th>Q35_B_Part_10</th>
      <th>Q35_B_OTHER</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>557</th>
      <td>639</td>
      <td>30-34</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Data Scientist</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1246</th>
      <td>592</td>
      <td>35-39</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Other</td>
      <td>&lt; 1 years</td>
      <td>NaN</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1566</th>
      <td>10700</td>
      <td>45-49</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1936</th>
      <td>475</td>
      <td>30-34</td>
      <td>Woman</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Business Analyst</td>
      <td>I have never written code</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Weights &amp; Biases</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Domino Model Monitor</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2521</th>
      <td>908</td>
      <td>55-59</td>
      <td>Woman</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>R</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4144</th>
      <td>385</td>
      <td>30-34</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>5-10 years</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5313</th>
      <td>738</td>
      <td>30-34</td>
      <td>Woman</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5649</th>
      <td>933</td>
      <td>25-29</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>5897</th>
      <td>201039</td>
      <td>25-29</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6459</th>
      <td>6528</td>
      <td>25-29</td>
      <td>Woman</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>I have never written code</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6827</th>
      <td>872</td>
      <td>30-34</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Machine Learning Engineer</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>Weights &amp; Biases</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6932</th>
      <td>637</td>
      <td>30-34</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7479</th>
      <td>727</td>
      <td>40-44</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Data Analyst</td>
      <td>5-10 years</td>
      <td>NaN</td>
      <td>R</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>7902</th>
      <td>236</td>
      <td>45-49</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Product/Project Manager</td>
      <td>&lt; 1 years</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9135</th>
      <td>626</td>
      <td>25-29</td>
      <td>Woman</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9236</th>
      <td>716</td>
      <td>25-29</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9303</th>
      <td>3560</td>
      <td>35-39</td>
      <td>Woman</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Data Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>R</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>9826</th>
      <td>1565</td>
      <td>60-69</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Data Scientist</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10473</th>
      <td>464</td>
      <td>40-44</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Statistician</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>10905</th>
      <td>779</td>
      <td>45-49</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>5-10 years</td>
      <td>NaN</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11224</th>
      <td>510</td>
      <td>35-39</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>&lt; 1 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11601</th>
      <td>75444</td>
      <td>35-39</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>11958</th>
      <td>161</td>
      <td>40-44</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Machine Learning Engineer</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12029</th>
      <td>131</td>
      <td>60-69</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Currently not employed</td>
      <td>20+ years</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12084</th>
      <td>1527</td>
      <td>50-54</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Data Scientist</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12191</th>
      <td>707</td>
      <td>35-39</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12537</th>
      <td>2998</td>
      <td>25-29</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>12999</th>
      <td>458</td>
      <td>25-29</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>13349</th>
      <td>947</td>
      <td>25-29</td>
      <td>Woman</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Statistician</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14580</th>
      <td>658</td>
      <td>45-49</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>14655</th>
      <td>892</td>
      <td>25-29</td>
      <td>Woman</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>SQL</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Trains</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15537</th>
      <td>289</td>
      <td>30-34</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Student</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>15793</th>
      <td>1162</td>
      <td>35-39</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16583</th>
      <td>280</td>
      <td>40-44</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Currently not employed</td>
      <td>10-20 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>16643</th>
      <td>1045</td>
      <td>55-59</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Product/Project Manager</td>
      <td>&lt; 1 years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>TensorBoard</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17400</th>
      <td>635</td>
      <td>45-49</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Software Engineer</td>
      <td>20+ years</td>
      <td>Python</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17616</th>
      <td>1166</td>
      <td>50-54</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Research Scientist</td>
      <td>1-2 years</td>
      <td>NaN</td>
      <td>R</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>17927</th>
      <td>652</td>
      <td>30-34</td>
      <td>Man</td>
      <td>South Korea</td>
      <td>Doctoral degree</td>
      <td>Machine Learning Engineer</td>
      <td>&lt; 1 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>None</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19867</th>
      <td>708</td>
      <td>25-29</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Data Scientist</td>
      <td>3-5 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>19990</th>
      <td>455</td>
      <td>25-29</td>
      <td>Man</td>
      <td>Republic of Korea</td>
      <td>Doctoral degree</td>
      <td>Machine Learning Engineer</td>
      <td>5-10 years</td>
      <td>Python</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>40 rows × 355 columns</p>
</div>
</div>



# Reference

* 이 포스트는 SeSAC 인공지능 자연어처리, 컴퓨터비전 기술을 활용한 응용 SW 개발자 양성 과정 - 나예진 강사님의 강의를 정리한 내용입니다.
* pandas: [What kind of data does pandas handle?](https://pandas.pydata.org/docs/getting_started/intro_tutorials/01_table_oriented.html)
* pandas: [How do I read and write tabular data?](https://pandas.pydata.org/docs/getting_started/intro_tutorials/02_read_write.html)
* SAURABH SINGH: [Iris.csv](https://www.kaggle.com/datasets/saurabh00007/iriscsv)
* kaggle: [2020 Kaggle ML & DS Survey](https://www.kaggle.com/competitions/kaggle-survey-2020/)
