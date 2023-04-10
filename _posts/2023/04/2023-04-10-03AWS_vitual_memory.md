---
layout: jupyter
title: linux 가상 메모리와 메모리 모니터링
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
toc_label: linux 가상 메모리와 메모리 모니터링
---
---
# linux 가상 메모리 설정

* swap 메모리를 할당한다. (128MB * 16개로 총 2GB.. t2.micro의 기본 메모리가 1GB이므로 보통 2배를 잡는다.)
{% highlight bash %}
$ sudo dd if=/dev/zero of=/swapfile bs=128M count=16
{% endhighlight bash %}

* swap 파일 권한을 업데이트한다.
{% highlight bash %}
$ sudo chmod 600 /swapfile
{% endhighlight bash %}

* swap 영역 설정
{% highlight bash %}
$ sudo mkswap /swapfile
{% endhighlight bash %}

* swap 공간에 swap 파일을 추가한다.
{% highlight bash %}
$ sudo swapon /swapfile
{% endhighlight bash %}

* swap 공간 확인
{% highlight bash %}
$ sudo swapon -s
{% endhighlight bash %}

* 부팅 시 스왑 파일 활성화를 설정에 추가한다.(/etc/fstab)
{% highlight bash %}
$ sudo vi /etc/fstab

#(내용 추가)
/swapfile swap swap defaults 0 0
{% endhighlight bash %}

* 메모리 상황 확인
{% highlight bash %}
$ free
{% endhighlight bash %}

# linux 실시간 모니터링

{% highlight bash %}
$ watch 'free -h'
{% endhighlight bash %}

free
* [-h] : 사람이 읽기 쉬운 단위로 출력
* [-b | -k | -m | -g] : 바이트, 키비바이트, 메비바이트, 기비바이트 단위로 출력
* [--tebi | --pebi] : 테비바이트, 페비바이트 단위로 출력
* [--kilo | --mega | --giga | --tera | --peta] : 킬로바이트, 메가바이트, 기기바이트, 페타바이트 단위로 출력
* [-w] : 와이드 모드로 cache와 buffers를 따로 출력
* [-c '반복'] : 지정한 반복 횟수 만큼 free를 연속해서 실행
* [-s '초'] : 지정한 초 만큼 딜레이를 두고 지속적으로 실행
* [-t] : 합계가 계산된 total 컬럼~줄을 추가로 출력

watch
* watch [출력 명령] : 기본 사용 (2초 간격 갱신)
* watch -d [출력 명령] : 기본사용 + 변경내용 표기
* watch -n [초단위 원하는 간격] : 간격을 설정 하여 갱신

# Reference

* WhatTap: [리눅스 free 명령어로 메모리 상태 확인하기](https://www.whatap.io/ko/blog/37/)
* 로웰이: [리눅스 watch를 사용하여 시스템 모니터링 하기](https://jesc1249.tistory.com/17)
