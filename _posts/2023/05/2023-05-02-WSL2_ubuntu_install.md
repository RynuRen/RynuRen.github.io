---
layout: jupyter
title: WSL2(Windows Subsystem Linux)로 windows환경에서 ubuntu설치
published: true
date: 2023-05-02
description: Notebook
comments: true
categories:
 - linux
tags:
 - WSL
 - linux
toc: true
toc_sticky: true
toc_label: WSL2로 windows환경에서 ubuntu설치
---
---
# WSL2 설정

프로젝트가 끝나고 AWS의 지원이 끝나 더 개발을 진행하려면 로컬 환경에서 구동해야 했다. 하지만 mecab은 linux에서 동작하고 AWS 구동을 가정하고 개발했기에 로컬에서도 linux로 서비스를 구동시킬 필요가 있었다. VirtualBox나 VMware로 linux 가상환경을 구성해도 가능하지만 Windows subsystem linux기능을 이용해 ubuntu에 서비스를 올려봤다.

* WSL2 요구사항
1. x64: Windows 10 버전 1903 이상, 빌드 18362 이상 또는 Windows 11  
ARM64: Windows 10 버전 2004 이상, 빌드 19041 이상 또는 Windows 11
2. Hyper-V 가상화를 지원하는 컴퓨터도 필요하다.

Windows 버전 확인은 `Win`{:.key} + `R`{:.key} 에서 `winver.exe` 를 실행하면 확인할 수 있다.

![img]({{ '/assets/images/2023/05/02/winver.png' | relative_url }})

* **Windows 업데이트를 모두 마친 후 진행할 것!**

* Windows 11 또는 1903, 1909버전이 아닌 Windows 10의 경우 Windows Terminal, PowerShell, 명령 프롬프트에서 다음을 실행하면 "Linux용 Windows 하위 시스템" 및 "가상 머신 플랫폼"  설치, WSL 커널, GUI 지원 앱 그리고, Linux(기본값 : Ubuntu) 설치까지 한 번에 끝난다.

{% highlight bash %}
wsl --install
{% endhighlight bash %}

* 아래 과정은 위 명령어로 설치할 수 없을 경우나 수동으로 설치하는 방법이다.

## WSL 기능 켜기

* 제어판 - 프로그램 및 기능 - Windows 기능 켜기/끄기 - 'Linux용 Windows 하위 시스템'과 '가성 머신 플랫폼'체크 - 확인

![img]({{ '/assets/images/2023/05/02/wsl.png' | relative_url }})

## 개발자 모드 켜기

* 설정 - 업데이트 및 보안 - 개발자용 - '개발자 모드' 켜기 - 컴퓨터 재시작

![img]({{ '/assets/images/2023/05/02/dev.png' | relative_url }})

## Powershell에서 wsl활성화

`Win`{:.key} + `X`{:.key} 에서 powershell을 관리자 권한으로 실행한다.

Linux용 Windows 하위 시스템 활성화
{% highlight bash %}
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
{% endhighlight bash %}

가상 머신 플랫폼 활성화
{% highlight bash %}
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
{% endhighlight bash %}

## Linux 커널 업데이트 패키지 다운로드

WSL2 내부의 Linux 커널의 서비스 방식을 업데이트 한다. 이 업데이트를 하지 않은 Linux는 wsl1으로 설치되니 주의하자.

> [x64 머신용 최신 WSL2 Linux 커널 업데이트 패키지](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)  
[ARM64 머신용 최신 WSL2 Linux 커널 업데이트 패키지](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_arm64.msi)

## WSL2 기본 버전 설정

위의 업데이트를 반드시 설치 후 진행하자. 바이오스를 확인하라는 메시지가 뜰 경우 바이오스에서 CPU 가상화 기능이 활성화 되어있는지 확인하자.

{% highlight bash %}
wsl --set-default-version 2
{% endhighlight bash %}

---
# Linux 배포판 설치와 나머지...

WSL 명령어를 통한 설치와 Microsoft Store를 동한 설치 두가지가 있는데 Microsoft Store를 통해 Ubuntu 22.04를 설치했다.

![img]({{ '/assets/images/2023/05/02/store_ubuntu.png' | relative_url }})

## 상태 확인

설치된 Linux 배포판과 WSL 버전확인

{% highlight bash %}
wsl --list --verbose 또는 wsl -l -v
{% endhighlight bash %}

![img]({{ '/assets/images/2023/05/02/state.png' | relative_url }})

## Windows에서 ubuntu로 파일 이동

AWS의 경우 cli 명령어를 이용해 파일을 복사 할 수 있었지만 WSL2의 ubuntu의 루트 파일 시스템이 ext4의 가상 하드디스크(ext4.vhdx)로 마운트 되어 windows의 파일 탐색기에서도 ubuntu의 루트 파일시스템에 접근, 수정이 가능하다.

![img]({{ '/assets/images/2023/05/02/fileexp.png' | relative_url }})

ubuntu에서 다음을 실행하면 해당 폴더를 windows 탐색기로 열 수 있다.
{% highlight bash %}
explorer.exe .
{% endhighlight bash %}

---

# Reference
* poeun: [윈도우10에서 리눅스(Linux) 설치하기 (Ubuntu on WSL2)](https://ingu627.github.io/tips/install_ubuntu/)
* BIG MAN:IT: [Windows 11 Windows 10 WSL2 설치하는 방법](https://kimsungjin.tistory.com/590)
* dev_beomgeun: [[Linux] 윈도우에서 WSL2 우분투 디렉토리 열기, 파일 옮기기](https://bbeomgeun.tistory.com/139)