---
layout: jupyter
title: AWS EC2 background 실행 및 종료
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
toc_label: AWS EC2 background 실행 및 종료
---
---
# EC2 서버 백그라운드 실행

* Spring Boot 백그라운드 실행
{% highlight bash %}
$ nohup java -jar aws_mint/spring/build/libs/news-0.0.1-SNAPSHOT.jar &
{% endhighlight bash %}

* 가상환경 실행, Flask 백그라운드 실행
{% highlight bash %}
$ source mint/bin/activate
$ nohup python aws_mint/flask/api.py &
{% endhighlight bash %}

* 백그라운드 로그 확인
{% highlight bash %}
$ tail -f nohup.out
{% endhighlight bash %}

## 백그라운드 실행 종료

* 포트에서 동작하고 있는 프로세스 id 확인
{% highlight bash %}
$ sudo lsof -t -i:8080
$ sudo lsof -t -i:5000
{% endhighlight bash %}

* 프로세스 종료
{% highlight bash %}
#($ kill -9 {PID})
$ kill -TERM {PID}
$ kill -INT {PID}
{% endhighlight bash %}

* signal 목록 확인
{% highlight bash %}
$ kill -l
{% endhighlight bash %}

# Reference

* lesstif: [Unix, Linux 에서 kill 명령어로 안전하게 프로세스 종료 시키는 방법](https://www.lesstif.com/system-admin/unix-linux-kill-12943674.html)
