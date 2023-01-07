---
title: Anaconda 가상환경 설정
published: true
date: 2023-01-04
description: Anaconda 가상환경 설정
comments: true
categories:
 - NLP
tags:
 - python
 - NLP
toc: true
toc_sticky: true
toc_label: Anaconda 가상환경 설정
use_math: true
---
---
# 가상환경(virtual environment)

독립된 공간을 만들어 주는 기능

## 가상환경이 필요한 이유

프로젝트 별로 필요한 패키지 버전이 다를 수 있으므로(버전별 호환성이 다르기 때문에) 해당 프로젝트에 맞는 버전의 패키지들만 설치된 환경이 필요하다.  
또한 해당 프로젝트를 팀원과 공유할 때 컴퓨터마다 환경이 다를 수 있으므로 동일한 환경설정이 필요하다.

## Anaconda 가상 환경

Python 3.3 이상부터 venv 모듈로 가상환경 설정이 가능하다.  
하지만 anaconda는 전용 가상 환경을 제공하므로 이 환경을 사용하는 것을 권장한다.

### conda 명령어

* 가상환경 생성
{% highlight dos %}
conda create -n 가상환경명 python=3.8
{% endhighlight dos %}

* 가상환경 들어가기
{% highlight dos %}
conda activate 가상환경명
{% endhighlight dos %}

* 가상환경 나가기 -> base로 돌아감
{% highlight dos %}
conda deactivate
{% endhighlight dos %}

* 현재 환경에 설치된 내역 표시
{% highlight dos %}
conda list
{% endhighlight dos %}
좌측에 pypi: pip로 설치된 항목

* 생성된 가상환경 목록출력
{% highlight dos %}
conda env list
{% endhighlight dos %}
또는
{% highlight dos %}
conda info --envs
{% endhighlight dos %}

* 패키지 설치
{% highlight dos %}
conda install 패키지명
pip install 패키지명
{% endhighlight dos %}

* 가상환경 삭제
{% highlight dos %}
conda env remove -n 가상환경명
{% endhighlight dos %}

* 가상환경 백업 (yml 또는 yaml)
{% highlight dos %}
conda env export > 백업파일명.yml
{% endhighlight dos %}

* 가상환경 복구
{% highlight dos %}
conda env create -f 백업파일명.yml
{% endhighlight dos %}

* 가상환경 복사
{% highlight dos %}
conda create -n 가상환경명_copy --clone 가상환경명
{% endhighlight dos %}

---
# Reference
* 코딩도장: [가상환경 사용하기](https://dojang.io/mod/page/view.php?id=2470)