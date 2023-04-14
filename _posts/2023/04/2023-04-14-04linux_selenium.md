---
layout: jupyter
title: Linux에 selenium 설치
published: true
date: 2023-04-14
description: Notebook
comments: true
categories:
 - aws
tags:
 - aws
toc: true
toc_sticky: true
toc_label: Linux에 selenium 설치
---
---
# linux 크롬, 셀레니움 구동

## 구글 크롬 설치
{% highlight bash %}
$ curl https://intoli.com/install-google-chrome.sh | bash
$ sudo mv /usr/bin/google-chrome-stable /usr/bin/google-chrome
$ google-chrome --version && which google-chrome
{% endhighlight bash %}

## 크롬 드라이버 설치
{% highlight bash %}
$ cd tmp/
{% endhighlight bash %}

* 크롬 버전에 맞춰 크롬드라이버 다운
{% highlight bash %}
$ wget https://chromedriver.storage.googleapis.com/111.0.5563.64/chromedriver_linux64.zip
$ unzip chromedriver_linux64.zip
$ sudo mv chromedriver /usr/bin/chromedriver
$ chromedriver --version
{% endhighlight bash %}