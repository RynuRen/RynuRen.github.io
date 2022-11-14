---
layout: jupyter
title: Python (10) - Data Crawling
published: true
date: 2022-11-11
description: 데이터 크롤링
comments: true
categories:
 - python
tags:
 - python
 - numpy
 - pandas
 - crawling
 - beautifulsoup
toc: true
toc_sticky: true
toc_label: Data Crawling
---
---
# 데이터 크롤링 기본

## BeautifulSoup 를 활용한 네이버 웹툰의 인기급상승 크롤링

![img]({{ '/assets/images/2022/11/11/img1.PNG' | relative_url }}){: .left-image }

## BeautifulSoup을 사용하는 이유

- 배우기 쉬워, 입문하기 좋음
- 속도가 빠름
- html을 파싱하기 편함

## 크롤링에서 BeautifulSoup을 사용하는 경우

* 해당 웹사이트가 정적인 페이지인 경우
* 로그인, 버튼 클릭등 페이지내에 동작이 필요하지 않은경우

<div class="in_prompt">
In&nbsp;[1]:
</div>

<div class="input_area" markdown="1">

```python
# API 통신을 할 때 사용하는 라이브러리
import requests
# 가져온 페이지의 html을 파싱하는 라이브러리
from bs4 import BeautifulSoup
```

</div>

<div class="in_prompt">
In&nbsp;[2]:
</div>

<div class="input_area" markdown="1">

```python
url = "https://comic.naver.com/webtoon/weekday"
# 지정한 url로 페이지를 가져오는 요청을 생성함
res = requests.get(url)
res.raise_for_status()
# 요청을 통해 가져온 페이지를 BeautifulSoup로 파싱
# lxml : html 및 xml의 파싱을 간편하게 도와주는 라이브러리
soup = BeautifulSoup(res.text, "lxml")
```

</div>

<div class="in_prompt">
In&nbsp;[3]:
</div>

<div class="input_area" markdown="1">

```python
# BeautifulSoup 객체의 자료형 출력
print(type(soup))
```

</div>

<div class="output_prompt">
Out&nbsp;[3]:
</div>

{:.output_stream}

```
<class 'bs4.BeautifulSoup'>

```

<div class="in_prompt">
In&nbsp;[4]:
</div>

<div class="input_area" markdown="1">

```python
# HTML 문서의 'head' 태그에 해당하는 내용 출력
print(soup.head)
```

</div>

<div class="output_prompt">
Out&nbsp;[4]:
</div>

{:.output_stream}

```
<head>
<meta content="IE=edge,chrome=1" http-equiv="X-UA-Compatible"/>
<meta content="text/html; charset=utf-8" http-equiv="Content-type"/>
<title>네이버 웹툰 &gt; 요일별  웹툰 &gt; 전체웹툰</title>
<meta content="네이버 웹툰" property="og:title"/>
<meta content="https://ssl.pstatic.net/static/comic/images/og_tag_v2.png" property="og:image"/>
<meta content="매일매일 새로운 재미, 네이버 웹툰." property="og:description"/>
<meta content="https://comic.naver.com/webtoon/weekday" property="og:url"/>
<meta content="article" property="og:type"/>
<meta content="네이버 웹툰" property="og:article:author"/>
<meta content="https://comic.naver.com" property="og:article:author:url"/>
<link href="https://ssl.pstatic.net/static/comic/favicon/webtoon_favicon_32x32.ico" rel="shortcut icon" type="image/x-icon"/>
<link href="/css/comic/comic_20221109182113.css" rel="stylesheet" style="text/css"/>
<!-- comicWeekdayJsFile -->
<script charset="utf-8" src="/aggregate/javascript/release/comic_weekday_20221109182113.js" type="text/javascript"></script>
<script type="text/javascript">
	//<![CDATA[
	var g_ssc = "comic.webtoon";
	var ccsrv = "cc.naver.com";
	//]]>

</script>
<script src="/aggregate/javascript/release/comic_gnb_20221109182113.js" type="text/javascript"></script>
</head>

```

<div class="in_prompt">
In&nbsp;[5]:
</div>

<div class="input_area" markdown="1">

```python
# HTML 문서의 'body' 태그에 해당하는 내용
print("------soup.body----------")
print(soup.body)
# HTML 문서의 'title' 태그에 해당하는 내용
print("------soup.title----------")
print(soup.title)
# HTML 문서의 'title' 태그명에 해당하는 내용
print("------soup.title.name----------")
print(soup.title.name)
# HTML 문서의 'title' 태그 내부의 글자 해당하는 내용
print("------soup.title.string----------")
print(soup.title.string)
```

</div>

<div class="output_prompt">
Out&nbsp;[5]:
</div>

{:.output_stream}

```
------soup.body----------
<body onload="">
<div class="end_page" id="wrap">
<div id="u_skip">
<a href="#menu" onclick="document.getElementById('menu').tabIndex=-1;document.getElementById('menu').focus();return false;"><span>메인 메뉴로 바로가기</span></a>
<a href="#content" onclick="document.getElementById('content').tabIndex=-1;document.getElementById('content').focus();return false;"><span>본문으로 바로가기</span></a>
</div>
<div id="header_wrap">
<div id="header">
<div id="gnb">
<script type="text/javascript">
        var gnb_option = {
            gnb_service : "comic",
            gnb_logout :  encodeURIComponent(window.location.href),
            gnb_login :   encodeURIComponent(window.location.href),
            gnb_template : "gnb_utf8",
            gnb_brightness : 1,
            gnb_item_hide_option: 0
        };
    </script>
<script charset="utf-8" src="https://ssl.pstatic.net/static.gn/templates/gnb_utf8?20221109182113" type="text/javascript"></script>
				
...
				
<script src="/aggregate/javascript/release/lcslog_20221109182113.js" type="text/javascript"></script>
<script type="text/javascript">
    try {
        var eventType = "onpageshow" in window ? "onpageshow" : "onload";
        var oldEvent = window[eventType];
        window[eventType] = function() {
            if (oldEvent) {
                oldEvent();
            }
            lcs_do();
        };
    } catch(ex) { }
</script>
</body>
------soup.title----------
<title>네이버 웹툰 &gt; 요일별  웹툰 &gt; 전체웹툰</title>
------soup.title.name----------
title
------soup.title.string----------
네이버 웹툰 > 요일별  웹툰 > 전체웹툰

```

<div class="in_prompt">
In&nbsp;[6]:
</div>

<div class="input_area" markdown="1">

```python
# ol 태그중에 class 명이 asideBoxRank인 html요소들을 가져옴
webtoonBox = soup.find("ol", attrs={"class": "asideBoxRank"})
print("--------webtoonBox--------")
print(webtoonBox)
# 추출한 html요소들 중 a태그에 해당되는 내용을 전부 가져옴
webtoons = webtoonBox.find_all("a")
print("--------webtoons--------")
print(webtoons)
```

</div>

<div class="output_prompt">
Out&nbsp;[6]:
</div>

{:.output_stream}

```
--------webtoonBox--------
<ol class="asideBoxRank" id="realTimeRankFavorite">
<li class="rank01">
<a href="/webtoon/detail?titleId=736277&amp;no=157" onclick="nclk_v2(event,'rnk*p.cont','736277','1')" title="싸움독학-153화 : 잡았다 이새끼들">싸움독학-153화 : 잡았다 이새끼들</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
<li class="rank02">
<a href="/webtoon/detail?titleId=758150&amp;no=106" onclick="nclk_v2(event,'rnk*p.cont','758150','2')" title="입학용병-105화">입학용병-105화</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
<li class="rank03">
<a href="/webtoon/detail?titleId=795262&amp;no=25" onclick="nclk_v2(event,'rnk*p.cont','795262','3')" title="사형소년-25화_미친개들">사형소년-25화_미친개들</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
<li class="rank04">
<a href="/webtoon/detail?titleId=710751&amp;no=217" onclick="nclk_v2(event,'rnk*p.cont','710751','4')" title="약한영웅-216화">약한영웅-216화</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
<li class="rank05">
<a href="/webtoon/detail?titleId=798664&amp;no=12" onclick="nclk_v2(event,'rnk*p.cont','798664','5')" title="자매전쟁-12 진짜는 누구 (4)">자매전쟁-12 진짜는 누구 (4)</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
<li class="rank06">
<a href="/webtoon/detail?titleId=785251&amp;no=51" onclick="nclk_v2(event,'rnk*p.cont','785251','6')" title="시월드가 내게 집착한다-50화">시월드가 내게 집착한다-50화</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
<li class="rank07">
<a href="/webtoon/detail?titleId=761104&amp;no=104" onclick="nclk_v2(event,'rnk*p.cont','761104','7')" title="곱게 키웠더니, 짐승-외전 11화">곱게 키웠더니, 짐승-외전 11화</a>
<span class="rankBox">
<img alt="순위상승" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_up.gif" title="순위상승" width="7"/>1
						
						
					
				</span>
</li>
<li class="rank08">
<a href="/webtoon/detail?titleId=774831&amp;no=72" onclick="nclk_v2(event,'rnk*p.cont','774831','8')" title="수희0(tngmlek0)-72화">수희0(tngmlek0)-72화</a>
<span class="rankBox">
<img alt="순위하락" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_down.gif" title="순위하락" width="7"/>1
						
						
						
					
				</span>
</li>
<li class="rank09">
<a href="/webtoon/detail?titleId=783524&amp;no=38" onclick="nclk_v2(event,'rnk*p.cont','783524','9')" title="고백 취소도 되나?-37화">고백 취소도 되나?-37화</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
<li class="rank10">
<a href="/webtoon/detail?titleId=799805&amp;no=12" onclick="nclk_v2(event,'rnk*p.cont','799805','10')" title="분신으로 자동사냥-12화 우장산 이글아이 (2)">분신으로 자동사냥-12화 우장산 이글아이 (2)</a>
<span class="rankBox">
<img alt="변동없음" height="10" src="https://ssl.pstatic.net/static/comic/images/migration/common/arrow_no.gif" title="변동없음" width="7"/> 0
						
					
				</span>
</li>
</ol>
--------webtoons--------
[<a href="/webtoon/detail?titleId=736277&amp;no=157" onclick="nclk_v2(event,'rnk*p.cont','736277','1')" title="싸움독학-153화 : 잡았다 이새끼들">싸움독학-153화 : 잡았다 이새끼들</a>, <a href="/webtoon/detail?titleId=758150&amp;no=106" onclick="nclk_v2(event,'rnk*p.cont','758150','2')" title="입학용병-105화">입학용병-105화</a>, <a href="/webtoon/detail?titleId=795262&amp;no=25" onclick="nclk_v2(event,'rnk*p.cont','795262','3')" title="사형소년-25화_미친개들">사형소년-25화_미친개들</a>, <a href="/webtoon/detail?titleId=710751&amp;no=217" onclick="nclk_v2(event,'rnk*p.cont','710751','4')" title="약한영웅-216화">약한영웅-216화</a>, <a href="/webtoon/detail?titleId=798664&amp;no=12" onclick="nclk_v2(event,'rnk*p.cont','798664','5')" title="자매전쟁-12 진짜는 누구 (4)">자매전쟁-12 진짜는 누구 (4)</a>, <a href="/webtoon/detail?titleId=785251&amp;no=51" onclick="nclk_v2(event,'rnk*p.cont','785251','6')" title="시월드가 내게 집착한다-50화">시월드가 내게 집착한다-50화</a>, <a href="/webtoon/detail?titleId=761104&amp;no=104" onclick="nclk_v2(event,'rnk*p.cont','761104','7')" title="곱게 키웠더니, 짐승-외전 11화">곱게 키웠더니, 짐승-외전 11화</a>, <a href="/webtoon/detail?titleId=774831&amp;no=72" onclick="nclk_v2(event,'rnk*p.cont','774831','8')" title="수희0(tngmlek0)-72화">수희0(tngmlek0)-72화</a>, <a href="/webtoon/detail?titleId=783524&amp;no=38" onclick="nclk_v2(event,'rnk*p.cont','783524','9')" title="고백 취소도 되나?-37화">고백 취소도 되나?-37화</a>, <a href="/webtoon/detail?titleId=799805&amp;no=12" onclick="nclk_v2(event,'rnk*p.cont','799805','10')" title="분신으로 자동사냥-12화 우장산 이글아이 (2)">분신으로 자동사냥-12화 우장산 이글아이 (2)</a>]

```

<div class="in_prompt">
In&nbsp;[7]:
</div>

<div class="input_area" markdown="1">

```python
# enumerate() 함수는 기본적으로 for문 및 반복문들의 실행 시 (idx, element) 형식으로 이루어진 튜플을 만들어줌
for test in enumerate(webtoons):
    print(test)

# a 태그를 가져온 리스트 중 title만 뽑아서 새로 리스트화
webtoonRankList1 = [(str(idx), webtoon.get("title"))
                    for (idx, webtoon) in enumerate(webtoons, start=1)]
print(webtoonRankList1)

# a 태그를 가져온 리스트 중 title만 뽑아서 새로 리스트화
webtoonRankList = []
for (idx, webtoon) in enumerate(webtoons, start=1):
    title = webtoon.get("title")
    print(f"#{str(idx)}: {title}")
    data = [idx, title.split('-')[0]]
    webtoonRankList.append(data)
print(webtoonRankList)
```

</div>

<div class="output_prompt">
Out&nbsp;[7]:
</div>

{:.output_stream}

```
(0, <a href="/webtoon/detail?titleId=736277&amp;no=157" onclick="nclk_v2(event,'rnk*p.cont','736277','1')" title="싸움독학-153화 : 잡았다 이새끼들">싸움독학-153화 : 잡았다 이새끼들</a>)
(1, <a href="/webtoon/detail?titleId=758150&amp;no=106" onclick="nclk_v2(event,'rnk*p.cont','758150','2')" title="입학용병-105화">입학용병-105화</a>)
(2, <a href="/webtoon/detail?titleId=795262&amp;no=25" onclick="nclk_v2(event,'rnk*p.cont','795262','3')" title="사형소년-25화_미친개들">사형소년-25화_미친개들</a>)
(3, <a href="/webtoon/detail?titleId=710751&amp;no=217" onclick="nclk_v2(event,'rnk*p.cont','710751','4')" title="약한영웅-216화">약한영웅-216화</a>)
(4, <a href="/webtoon/detail?titleId=798664&amp;no=12" onclick="nclk_v2(event,'rnk*p.cont','798664','5')" title="자매전쟁-12 진짜는 누구 (4)">자매전쟁-12 진짜는 누구 (4)</a>)
(5, <a href="/webtoon/detail?titleId=785251&amp;no=51" onclick="nclk_v2(event,'rnk*p.cont','785251','6')" title="시월드가 내게 집착한다-50화">시월드가 내게 집착한다-50화</a>)
(6, <a href="/webtoon/detail?titleId=761104&amp;no=104" onclick="nclk_v2(event,'rnk*p.cont','761104','7')" title="곱게 키웠더니, 짐승-외전 11화">곱게 키웠더니, 짐승-외전 11화</a>)
(7, <a href="/webtoon/detail?titleId=774831&amp;no=72" onclick="nclk_v2(event,'rnk*p.cont','774831','8')" title="수희0(tngmlek0)-72화">수희0(tngmlek0)-72화</a>)
(8, <a href="/webtoon/detail?titleId=783524&amp;no=38" onclick="nclk_v2(event,'rnk*p.cont','783524','9')" title="고백 취소도 되나?-37화">고백 취소도 되나?-37화</a>)
(9, <a href="/webtoon/detail?titleId=799805&amp;no=12" onclick="nclk_v2(event,'rnk*p.cont','799805','10')" title="분신으로 자동사냥-12화 우장산 이글아이 (2)">분신으로 자동사냥-12화 우장산 이글아이 (2)</a>)
[('1', '싸움독학-153화 : 잡았다 이새끼들'), ('2', '입학용병-105화'), ('3', '사형소년-25화_미친개들'), ('4', '약한영웅-216화'), ('5', '자매전쟁-12 진짜는 누구 (4)'), ('6', '시월드가 내게 집착한다-50화'), ('7', '곱게 키웠더니, 짐승-외전 11화'), ('8', '수희0(tngmlek0)-72화'), ('9', '고백 취소도 되나?-37화'), ('10', '분신으로 자동사냥-12화 우장산 이글아이 (2)')]
#1: 싸움독학-153화 : 잡았다 이새끼들
#2: 입학용병-105화
#3: 사형소년-25화_미친개들
#4: 약한영웅-216화
#5: 자매전쟁-12 진짜는 누구 (4)
#6: 시월드가 내게 집착한다-50화
#7: 곱게 키웠더니, 짐승-외전 11화
#8: 수희0(tngmlek0)-72화
#9: 고백 취소도 되나?-37화
#10: 분신으로 자동사냥-12화 우장산 이글아이 (2)
[[1, '싸움독학'], [2, '입학용병'], [3, '사형소년'], [4, '약한영웅'], [5, '자매전쟁'], [6, '시월드가 내게 집착한다'], [7, '곱게 키웠더니, 짐승'], [8, '수희0(tngmlek0)'], [9, '고백 취소도 되나?'], [10, '분신으로 자동사냥']]

```

<div class="in_prompt">
In&nbsp;[8]:
</div>

<div class="input_area" markdown="1">

```python
import pandas as pd
# 데이터 프레임화 
pd.DataFrame(webtoonRankList, columns=["ranks", "title"])
# pd.DataFrame(webtoonRankList1, columns=["ranks", "title"])
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
      <th>ranks</th>
      <th>title</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>싸움독학</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>입학용병</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>사형소년</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>약한영웅</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>자매전쟁</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>시월드가 내게 집착한다</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>곱게 키웠더니, 짐승</td>
    </tr>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>수희0(tngmlek0)</td>
    </tr>
    <tr>
      <th>8</th>
      <td>9</td>
      <td>고백 취소도 되나?</td>
    </tr>
    <tr>
      <th>9</th>
      <td>10</td>
      <td>분신으로 자동사냥</td>
    </tr>
  </tbody>
</table>
</div>
</div>



# 응용해보기

## BeautifulSoup 를 활용하여 요일별 전체웹툰 크롤링

<div class="in_prompt">
In&nbsp;[9]:
</div>

<div class="input_area" markdown="1">

```python
url2 = "https://comic.naver.com/webtoon/weekday"
res2 = requests.get(url2)
res2.raise_for_status()

soup2 = BeautifulSoup(res2.text, "lxml")
```

</div>

<div class="in_prompt">
In&nbsp;[10]:
</div>

<div class="input_area" markdown="1">

```python
webtoonByDayClass = soup2.find("div", attrs={"class": "list_area daily_all"})

webtoonByDayImg = webtoonByDayClass.find_all('img')
webtoonByDayList = [(str(idx), webtoon.get("title")) for (idx, webtoon) in enumerate(webtoonByDayImg, start=1)]
print(webtoonByDayList)
```

</div>

<div class="output_prompt">
Out&nbsp;[10]:
</div>

{:.output_stream}

```
[('1', '참교육'), ('2', '뷰티풀 군바리'), ('3', '퀘스트지상주의'), ('4', '퍼니게임'), ('5', '장씨세가 호위무사'), ('6', '윈드브레이커'), ('7', '팔이피플'), ('8', '신화급 귀속 아이템을 손에 넣었다'), ('9', '버림받은 왕녀의 은밀한 침실'), ('10', '앵무살수'), ('11', '호랑신랑뎐'), ('12', '잔불의 기사'), ('13', '소녀의 세계'), ('14', '똑 닮은 딸'), ('15', '절대검감'), ('16', '인섹터'), ('17', '사이다걸'), ('18', '백수세끼'), ('19', '히어로메이커'), ('20', '리턴 투 플레이어'), ('21', '물어보는 사이'), ('22', '루루라라 우리네 인생'), ('23', '랭커'), ('24', '꼬리잡기'), ('25', '북부 공작님을 유혹하겠습니다'), ('26', '이별 후 사내 결혼'), ('27', '순정말고 순종'), ('28', '결혼생활 그림일기'), ('29', '또다시, 계약 부부'), ('30', '더블클릭'), ('31', '아, 쫌 참으세요 영주님!'), ('32', '헬크래프트'), ('33', '황제와의 하룻밤'), ('34', '수영만화일기'), ('35', '오빠집이 비어서'), ('36', '제왕: 빛과 그림자'), ('37', '퇴근 후에 만나요'), ('38', '꿈의 기업'), ('39', '도깨비 부른다'), ('40', '불청객'), ('41', '다시쓰는 연애사'), ('42', '파운더'), ('43', '미니어처 생활백서'), ('44', '이제야 연애'), ('45', '집사, 주세요!'), ('46', '우산 없는 애'), ('47', '하루의 하루'), ('48', '아슈타르테'), ('49', '원작은 완결난 지 한참 됐습니다만'), ('50', '최후의 금빛아이'), ('51', '오늘의 비너스'), ('52', '굿바이 유교보이'), ('53', '그림자 신부'), ('54', '입술이 예쁜 남자'), ('55', '메리의 불타는 행복회로'), ('56', '신군'), ('57', '레지나레나 - 용서받지 못한 그대에게'), ('58', '버그이터'), ('59', '백호랑'), ('60', '파견체'), ('61', '모스크바의 여명'), ('62', '경비실에서 안내방송 드립니다'), ('63', '루크 비셸 따라잡기'), ('64', '아마도'), ('65', '나만의 고막남친'), ('66', '악녀 18세 공략기'), ('67', '매지컬 급식:암살법사'), ('68', '지옥연애환담'), ('69', '세번째 로망스'), ('70', '행운을 부탁해!'), ('71', '모노마니아'), ('72', '역주행!'), ('73', '슈퍼스타 천대리'), ('74', '사막에 핀 달'), ('75', '흔들리는 세계로부터'), ('76', '결혼공략'), ('77', '달로 만든 아이'), ('78', '디나운스'), ('79', '별을 쫓는 소년들'), ('80', '오로지 오로라'), ('81', '김부장'), ('82', '대학원 탈출일지'), ('83', '내가 키운 S급들'), ('84', '여신강림'), ('85', '마루는 강쥐'), ('86', '1을 줄게'), ('87', '멸망 이후의 세계'), ('88', '하루만 네가 되고 싶어'), ('89', '놓지마 정신줄 시즌3'), ('90', '중증외상센터 : 골든 아워'), ('91', '용사가 돌아왔다'), ('92', '삼국지톡'), ('93', '헬58'), ('94', '악몽의 형상'), ('95', '천마는 평범하게 살 수 없다'), ('96', '집이 없어'), ('97', '하북팽가 막내아들'), ('98', '랜덤채팅의 그녀!'), ('99', '호랑이 들어와요'), ('100', '시체기사 군터'), ('101', '올가미'), ('102', '초인의 게임'), ('103', '햄버거가 제일 좋아'), ('104', '원주민 공포만화'), ('105', '시한부인 줄 알았어요!'), ('106', '윌유메리미'), ('107', '나타나주세요!'), ('108', '웅크'), ('109', '덴큐'), ('110', '애옹식당'), ('111', '사표내고 이계에서 힐링합니다'), ('112', '위아더좀비'), ('113', '늑대처럼 홀로'), ('114', '용왕님의 셰프가 되었습니다'), ('115', '택배 왔습니다'), ('116', '왕게임'), ('117', '견우와 선녀'), ('118', '헥토파스칼'), ('119', '로잘린 보가트'), ('120', '빌런투킬'), ('121', '빅맨'), ('122', '에이머'), ('123', '은주의 방 2~3부'), ('124', '플레이, 플리'), ('125', '아이즈'), ('126', '또 다른 사랑'), ('127', '내남친 킹카만들기'), ('128', '나의 플랏메이트'), ('129', '한입만!'), ('130', '제로게임'), ('131', '사공은주'), ('132', '열녀박씨 계약결혼뎐'), ('133', '여우자매'), ('134', '1331'), ('135', '붉은 이정표'), ('136', '풋내기들'), ('137', '쿠쿠쿠쿠'), ('138', '필리아로제 - 가시왕관의 예언'), ('139', '주인님을 잡아먹는 방법'), ('140', '짝사랑의 마침표'), ('141', '벽간소음'), ('142', '이 결혼, 새로고침'), ('143', 'NG불가'), ('144', '이븐 모어'), ('145', '나는 어디에나 있다'), ('146', '테러사이트'), ('147', '그 남주와 이별하는 방법'), ('148', '마침내 사랑이에요 마왕님!'), ('149', '그녀석 정복기'), ('150', 'AI 유하'), ('151', '대신 심부름을 해다오'), ('152', '원수가 나를 유혹할 때'), ('153', '교환학생'), ('154', '태시트'), ('155', '급식러너'), ('156', '달이 사라진 밤'), ('157', '하나in세인'), ('158', '잿빛오름'), ('159', '찐:종합게임동아리'), ('160', '너의 순정, 나의 순정'), ('161', '마녀이야기'), ('162', '지금 우리 연구소는'), ('163', '내 남편과 결혼해줘'), ('164', '전지적 독자 시점'), ('165', '헬퍼 2 : 킬베로스'), ('166', '먹는 인생'), ('167', '격기3반'), ('168', '일렉시드'), ('169', '비즈니스 여친'), ('170', '무림서부'), ('171', '여고생 드래곤'), ('172', '급식아빠'), ('173', '튜토리얼 탑의 고인물'), ('174', '내가 죽기로 결심한 것은'), ('175', '나쁜사람'), ('176', '이십팔세기 광팬'), ('177', '연놈'), ('178', '빌드업'), ('179', '마른 가지에 바람처럼'), ('180', '미래의 골동품 가게'), ('181', '칼에 취한 밤을 걷다'), ('182', '안녕, 나의 수집'), ('183', '사상최강'), ('184', '고삼무쌍'), ('185', '쓰레기는 쓰레기통에!'), ('186', '베스트 프렌드'), ('187', '세상은 돈과 권력'), ('188', '운명을 보는 회사원'), ('189', '고교흥신소'), ('190', '사내고충처리반'), ('191', '아도나이'), ('192', '판타지 여동생!'), ('193', '물고기로 살아남기'), ('194', '하렘의 남자들'), ('195', '언덕 위의 제임스'), ('196', '가짜 동맹'), ('197', '무용과 남학생'), ('198', '하얀 사자의 비밀 신부'), ('199', '수요웹툰의 나강림'), ('200', '마녀와 용의 신혼일기'), ('201', '얼굴천재'), ('202', '칼부림'), ('203', '괴물공작의 딸'), ('204', '로어 올림푸스'), ('205', '영웅&마왕&악당'), ('206', '헤어지면 죽음'), ('207', '그 남자의 은밀한 하루'), ('208', '반귀'), ('209', '주부 육성중'), ('210', '인과관계'), ('211', '연애고수'), ('212', '좀간'), ('213', '두 번 사는 프로듀서'), ('214', '범이올시다!'), ('215', '밀행'), ('216', '선배는 나빠요!'), ('217', '홀리데이'), ('218', '옆집남자 친구'), ('219', '그림자의 밤'), ('220', '내겐 너무 소란한 결혼'), ('221', '안미운 우리들'), ('222', '메모리얼'), ('223', '잿빛도 색이다'), ('224', '나의 계절'), ('225', '사랑과 평강의 온달!'), ('226', '구사일생 로맨스'), ('227', '재생존경쟁'), ('228', '컨트롤'), ('229', '멸종위기종인간'), ('230', '어느 백작 영애의 이중생활'), ('231', '반짝반짝 작은 눈'), ('232', '이별학'), ('233', '클로닝'), ('234', '해귀'), ('235', '혁명 뒤 공주는'), ('236', '하지만 너는 2D잖아'), ('237', '낙원의 이론'), ('238', '수호하는 너에게'), ('239', '프로듀스 온리원'), ('240', '이 짝사랑은 억울하다!'), ('241', '필생기'), ('242', '오직, 밝은 미래'), ('243', '우리 은하'), ('244', '산의 시간'), ('245', '아이돌의 비밀 스터디'), ('246', '연애혁명'), ('247', '나노마신'), ('248', '2022 몰래보는 로맨스'), ('249', '남편을 죽여줘요'), ('250', '천마육성'), ('251', '현실퀘스트'), ('252', '호랑신랑뎐'), ('253', '나 혼자 네크로맨서'), ('254', '뮤즈 온 유명'), ('255', '무사만리행'), ('256', '재벌집 막내아들'), ('257', '회귀한 천재 헌터의 슬기로운 청소생활'), ('258', '이상한 변호사 우영우'), ('259', '별을 삼킨 너에게'), ('260', '노답소녀'), ('261', '버려진 나의 최애를 위하여'), ('262', '비서 일탈'), ('263', '루루라라 우리네 인생'), ('264', '쿠베라'), ('265', '순정빌런'), ('266', '던전 씹어먹는 아티팩트'), ('267', '최강전설 강해효'), ('268', '꽃만 키우는데 너무강함'), ('269', '트롤트랩'), ('270', '게임 최강 트롤러'), ('271', '코인 리벤지'), ('272', '국세청 망나니'), ('273', '완벽한 결혼의 정석'), ('274', '누나! 나 무서워'), ('275', '규격 외 혈통 천재'), ('276', '마왕까지 한 걸음'), ('277', '만능잡캐'), ('278', '순수한 동거생활'), ('279', '네가 죽기를 바랄 때가 있었다'), ('280', '수영만화일기'), ('281', '선남친 후연애'), ('282', '황제사냥'), ('283', '독거미'), ('284', '아인슈페너'), ('285', '킬더킹'), ('286', '위대한 겸상'), ('287', '관계중독'), ('288', '가족같은 XX'), ('289', '개와 사람의 시간'), ('290', '소녀180'), ('291', '오빠세끼'), ('292', '진짜 진짜 이혼해'), ('293', '만물의 영장'), ('294', '베니루 BAENIRU'), ('295', '두 번째 딸로 태어났습니다'), ('296', '1학년 9반'), ('297', '달의 요람'), ('298', '천재의 게임방송'), ('299', 'THE 런웨이'), ('300', '나의 불편한 상사'), ('301', '오, 친애하는 숙적'), ('302', '마녀의 심판은 꽃이 된다'), ('303', '환상연가'), ('304', '부캐인생'), ('305', '황궁에 핀 꽃은, 미쳤다'), ('306', '따개비'), ('307', '방과후 레시피'), ('308', '그 황제가 시곗바늘을 되돌린 사연'), ('309', '이계 무슨 황비'), ('310', '베어케어'), ('311', '아빠같은 남자'), ('312', '자취방 신선들'), ('313', '가상&RPG'), ('314', '온새미로'), ('315', '온실 속 화초'), ('316', '시에라'), ('317', '돌아온 여기사'), ('318', '카루나'), ('319', '권리행사자'), ('320', '배트맨: 웨인 패밀리 어드벤처'), ('321', '로맨틱 태평수산'), ('322', '나쁜 마법사의 꿈'), ('323', '연애의 발견'), ('324', '널 사랑하는 죽은 형'), ('325', '보물과 괴물의 도시'), ('326', '유월의 소한'), ('327', '사랑하는 여배우들'), ('328', '모어 라이프'), ('329', '혼모노트'), ('330', '외모지상주의'), ('331', '대학원 탈출일지'), ('332', '광마회귀'), ('333', '나 혼자 만렙 뉴비'), ('334', '어쩌다보니 천생연분'), ('335', '역대급 영지 설계사'), ('336', '데드퀸'), ('337', '개를 낳았다'), ('338', '1초'), ('339', '세기말 풋사과 보습학원'), ('340', '언다잉'), ('341', '삼국지톡'), ('342', '상남자'), ('343', '낙향문사전'), ('344', '언니, 이번 생엔 내가 왕비야'), ('345', '사신'), ('346', '서울역 드루이드'), ('347', '천하제일 대사형'), ('348', '소름일기'), ('349', '말년용사'), ('350', '히어로 킬러'), ('351', '뜨거운 홍차'), ('352', 'A.I. 닥터'), ('353', '그렇고 그런 바람에'), ('354', '더 게이머'), ('355', '웅크'), ('356', '지옥 키우기'), ('357', '폭군님은 착하게 살고 싶어'), ('358', '온리호프'), ('359', '여자를 사귀고 싶다'), ('360', '플레이어'), ('361', '푸쉬오프'), ('362', '성스러운 그대 이르시길'), ('363', '이터널 포스'), ('364', '하나는 적고 둘은 너무 많아'), ('365', '기억해줘'), ('366', '너의 미소가 함정'), ('367', '선을 넘은 연애'), ('368', '후덜덜덜 남극전자'), ('369', '킬 더 드래곤'), ('370', '문제적 왕자님'), ('371', '스포'), ('372', '절대복종'), ('373', '세라는 망돌'), ('374', '멜빈이 그들에게 남긴 것'), ('375', '달이 없는 나라'), ('376', '메소드 연기법'), ('377', '상위1%'), ('378', '여름여자 하보이'), ('379', '네버엔딩달링'), ('380', '몽홀'), ('381', '솔트앤페퍼'), ('382', '로또 황녀님'), ('383', '스토커의 하루'), ('384', '쿠쿠쿠쿠'), ('385', '백년게임'), ('386', '트럼프'), ('387', '악마라고 불러다오'), ('388', '지니오패스'), ('389', '아찔한 전남편'), ('390', '미친 후작을 길들이고 말았다'), ('391', '이게 아닌데'), ('392', '인피니티'), ('393', '재앙의 날'), ('394', '태권보이'), ('395', '미드우트'), ('396', '비밀친구'), ('397', '드래곤의 심장을 가지고 있습니다'), ('398', '짝사랑 마들렌'), ('399', '별빛 커튼콜'), ('400', '천상의 주인'), ('401', '합법해적 파르페'), ('402', '여름의 너에게'), ('403', '너의 키스씬'), ('404', '스치면 인연 스며들면 사랑'), ('405', '팬시X팬시'), ('406', '서울시 천사주의'), ('407', '너에게 입덕중'), ('408', '99강화나무몽둥이'), ('409', '호랑이형님'), ('410', '2022 몰래보는 로맨스'), ('411', '취사병 전설이 되다'), ('412', '먹는 인생'), ('413', '프리드로우'), ('414', '놓지마 정신줄 시즌3'), ('415', '여고생 드래곤'), ('416', '세레나'), ('417', '망나니 소교주로 환생했다'), ('418', '은탄'), ('419', '악몽의 형상'), ('420', '이십팔세기 광팬'), ('421', '아홉수 우리들'), ('422', '스터디그룹'), ('423', '민간인 통제구역 - 일급기밀'), ('424', '나이트런'), ('425', '작전명 순정'), ('426', '청춘 블라썸'), ('427', '윌유메리미'), ('428', '반드시 해피엔딩'), ('429', '나태 공자, 노력 천재 되다'), ('430', '간첩 18세'), ('431', '소공녀 민트'), ('432', '나를 바꿔줘'), ('433', '어글리후드'), ('434', '물위의 우리'), ('435', '존잘주의'), ('436', '아사'), ('437', '미나 이퀄'), ('438', '남편 먹는 여자'), ('439', '탑코너'), ('440', '겨울특강'), ('441', '용두사망 소설 속의 악녀가 되었다'), ('442', '좀비 파이트'), ('443', '태백 : 튜토리얼 맨'), ('444', '아이돌만 하고 싶었는데'), ('445', '시선 끝 브로콜리'), ('446', '홍시는 날 좋아해!'), ('447', '디펜스 게임의 폭군이 되었다'), ('448', '용사참수인'), ('449', '남편을 만렙으로 키우려 합니다'), ('450', '엑스애쉬'), ('451', '왕세자 입학도'), ('452', '팬인데 왜요'), ('453', '대충 캠퍼스로맨스임'), ('454', '같은 학교 친구'), ('455', '이계진입 리로디드'), ('456', '공유몽'), ('457', '폭군 남편과 이혼하겠습니다'), ('458', '지옥급식'), ('459', '광해의 연인'), ('460', '완벽한 부부는 없다'), ('461', '묘령의 황자'), ('462', '메트로헌터'), ('463', '최면학교'), ('464', '마법사가 죽음을 맞이하는 방법'), ('465', '최강부캐'), ('466', '은둔코인'), ('467', '복수를 위한 결혼동맹'), ('468', '키스 식스 센스'), ('469', '다비, 아찔하게 흐르는'), ('470', '전생연분'), ('471', '소년 검사'), ('472', '적월의 나라'), ('473', '중매쟁이 아가 황녀님'), ('474', '모두 너였다'), ('475', '왕년엔 용사님'), ('476', '배달의 신'), ('477', '청춘일지'), ('478', '7FATES: CHAKHO'), ('479', '키스의 여왕'), ('480', '도사 가온'), ('481', '좋아해 아니 싫어해'), ('482', '아가사'), ('483', '이건 그냥 연애 이야기'), ('484', '밤을 깨우는 마법'), ('485', '스트릿 워크아웃'), ('486', '숨겨진 성녀'), ('487', '아침을 지나 밤으로'), ('488', '보고 있지?'), ('489', '실버 쥬얼'), ('490', '보듬보듬'), ('491', '싸움독학'), ('492', '입학용병'), ('493', '약한영웅'), ('494', '사형소년'), ('495', '수희0(tngmlek0)'), ('496', '자매전쟁'), ('497', '칼끝에 입술'), ('498', '시월드가 내게 집착한다'), ('499', '천하제일인'), ('500', '안녕, 나의 수집'), ('501', '약빨이 신선함'), ('502', '살아남은 로맨스'), ('503', '가비지타임'), ('504', '판사 이한영'), ('505', '곱게 키웠더니, 짐승'), ('506', '분신으로 자동사냥'), ('507', '무서운게 딱좋아!'), ('508', '내일'), ('509', '아카데미에 위장취업당했다'), ('510', '백설을 위하여'), ('511', '오로지 너를 이기고 싶어'), ('512', '마법스크롤 상인 지오'), ('513', '어느날 갑자기 서울은'), ('514', '일타강사 백사부'), ('515', '노량진 공격대'), ('516', '합격시켜주세용'), ('517', '경자 전성시대'), ('518', '헬스던전'), ('519', '천년간 노려왔습니다'), ('520', '위닝샷!'), ('521', '폰투스 : 극야2'), ('522', '장풍전'), ('523', '동생친구'), ('524', '고백 취소도 되나?'), ('525', '랑데뷰'), ('526', '청춘계시록'), ('527', '완벽한 파트너'), ('528', '손 안의 안단테'), ('529', '천치전능'), ('530', '평행도시'), ('531', '생존로그'), ('532', '장미같은 소리'), ('533', '몸이 바뀌는 사정'), ('534', '결혼까지 망상했어!'), ('535', '위험한 남편을 길들이는 법'), ('536', '키미앤조이'), ('537', '108명의 그녀들'), ('538', '홍 의관의 은밀한 비밀'), ('539', '마왕의 고백'), ('540', '나의 작은 서점'), ('541', '하렘에서 살아남기'), ('542', '여우애담'), ('543', '전설의 화석'), ('544', '돌&아이'), ('545', '내곁엔 없을까'), ('546', '킬링킬러'), ('547', '그림자 잡기'), ('548', '데빌샷'), ('549', 'DARK MOON: 달의 제단'), ('550', '후궁 스캔들'), ('551', '소녀재판'), ('552', '생존고백'), ('553', '마도'), ('554', '보스의 노골적 취향'), ('555', '철수와 영희 이야기'), ('556', '6월의 라벤더'), ('557', '마섹남 - 마술하는 섹시한 남자'), ('558', '보통아이'), ('559', '패션쇼'), ('560', '사람은 고쳐 쓰는 게 아니야!'), ('561', '샤인 스타'), ('562', '블러드 리벤저'), ('563', '거래하실래요?'), ('564', '아마도, 굿모닝'), ('565', '밀실 마피아 게임'), ('566', '조선여우스캔들'), ('567', '독신마법사 기숙아파트'), ('568', '어떤소란'), ('569', '구해줘, 호구!')]

```

<div class="in_prompt">
In&nbsp;[11]:
</div>

<div class="input_area" markdown="1">

```python
pd.DataFrame(webtoonByDayList, columns=["순번", "제목"])
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
      <th>순번</th>
      <th>제목</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>참교육</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>뷰티풀 군바리</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>퀘스트지상주의</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>퍼니게임</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>장씨세가 호위무사</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>564</th>
      <td>565</td>
      <td>밀실 마피아 게임</td>
    </tr>
    <tr>
      <th>565</th>
      <td>566</td>
      <td>조선여우스캔들</td>
    </tr>
    <tr>
      <th>566</th>
      <td>567</td>
      <td>독신마법사 기숙아파트</td>
    </tr>
    <tr>
      <th>567</th>
      <td>568</td>
      <td>어떤소란</td>
    </tr>
    <tr>
      <th>568</th>
      <td>569</td>
      <td>구해줘, 호구!</td>
    </tr>
  </tbody>
</table>
<p>569 rows × 2 columns</p>
</div>
</div>



## 썸네일링크와 요일태깅 추가

<div class="in_prompt">
In&nbsp;[12]:
</div>

<div class="input_area" markdown="1">

```python
# 요일까지 가져오기 위해서는 다른 col_inner라는 태그안의 h4태그의 text를 가져와야함.
webtoonByDayCol = webtoonByDayClass.find_all("div", attrs = {"class": "col_inner"})
```

</div>

<div class="in_prompt">
In&nbsp;[13]:
</div>

<div class="input_area" markdown="1">

```python
webtoonByDayListWithImg = []
i = 1

for webtoon in webtoonByDayCol:
    # 가져온 html 태그중 h4 태그의 text를 가져옴
    h4 = webtoon.find("h4").text
    # 가져온 html 태그중 img 태그를 모두 가져와서 리스트화
    imgs = webtoon.find_all("img")
#     print(h4)
#     print(imgs)
    for img in imgs:
        title = img.get("title")
        # img 태그 중 src 요소에 섬네일 주소가 담겨있음.
        thumbnail = img.get("src")
        if title == None:
            continue
        print(f"#{str(i)}: {h4}, {title}, {thumbnail}")
        data = [str(i), h4, title, thumbnail]
        i += 1
        webtoonByDayListWithImg.append(data)
print(webtoonByDayListWithImg)
```

</div>

<div class="output_prompt">
Out&nbsp;[13]:
</div>

{:.output_stream}

```
#1: 월요 웹툰, 참교육, https://shared-comic.pstatic.net/thumb/webtoon/758037/thumbnail/thumbnail_IMAG21_15cb2611-34c0-4f02-a689-41d0b1016579.jpg
#2: 월요 웹툰, 뷰티풀 군바리, https://shared-comic.pstatic.net/thumb/webtoon/648419/thumbnail/thumbnail_IMAG21_d9398229-cbfd-47dc-9208-0a6fb936f3a7.jpg
#3: 월요 웹툰, 퀘스트지상주의, https://shared-comic.pstatic.net/thumb/webtoon/783052/thumbnail/thumbnail_IMAG21_e14cbea7-8416-40e7-9aae-ccff970ac88f.jpg
#4: 월요 웹툰, 퍼니게임, https://shared-comic.pstatic.net/thumb/webtoon/801035/thumbnail/thumbnail_IMAG21_01fd148f-edb2-4ada-9571-910981ec3376.jpg
#5: 월요 웹툰, 장씨세가 호위무사, https://shared-comic.pstatic.net/thumb/webtoon/728750/thumbnail/thumbnail_IMAG21_47c21251-b213-4882-bacc-15adce1acfc8.jpg
#6: 월요 웹툰, 윈드브레이커, https://shared-comic.pstatic.net/thumb/webtoon/602910/thumbnail/thumbnail_IMAG21_e861f2cf-6157-4d33-8e02-7b4cbf0a8baf.jpg
#7: 월요 웹툰, 팔이피플, https://shared-comic.pstatic.net/thumb/webtoon/774863/thumbnail/thumbnail_IMAG21_3689684179498578529.jpg
#8: 월요 웹툰, 신화급 귀속 아이템을 손에 넣었다, https://shared-comic.pstatic.net/thumb/webtoon/795297/thumbnail/thumbnail_IMAG21_4134925888999731505.jpg
#9: 월요 웹툰, 버림받은 왕녀의 은밀한 침실, https://shared-comic.pstatic.net/thumb/webtoon/796867/thumbnail/thumbnail_IMAG21_77aa5064-b42b-4838-912e-2c6266c53a74.jpg
#10: 월요 웹툰, 앵무살수, https://shared-comic.pstatic.net/thumb/webtoon/739115/thumbnail/thumbnail_IMAG21_7077747896626722102.jpg

...

#560: 일요 웹툰, 사람은 고쳐 쓰는 게 아니야!, https://shared-comic.pstatic.net/thumb/webtoon/758664/thumbnail/thumbnail_IMAG21_3630856102589116464.jpg
#561: 일요 웹툰, 샤인 스타, https://shared-comic.pstatic.net/thumb/webtoon/758665/thumbnail/thumbnail_IMAG21_3617063617572387171.jpg
#562: 일요 웹툰, 블러드 리벤저, https://shared-comic.pstatic.net/thumb/webtoon/783599/thumbnail/thumbnail_IMAG21_629c1cd8-a02c-4ee3-80a8-2da6c5c6a57e.jpg
#563: 일요 웹툰, 거래하실래요?, https://shared-comic.pstatic.net/thumb/webtoon/773524/thumbnail/thumbnail_IMAG21_7149293130308793392.jpg
#564: 일요 웹툰, 아마도, 굿모닝, https://shared-comic.pstatic.net/thumb/webtoon/795041/thumbnail/thumbnail_IMAG21_02f63d85-3d3c-468b-9b4c-a61203785747.jpg
#565: 일요 웹툰, 밀실 마피아 게임, https://shared-comic.pstatic.net/thumb/webtoon/794224/thumbnail/thumbnail_IMAG21_c063bfcc-d189-45c3-a29d-4e453d8b134b.jpg
#566: 일요 웹툰, 조선여우스캔들, https://shared-comic.pstatic.net/thumb/webtoon/758676/thumbnail/thumbnail_IMAG21_3991651857397657649.jpg
#567: 일요 웹툰, 독신마법사 기숙아파트, https://shared-comic.pstatic.net/thumb/webtoon/776143/thumbnail/thumbnail_IMAG21_4134977780861710902.jpg
#568: 일요 웹툰, 어떤소란, https://shared-comic.pstatic.net/thumb/webtoon/799213/thumbnail/thumbnail_IMAG21_3d88ad40-2cca-4255-a00c-a5d9dab3c29f.jpg
#569: 일요 웹툰, 구해줘, 호구!, https://shared-comic.pstatic.net/thumb/webtoon/785812/thumbnail/thumbnail_IMAG21_7291439068095145059.jpg
[['1', '월요 웹툰', '참교육', 'https://shared-comic.pstatic.net/thumb/webtoon/758037/thumbnail/thumbnail_IMAG21_15cb2611-34c0-4f02-a689-41d0b1016579.jpg'], ['2', '월요 웹툰', '뷰티풀 군바리', 'https://shared-comic.pstatic.net/thumb/webtoon/648419/thumbnail/thumbnail_IMAG21_d9398229-cbfd-47dc-9208-0a6fb936f3a7.jpg'], ['3', '월요 웹툰', '퀘스트지상주의', 'https://shared-comic.pstatic.net/thumb/webtoon/783052/thumbnail/thumbnail_IMAG21_e14cbea7-8416-40e7-9aae-ccff970ac88f.jpg'], ['4', '월요 웹툰', '퍼니게임', 'https://shared-comic.pstatic.net/thumb/webtoon/801035/thumbnail/thumbnail_IMAG21_01fd148f-edb2-4ada-9571-910981ec3376.jpg'], ['5', '월요 웹툰', '장씨세가 호위무사', 'https://shared-comic.pstatic.net/thumb/webtoon/728750/thumbnail/thumbnail_IMAG21_47c21251-b213-4882-bacc-15adce1acfc8.jpg'], ['6', '월요 웹툰', '윈드브레이커', 'https://shared-comic.pstatic.net/thumb/webtoon/602910/thumbnail/thumbnail_IMAG21_e861f2cf-6157-4d33-8e02-7b4cbf0a8baf.jpg'], ['7', '월요 웹툰', '팔이피플', 'https://shared-comic.pstatic.net/thumb/webtoon/774863/thumbnail/thumbnail_IMAG21_3689684179498578529.jpg'], ['8', '월요 웹툰', '신화급 귀속 아이템을 손에 넣었다', 'https://shared-comic.pstatic.net/thumb/webtoon/795297/thumbnail/thumbnail_IMAG21_4134925888999731505.jpg'], ['9', '월요 웹툰', '버림받은 왕녀의 은밀한 침실', 'https://shared-comic.pstatic.net/thumb/webtoon/796867/thumbnail/thumbnail_IMAG21_77aa5064-b42b-4838-912e-2c6266c53a74.jpg'], ['10', '월요 웹툰', '앵무살수', 'https://shared-comic.pstatic.net/thumb/webtoon/739115/thumbnail/thumbnail_IMAG21_7077747896626722102.jpg'],

...

['560', '일요 웹툰', '사람은 고쳐 쓰는 게 아니야!', 'https://shared-comic.pstatic.net/thumb/webtoon/758664/thumbnail/thumbnail_IMAG21_3630856102589116464.jpg'], ['561', '일요 웹툰', '샤인 스타', 'https://shared-comic.pstatic.net/thumb/webtoon/758665/thumbnail/thumbnail_IMAG21_3617063617572387171.jpg'], ['562', '일요 웹툰', '블러드 리벤저', 'https://shared-comic.pstatic.net/thumb/webtoon/783599/thumbnail/thumbnail_IMAG21_629c1cd8-a02c-4ee3-80a8-2da6c5c6a57e.jpg'], ['563', '일요 웹툰', '거래하실래요?', 'https://shared-comic.pstatic.net/thumb/webtoon/773524/thumbnail/thumbnail_IMAG21_7149293130308793392.jpg'], ['564', '일요 웹툰', '아마도, 굿모닝', 'https://shared-comic.pstatic.net/thumb/webtoon/795041/thumbnail/thumbnail_IMAG21_02f63d85-3d3c-468b-9b4c-a61203785747.jpg'], ['565', '일요 웹툰', '밀실 마피아 게임', 'https://shared-comic.pstatic.net/thumb/webtoon/794224/thumbnail/thumbnail_IMAG21_c063bfcc-d189-45c3-a29d-4e453d8b134b.jpg'], ['566', '일요 웹툰', '조선여우스캔들', 'https://shared-comic.pstatic.net/thumb/webtoon/758676/thumbnail/thumbnail_IMAG21_3991651857397657649.jpg'], ['567', '일요 웹툰', '독신마법사 기숙아파트', 'https://shared-comic.pstatic.net/thumb/webtoon/776143/thumbnail/thumbnail_IMAG21_4134977780861710902.jpg'], ['568', '일요 웹툰', '어떤소란', 'https://shared-comic.pstatic.net/thumb/webtoon/799213/thumbnail/thumbnail_IMAG21_3d88ad40-2cca-4255-a00c-a5d9dab3c29f.jpg'], ['569', '일요 웹툰', '구해줘, 호구!', 'https://shared-comic.pstatic.net/thumb/webtoon/785812/thumbnail/thumbnail_IMAG21_7291439068095145059.jpg']]

```

<div class="in_prompt">
In&nbsp;[14]:
</div>

<div class="input_area" markdown="1">

```python
pd.DataFrame(webtoonByDayListWithImg, columns=["순번", "요일", "제목", "썸네일"])
```

</div>

<div class="output_prompt">
Out&nbsp;[14]:
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
      <th>순번</th>
      <th>요일</th>
      <th>제목</th>
      <th>썸네일</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>월요 웹툰</td>
      <td>참교육</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>월요 웹툰</td>
      <td>뷰티풀 군바리</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>월요 웹툰</td>
      <td>퀘스트지상주의</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>월요 웹툰</td>
      <td>퍼니게임</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>월요 웹툰</td>
      <td>장씨세가 호위무사</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>564</th>
      <td>565</td>
      <td>일요 웹툰</td>
      <td>밀실 마피아 게임</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>565</th>
      <td>566</td>
      <td>일요 웹툰</td>
      <td>조선여우스캔들</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>566</th>
      <td>567</td>
      <td>일요 웹툰</td>
      <td>독신마법사 기숙아파트</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>567</th>
      <td>568</td>
      <td>일요 웹툰</td>
      <td>어떤소란</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
    <tr>
      <th>568</th>
      <td>569</td>
      <td>일요 웹툰</td>
      <td>구해줘, 호구!</td>
      <td>https://shared-comic.pstatic.net/thumb/webtoon...</td>
    </tr>
  </tbody>
</table>
<p>569 rows × 4 columns</p>
</div>
</div>



# API를 활용한 데이터 크롤링

[네이버 개발자 센터](https://developers.naver.com/)

<div class="in_prompt">
In&nbsp;[15]:
</div>

<div class="input_area" markdown="1">

```python
import os
import sys
import urllib.request
import json
```

</div>

<div class="in_prompt">
In&nbsp;[16]:
</div>

<div class="input_area" markdown="1">

```python
client_id = "********************"
client_secret = "**********"
```

</div>

* url에 넣는 요청

|요청|변수타입|필수여부|기본값|설명|
|-|-|:-:|-|-|
|query|string|Y|-|검색을 원하는 문자열로서 UTF-8로 인코딩|
|display|integer|N|10(기본값),100(최대)|검색 결과 출력 건수 지정|
|start|integer|N|1(기본값), 1000(최대)|검색 시작 위치로 최대 1000까지 가능|
|sort|string|N|sim(기본값), date|정렬 옵션: sim (유사도순), date (날짜순)|

<div class="in_prompt">
In&nbsp;[17]:
</div>

<div class="input_area" markdown="1">

```python
# 검색 키워드
searchText = input("검색키워드 : ")
query = urllib.parse.quote(searchText)
# 출력건수(최대 100건)
display = input("검색 갯수(최대100건) : ")
# url에 요청 변수들 중 필수 변수는 반드시 넣어주어야 하며, 이외의 변수는 선택사항
url = "https://openapi.naver.com/v1/search/blog"
query = f"?query={query}&display={display}&start=1&sort=sim"
# query = f"?query={query}&display={displqy}&start={start}&sort={sort}"
# url과 query를 연결해 요청을 생성
request = urllib.request.Request(url + query)
# 네이버 api를 활용하기 위한 key값들을 요청 헤더에 추가
request.add_header("X-Naver-Client-Id",client_id)
request.add_header("X-Naver-Client-Secret",client_secret)
# 위에서 생성한 요청에 대한 응답을 받아옴
response = urllib.request.urlopen(request)
print("getcode : ", response.getcode())
print("geturl : ", response.geturl())
# http의 응답코드를 가져옴
rescode = response.getcode()
# 200인 경우 제대로된 응답을 가져온 경우이다.
if(rescode==200):
    response_body = response.read()
    body = response_body.decode("utf-8")
    print("body", body)
    # 200이 아닌 경우 에러코드이며
    # 네이버 api의 에러코드의 경우 developers.naver.com/docs/common/openapiguide/errorcode.md에서 확인
else:
    print("Error Code:" + rescode)
```

</div>

<div class="output_prompt">
Out&nbsp;[17]:
</div>

{:.output_stream}

```
검색키워드 :  문래동 맛집
검색 갯수(최대100건) :  5

```

{:.output_stream}

```
getcode :  200
geturl :  https://openapi.naver.com/v1/search/blog?query=%EB%AC%B8%EB%9E%98%EB%8F%99%20%EB%A7%9B%EC%A7%91&display=5&start=1&sort=sim
body {
	"lastBuildDate":"Sun, 13 Nov 2022 13:53:57 +0900",
	"total":73203,
	"start":1,
	"display":5,
	"items":[
		{
			"title":"영등포 <b>문래동<\/b> 소갈비 <b>맛집<\/b> 갈빗 연탄구이 후기",
			"link":"https:\/\/blog.naver.com\/kim8327809\/222910290145",
			"description":"ㅎㅎㅎ) 다음날 체크아웃 하고 영등포 문래동 근처 맛집을 가기로 했어요! 남푠이 전날부터 광클해서... 좋았습니다^0^ 문래동에서 가볍게 소갈비와 함께 쏘맥한잔 즐길 수 있어서 좋았던 <b>문래동 맛집<\/b>... ",
			"bloggername":"쮸쮸chu블로그",
			"bloggerlink":"blog.naver.com\/kim8327809",
			"postdate":"20221105"
		},
		{
			"title":"<b>문래동 맛집<\/b> 월화고기♡",
			"link":"https:\/\/blog.naver.com\/cateye99\/222668909317",
			"description":"간만에 와이프와 함께 오봇한 데이트를 하기 위해 <b>문래동 맛집<\/b> 추천 받은 곳으로 가봤어요. 음식 맛과... 같네요~ <b>문래동 맛집<\/b> 실내는 생각보다 넓더라고요~ 주말 이른 저녁시간이었음에도 불구하고 많은... ",
			"bloggername":"케이부부의 달콤한 이야기_♡",
			"bloggerlink":"blog.naver.com\/cateye99",
			"postdate":"20220311"
		},
		{
			"title":"<b>문래동 맛집<\/b> 숙성 생고기 전문점 문래그집",
			"link":"https:\/\/blog.naver.com\/28ssan\/222846592240",
			"description":"<b>문래동 맛집<\/b> 문래그집은 드럼통 8개의 아담한 식당으로 잠시후에 빈자리 없이 꽉 차는데 대부분 20~30대의 젊은 층이고 주로 커플들이어서 문래동이 을지로(힙지로)처럼 젊음의 거리로 변신하는게 아닌가... ",
			"bloggername":"작은반달곰 료료",
			"bloggerlink":"blog.naver.com\/28ssan",
			"postdate":"20220814"
		},
		{
			"title":"[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 <b>문래동 맛집<\/b>",
			"link":"https:\/\/blog.naver.com\/berta502\/222873400603",
			"description":"외관 문래동 삼겹살 맛집 문래동냉삼. 냉삼집 답게 레트로한 외관이 눈길을 사로잡는다. 점심가능 사전조사로 다른 후기들을 보고왔는데 주변 직장인들은 <b>문래동 맛집<\/b>으로 소문나서 점심식사로도 많이... ",
			"bloggername":"떼히데이",
			"bloggerlink":"blog.naver.com\/berta502",
			"postdate":"20220913"
		},
		{
			"title":"<b>문래동 맛집<\/b> 바삭하고 촉촉한 그믐족발",
			"link":"https:\/\/blog.naver.com\/qufzhd12\/222904239933",
			"description":"오늘은 <b>문래동 맛집<\/b>이에요~ ㅎㅎㅎ 제가 한번 먹고 반한 그믐족발인데요. 여기가 SNs에서 영등포... 근데 그믐족발이 <b>문래동 맛집<\/b>이면서도 술집으로도 사람들이 많이 찾아서 그런지 이날은 유독 서너 명 정도의... ",
			"bloggername":"두덩이님의 블로그",
			"bloggerlink":"blog.naver.com\/qufzhd12",
			"postdate":"20221027"
		}
	]
}

```

<div class="in_prompt">
In&nbsp;[18]:
</div>

<div class="input_area" markdown="1">

```python
# String 형태로 return 된 body값을 다루기 편리하도록 json형태로 변환
data = json.loads(body)
body
```

</div>

<div class="output_prompt">
Out&nbsp;[18]:
</div>




{:.output_data_text}

```
'{\n\t"lastBuildDate":"Sun, 13 Nov 2022 13:53:57 +0900",\n\t"total":73203,\n\t"start":1,\n\t"display":5,\n\t"items":[\n\t\t{\n\t\t\t"title":"영등포 <b>문래동<\\/b> 소갈비 <b>맛집<\\/b> 갈빗 연탄구이 후기",\n\t\t\t"link":"https:\\/\\/blog.naver.com\\/kim8327809\\/222910290145",\n\t\t\t"description":"ㅎㅎㅎ) 다음날 체크아웃 하고 영등포 문래동 근처 맛집을 가기로 했어요! 남푠이 전날부터 광클해서... 좋았습니다^0^ 문래동에서 가볍게 소갈비와 함께 쏘맥한잔 즐길 수 있어서 좋았던 <b>문래동 맛집<\\/b>... ",\n\t\t\t"bloggername":"쮸쮸chu블로그",\n\t\t\t"bloggerlink":"blog.naver.com\\/kim8327809",\n\t\t\t"postdate":"20221105"\n\t\t},\n\t\t{\n\t\t\t"title":"<b>문래동 맛집<\\/b> 월화고기♡",\n\t\t\t"link":"https:\\/\\/blog.naver.com\\/cateye99\\/222668909317",\n\t\t\t"description":"간만에 와이프와 함께 오봇한 데이트를 하기 위해 <b>문래동 맛집<\\/b> 추천 받은 곳으로 가봤어요. 음식 맛과... 같네요~ <b>문래동 맛집<\\/b> 실내는 생각보다 넓더라고요~ 주말 이른 저녁시간이었음에도 불구하고 많은... ",\n\t\t\t"bloggername":"케이부부의 달콤한 이야기_♡",\n\t\t\t"bloggerlink":"blog.naver.com\\/cateye99",\n\t\t\t"postdate":"20220311"\n\t\t},\n\t\t{\n\t\t\t"title":"<b>문래동 맛집<\\/b> 숙성 생고기 전문점 문래그집",\n\t\t\t"link":"https:\\/\\/blog.naver.com\\/28ssan\\/222846592240",\n\t\t\t"description":"<b>문래동 맛집<\\/b> 문래그집은 드럼통 8개의 아담한 식당으로 잠시후에 빈자리 없이 꽉 차는데 대부분 20~30대의 젊은 층이고 주로 커플들이어서 문래동이 을지로(힙지로)처럼 젊음의 거리로 변신하는게 아닌가... ",\n\t\t\t"bloggername":"작은반달곰 료료",\n\t\t\t"bloggerlink":"blog.naver.com\\/28ssan",\n\t\t\t"postdate":"20220814"\n\t\t},\n\t\t{\n\t\t\t"title":"[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 <b>문래동 맛집<\\/b>",\n\t\t\t"link":"https:\\/\\/blog.naver.com\\/berta502\\/222873400603",\n\t\t\t"description":"외관 문래동 삼겹살 맛집 문래동냉삼. 냉삼집 답게 레트로한 외관이 눈길을 사로잡는다. 점심가능 사전조사로 다른 후기들을 보고왔는데 주변 직장인들은 <b>문래동 맛집<\\/b>으로 소문나서 점심식사로도 많이... ",\n\t\t\t"bloggername":"떼히데이",\n\t\t\t"bloggerlink":"blog.naver.com\\/berta502",\n\t\t\t"postdate":"20220913"\n\t\t},\n\t\t{\n\t\t\t"title":"<b>문래동 맛집<\\/b> 바삭하고 촉촉한 그믐족발",\n\t\t\t"link":"https:\\/\\/blog.naver.com\\/qufzhd12\\/222904239933",\n\t\t\t"description":"오늘은 <b>문래동 맛집<\\/b>이에요~ ㅎㅎㅎ 제가 한번 먹고 반한 그믐족발인데요. 여기가 SNs에서 영등포... 근데 그믐족발이 <b>문래동 맛집<\\/b>이면서도 술집으로도 사람들이 많이 찾아서 그런지 이날은 유독 서너 명 정도의... ",\n\t\t\t"bloggername":"두덩이님의 블로그",\n\t\t\t"bloggerlink":"blog.naver.com\\/qufzhd12",\n\t\t\t"postdate":"20221027"\n\t\t}\n\t]\n}'
```



<div class="in_prompt">
In&nbsp;[19]:
</div>

<div class="input_area" markdown="1">

```python
# json은 dictionary 자료형과 비슷하다고 생각하면 되며, dictionary처럼 key값으로 value값을 불러올 수 있음 
print("lastBuildDate : ", data["lastBuildDate"])
print("total : ",data["total"])
print("start : ",data["start"])
print("display : ",data["display"])
print("items : ",data["items"])
```

</div>

<div class="output_prompt">
Out&nbsp;[19]:
</div>

{:.output_stream}

```
lastBuildDate :  Sun, 13 Nov 2022 13:53:57 +0900
total :  73203
start :  1
display :  5
items :  [{'title': '영등포 <b>문래동</b> 소갈비 <b>맛집</b> 갈빗 연탄구이 후기', 'link': 'https://blog.naver.com/kim8327809/222910290145', 'description': 'ㅎㅎㅎ) 다음날 체크아웃 하고 영등포 문래동 근처 맛집을 가기로 했어요! 남푠이 전날부터 광클해서... 좋았습니다^0^ 문래동에서 가볍게 소갈비와 함께 쏘맥한잔 즐길 수 있어서 좋았던 <b>문래동 맛집</b>... ', 'bloggername': '쮸쮸chu블로그', 'bloggerlink': 'blog.naver.com/kim8327809', 'postdate': '20221105'}, {'title': '<b>문래동 맛집</b> 월화고기♡', 'link': 'https://blog.naver.com/cateye99/222668909317', 'description': '간만에 와이프와 함께 오봇한 데이트를 하기 위해 <b>문래동 맛집</b> 추천 받은 곳으로 가봤어요. 음식 맛과... 같네요~ <b>문래동 맛집</b> 실내는 생각보다 넓더라고요~ 주말 이른 저녁시간이었음에도 불구하고 많은... ', 'bloggername': '케이부부의 달콤한 이야기_♡', 'bloggerlink': 'blog.naver.com/cateye99', 'postdate': '20220311'}, {'title': '<b>문래동 맛집</b> 숙성 생고기 전문점 문래그집', 'link': 'https://blog.naver.com/28ssan/222846592240', 'description': '<b>문래동 맛집</b> 문래그집은 드럼통 8개의 아담한 식당으로 잠시후에 빈자리 없이 꽉 차는데 대부분 20~30대의 젊은 층이고 주로 커플들이어서 문래동이 을지로(힙지로)처럼 젊음의 거리로 변신하는게 아닌가... ', 'bloggername': '작은반달곰 료료', 'bloggerlink': 'blog.naver.com/28ssan', 'postdate': '20220814'}, {'title': '[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 <b>문래동 맛집</b>', 'link': 'https://blog.naver.com/berta502/222873400603', 'description': '외관 문래동 삼겹살 맛집 문래동냉삼. 냉삼집 답게 레트로한 외관이 눈길을 사로잡는다. 점심가능 사전조사로 다른 후기들을 보고왔는데 주변 직장인들은 <b>문래동 맛집</b>으로 소문나서 점심식사로도 많이... ', 'bloggername': '떼히데이', 'bloggerlink': 'blog.naver.com/berta502', 'postdate': '20220913'}, {'title': '<b>문래동 맛집</b> 바삭하고 촉촉한 그믐족발', 'link': 'https://blog.naver.com/qufzhd12/222904239933', 'description': '오늘은 <b>문래동 맛집</b>이에요~ ㅎㅎㅎ 제가 한번 먹고 반한 그믐족발인데요. 여기가 SNs에서 영등포... 근데 그믐족발이 <b>문래동 맛집</b>이면서도 술집으로도 사람들이 많이 찾아서 그런지 이날은 유독 서너 명 정도의... ', 'bloggername': '두덩이님의 블로그', 'bloggerlink': 'blog.naver.com/qufzhd12', 'postdate': '20221027'}]

```

<div class="in_prompt">
In&nbsp;[20]:
</div>

<div class="input_area" markdown="1">

```python
# 필요한 부분은 items에 딕셔너리의 리스트 형태로 존재하는 것을 알 수 있음
data["items"]
```

</div>

<div class="output_prompt">
Out&nbsp;[20]:
</div>




{:.output_data_text}

```
[{'title': '영등포 <b>문래동</b> 소갈비 <b>맛집</b> 갈빗 연탄구이 후기',
  'link': 'https://blog.naver.com/kim8327809/222910290145',
  'description': 'ㅎㅎㅎ) 다음날 체크아웃 하고 영등포 문래동 근처 맛집을 가기로 했어요! 남푠이 전날부터 광클해서... 좋았습니다^0^ 문래동에서 가볍게 소갈비와 함께 쏘맥한잔 즐길 수 있어서 좋았던 <b>문래동 맛집</b>... ',
  'bloggername': '쮸쮸chu블로그',
  'bloggerlink': 'blog.naver.com/kim8327809',
  'postdate': '20221105'},
 {'title': '<b>문래동 맛집</b> 월화고기♡',
  'link': 'https://blog.naver.com/cateye99/222668909317',
  'description': '간만에 와이프와 함께 오봇한 데이트를 하기 위해 <b>문래동 맛집</b> 추천 받은 곳으로 가봤어요. 음식 맛과... 같네요~ <b>문래동 맛집</b> 실내는 생각보다 넓더라고요~ 주말 이른 저녁시간이었음에도 불구하고 많은... ',
  'bloggername': '케이부부의 달콤한 이야기_♡',
  'bloggerlink': 'blog.naver.com/cateye99',
  'postdate': '20220311'},
 {'title': '<b>문래동 맛집</b> 숙성 생고기 전문점 문래그집',
  'link': 'https://blog.naver.com/28ssan/222846592240',
  'description': '<b>문래동 맛집</b> 문래그집은 드럼통 8개의 아담한 식당으로 잠시후에 빈자리 없이 꽉 차는데 대부분 20~30대의 젊은 층이고 주로 커플들이어서 문래동이 을지로(힙지로)처럼 젊음의 거리로 변신하는게 아닌가... ',
  'bloggername': '작은반달곰 료료',
  'bloggerlink': 'blog.naver.com/28ssan',
  'postdate': '20220814'},
 {'title': '[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 <b>문래동 맛집</b>',
  'link': 'https://blog.naver.com/berta502/222873400603',
  'description': '외관 문래동 삼겹살 맛집 문래동냉삼. 냉삼집 답게 레트로한 외관이 눈길을 사로잡는다. 점심가능 사전조사로 다른 후기들을 보고왔는데 주변 직장인들은 <b>문래동 맛집</b>으로 소문나서 점심식사로도 많이... ',
  'bloggername': '떼히데이',
  'bloggerlink': 'blog.naver.com/berta502',
  'postdate': '20220913'},
 {'title': '<b>문래동 맛집</b> 바삭하고 촉촉한 그믐족발',
  'link': 'https://blog.naver.com/qufzhd12/222904239933',
  'description': '오늘은 <b>문래동 맛집</b>이에요~ ㅎㅎㅎ 제가 한번 먹고 반한 그믐족발인데요. 여기가 SNs에서 영등포... 근데 그믐족발이 <b>문래동 맛집</b>이면서도 술집으로도 사람들이 많이 찾아서 그런지 이날은 유독 서너 명 정도의... ',
  'bloggername': '두덩이님의 블로그',
  'bloggerlink': 'blog.naver.com/qufzhd12',
  'postdate': '20221027'}]
```



<div class="in_prompt">
In&nbsp;[21]:
</div>

<div class="input_area" markdown="1">

```python
# 제목만 따로 뽑아 리스트화 시킴
titleList = [i["title"] for i in data["items"]]

print("1: ",titleList)
```

</div>

<div class="output_prompt">
Out&nbsp;[21]:
</div>

{:.output_stream}

```
1:  ['영등포 <b>문래동</b> 소갈비 <b>맛집</b> 갈빗 연탄구이 후기', '<b>문래동 맛집</b> 월화고기♡', '<b>문래동 맛집</b> 숙성 생고기 전문점 문래그집', '[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 <b>문래동 맛집</b>', '<b>문래동 맛집</b> 바삭하고 촉촉한 그믐족발']

```

<div class="in_prompt">
In&nbsp;[22]:
</div>

<div class="input_area" markdown="1">

```python
# 문장 내의 불필요한 문자 제거1
titleList = [i["title"].replace("<b>", "") for i in data["items"]]
print("2: ", titleList)
# 문장 내의 불필요한 문자 제거2
titleList = [i["title"].replace("<b>", "").replace("</b>", "") for i in data["items"]]
print("3: ", titleList)
```

</div>

<div class="output_prompt">
Out&nbsp;[22]:
</div>

{:.output_stream}

```
2:  ['영등포 문래동</b> 소갈비 맛집</b> 갈빗 연탄구이 후기', '문래동 맛집</b> 월화고기♡', '문래동 맛집</b> 숙성 생고기 전문점 문래그집', '[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 문래동 맛집</b>', '문래동 맛집</b> 바삭하고 촉촉한 그믐족발']
3:  ['영등포 문래동 소갈비 맛집 갈빗 연탄구이 후기', '문래동 맛집 월화고기♡', '문래동 맛집 숙성 생고기 전문점 문래그집', '[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 문래동 맛집', '문래동 맛집 바삭하고 촉촉한 그믐족발']

```

<div class="in_prompt">
In&nbsp;[23]:
</div>

<div class="input_area" markdown="1">

```python
# 링크만 따로 뽀아 리스트화 시킴
linkList = [i["link"] for i in data["items"]]
print(linkList)
```

</div>

<div class="output_prompt">
Out&nbsp;[23]:
</div>

{:.output_stream}

```
['https://blog.naver.com/kim8327809/222910290145', 'https://blog.naver.com/cateye99/222668909317', 'https://blog.naver.com/28ssan/222846592240', 'https://blog.naver.com/berta502/222873400603', 'https://blog.naver.com/qufzhd12/222904239933']

```

<div class="in_prompt">
In&nbsp;[24]:
</div>

<div class="input_area" markdown="1">

```python
df = pd.DataFrame({'제목':titleList, '썸네일 링크':linkList})
df
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
      <th>제목</th>
      <th>썸네일 링크</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>영등포 문래동 소갈비 맛집 갈빗 연탄구이 후기</td>
      <td>https://blog.naver.com/kim8327809/222910290145</td>
    </tr>
    <tr>
      <th>1</th>
      <td>문래동 맛집 월화고기♡</td>
      <td>https://blog.naver.com/cateye99/222668909317</td>
    </tr>
    <tr>
      <th>2</th>
      <td>문래동 맛집 숙성 생고기 전문점 문래그집</td>
      <td>https://blog.naver.com/28ssan/222846592240</td>
    </tr>
    <tr>
      <th>3</th>
      <td>[문래 문래동냉삼] 선도가... 사이드메뉴가 미친 문래동 맛집</td>
      <td>https://blog.naver.com/berta502/222873400603</td>
    </tr>
    <tr>
      <th>4</th>
      <td>문래동 맛집 바삭하고 촉촉한 그믐족발</td>
      <td>https://blog.naver.com/qufzhd12/222904239933</td>
    </tr>
  </tbody>
</table>
</div>
</div>

# Reference

* 이 포스트는 SeSAC 인공지능 자연어처리, 컴퓨터비전 기술을 활용한 응용 SW 개발자 양성 과정 - 나예진 강사님의 강의를 정리한 내용입니다.
