---
layout: jupyter
title: Github profile 꾸미기
published: true
date: 2023-04-22
description: Notebook
comments: true
categories:
 - github
tags:
 - github
toc: true
toc_sticky: true
toc_label: Github profile 꾸미기
---
---
# Github profile 꾸미기

프로젝트들을 정리하다 잘 꾸며진 github profile을 보게 되었다.  
뭔가 내 profile과는 다른데? 라는 생각에 이것저것 알아보고 profile을 꾸미고 정리한 기록이다.

## profile용 repository 만들기
{username}.github.io의 이름으로 repository를 생성하면 이 블로그처럼 [GitHub pages](https://pages.github.com/){: target="_blank"}를 활용해 블로그를 생성할 수 있다.  
이 처럼 Github에는 여러 이스터 에그가 있는데, profile 또한 이스터 에그 중 하나이다.  
username과 동일한 repository를 생성하면 `You found a secret!` 메시지와 함께 `README.md`파일로 github profile을 작성할 수 있다고 알려준다.

![img]({{ '/assets/images/2023/04/22/create_repository.png' | relative_url }})  
(이미 생성한 상태라 중복 메시지가 떴다.)

![img]({{ '/assets/images/2023/04/22/add_readme.png' | relative_url }})  
(Add a README file 옵션을 추가하자.)

## README.md 꾸미기
이렇게 생성된 README.md의 내용이 profile에 노출된다. 이 파일을 꾸며보자.

### Badge (뱃지)
[Shields.io](https://shields.io/){: target="_blank"}를 이용해 뱃지를 넣어 연락처와 기술스택을 표시할 수 있다.

![img]({{ '/assets/images/2023/04/22/badge.png' | relative_url }})

`https://img.shields.io/badge/<LABEL>-<MESSAGE>-<COLOR>` 형태를 기본으로 `style`, `logo`, `logocolor` 파라미터를 추가로 지정해 뱃지를 생성할 수 있다. 

파이썬 뱃지 <img src="https://img.shields.io/badge/python-3776AB?style=flat-square&logo=Python&logoColor=white"/> 를 예를 들면 다음과 같다.

{% highlight html %}
<img src="https://img.shields.io/badge/python-3776AB?style=flat-square&logo=Python&logoColor=white"/>
{% endhighlight html %}

로고와 색상은 [Simple Icons](https://simpleicons.org/){: target="_blank"}에서 검색, 확인 할 수 있다.

#### 링크 걸기
뱃지에 이메일이나 블로그로 링크를 걸고 싶으면 `<a href="링크"> 뱃지코드 </a>`를 쓰자.

<a href="https://github.com/RynuRen" target="_blank"><img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=GitHub&logoColor=white"/></a>

{% highlight html %}
<a href="https://github.com/RynuRen"><img src="https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=GitHub&logoColor=white"/></a>
{% endhighlight html %}

(참고로 README.md에서 새창에서 링크 열기 옵션인 `target="_blank"`는 먹히지 않는다.)

---

### 백준 티어

* Mazassumnida v.1.0  
<a href="https://www.acmicpc.net/user/pros0327" target="_blank"><img src="http://mazassumnida.wtf/api/generate_badge?boj=pros0327"/></a>
<br>


* Mazassumnida v.2.0  
<a href="https://www.acmicpc.net/user/pros0327" target="_blank"><img src="http://mazassumnida.wtf/api/v2/generate_badge?boj=pros0327"/></a>
<br>

* Mazassumnida v.mini  
<a href="https://www.acmicpc.net/user/pros0327" target="_blank"><img src="http://mazassumnida.wtf/api/mini/generate_badge?boj=pros0327"/></a>
<br>

맞았습니다.. [mazassumnida](https://github.com/mazassumnida/mazassumnida){: target="_blank"}에서 여러 버전과 자세한 사용법을 확인할 수 있다.

* markdown  
{% highlight md %}
[![Solved.ac프로필](http://mazassumnida.wtf/api/v2/generate_badge?boj={handle})](https://solved.ac/{handle})
{% endhighlight md %}

* html  
{% highlight html %}
<a href="https://solved.ac/{handle}"><img src="http://mazassumnida.wtf/api/v2/generate_badge?boj={handle}"/></a>
{% endhighlight html %}

{handle}에 백준 아이디를 넣어주면 된다.

---

### Most Used Langauages
git에 commit한 언어들의 통계를 보여준다.

<img src="https://github-readme-stats.vercel.app/api/top-langs/?username=RynuRen&layout=compact&hide=jupyter%20notebook,javascript,html,scss,css,ruby&theme=gruvbox&langs_count=6"/>

{% highlight html %}
<img src="https://github-readme-stats.vercel.app/api/top-langs/?username={username}"/>
{% endhighlight html %}

username에 git의 username을 넣어주면 된다. 추가로 파라미터를 더해서 원하는 형태로 꾸며주면 된다.  
위의 양식은 `layout`, `hide`, `theme`, `langs_count`를 추가로 지정했다.  
`layout`는 default와 compact 둘 중 하나를 지정할 수 있다.  
`hide`는 카운트 하지 않을 언어, `theme`는 테마, `langs_count`는 총 표시할 언어이다.

자세한 설정은 [Most Used Langauages](https://github.com/anuraghazra/github-readme-stats#user-content-top-languages-card){: target="_blank"}에서 확인할 수 있다.

---

### 방문자 수

![img]({{ '/assets/images/2023/04/22/counter.png' | relative_url }})

해당 페이지의 방문자 수를 카운트 해준다.

[HITS](https://hits.seeyoufarm.com/#badge){: target="_blank"}에서 뱃지 커스터 마이징이 가능하다.  
생성된 뱃지의 markdonw과 html형식을 복사해서 그대로 사용하면 된다.

[hit-counter](https://github.com/gjbae1212/hit-counter){: target="_blank"}에서 설명과 코드를 볼 수 있다.

---

## Pined repo 꾸미기
이제 pinned repository 구역을 꾸며보자

![img]({{ '/assets/images/2023/04/22/pinned.png' | relative_url }})

이 부분을 꾸민다.

### github gist 생성
github gist는 짧은 코드, 메모 등을 기록 또는 공유 목적으로 사용할 수 있는 무료 서비스이다.  
gist는 요점, 요지 라는 뜻이다.

각 기능마다 사용할 gist를 생성한다.

![img]({{ '/assets/images/2023/04/22/new_gist.png' | relative_url }})  
(생성 메뉴는 우측상단의 +옆의 삼각형을 누르면 나온다)

[https://gist.github.com/new](https://gist.github.com/new){: target="_blank"}

![img]({{ '/assets/images/2023/04/22/new_gist2.png' | relative_url }})  
(description은 비워두고 1이라고 된 부분을 아무렇게나 입력하자. 나중에 기능에 맞게 바뀐다.)

![img]({{ '/assets/images/2023/04/22/new_gist3.png' | relative_url }})  
(public으로 생성해야 한다.)

![img]({{ '/assets/images/2023/04/22/gist.png' | relative_url }})  
(빨간밑줄 부분을 메모해 놓자. gist의 주소값이다.)

### token 발급
내 commit과 github기록들을 분석한 내용이 필요한 기능이므로 이에 대한 접근 권한이 필요하다.  
token을 발급해 권한을 부여할 수 있다.

![img]({{ '/assets/images/2023/04/22/token1.png' | relative_url }})
![img]({{ '/assets/images/2023/04/22/token2.png' | relative_url }})
![img]({{ '/assets/images/2023/04/22/token3.png' | relative_url }})
![img]({{ '/assets/images/2023/04/22/token4.png' | relative_url }})  
(Generate new token을 누르고 classic을 고르면 된다. 아래 링크로 한번에 갈 수 있다.)

[https://github.com/settings/tokens/new](https://github.com/settings/tokens/new){: target="_blank"}

![img]({{ '/assets/images/2023/04/22/token5.png' | relative_url }})  
![img]({{ '/assets/images/2023/04/22/token6.png' | relative_url }})  
(Note를 `GH_TOKEN`로 채우고 아래에 `repo`와 `gist`를 체크해준다. 그리고 `Generate token`을 해준다.)

생성된 token의 값을 복사해두자. 이 페이지를 벗어나면 다시 확인할 수 없으니 주의.

---

### productive-box (커밋 시각 통계)

![img]({{ '/assets/images/2023/04/22/productive-box.png' | relative_url }})

[productive-box](https://github.com/maxam2017/productive-box){: target="_blank"} repository를 fork한다.

![img]({{ '/assets/images/2023/04/22/fork.png' | relative_url }})

fork한 repository의 Settings 탭으로 가자.

![img]({{ '/assets/images/2023/04/22/repo_setting.png' | relative_url }})  
(좌측 메뉴에 `Secret and variable` - `Actions`로 가자.)

`New repository secret`를 눌러 위에서 발급한 token의 이름과 값을 넣고 `Add secret`한다.

secret를 하나더 생성하는데 이번에는 `GIST_ID` 이름으로 값은 위에서 생성해 메모해둔 gist의 주소값을 넣고 생성한다.

이제 repository 메뉴의 Actions 탭으로 가자. `enable`해주고 `Update gist`도 `enable`해주면 된다.

README.md의 아무 내용을 수정하고 commit하면 Actions가 실행되고 자동으로 update도 된다. Actions에서 확인하자.

혹시 `build`에서 오류가 난다면 다음을 확인하자.

![img]({{ '/assets/images/2023/04/22/actions.png' | relative_url }})  
(Settings 탭으로 가 `Actions` - `General`로 가자.)

![img]({{ '/assets/images/2023/04/22/workflow-permissions.png' | relative_url }})
(Workflow permissions의 `Read and write permissions`를 체크하고 save하자. Actions로 돌아가 `build`를 재시도한다.)

---

### github-stats-box (깃허브 스탯)

![img]({{ '/assets/images/2023/04/22/github-stats-box.png' | relative_url }})

[github-stats-box](https://github.com/bokub/github-stats-box){: target="_blank"} repository를 fork한다.

위 `productive-box`의 진행을 그대로 따라간다.

다만 `GITS_ID`는 secret에 추가하는 것이 아니고 코드를 직접 수정해야 한다.

fork한 repository의 `github-stats-box/.github/workflows/run.yml`를 열고 해당 부분을 수정하자.

![img]({{ '/assets/images/2023/04/22/statsbox.png' | relative_url }})  
(아래의 `ALL_COMMITS`를 false로 바꿔주면 이전 1년간의 commit만을 체크하고 `K_FORMAT`를 true로 바꿔주면 표시단위가 k가 된다.)

---

### pinned 추가
이제 profile로 돌아가 Pinned 우측의 Customize your pins를 눌러 해당 gist를 추가하자.

순서를 보기 좋게 변형한다.

![img]({{ '/assets/images/2023/04/22/complete.png' | relative_url }})


## and More...

* 다양한 README.md 위젯: [github-readme-stats](https://github.com/anuraghazra/github-readme-stats)  
stats-card, Pins, 사용시간 등을 README.md에 추가하는 등 여러 위젯들이 정리되어 있다.

* Header와 Footer README.md에 추가: [capsule-render](https://github.com/kyechan99/capsule-render)  
README.md에 다양한 커스터마이징된 header와 footer를 추가할 수 있다.

* 더 많은 gists 라이브러리 찾아보기: [awesome-pinned-gists](https://github.com/matchai/awesome-pinned-gists)  
`Twitter`, `Spotify`, `Steam`, `Youtube` 연동 등 pinned gist를 커스터마이징 할 수 있는 여러 라이브러리들이 정리되어 있다.

---

# Reference
* cowkite: [Github Profile 꾸미기](http://blog.cowkite.com/blog/2102241544/)
* 팔만코딩경: [깃허브 프로필 꾸미기!](https://80000coding.oopy.io/865f4b2a-5198-49e8-a173-0f893a4fed45)
* 점이: [[Github] github 프로필 꾸미기](https://doteloper.tistory.com/78)
* seondal: [Github Readme 꾸미기 총정리](https://velog.io/@seondal/Github-Readme-%EA%BE%B8%EB%AF%B8%EA%B8%B0-%EC%B4%9D%EC%A0%95%EB%A6%AC)