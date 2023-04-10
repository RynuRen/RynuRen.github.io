---
layout: jupyter
title: AWS linux에 python, java 설치
published: true
date: 2023-04-10
description: Notebook
comments: true
categories:
 - aws
tags:
 - aws
toc: true
toc_sticky: true
toc_label: AWS linux에 python, java 설치
---
---
# AWS linux에 ptyhon, java 설치

## python 3.8 설치

* 먼저 pip를 설치한다.
{% highlight bash %}
$ sudo yum install -y python-pip
{% endhighlight bash %}

* Extras 소프트웨어 패키지 라이브러리가 설치되어 있는지 확인한다.
{% highlight bash %}
$ /usr/bin/amazon-linux-extras
{% endhighlight bash %}

* 설치 되어있지 않으면 설치한다.
{% highlight bash %}
$ sudo yum install -y amazon-linux-extras
{% endhighlight bash %}

* 설치가능한 python(3.8)을 확인하고 활성화한다.
{% highlight bash %}
$ amazon-linux-extras | grep python
$ sudo amazon-linux-extras enable python3.8
{% endhighlight bash %}

* yum으로 설치한다.
{% highlight bash %}
$ sudo yum install -y python3.8
{% endhighlight bash %}

## 가상환경 설정

* 파이썬 가상환경 설정 라이브러리를 설치한다.
{% highlight bash %}
$ pip install virtualenv
$ virtualenv --version
{% endhighlight bash %}

* 프로젝트를 위한 새로운 가상 환경을 생성한다.
{% highlight bash %}
$ virtualenv mint
{% endhighlight bash %}

* 해당 환경에 Python 인터프리터를 지정한다.
{% highlight bash %}
$ virtualenv -p /usr/bin/python3.8 mint
{% endhighlight bash %}

* 가상 환경을 활성화한다.
{% highlight bash %}
$ source mint/bin/activate
{% endhighlight bash %}

## git

* yum을 통해 git을 설치한다.
{% highlight bash %}
$ sudo yum install -y git
{% endhighlight bash %}

### private repository 이용하기

* SSH 키 생성
{% highlight bash %}
$ ssh-keygen

$ cd ~/.ssh
$ ls -al
$ cat id_rsa.pub
# => 키복사 => github 키등록
{% endhighlight bash %}

* 용량이 큰 package가 포함된 requirements.txt를 설치할 경우 옵션을 추가한다.
{% highlight bash %}
$ pip install -r requirements.txt --no-cache-dir
{% endhighlight bash %}

## Java-17 설치

{% highlight bash %}
$ sudo yum install -y java-17-amazon-corretto
{% endhighlight bash %}

## Spring Boot 배포

* spring boot gradle build  
gradle을 build하기 위해 필요한 파일: (src, gradle 폴더), build.gradle, gradlew, settings.gradle

clone한 project의 spring boot폴더(gradlew파일이 있는)로 접근한다.
* gradlew에 권한을 부여한다.
{% highlight bash %}
$ sudo chmod 777 ./gradlew
{% endhighlight bash %}

* gradle build를 수행한다.
{% highlight bash %}
$ ./gradlew build
{% endhighlight bash %}

* 생성된 jar파일을 실행한다.
{% highlight bash %}
$ java -jar aws_mint/spring/build/libs/news-0.0.1-SNAPSHOT.jar
{% endhighlight bash %}

* dev 옵션으로 실행하려면 명령어를 추가한다.
{% highlight bash %}
$ java -jar -Dspring.profiles.active=dev aws_mint/spring/build/libs/news-0.0.1-SNAPSHOT.jar
{% endhighlight bash %}

## 포트 리다이렉트
기본 http포트인 80으로의 접근을 spring boot기본 8080포트로 리다이렉트 시켜 주소 뒤에 포트번호를 붙이지 않아도 spring boot로 접속이 가능하도록 한다.

* 관리자 권한을 획득한다.
{% highlight bash %}
$ sudo su
{% endhighlight bash %}

* 포트 리다이렉트 설정
{% highlight bash %}
\# iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
{% endhighlight bash %}