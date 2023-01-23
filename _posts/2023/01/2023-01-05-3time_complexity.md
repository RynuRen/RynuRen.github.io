---
layout: jupyter
title: 시간 복잡도
published: true
date: 2023-01-05
description: 시간 복잡도
comments: true
categories:
 - python
tags:
 - python
 - coding test
toc: true
toc_sticky: true
toc_label: 시간 복잡도
use_math: true
---
---
## 시간 복잡도
공간 복잡도: 알고리즘 수행에 필요한 메모리 양을 평가

입력값의 변화에 따라 연산을 실행할 때, 연산 횟수에 비해 시간이 얼마만큼 걸리는가?

효율적인 알고리즘을 구현한다는 것은 입력값이 커짐에 따라 증가하는 시간의 비율을 최소화한 알고리즘을 구성했다는 이야기

* 시간 복잡도를 표기하는 방법  
Big-O(빅-오)-최악  
Big-Ω(빅-오메가)-최선  
Big-θ(빅-세타)-평균


### 빅오 표기법

최악의 경우도 고려하여 대비

![img](https://i0.wp.com/hanamon.kr/wp-content/uploads/2021/07/Big-O-Complexity-Chart.png?resize=1080%2C723&ssl=1)

$$O(1) < O(logn) < O(n) < O(nlogn) < O(N^2) < O(2^n) < O(n!)$$

* $$𝑂(1)$$: 리스트 인덱싱, 일반 출력
* $$𝑂(logn)$$: 이분탐색(BST;Binary Search Tree)
* $$𝑂(n)$$: 입력값이 증가함에 따라 시간 또한 같은 비율로 증가 - for문
* $$𝑂(nlogn)$$: 퀵정렬, 힙정렬, 병합정렬
* $$𝑂(n^2)$$: 이중for문 이상
* $$𝑂(2^n)$$: 재귀 피보나치(함수안에 함수2개 호출)

