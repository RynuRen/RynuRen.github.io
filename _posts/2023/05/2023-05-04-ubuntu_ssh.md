---
layout: jupyter
title: ubuntu 서버 SSH 접속 설정
published: true
date: 2023-05-04
description: Notebook
comments: true
categories:
 - linux
tags:
 - WSL
 - linux
toc: true
toc_sticky: true
toc_label: ubuntu 서버 SSH 접속 설정
---
---
# SSH 설치와 port 설정

이전 post에서 WSL2로 ubuntu를 구동했다.  
AWS에서 구동시킬 때와 같은 환경을 설정하기 위해 terminus로 SSH 접속을 해보기로 하자.

Amazon Linux는 CentOS와 같은 RedHat 계열이라 패키지 관리를 rpm을 사용해 yum 명령어로 컨트롤한다.  
하지만 ubuntu는 데비안 기반이므로 dpkg로 패키지 관리를 하고 apt 명령어로 컨트롤 한다.

우선 apt를 업데이트 해준다.

{% highlight bash %}
$ sudo apt update -y && apt upgrade -y
{% endhighlight bash %}

다음 명령어로 SSH 서버를 설치할 수 있다.

{% highlight bash %}
$ sudo apt-get install openssh-server
{% endhighlight bash %}

기본 설정 포트는 `22` 인데, 널리 알려져 있기 때문에 외부에서 수시로 접속을 시도한다. 
 보안을 위해 바꿔서 사용하도록 하자.  
다음 위치의 파일에 정보가 있다.

{% highlight bash %}
$ sudo vi /etc/ssh/sshd_config
{% endhighlight bash %}

![img]({{ '/assets/images/2023/05/04/port.png' | relative_url }})

![img]({{ '/assets/images/2023/05/04/pass.png' | relative_url }})  
(추가로 SSH접속에 아이디/패스워드 방식을 사용하려면 위의 항목을 yes로 변경해야 한다.)

다음 명령어로 호스트키를 생성해주자.
{% highlight bash %}
$ sudo ssh-keygen -A
{% endhighlight bash %}

변경된 정보들을 반영해서 SSH를 재시작 해준다.

* SSH 상태 확인과 시작, 종료  
{% highlight bash %}
# ssh 서비스 상태 확인
$ sudo service ssh status

# ssh 서비스 시작
$ sudo service ssh start

# ssh 서비스 종료
$ sudo service ssh stop

# ssh 서비스 재시작
$ sudo service ssh restart
{% endhighlight bash %}

다음 명령어로 ubuntu 내부 IP를 알 수 있다.
{% highlight bash %}
hostname -I
{% endhighlight bash %}

해당 IP는 내부망에서 사용하는 IP이고 외부에서 접속하려면 추가적인 작업이 필요하다.  
WSL이 실행될 때마다 변경되는 내부IP도 고정해야 하고, 해당 IP의 port를 공유기에서 port-forwarding으로 열어줘야 한다.  
본 post의 목적은 AWS에 올렸던 프로젝트의 local에서의 구동이므로 추후 기회가 된다면...

---

# SSH 서버 접속

![img]({{ '/assets/images/2023/05/04/termius.png' | relative_url }})  
(putty로 접속해도 되지만 [Termius](https://termius.com/)가 더 익숙하다..)

위에서 알아낸 내부IP와 port, linux 사용자 이름과 비밀번호를 입력하고 접속을 하면 된다.

---

# WSL2 ubuntu 종료 및 재시작

WSL2에서 터미널을 종료해도 ubuntu는 종료되지 않고 백그라운드에 작동한다.  
linux 내에서 `shutdown` 이나 `reboot`를 사용하려고 해도 WSL에서는 systemd를 허용하지 않기에 작동하지 않는다.  
따라서 일반 linux와는 다른 방식으로 종료 및 재시작을 해야한다.

Windows 터미널에서 다음 명령어를 입력하면 WSL에 실행중인 모든 linux를 종료할 수 있다.

{% highlight bash %}
wsl --shutdown
{% endhighlight bash %}

앞선 post에서 WSL에 설치된 linux의 정보를 확인 할 수 있었다.

{% highlight bash %}
wsl --list --verbose 또는 wsl -l -v
{% endhighlight bash %}

![img]({{ '/assets/images/2023/05/02/state.png' | relative_url }})

여기서 linux의 NAME정보를 확인하고 특정 linux만 종료할 수 있다.

{% highlight bash %}
wsl -t Ubuntu
{% endhighlight bash %}

재부팅의 경우 위의 방법으로 시스템을 종료한 뒤 다시 실행해 주면 된다.  
~~Windows를 재부팅하는 방법도 있다~~

---

# Reference
* 파랑ㅇ: [우분투 서버 SSH로 접속 세팅하기](https://ca.ramel.be/74)
* JooTC: [WSL 종료 및 재부팅 방법](https://jootc.com/p/202007093546)