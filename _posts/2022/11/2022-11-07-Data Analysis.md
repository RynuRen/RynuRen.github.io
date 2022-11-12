---
title: Data Analysis
published: true
date: 2022-11-07
description: 데이터 분석 개요
comments: true
categories:
 - python
tags:
 - python
toc: true
toc_sticky: true
toc_label: Data Analysis
---
---
# 데이터 분석의 목적
* 데이터 분석가의 핵심 역량
: **좋은 질문**을 할 수 있는 역량<br>
필요한 **데이터를 선별하고 검증**할 수 있는 역량<br>
데이터 **해석 능력을 기반으로 쓸모 있는 결론**을 만들어내는 역량<br>
**가설 기반 A/B 테스트**를 수행하여 **결과를 판별**할 수 있는 역량<br>
**의사결정자들도 이해하기 쉽게 분석 결과를 표현**할 수 있는 역량<br>
**데이터 스토리텔링**을 통해 의사결정자들이 전체 그림을 이해하고 분석 결과에 따라 실행하게 하는 역량

* 데이터 분석 플로우
: 데이터 수집 - 데이터 탐색 - 데이터 전처리 - 데이터 모델링

* 목표
: 텍스트마이닝에 필요한 데이터 분석의 기본개념과 환경 조성<br>
텍스트마이닝의 기본절차 전처리부터 시각화까지<br>
웹크롤링 및 API를 활용한 텍스트마이닝 실습

---
# 기초적인 데이터 분석 방법
대용량 데이터에 대한 탐색적 데이터 분석(Exploratory Data Analysis)

* 데이터마이닝(Data mining)
: 대용량의 데이터로부터 이들 데이터 내에 존재하는 관계, 패턴, 규칙 등을 탐색하고 모형화함으로써 유용한 지식을 추출하는 일련의 과정들

* 특징
: 대용량의 관측 가능한 자료(비 계획적으로 모아진 자료)를 다룸<br>
경험적 방법을 중시<br>
일반화(generalizaion)에 초점을 둠 (현재의 자료보다 미래의 자료를 잘 설명할 수 있는 모형을 추구)<br>
통계학과 컴퓨터 공학(특히 인공지능)이 결합한 방법론을 개발하고 이를 경영, 경제, 정보기술(IT)분야에서 사용<br>
컴퓨터 중심의 기법

* 데이터 마이닝의 활용분야
: KDD(Knowledge Discovery In Database)<br>
기계학습(Machine Learning)<br>
패턴인식(Pattern Recognition)<br>
통계학(Statistics)

* 데이터 마이닝의 용어

|통계학|기계학습
|:-:|:-:|
|Variable|Feature
|Predicted|Output
|Independent Variable|Input
|Dependent Variable|Target
|Regression/Classification|Supervised Learning
|Clustering|Unsupervised Learning
{:.inner-borders}

* 데이터 마이닝의 5단계
1. Sampling 단계
: 적절한 양의 표본을 원 자료로부터 추출하는 단계<br>
시간과 비용의 절약과 함께 효율적인 ㅁ형의 구축에 필수적임<br>
임의추출법, 층화추출법 등이 있음
2. Exploration 단계
: 여러가지 잘의 탐색을 통해 기본적인 정보(기초통계자료, 도수분포표, 평균, 분산, 비율 등)를 확인하는 단계
3. Modification 단계 (전처리)
: 데이터의 효율적인 사용을 위한 변수의 변환, 수량화, 그룹화 등을 통하여 데이터를 변환하는 단계
4. Modeling 단계
: 분석 목적에 따라 적절한 기법을 사용하여 예측모형을 만드는 단계
5. Assessment 단계
: 모형화의 결과에 대한 신뢰성, 유용성 등을 평가하는 단계

---
# 데이터 분석 프로그램 툴 활용 - Python
Anaconda framework가 유용하나 유료..<br>
Google Colaboratory(Colab)을 통해 부라우저 내에서 Python 스크립트를 작성하고 실행

> Google Colab 단축키 : `Ctrl`{:.key} + `m`{:.key} + `h`{:.key}