---
layout: jupyter
title: Python (9) - Pandas 공공데이터 분석
published: true
date: 2022-11-11
description: Pandas를 이용한 공공데이터 분석
comments: true
categories:
 - python
tags:
 - python
 - numpy
 - pandas
toc: true
toc_sticky: true
toc_label: 공공데이터 분석
---
---
# 공공데이터 분석

**들어가며**

- 공공데이터를 분석
- 공공데이터포털(data.go.kr)에 다양한 데이터가 공개되어 있다.
- 그 중에 카페(라는 업종분류)들에 대해서 현황을 조사한다.

**명세사항**
1. 전국 카페 데이터를 모두 수집해야한다.
2. 지역별 or 브랜드별 점포 현황을 확인한다.
3. 분석 결과를 시각화한다. 

공공데이터포털: [소상공인시장진흥공단_상가(상권)정보_20220930](https://www.data.go.kr/data/15083033/fileData.do)

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
import pandas as pd
import numpy as np
```

</div>

# 데이터 불러오기

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
temp = pd.read_csv("data\datapotal\소상공인시장진흥공단_상가(상권)정보_인천_202209.csv", 
                    encoding='utf-8')
temp
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>27458653</td>
      <td>칼스배드영수학원</td>
      <td>NaN</td>
      <td>R</td>
      <td>학문/교육</td>
      <td>R01</td>
      <td>학원-보습교습입시</td>
      <td>R01A01</td>
      <td>학원-입시</td>
      <td>P85501</td>
      <td>...</td>
      <td>2824510500100430000000001</td>
      <td>라임빌</td>
      <td>인천광역시 계양구 임학서로41번길 5, (임학동, 라임빌)</td>
      <td>407814.0</td>
      <td>21030</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.731284</td>
      <td>37.546507</td>
    </tr>
    <tr>
      <th>1</th>
      <td>22882934</td>
      <td>간석미용실</td>
      <td>NaN</td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F01</td>
      <td>이/미용/건강</td>
      <td>F01A01</td>
      <td>여성미용실</td>
      <td>S96112</td>
      <td>...</td>
      <td>2820010200101900026021270</td>
      <td>NaN</td>
      <td>인천광역시 남동구 석촌로14번길 5, (간석동)</td>
      <td>405803.0</td>
      <td>21545</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.709349</td>
      <td>37.461969</td>
    </tr>
    <tr>
      <th>2</th>
      <td>24444979</td>
      <td>라헬</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D05</td>
      <td>의복의류</td>
      <td>D05A01</td>
      <td>일반의류</td>
      <td>G47416</td>
      <td>...</td>
      <td>2818510500109230000007532</td>
      <td>금호동아아파트</td>
      <td>인천광역시 연수구 청능대로 124, (동춘동)</td>
      <td>406775.0</td>
      <td>21967</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.677224</td>
      <td>37.410678</td>
    </tr>
    <tr>
      <th>3</th>
      <td>24872335</td>
      <td>교동상회</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D06</td>
      <td>가방/신발/액세서리</td>
      <td>D06A07</td>
      <td>양품점</td>
      <td>G47419</td>
      <td>...</td>
      <td>2871040023105550000046629</td>
      <td>NaN</td>
      <td>인천광역시 강화군 교동면 교동남로423번길 20, (상용리)</td>
      <td>417921.0</td>
      <td>23001</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.316681</td>
      <td>37.779079</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23397108</td>
      <td>호텔써클</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>2817010200106280011000001</td>
      <td>화이트캐슬</td>
      <td>인천광역시 미추홀구 토금중로3번길 28, (용현동)</td>
      <td>402834.0</td>
      <td>22186</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.635196</td>
      <td>37.455288</td>
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
      <th>120909</th>
      <td>18766857</td>
      <td>쌍용플레이방</td>
      <td>NaN</td>
      <td>N</td>
      <td>관광/여가/오락</td>
      <td>N01</td>
      <td>PC/오락/당구/볼링등</td>
      <td>N01A04</td>
      <td>오락용사격장</td>
      <td>R91229</td>
      <td>...</td>
      <td>2824510600102130002018103</td>
      <td>초정마을두산쌍용아파트</td>
      <td>인천광역시 계양구 계양문화로 142, (용종동)</td>
      <td>407054.0</td>
      <td>21064</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>126.740974</td>
      <td>37.540258</td>
    </tr>
    <tr>
      <th>120910</th>
      <td>18758691</td>
      <td>컴포즈커피</td>
      <td>주안센트레빌점</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q12</td>
      <td>커피점/카페</td>
      <td>Q12A01</td>
      <td>커피전문점/카페/다방</td>
      <td>I56220</td>
      <td>...</td>
      <td>2817710500100180071000001</td>
      <td>NaN</td>
      <td>인천광역시 미추홀구 석정로433번길 31, (주안동)</td>
      <td>402836.0</td>
      <td>22124</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.684375</td>
      <td>37.468028</td>
    </tr>
    <tr>
      <th>120911</th>
      <td>18772273</td>
      <td>교동카센터</td>
      <td>NaN</td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F14</td>
      <td>자동차/이륜차</td>
      <td>F14A01</td>
      <td>자동차정비/카센타</td>
      <td>NaN</td>
      <td>...</td>
      <td>2871040021104420006000001</td>
      <td>NaN</td>
      <td>인천광역시 강화군 교동면 교동서로 9-31, (대룡리)</td>
      <td>417922.0</td>
      <td>23002</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.278870</td>
      <td>37.779109</td>
    </tr>
    <tr>
      <th>120912</th>
      <td>18774152</td>
      <td>다하</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D05</td>
      <td>의복의류</td>
      <td>D05A02</td>
      <td>캐쥬얼/스포츠의류</td>
      <td>G47416</td>
      <td>...</td>
      <td>2823710100101190015103641</td>
      <td>그랜드캐슬</td>
      <td>인천광역시 부평구 부흥북로96번길 20-4, (부평동, 그랜드캐슬)</td>
      <td>403817.0</td>
      <td>21353</td>
      <td>1</td>
      <td>8</td>
      <td>NaN</td>
      <td>126.732476</td>
      <td>37.500144</td>
    </tr>
    <tr>
      <th>120913</th>
      <td>18750230</td>
      <td>에어솔환경</td>
      <td>경인지점</td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F02</td>
      <td>세탁/가사서비스</td>
      <td>F02A05</td>
      <td>청소/소독</td>
      <td>NaN</td>
      <td>...</td>
      <td>2826010500103090006010567</td>
      <td>해피타운</td>
      <td>인천광역시 서구 심곡로208번길 13, (공촌동)</td>
      <td>404836.0</td>
      <td>22709</td>
      <td>가</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.683688</td>
      <td>37.551626</td>
    </tr>
  </tbody>
</table>
<p>120914 rows × 39 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
# data 폴더에 있는 모든 csv 파일을 읽어오기 위해 glob을 사용
from glob import glob

# csv 목록 불러오기
file_names = glob("data\datapotal\*.csv")
total = pd.DataFrame()
# 모든 csv 병합하기
for file_name in file_names:
    temp = pd.read_csv(file_name, encoding='utf-8', low_memory=False)
    total = pd.concat([total, temp])

# reset index
total.reset_index(inplace=True, drop=True)
total
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>25033300</td>
      <td>동그라미중고타이어</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D23</td>
      <td>자동차/자동차용품</td>
      <td>D23A04</td>
      <td>타이어판매</td>
      <td>G45211</td>
      <td>...</td>
      <td>4215011100110960006010791</td>
      <td>NaN</td>
      <td>강원도 강릉시 가작로 270, (포남동)</td>
      <td>210954.0</td>
      <td>25488</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.904472</td>
      <td>37.770252</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17174549</td>
      <td>세인트존스호텔Ohcrab</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4215011300100010001017124</td>
      <td>세인트존스호텔</td>
      <td>강원도 강릉시 창해로 307, (강문동)</td>
      <td>210120.0</td>
      <td>25467</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.920908</td>
      <td>37.791299</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17174079</td>
      <td>평창라마다호텔</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4276038024102450036000001</td>
      <td>NaN</td>
      <td>강원도 평창군 대관령면 오목길 107, (횡계리)</td>
      <td>232954.0</td>
      <td>25342</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.717971</td>
      <td>37.660051</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17173904</td>
      <td>호텔탑스텐스카이라운지</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4215035029100920001000002</td>
      <td>NaN</td>
      <td>강원도 강릉시 옥계면 헌화로 455-34, (금진리)</td>
      <td>210831.0</td>
      <td>25633</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>129.052902</td>
      <td>37.654680</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24412526</td>
      <td>레이디가구</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D15</td>
      <td>가구소매</td>
      <td>D15A01</td>
      <td>일반가구소매</td>
      <td>G47520</td>
      <td>...</td>
      <td>4213011500111400020035715</td>
      <td>NaN</td>
      <td>강원도 원주시 송삼길 156-19, (무실동)</td>
      <td>220150.0</td>
      <td>26385</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.917307</td>
      <td>37.327668</td>
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
      <th>2446766</th>
      <td>18759758</td>
      <td>올나노</td>
      <td>충주점</td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F02</td>
      <td>세탁/가사서비스</td>
      <td>F02A05</td>
      <td>청소/소독</td>
      <td>NaN</td>
      <td>...</td>
      <td>4377035029104150000000001</td>
      <td>NaN</td>
      <td>충청북도 음성군 삼성면 대덕로 138-38, (대정리)</td>
      <td>369831.0</td>
      <td>27648</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.518699</td>
      <td>37.025560</td>
    </tr>
    <tr>
      <th>2446767</th>
      <td>18753428</td>
      <td>스테이퓨전바</td>
      <td>NaN</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>4311110900102270029047521</td>
      <td>NaN</td>
      <td>충청북도 청주시 상당구 무심동로390번길 15, (서문동)</td>
      <td>360130.0</td>
      <td>28528</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>127.484757</td>
      <td>36.634341</td>
    </tr>
    <tr>
      <th>2446768</th>
      <td>18769189</td>
      <td>백마탁송대리서비스</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D02</td>
      <td>선물/팬시/기념품</td>
      <td>D02A02</td>
      <td>꽃집/꽃배달</td>
      <td>G47851</td>
      <td>...</td>
      <td>4311311300108780000009545</td>
      <td>NaN</td>
      <td>충청북도 청주시 흥덕구 풍년로198번길 56-1, (가경동)</td>
      <td>361800.0</td>
      <td>28389</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>127.432312</td>
      <td>36.631408</td>
    </tr>
    <tr>
      <th>2446769</th>
      <td>18768587</td>
      <td>구제언니</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D05</td>
      <td>의복의류</td>
      <td>D05A07</td>
      <td>셔츠/내의/속옷</td>
      <td>NaN</td>
      <td>...</td>
      <td>4311311400120810000012970</td>
      <td>NaN</td>
      <td>충청북도 청주시 흥덕구 풍산로118번길 27-1, (복대동)</td>
      <td>361814.0</td>
      <td>28605</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.443014</td>
      <td>36.628061</td>
    </tr>
    <tr>
      <th>2446770</th>
      <td>18762532</td>
      <td>엠모터스</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D23</td>
      <td>자동차/자동차용품</td>
      <td>D23A08</td>
      <td>중고타이어판매</td>
      <td>G45220</td>
      <td>...</td>
      <td>4375033027103230005026907</td>
      <td>NaN</td>
      <td>충청북도 진천군 문백면 문진로 491, (봉죽리)</td>
      <td>365861.0</td>
      <td>27869</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.434572</td>
      <td>36.784046</td>
    </tr>
  </tbody>
</table>
<p>2446771 rows × 39 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
# 분석에 필요한 column을 고르기
data = total[['상호명', '지점명', '상권업종대분류명', '상권업종중분류명', '시도명', '시군구명', '행정동명']]
data
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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>동그라미중고타이어</td>
      <td>NaN</td>
      <td>소매</td>
      <td>자동차/자동차용품</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>포남1동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>세인트존스호텔Ohcrab</td>
      <td>NaN</td>
      <td>숙박</td>
      <td>호텔/콘도</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>초당동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>평창라마다호텔</td>
      <td>NaN</td>
      <td>숙박</td>
      <td>호텔/콘도</td>
      <td>강원도</td>
      <td>평창군</td>
      <td>대관령면</td>
    </tr>
    <tr>
      <th>3</th>
      <td>호텔탑스텐스카이라운지</td>
      <td>NaN</td>
      <td>숙박</td>
      <td>호텔/콘도</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>옥계면</td>
    </tr>
    <tr>
      <th>4</th>
      <td>레이디가구</td>
      <td>NaN</td>
      <td>소매</td>
      <td>가구소매</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>무실동</td>
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
    </tr>
    <tr>
      <th>2446766</th>
      <td>올나노</td>
      <td>충주점</td>
      <td>생활서비스</td>
      <td>세탁/가사서비스</td>
      <td>충청북도</td>
      <td>음성군</td>
      <td>삼성면</td>
    </tr>
    <tr>
      <th>2446767</th>
      <td>스테이퓨전바</td>
      <td>NaN</td>
      <td>음식</td>
      <td>한식</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>성안동</td>
    </tr>
    <tr>
      <th>2446768</th>
      <td>백마탁송대리서비스</td>
      <td>NaN</td>
      <td>소매</td>
      <td>선물/팬시/기념품</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>가경동</td>
    </tr>
    <tr>
      <th>2446769</th>
      <td>구제언니</td>
      <td>NaN</td>
      <td>소매</td>
      <td>의복의류</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>복대2동</td>
    </tr>
    <tr>
      <th>2446770</th>
      <td>엠모터스</td>
      <td>NaN</td>
      <td>소매</td>
      <td>자동차/자동차용품</td>
      <td>충청북도</td>
      <td>진천군</td>
      <td>문백면</td>
    </tr>
  </tbody>
</table>
<p>2446771 rows × 7 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
total.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2446771 entries, 0 to 2446770
Data columns (total 39 columns):
 #   Column     Dtype  
---  ------     -----  
 0   상가업소번호     int64  
 1   상호명        object 
 2   지점명        object 
 3   상권업종대분류코드  object 
 4   상권업종대분류명   object 
 5   상권업종중분류코드  object 
 6   상권업종중분류명   object 
 7   상권업종소분류코드  object 
 8   상권업종소분류명   object 
 9   표준산업분류코드   object 
 10  표준산업분류명    object 
 11  시도코드       int64  
 12  시도명        object 
 13  시군구코드      int64  
 14  시군구명       object 
 15  행정동코드      int64  
 16  행정동명       object 
 17  법정동코드      int64  
 18  법정동명       object 
 19  지번코드       int64  
 20  대지구분코드     int64  
 21  대지구분명      object 
 22  지번본번지      int64  
 23  지번부번지      float64
 24  지번주소       object 
 25  도로명코드      int64  
 26  도로명        object 
 27  건물본번지      int64  
 28  건물부번지      float64
 29  건물관리번호     object 
 30  건물명        object 
 31  도로명주소      object 
 32  구우편번호      float64
 33  신우편번호      int64  
 34  동정보        object 
 35  층정보        object 
 36  호정보        float64
 37  경도         float64
 38  위도         float64
dtypes: float64(6), int64(11), object(22)
memory usage: 728.0+ MB

```

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
data.info()
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2446771 entries, 0 to 2446770
Data columns (total 7 columns):
 #   Column    Dtype 
---  ------    ----- 
 0   상호명       object
 1   지점명       object
 2   상권업종대분류명  object
 3   상권업종중분류명  object
 4   시도명       object
 5   시군구명      object
 6   행정동명      object
dtypes: object(7)
memory usage: 130.7+ MB

```

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
# 메모리 낭비를 막기 위해 필요없는 변수는 제거
del temp
```

</div>

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
total.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[8]:
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
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>25033300</td>
      <td>동그라미중고타이어</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D23</td>
      <td>자동차/자동차용품</td>
      <td>D23A04</td>
      <td>타이어판매</td>
      <td>G45211</td>
      <td>...</td>
      <td>4215011100110960006010791</td>
      <td>NaN</td>
      <td>강원도 강릉시 가작로 270, (포남동)</td>
      <td>210954.0</td>
      <td>25488</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.904472</td>
      <td>37.770252</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17174549</td>
      <td>세인트존스호텔Ohcrab</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4215011300100010001017124</td>
      <td>세인트존스호텔</td>
      <td>강원도 강릉시 창해로 307, (강문동)</td>
      <td>210120.0</td>
      <td>25467</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.920908</td>
      <td>37.791299</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17174079</td>
      <td>평창라마다호텔</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4276038024102450036000001</td>
      <td>NaN</td>
      <td>강원도 평창군 대관령면 오목길 107, (횡계리)</td>
      <td>232954.0</td>
      <td>25342</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.717971</td>
      <td>37.660051</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17173904</td>
      <td>호텔탑스텐스카이라운지</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4215035029100920001000002</td>
      <td>NaN</td>
      <td>강원도 강릉시 옥계면 헌화로 455-34, (금진리)</td>
      <td>210831.0</td>
      <td>25633</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>129.052902</td>
      <td>37.654680</td>
    </tr>
    <tr>
      <th>4</th>
      <td>24412526</td>
      <td>레이디가구</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D15</td>
      <td>가구소매</td>
      <td>D15A01</td>
      <td>일반가구소매</td>
      <td>G47520</td>
      <td>...</td>
      <td>4213011500111400020035715</td>
      <td>NaN</td>
      <td>강원도 원주시 송삼길 156-19, (무실동)</td>
      <td>220150.0</td>
      <td>26385</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.917307</td>
      <td>37.327668</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 39 columns</p>
</div>
</div>



# 데이터 보기

## 전국 커피 전문점

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
#set(data["상권업종대분류명"])
set(data["상권업종중분류명"])
```

</div>

<div class="output_prompt">
Out&nbsp;[9]:
</div>




{:.output_data_text}

```
{'PC/오락/당구/볼링등',
 '가구소매',
 '가방/신발/액세서리',
 '가전제품소매',
 '가정/주방/인테리어',
 '개인/가정용품수리',
 '개인서비스',

...

 '커피점/카페',
 '패스트푸드',
 '페인트/유리제품소매',
 '평가/개발/관리',
 '학문교육기타',
 '학원-보습교습입시',
 '학원-어학',
 '학원-예능취미체육',
 '학원-음악미술무용',
 '학원-자격/국가고시',
 '학원-창업취업취미',
 '학원-컴퓨터',
 '학원기타',
 '한식',
 '행사/이벤트',
 '호텔/콘도',
 '화장품소매'}
```



<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
# 카페만 뽑아내기
df_coffee = data[data["상권업종중분류명"] == "커피점/카페"]
print(type(df_coffee))
print(df_coffee.index)

# index를 다시 세팅
df_coffee.index = range(len(df_coffee))
print("전국 커피 전문점 점포 수 : ", len(df_coffee))
df_coffee
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>

{:.output_stream}

```
<class 'pandas.core.frame.DataFrame'>
Int64Index([     13,      39,      56,      83,     270,     274,     345,
                462,     544,     610,
            ...
            2446579, 2446622, 2446641, 2446653, 2446704, 2446705, 2446708,
            2446734, 2446754, 2446757],
           dtype='int64', length=114428)
전국 커피 전문점 점포 수 :  114428

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>키즈까페아이사랑</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>성덕동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>힐링</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단구동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>드롭탑</td>
      <td>속초엑스포점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>속초시</td>
      <td>조양동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>상유재카페</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>정선군</td>
      <td>정선읍</td>
    </tr>
    <tr>
      <th>4</th>
      <td>수정다방</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>고성군</td>
      <td>거진읍</td>
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
    </tr>
    <tr>
      <th>114423</th>
      <td>커피텐타</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>청전동</td>
    </tr>
    <tr>
      <th>114424</th>
      <td>컴포즈커피</td>
      <td>충주건국대점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>충주시</td>
      <td>달천동</td>
    </tr>
    <tr>
      <th>114425</th>
      <td>에이맨</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>복대2동</td>
    </tr>
    <tr>
      <th>114426</th>
      <td>달무지개</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>오창읍</td>
    </tr>
    <tr>
      <th>114427</th>
      <td>홀덤킹</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>중앙동</td>
    </tr>
  </tbody>
</table>
<p>114428 rows × 7 columns</p>
</div>
</div>



## 서울내 커피 전문점

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
set(data["시도명"])
```

</div>

<div class="output_prompt">
Out&nbsp;[11]:
</div>




{:.output_data_text}

```
{'강원도',
 '경기도',
 '경상남도',
 '경상북도',
 '광주광역시',
 '대구광역시',
 '대전광역시',
 '부산광역시',
 '서울특별시',
 '세종특별자치시',
 '울산광역시',
 '인천광역시',
 '전라남도',
 '전라북도',
 '제주특별자치도',
 '충청남도',
 '충청북도'}
```



<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
# 카페 중에 "서울"에 위치하고 있는 점포만 뽑아내기
df_seoul_coffee = data[(data["상권업종중분류명"] == "커피점/카페") & (data["시도명"] == "서울특별시")]
df_seoul_coffee.index = range(len(df_seoul_coffee))
print('서울시 내 커피 전문점 점포 수 :', len(df_seoul_coffee))
df_seoul_coffee
```

</div>

<div class="output_prompt">
Out&nbsp;[12]:
</div>

{:.output_stream}

```
서울시 내 커피 전문점 점포 수 : 20842

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>커피빈</td>
      <td>코리아대학로대명거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>혜화동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>요거프레소</td>
      <td>쌍문점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>도봉구</td>
      <td>쌍문2동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>우성커피숍</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>양천구</td>
      <td>신월4동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>버블베어</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강서구</td>
      <td>방화3동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>알뤼르</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>대치4동</td>
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
    </tr>
    <tr>
      <th>20837</th>
      <td>공유KYOUYUU</td>
      <td>2호점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>성동구</td>
      <td>용답동</td>
    </tr>
    <tr>
      <th>20838</th>
      <td>운비차실</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>중구</td>
      <td>회현동</td>
    </tr>
    <tr>
      <th>20839</th>
      <td>풀꽃</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강동구</td>
      <td>암사3동</td>
    </tr>
    <tr>
      <th>20840</th>
      <td>댄싱컵</td>
      <td>가재울점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서대문구</td>
      <td>남가좌2동</td>
    </tr>
    <tr>
      <th>20841</th>
      <td>플랫카페</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>광진구</td>
      <td>구의1동</td>
    </tr>
  </tbody>
</table>
<p>20842 rows × 7 columns</p>
</div>
</div>



## 브랜드 커피 전문점 전국, 서울 점포 수

### 전국 스타벅스

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
df_coffee_star = data[data["상호명"] == "스타벅스"] # 완전히 일치하는 결과만 나옴
df_coffee_star
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>8605</th>
      <td>스타벅스</td>
      <td>춘천명동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>약사명동</td>
    </tr>
    <tr>
      <th>10392</th>
      <td>스타벅스</td>
      <td>원주터미널점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>무실동</td>
    </tr>
    <tr>
      <th>14450</th>
      <td>스타벅스</td>
      <td>춘천온의점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>강남동</td>
    </tr>
    <tr>
      <th>16562</th>
      <td>스타벅스</td>
      <td>춘천구봉산R점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>동면</td>
    </tr>
    <tr>
      <th>18068</th>
      <td>스타벅스</td>
      <td>속초DT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>속초시</td>
      <td>조양동</td>
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
    </tr>
    <tr>
      <th>2420924</th>
      <td>스타벅스</td>
      <td>청주율량DT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>율량.사천동</td>
    </tr>
    <tr>
      <th>2425217</th>
      <td>스타벅스</td>
      <td>충주연수점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>충주시</td>
      <td>연수동</td>
    </tr>
    <tr>
      <th>2427947</th>
      <td>스타벅스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>음성군</td>
      <td>원남면</td>
    </tr>
    <tr>
      <th>2435007</th>
      <td>스타벅스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>청전동</td>
    </tr>
    <tr>
      <th>2439507</th>
      <td>스타벅스</td>
      <td>청주복대지웰점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>복대1동</td>
    </tr>
  </tbody>
</table>
<p>1209 rows × 7 columns</p>
</div>
</div>



<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
df_coffee["상호명"].str.contains("스타벅스") # 문자열을 포함하는 경우도 나옴
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
</div>




{:.output_data_text}

```
0         False
1         False
2         False
3         False
4         False
          ...  
114423    False
114424    False
114425    False
114426    False
114427    False
Name: 상호명, Length: 114428, dtype: bool
```



<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
# 이번엔 전국에 있는 스타벅스를 뽑아내자
df_starbucks = df_coffee[df_coffee["상호명"].str.contains("스타벅스")]
df_starbucks.index = range(len(df_starbucks))
print('전국 스타벅스 점포 수 :', len(df_starbucks))
df_starbucks
```

</div>

<div class="output_prompt">
Out&nbsp;[15]:
</div>

{:.output_stream}

```
전국 스타벅스 점포 수 : 1498

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>스타벅스강릉안목항점</td>
      <td>강릉안목항점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>송정동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>스타벅스춘천후평DT점</td>
      <td>춘천후평DT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>후평3동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>스타벅스</td>
      <td>춘천명동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>약사명동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>스타벅스설악워터피아점</td>
      <td>설악워터피아점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>속초시</td>
      <td>영랑동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>스타벅스</td>
      <td>원주터미널점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>무실동</td>
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
    </tr>
    <tr>
      <th>1493</th>
      <td>스타벅스</td>
      <td>청주율량DT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>율량.사천동</td>
    </tr>
    <tr>
      <th>1494</th>
      <td>스타벅스</td>
      <td>충주연수점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>충주시</td>
      <td>연수동</td>
    </tr>
    <tr>
      <th>1495</th>
      <td>스타벅스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>음성군</td>
      <td>원남면</td>
    </tr>
    <tr>
      <th>1496</th>
      <td>스타벅스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>청전동</td>
    </tr>
    <tr>
      <th>1497</th>
      <td>스타벅스</td>
      <td>청주복대지웰점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>복대1동</td>
    </tr>
  </tbody>
</table>
<p>1498 rows × 7 columns</p>
</div>
</div>



### 서울 스타벅스

<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
# 이번엔 서울에 있는 스타벅스를 뽑아내자
df_seoul_starbucks = df_starbucks[df_starbucks["시도명"] == "서울특별시"]
df_seoul_starbucks.index = range(len(df_seoul_starbucks))
print('서울시 내 스타벅스 점포 수 :', len(df_seoul_starbucks))
df_seoul_starbucks.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[16]:
</div>

{:.output_stream}

```
서울시 내 스타벅스 점포 수 : 460

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>스타벅스</td>
      <td>동숭로아트점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>이화동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>스타벅스남부터미널2점</td>
      <td>남부터미널2점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초3동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>스타벅스</td>
      <td>현대목동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>양천구</td>
      <td>목1동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>스타벅스미아사거리역점</td>
      <td>미아사거리역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강북구</td>
      <td>송중동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>스타벅스</td>
      <td>가로수길점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>신사동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 전국 이디야

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
# 이번엔 전국에 있는 이디야를 뽑아내자
df_ediya = df_coffee[df_coffee["상호명"].str.contains("이디야")]
df_ediya.index = range(len(df_ediya))
print('전국 이디야 점포 수 :', len(df_ediya))
df_ediya
```

</div>

<div class="output_prompt">
Out&nbsp;[17]:
</div>

{:.output_stream}

```
전국 이디야 점포 수 : 2157

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>이디야커피</td>
      <td>원주반곡동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>반곡관설동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>이디야커피</td>
      <td>춘천제일점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>강남동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>이디야커피</td>
      <td>흥업점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>흥업면</td>
    </tr>
    <tr>
      <th>3</th>
      <td>이디야커피</td>
      <td>정동진역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>강동면</td>
    </tr>
    <tr>
      <th>4</th>
      <td>이디야커피</td>
      <td>속초동명항점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>속초시</td>
      <td>동명동</td>
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
    </tr>
    <tr>
      <th>2152</th>
      <td>이디야커피</td>
      <td>사직중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>사직2동</td>
    </tr>
    <tr>
      <th>2153</th>
      <td>이디야커피</td>
      <td>증평송산점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>증평군</td>
      <td>증평읍</td>
    </tr>
    <tr>
      <th>2154</th>
      <td>이디야커피</td>
      <td>제천장락중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>교동</td>
    </tr>
    <tr>
      <th>2155</th>
      <td>이디야커피</td>
      <td>제천의림지DI점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>의림지동</td>
    </tr>
    <tr>
      <th>2156</th>
      <td>이디야커피</td>
      <td>청주운동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시</td>
      <td>용암2동</td>
    </tr>
  </tbody>
</table>
<p>2157 rows × 7 columns</p>
</div>
</div>



### 서울 이디야

<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
# 이번엔 서울에 있는 이디야를 뽑아내자
df_seoul_ediya = df_ediya[df_ediya["시도명"] == "서울특별시"]
df_seoul_ediya.index = range(len(df_seoul_ediya))
print('서울시 내 이디야 점포 수 :', len(df_seoul_ediya))
df_seoul_ediya.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[18]:
</div>

{:.output_stream}

```
서울시 내 이디야 점포 수 : 427

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>이디야커피</td>
      <td>신길역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>영등포구</td>
      <td>신길1동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>이디야커피</td>
      <td>라이프점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>영등포구</td>
      <td>여의동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>이디야커피양재AT점</td>
      <td>양재AT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>양재2동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>이디야커피</td>
      <td>시흥점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>금천구</td>
      <td>시흥2동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>이디야커피</td>
      <td>개봉중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>구로구</td>
      <td>개봉3동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 전국 커피빈

<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
df_coffeebean = df_coffee[df_coffee['상호명'].str.contains('커피빈')]
df_coffeebean.index = range(len(df_coffeebean))
print('전국 커피빈 점포 수 :', len(df_coffeebean))
df_coffeebean.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>

{:.output_stream}

```
전국 커피빈 점포 수 : 252

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>커피빈</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>묵호동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>커피빈</td>
      <td>코리아원주AK플라자점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단계동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>베가커피빈스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>경기도</td>
      <td>수원시</td>
      <td>율천동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>커피빈</td>
      <td>현대프리미엄아울렛김포점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>경기도</td>
      <td>김포시</td>
      <td>고촌읍</td>
    </tr>
    <tr>
      <th>4</th>
      <td>커피빈</td>
      <td>중동현대백화점U-PLEX점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>경기도</td>
      <td>부천시</td>
      <td>신중동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 서울 커피빈

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
df_seoul_coffeebean = df_seoul_coffee[df_seoul_coffee['상호명'].str.contains('커피빈')]
df_seoul_coffeebean.index = range(len(df_seoul_coffeebean))
print('서울시 내 커피빈 점포 수 :', len(df_seoul_coffeebean))
df_seoul_coffeebean.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
</div>

{:.output_stream}

```
서울시 내 커피빈 점포 수 : 147

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>커피빈</td>
      <td>코리아대학로대명거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>혜화동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>커피빈코리아낙성대역점</td>
      <td>코리아낙성대역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>관악구</td>
      <td>행운동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>커피빈</td>
      <td>코리아청담에스점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>청담동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>커피빈</td>
      <td>코리아강남역랭기지타워점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>역삼1동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>커피빈</td>
      <td>코리아청담성당점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>청담동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 전국 투썸

<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
df_2some = df_coffee[df_coffee['상호명'].str.contains('투썸')]
df_2some.index = range(len(df_2some))
print('전국 투썸플레이스 점포 수 :', len(df_2some))
df_2some.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>

{:.output_stream}

```
전국 투썸플레이스 점포 수 : 1127

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>투썸플레이스</td>
      <td>춘천명동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>조운동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>투썸플레이스</td>
      <td>소양강댐점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>신북읍</td>
    </tr>
    <tr>
      <th>2</th>
      <td>투썸플레이스</td>
      <td>용평리조트점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>평창군</td>
      <td>대관령면</td>
    </tr>
    <tr>
      <th>3</th>
      <td>투썸플레이스</td>
      <td>철원와수점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>철원군</td>
      <td>서면</td>
    </tr>
    <tr>
      <th>4</th>
      <td>투썸플레이스동해중앙점</td>
      <td>동해중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>천곡동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 서울 투썸

<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
df_seoul_2some = df_seoul_coffee[df_seoul_coffee['상호명'].str.contains('투썸')]
df_seoul_2some.index = range(len(df_seoul_2some))
print('서울시 내 투썸플레이스 점포 수 :', len(df_seoul_2some))
df_seoul_2some.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[22]:
</div>

{:.output_stream}

```
서울시 내 투썸플레이스 점포 수 : 255

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>투썸플레이스서울대역중앙점</td>
      <td>서울대역중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>관악구</td>
      <td>중앙동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>투썸플레이스</td>
      <td>씨제이프레시웨이강남세브란스병원점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>도곡1동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>투썸플레이스</td>
      <td>LG광화문빌딩점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>사직동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>투썸플레이스</td>
      <td>가락시장역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>송파구</td>
      <td>가락본동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>투썸플레이스</td>
      <td>CGV수유</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강북구</td>
      <td>수유3동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 전국 빽다방

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
df_bbaek = df_coffee[df_coffee['상호명'].str.contains('빽다방')]
df_bbaek.index = range(len(df_bbaek))
print('전국 빽다방 점포 수 :', len(df_bbaek))
df_bbaek.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[23]:
</div>

{:.output_stream}

```
전국 빽다방 점포 수 : 908

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>빽다방</td>
      <td>춘천석사CGV점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>석사동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>빽다방동해천곡점</td>
      <td>동해천곡점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>천곡동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>빽다방</td>
      <td>원주중앙1호점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>일산동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>빽다방</td>
      <td>삼척대학로점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>삼척시</td>
      <td>성내동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>빽다방</td>
      <td>홍천중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>홍천군</td>
      <td>홍천읍</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 서울 빽다방

<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
df_seoul_bbaek = df_seoul_coffee[df_seoul_coffee['상호명'].str.contains('빽다방')]
df_seoul_bbaek.index = range(len(df_seoul_bbaek))
print('서울시 내 빽다방 점포 수 :', len(df_seoul_bbaek))
df_seoul_bbaek.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[24]:
</div>

{:.output_stream}

```
서울시 내 빽다방 점포 수 : 170

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>빽다방공덕새창로점</td>
      <td>공덕새창로점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>마포구</td>
      <td>도화동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>빽다방서초우성점</td>
      <td>서초우성점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초2동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>빽다방중계은행사거리점</td>
      <td>중계은행사거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>노원구</td>
      <td>중계1동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>빽다방성신여대점</td>
      <td>성신여대점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>성북구</td>
      <td>동선동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>빽다방신림역1호점</td>
      <td>신림역1호점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>관악구</td>
      <td>서원동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 전국 할리스

<div class="in_prompt">
In&nbsp;[25]:
</div>

<div class="input_area" markdown="1">

```python
df_hollys = df_coffee[df_coffee['상호명'].str.contains('할리스')]
df_hollys.index = range(len(df_hollys))
print('전국 할리스 점포 수 :', len(df_hollys))
df_hollys.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[25]:
</div>

{:.output_stream}

```
전국 할리스 점포 수 : 562

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>할리스커피</td>
      <td>중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>중앙동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>할리스커피동해묵호점</td>
      <td>동해묵호점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>천곡동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>할리스커피강릉점</td>
      <td>강릉점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>중앙동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>할리스커피</td>
      <td>강릉휴게소인천방면</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>성산면</td>
    </tr>
    <tr>
      <th>4</th>
      <td>할리스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>송정동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 서울 할리스

<div class="in_prompt">
In&nbsp;[26]:
</div>

<div class="input_area" markdown="1">

```python
df_seoul_hollys = df_seoul_coffee[df_seoul_coffee['상호명'].str.contains('할리스')]
df_seoul_hollys.index = range(len(df_seoul_hollys))
print('서울시 내 할리스 점포 수 :', len(df_seoul_hollys))
df_seoul_hollys.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[26]:
</div>

{:.output_stream}

```
서울시 내 할리스 점포 수 : 141

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>할리스커피남부터미널</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초1동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>할리스</td>
      <td>백석예술대점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>방배3동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>할리스커피마포역점</td>
      <td>마포역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>마포구</td>
      <td>용강동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>할리스커피</td>
      <td>목동예술인회관점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>양천구</td>
      <td>목1동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>할리스커피</td>
      <td>르네상스사거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>역삼2동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 전국 메가커피

<div class="in_prompt">
In&nbsp;[27]:
</div>

<div class="input_area" markdown="1">

```python
df_mega = df_coffee[df_coffee['상호명'].str.contains('메가')]
df_mega.index = range(len(df_mega))
print('전국 메가커피 점포 수 :', len(df_mega))
df_mega.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[27]:
</div>

{:.output_stream}

```
전국 메가커피 점포 수 : 1857

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>메가커피인제점</td>
      <td>인제점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>인제군</td>
      <td>인제읍</td>
    </tr>
    <tr>
      <th>1</th>
      <td>메가엠지씨커피</td>
      <td>춘천팔호광장점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>효자2동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>메가엠지씨커피</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>후평3동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>메가MGC커피</td>
      <td>춘천석사점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>석사동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>메가엠지씨커피</td>
      <td>동해시청점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>천곡동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



### 서울 메가커피

<div class="in_prompt">
In&nbsp;[28]:
</div>

<div class="input_area" markdown="1">

```python
df_seoul_mega = df_seoul_coffee[df_seoul_coffee['상호명'].str.contains('메가')]
df_seoul_mega.index = range(len(df_seoul_mega))
print('서울시 내 메가커피 점포 수 :', len(df_seoul_mega))
df_seoul_mega.head()
```

</div>

<div class="output_prompt">
Out&nbsp;[28]:
</div>

{:.output_stream}

```
서울시 내 메가커피 점포 수 : 427

```




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
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>메가커피</td>
      <td>아차산역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>광진구</td>
      <td>중곡4동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>메가커피</td>
      <td>신설동역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>동대문구</td>
      <td>용신동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>메가엠지씨커피</td>
      <td>영등포역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>영등포구</td>
      <td>영등포동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>메가엠지씨커피</td>
      <td>강동구청점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강동구</td>
      <td>성내1동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>메가커피</td>
      <td>개봉사거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>구로구</td>
      <td>개봉1동</td>
    </tr>
  </tbody>
</table>
</div>
</div>



## 커피전문점 별 비율 비교(주요 브랜드 위주)

BRI2022 [커피전문점 브랜드 2022년 8월 빅데이터 분석결과](https://brikorea.com/bbs/board.php?bo_table=rep_1&wr_id=1632)

1. 스타벅스
2. 메가커피
3. 투썸플레이스
4. 이디야
5. 빽다방

**변수**

- 전체 점포 : data
- 전체/서울 커피전문점 : df_coffee / df_seoul_coffee

- 전체/서울 스타벅스 : df_starbucks / df_seoul_starbucks
- 전체/서울 이디야 : df_ediya / df_seoul_ediya
- 전체/서울 커피빈 : df_coffeebean / df_seoul_coffeebean
- 전체/서울 투썸플레이스 : df_2some / df_seoul_2some
- 전체/서울 빽다방 : df_bbaek / df_seoul_bbaek
- 전체/서울 할리스 : df_hollys / df_seoul_hollys
- 전체/서울 메가커피 : df_mega / df_seoul_mega

### 전체 커피전문점 내 주요 커피브랜드 입점 비율 

<div class="in_prompt">
In&nbsp;[29]:
</div>

<div class="input_area" markdown="1">

```python
print("**** 전국 커피전문점중 주요 5대 커피브랜드 입점 비율 ****")
print("주요 5대 커피브랜드 전국 입점 비율 : %.3f%%"\
      % ((len(df_starbucks)+len(df_2some)+len(df_ediya)+len(df_mega)+len(df_coffeebean))
        / len(df_coffee) * 100))
print("1. 스타벅스 : %.3f%%" % (len(df_starbucks) / len(df_coffee) * 100))
print("2. 메가커피 : %.3f%%" % (len(df_mega) / len(df_coffee) * 100))
print("3. 투썸플레이스 : %.3f%%" % (len(df_2some) / len(df_coffee) * 100))
print("3. 이디야 : %.3f%%" % (len(df_ediya) / len(df_coffee) * 100))
print("5. 빽다방 : %.3f%%" % (len(df_bbaek) / len(df_coffee) * 100))
```

</div>

<div class="output_prompt">
Out&nbsp;[29]:
</div>

{:.output_stream}

```
**** 전국 커피전문점중 주요 5대 커피브랜드 입점 비율 ****
주요 5대 커피브랜드 전국 입점 비율 : 6.022%
1. 스타벅스 : 1.309%
2. 메가커피 : 1.623%
3. 투썸플레이스 : 0.985%
3. 이디야 : 1.885%
5. 빽다방 : 0.794%

```

### 서울 커피전문점 내 주요 커피브랜드 입점 비율

<div class="in_prompt">
In&nbsp;[30]:
</div>

<div class="input_area" markdown="1">

```python
print("스타벅스 : %.3f%%" % (len(df_seoul_starbucks) / len(df_seoul_coffee) * 100))
print("이디야 : %.3f%%" % (len(df_seoul_ediya) / len(df_seoul_coffee)* 100))
print("커피빈 : %.3f%%" % (len(df_seoul_coffeebean) / len(df_seoul_coffee)* 100))
print("투썸플레이스 : %.3f%%" % (len(df_seoul_2some) / len(df_seoul_coffee)* 100))
print("빽다방 : %.3f%%" % (len(df_seoul_bbaek) / len(df_seoul_coffee)* 100))
print("할리스 : %.3f%%" % (len(df_seoul_hollys) / len(df_seoul_coffee)* 100))
print("메가커피 : %.3f%%" % (len(df_seoul_mega) / len(df_seoul_coffee)* 100))
```

</div>

<div class="output_prompt">
Out&nbsp;[30]:
</div>

{:.output_stream}

```
스타벅스 : 2.207%
이디야 : 2.049%
커피빈 : 0.705%
투썸플레이스 : 1.223%
빽다방 : 0.816%
할리스 : 0.677%
메가커피 : 2.049%

```

### 각 커피브랜드별 서울 입점 비율

<div class="in_prompt">
In&nbsp;[31]:
</div>

<div class="input_area" markdown="1">

```python
print("**** 주요 5대 커피브랜드별 서울 입점 비율 ****")
print("1. 스타벅스 : %.3f%%" % (len(df_seoul_starbucks) / len(df_starbucks) * 100))
print("2. 메가커피 : %.3f%%" % (len(df_seoul_mega) / len(df_mega) * 100))
print("3. 투썸플레이스 : %.3f%%" % (len(df_seoul_2some) / len(df_2some) * 100))
print("4. 이디야 : %.3f%%" % (len(df_seoul_ediya) / len(df_ediya) * 100))
print("5. 빽다방 : %.3f%%" % (len(df_seoul_bbaek) / len(df_bbaek) * 100))
```

</div>

<div class="output_prompt">
Out&nbsp;[31]:
</div>

{:.output_stream}

```
**** 주요 5대 커피브랜드별 서울 입점 비율 ****
1. 스타벅스 : 30.708%
2. 메가커피 : 22.994%
3. 투썸플레이스 : 22.626%
4. 이디야 : 19.796%
5. 빽다방 : 18.722%

```

<div class="in_prompt">
In&nbsp;[32]:
</div>

<div class="input_area" markdown="1">

```python
# 각 구별로 스타벅스가 얼마나 있는지 확인합니다.
starbucks_gu = df_seoul_starbucks.groupby('시군구명')['상호명'].count().to_frame().sort_values(by='상호명', ascending=False)
starbucks_gu = starbucks_gu.reset_index()
starbucks_gu = starbucks_gu.set_index('시군구명')
starbucks_gu
```

</div>

<div class="output_prompt">
Out&nbsp;[32]:
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
      <th>상호명</th>
    </tr>
    <tr>
      <th>시군구명</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>76</td>
    </tr>
    <tr>
      <th>중구</th>
      <td>42</td>
    </tr>
    <tr>
      <th>서초구</th>
      <td>37</td>
    </tr>
    <tr>
      <th>송파구</th>
      <td>33</td>
    </tr>
    <tr>
      <th>영등포구</th>
      <td>28</td>
    </tr>
    <tr>
      <th>종로구</th>
      <td>27</td>
    </tr>
    <tr>
      <th>마포구</th>
      <td>26</td>
    </tr>
    <tr>
      <th>강서구</th>
      <td>16</td>
    </tr>
    <tr>
      <th>용산구</th>
      <td>16</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>16</td>
    </tr>
    <tr>
      <th>성북구</th>
      <td>15</td>
    </tr>
    <tr>
      <th>서대문구</th>
      <td>15</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>13</td>
    </tr>
    <tr>
      <th>양천구</th>
      <td>13</td>
    </tr>
    <tr>
      <th>노원구</th>
      <td>11</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>11</td>
    </tr>
    <tr>
      <th>금천구</th>
      <td>11</td>
    </tr>
    <tr>
      <th>동작구</th>
      <td>11</td>
    </tr>
    <tr>
      <th>구로구</th>
      <td>10</td>
    </tr>
    <tr>
      <th>동대문구</th>
      <td>9</td>
    </tr>
    <tr>
      <th>은평구</th>
      <td>6</td>
    </tr>
    <tr>
      <th>중랑구</th>
      <td>6</td>
    </tr>
    <tr>
      <th>성동구</th>
      <td>5</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>5</td>
    </tr>
    <tr>
      <th>도봉구</th>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>
</div>



# 데이터 시각화

<div class="in_prompt">
In&nbsp;[33]:
</div>

<div class="input_area" markdown="1">

```python
# 시각화를 위한 라이브러리를 불러오기
import seaborn as sns
import matplotlib.pyplot as plt
import platform

from matplotlib import font_manager, rc
%matplotlib inline
```

</div>

<div class="in_prompt">
In&nbsp;[34]:
</div>

<div class="input_area" markdown="1">

```python
# macos에서 사용가능한 한글 글꼴 확인 코드
# [f.name for f in font_manager.fontManager.ttflist if 'Neo' in f.name]
```

</div>

<div class="in_prompt">
In&nbsp;[35]:
</div>

<div class="input_area" markdown="1">

```python
## 운영체제별 글꼴 세팅

path = "c:/Windows/Fonts/malgun.ttf"
if platform.system() == 'Darwin':
    font_name = 'Apple SD Gothic Neo'
    rc('font', family='Apple SD Gothic Neo')
elif platform.system() == 'Windows':
    font_name = font_manager.FontProperties(fname=path).get_name()
    rc('font', family=font_name)
else:
    font_name = font_manager.FontProperties(fname="/usr/share/fonts/nanumfont/NanumGothic.ttf")
    rc('font', family="NanumGothic")
```

</div>

<div class="in_prompt">
In&nbsp;[36]:
</div>

<div class="input_area" markdown="1">

```python
# 주요 5대 커피브랜드 서울 입점 비율을 시각화합니다.
starbucks_rate = (len(df_seoul_starbucks) / len(df_seoul_coffee) * 100)
mega_rate = (len(df_seoul_mega) / len(df_seoul_coffee)* 100)
twosome_rate = (len(df_seoul_2some) / len(df_seoul_coffee)* 100)
ediya_rate = (len(df_seoul_ediya) / len(df_seoul_coffee)* 100)
bbaek_rate = (len(df_seoul_bbaek) / len(df_seoul_coffee)* 100)

X = ["스타벅스", "메가커피", "투썸플레이스", "이디야", "빽다방"]
y = [starbucks_rate, mega_rate, twosome_rate, ediya_rate, bbaek_rate]

plt.figure(figsize=(12, 12))
plt.title("2022.09 주요 5대 커피브랜드 서울 입점 비율", fontdict={"fontsize" : 20})
plt.xticks(fontsize=16)
plt.yticks(fontsize=16)
sns.barplot(x=X, y=y)
plt.savefig("coffee_barplot.png")
plt.show()
```

</div>

<div class="output_prompt">
Out&nbsp;[36]:
</div>


    
![img]({{ '/assets/images/2022/11/11/2022-11-11-Python%20%289%29%20-%20Pandas_65_0.png' | relative_url }}){: .left-image }
    


# 추가

## 인천 연수구에 있는 커피 브랜드 별 점포수 구하기

1. 시도명에서 인천광역시, 시군구에서 연수구만 뽑기
2. 상호명이 서로 다른 데이터 셋 구성
3. 상호명에 따른 데이터 갯수 구하기
4. 데이터프레임 형태로 출력

<div class="in_prompt">
In&nbsp;[37]:
</div>

<div class="input_area" markdown="1">

```python
df_coffee_yeonsu = df_coffee[(df_coffee["시도명"] == "인천광역시") & (df_coffee["시군구명"] == "연수구")]
print("인천 연수구에 있는 커피 점포 수 : ", len(df_coffee_yeonsu))
```

</div>

<div class="output_prompt">
Out&nbsp;[37]:
</div>

{:.output_stream}

```
인천 연수구에 있는 커피 점포 수 :  748

```

<div class="in_prompt">
In&nbsp;[38]:
</div>

<div class="input_area" markdown="1">

```python
df_coffee_yeonsu_brand = set(df_coffee_yeonsu["상호명"])
print("인천 연수구에 있는 커피 브랜드 : ", len(df_coffee_yeonsu_brand))
df_coffee_yeonsu_brand
```

</div>

<div class="output_prompt">
Out&nbsp;[38]:
</div>

{:.output_stream}

```
인천 연수구에 있는 커피 브랜드 :  571

```




{:.output_data_text}

```
{'11브레드',
 '1927커피잘쓰다현대아울렛송도점',
 '5스타PC카페',
 '79파운야드',
 '88스트리트커피랩',
 '8블럭',
 'C27',
 'CAFE719',
 'CAISSON24',
 'COFFEEBAY송도',

...

 '해긴',
 '향수',
 '허브네일',
 '헤이로다',
 '헤이미쉬',
 '헤이트래블러',
 '호구커피',
 '홀린이홀덤',
 '홍루이젠',
 '화이트펜슬',
 '희차',
 '힐링카페숨소리'}
```



<div class="in_prompt">
In&nbsp;[39]:
</div>

<div class="input_area" markdown="1">

```python
# 상호명으로 그룹을 묶어 count, 내림차순으로 정렬
df_result = pd.DataFrame(df_coffee_yeonsu.groupby(["상호명"]).count()["시도명"]).sort_values(["시도명"], ascending=False)
df_result.columns = ["점포수"]
df_result.head(10)
```

</div>

<div class="output_prompt">
Out&nbsp;[39]:
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
      <th>점포수</th>
    </tr>
    <tr>
      <th>상호명</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>카페</th>
      <td>35</td>
    </tr>
    <tr>
      <th>이디야커피</th>
      <td>18</td>
    </tr>
    <tr>
      <th>컴포즈커피</th>
      <td>13</td>
    </tr>
    <tr>
      <th>더벤티</th>
      <td>12</td>
    </tr>
    <tr>
      <th>투썸플레이스</th>
      <td>11</td>
    </tr>
    <tr>
      <th>스타벅스</th>
      <td>10</td>
    </tr>
    <tr>
      <th>빽다방</th>
      <td>10</td>
    </tr>
    <tr>
      <th>메가엠지씨커피</th>
      <td>9</td>
    </tr>
    <tr>
      <th>청년다방</th>
      <td>6</td>
    </tr>
    <tr>
      <th>커피에반하다</th>
      <td>6</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[40]:
</div>

<div class="input_area" markdown="1">

```python
# 카페 row 삭제
df_result.drop(index="카페", inplace=True)
df_result.head(10)
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
      <th>점포수</th>
    </tr>
    <tr>
      <th>상호명</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>이디야커피</th>
      <td>18</td>
    </tr>
    <tr>
      <th>컴포즈커피</th>
      <td>13</td>
    </tr>
    <tr>
      <th>더벤티</th>
      <td>12</td>
    </tr>
    <tr>
      <th>투썸플레이스</th>
      <td>11</td>
    </tr>
    <tr>
      <th>스타벅스</th>
      <td>10</td>
    </tr>
    <tr>
      <th>빽다방</th>
      <td>10</td>
    </tr>
    <tr>
      <th>메가엠지씨커피</th>
      <td>9</td>
    </tr>
    <tr>
      <th>청년다방</th>
      <td>6</td>
    </tr>
    <tr>
      <th>커피에반하다</th>
      <td>6</td>
    </tr>
    <tr>
      <th>팔공티</th>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>
</div>



## 서울 광진구에 있는 커피 브랜드 별 점포수 구하기

<div class="in_prompt">
In&nbsp;[41]:
</div>

<div class="input_area" markdown="1">

```python
df_coffee_gwangjin = df_seoul_coffee[df_seoul_coffee["시군구명"] == "광진구"]
print("서울 광진구에 있는 커피 점포 수 : ", len(df_coffee_gwangjin))
```

</div>

<div class="output_prompt">
Out&nbsp;[41]:
</div>

{:.output_stream}

```
서울 광진구에 있는 커피 점포 수 :  837

```

<div class="in_prompt">
In&nbsp;[42]:
</div>

<div class="input_area" markdown="1">

```python
df_coffee_gwangjin_brand = set(df_coffee_gwangjin["상호명"])
print("인천 연수구에 있는 커피 브랜드 : ", len(df_coffee_gwangjin_brand))
df_coffee_gwangjin_brand
```

</div>

<div class="output_prompt">
Out&nbsp;[42]:
</div>

{:.output_stream}

```
인천 연수구에 있는 커피 브랜드 :  717

```




{:.output_data_text}

```
{'19티',
 '247커피',
 '29-77카페',
 '550커피',
 '597카페',
 '87커피로스터',
 '8커피앤비어',
 '99도버블티',
 'AZUR아쥐르',
 'BANANATALK',

...

 '홍과자점',
 '홍카페',
 '화양과자',
 '화양연화',
 '화이트펜슬스터디카페',
 '휴온',
 '흐룻',
 '히어로',
 '히읗히읗',
 '힐링카페지유'}
```



<div class="in_prompt">
In&nbsp;[43]:
</div>

<div class="input_area" markdown="1">

```python
# 상호명으로 그룹을 묶어 count, 내림차순으로 정렬
df_result = pd.DataFrame(df_coffee_gwangjin.groupby(["상호명"]).count()["시도명"]).sort_values(["시도명"], ascending=False)
df_result.columns = ["점포수"]
df_result.head(10)
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
      <th>점포수</th>
    </tr>
    <tr>
      <th>상호명</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>카페</th>
      <td>29</td>
    </tr>
    <tr>
      <th>스타벅스</th>
      <td>13</td>
    </tr>
    <tr>
      <th>이디야커피</th>
      <td>12</td>
    </tr>
    <tr>
      <th>컴포즈커피</th>
      <td>10</td>
    </tr>
    <tr>
      <th>커피베이</th>
      <td>7</td>
    </tr>
    <tr>
      <th>빽다방</th>
      <td>7</td>
    </tr>
    <tr>
      <th>메가엠지씨커피</th>
      <td>6</td>
    </tr>
    <tr>
      <th>투썸플레이스</th>
      <td>6</td>
    </tr>
    <tr>
      <th>커피나무</th>
      <td>4</td>
    </tr>
    <tr>
      <th>디저트39</th>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>
</div>



<div class="in_prompt">
In&nbsp;[44]:
</div>

<div class="input_area" markdown="1">

```python
# 카페 row 삭제
df_result.drop(index="카페", inplace=True)
df_result.head(10)
```

</div>

<div class="output_prompt">
Out&nbsp;[44]:
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
      <th>점포수</th>
    </tr>
    <tr>
      <th>상호명</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>스타벅스</th>
      <td>13</td>
    </tr>
    <tr>
      <th>이디야커피</th>
      <td>12</td>
    </tr>
    <tr>
      <th>컴포즈커피</th>
      <td>10</td>
    </tr>
    <tr>
      <th>커피베이</th>
      <td>7</td>
    </tr>
    <tr>
      <th>빽다방</th>
      <td>7</td>
    </tr>
    <tr>
      <th>메가엠지씨커피</th>
      <td>6</td>
    </tr>
    <tr>
      <th>투썸플레이스</th>
      <td>6</td>
    </tr>
    <tr>
      <th>커피나무</th>
      <td>4</td>
    </tr>
    <tr>
      <th>디저트39</th>
      <td>4</td>
    </tr>
    <tr>
      <th>탐앤탐스</th>
      <td>4</td>
    </tr>
  </tbody>
</table>
</div>
</div>


