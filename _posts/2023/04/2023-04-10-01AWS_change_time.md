---
layout: jupyter
title: AWS EC2와 RDS 시간 변경
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
toc_label: EC2와 RDS 시간 변경
---
---
# 시간 변경
시계을 데이터를 다루다 보면 구동중인 서버의 localtime에 따라 시간 표시방식에 차이가 생긴다.  
spring boot로 게시판을 구현한다면 작성시간, 수정시간, 회원가입일 등 서비스 중인 지역의 시간에 맞춰 표시해야 한다.

## AWS EC2 linux 시간 변경

1) 관리자 권한을 획득한다.
{% highlight bash %}
$ sudo su - root
{% endhighlight bash %}

2) 기존시간을 삭제한다.
{% highlight bash %}
\# sudo rm /etc/localtime
{% endhighlight bash %}

3) 한국시간 링크파일을 생성한다.
{% highlight bash %}
\# sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
{% endhighlight bash %}

4) 재시작 시에도 적용되게 설정파일을 수정한다.
{% highlight bash %}
\# sudo vi /etc/sysconfig/clock
#(편집)
ZONE="Asia/Seoul"
UTC=true
{% endhighlight bash %}

## AWS RDS 시간 변경

1) 파라미터 그룹을 생성하거나 기본 파라미터 그룹을 변경한다.  

![img]({{ '/assets/images/2023/04/10/img01.png' | relative_url }})

2) time_zone을 Asia/Seoul로 지정한다.

![img]({{ '/assets/images/2023/04/10/img02.png' | relative_url }})

3) 변경하고자 하는 DB를 수정 -> 추가사항의 DB 파라미터 그룹을 생성한 파라미터 그룹으로 지정한다

![img]({{ '/assets/images/2023/04/10/img03.png' | relative_url }})

4) 수정완료 후 DB 재시작하고 SELECT now() 쿼리문으로 확인 가능하다.
