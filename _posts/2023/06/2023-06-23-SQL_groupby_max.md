---
title: MySQL GROUP BY와 MAX
published: true
date: 2023-06-23
description: MySQL GROUP BY와 MAX
comments: true
categories:
 - mysql
tags:
 - mysql
 - coding tip
toc: true
toc_sticky: true
toc_label: GROUP BY와 MAX
---
---
# GROUP BY와 MAX

어떤 컬럼을 기준으로 묶어 해당 분류의 가장 큰 값을 가지는 열의 정보를 출력하고자 할 때 GROUP BY로 묶은 후 MAX를 사용하는 것을 생각할 수 있다.  
이 때 주의해야 할 것은 GROUP BY로 묶은 후 MAX를 사용하면 묶은 후 이기에 해당 분류의 가장 위의 열이 MAX의 결과로 나오게 된다.  
해당 열이 MAX라면 얼핏 맞는 쿼리라 생각할 수 있지만 다음의 예제를 보면 잘못된 쿼리임을 알 수 있다.

{% highlight sql %}
SELECT * FROM testbl;
+-----+------+------+--------+
| sid | ssid | sort | status |
+-----+------+------+--------+
|   1 | aa   |   23 | b      |
|   2 | aa   |   11 | b      |
|   3 | aa   |   33 | c      |
|   4 | aa   |   32 | d      |
|   5 | bb   |   23 | a      |
|   6 | bb   |   67 | c      |
|   7 | bb   |   34 | a      |
|   8 | bb   |   77 | d      |
|   9 | cc   |   11 | a      |
|  10 | cc   |   22 | a      |
|  11 | cc   |   32 | d      |
|  12 | cc   |   23 | c      |
|  13 | cc   |   32 | b      |
+-----+------+------+--------+
{% endhighlight sql %}

위의 testbl 테이블에서 ssid를 기준으로 GROUP BY로 묶은 후 해당 분류마다 sort값이 가장 큰 정보를 출력해보자.

{% highlight sql %}
SELECT ssid, MAX(sort), status FROM testbl GROUP BY ssid;
+------+-----------+--------+
| ssid | MAX(sort) | status |
+------+-----------+--------+
| aa   |        33 | b      |
| bb   |        77 | a      |
| cc   |        32 | a      |
+------+-----------+--------+
{% endhighlight sql %}

MAX값만 보면 잘 출력된 것 처럼 보이지만 status 항목을 살펴보면 해당 sort값의 status를 가져온게 아니라 해당 분류중 가장 위의 값을 가져온 것이다.

MAX(sort) 값의 데이터를 가져오려면 서브쿼리를 사용해 JOIN하거나 WHERE절에서 조건을 설정해주면 된다.

* JOIN
{% highlight sql %}
SELECT t1.ssid, t1.sort, t1.status
FROM (SELECT ssid, MAX(sort) max_sort
    FROM testbl
    GROUP BY ssid
    ) AS t2
    INNER JOIN testbl AS t1
    ON t2.ssid = t1.ssid AND t2.max_sort = t1.sort;
{% endhighlight sql %}

* WHERE
{% highlight sql %}
SELECT t1.ssid, t1.sort, t1.status
FROM testbl as t1, 
    (SELECT ssid, MAX(sort) max_sort
    FROM testbl
    GROUP BY ssid
    ) AS t2
WHERE t1.sort = t2.max_sort AND t1.ssid = t2.ssid;
{% endhighlight sql %}

또한 FROM절에서 서브쿼리를 사용하는 것이 아니라 WHERE절에서 사용해서 작성할 수도 있다.
{% highlight sql %}
SELECT ssid, sort, status
FROM testbl
WHERE (ssid, sort) IN (
    SELECT ssid, MAX(sort)
    FROM testbl
    GROUP BY ssid);
{% endhighlight sql %}

# 예제 문제
[식품분류별 가장 비싼 식품의 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/131116)

식품분류별로 GROUP BY를 실행해 각 분류별 MAX(PRICE) 데이터의 정보를 출력해야 한다. 추가로 출력할 식품분류는 '과자', '국', '김치', '식용유'로 한정하는 조건이 있다.

먼저 FROM절에서 서브쿼리를 사용한 방법이다.

{% highlight sql %}
SELECT O.CATEGORY, O.PRICE MAX_PRICE, O.PRODUCT_NAME
FROM (SELECT CATEGORY, MAX(PRICE) MAX_PRICE
      FROM FOOD_PRODUCT
      GROUP BY CATEGORY) X INNER JOIN FOOD_PRODUCT O
      ON X.CATEGORY = O.CATEGORY AND X.MAX_PRICE = O.PRICE
WHERE O.CATEGORY IN ('과자', '국', '김치', '식용유')
ORDER BY 2 DESC;
{% endhighlight sql %}

다음은 WHERE절에서 서브쿼리를 사용한 방법이다.

{% highlight sql %}
SELECT CATEGORY, PRICE, PRODUCT_NAME
FROM FOOD_PRODUCT
WHERE (CATEGORY, PRICE) IN (
    SELECT CATEGORY, MAX(PRICE)
    FROM FOOD_PRODUCT
    GROUP BY CATEGORY) AND CATEGORY IN ('과자', '국', '김치', '식용유')
ORDER BY 2 DESC;
{% endhighlight sql %}

---
# Reference
* b1ix: [group by로 뽑아온 값중에 가장큰 값(max)의 상태값을 가져오기](http://b1ix.net/87)