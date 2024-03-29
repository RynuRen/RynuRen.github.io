---
title: Spring (1) - 개발 환경 준비
published: true
date:   2022-10-17
description: Spring Boot를 사용하기 위한 환경
comments: true
categories:
 - spring boot
tags:
 - spring boot
 - java
toc: true
toc_sticky: true
toc_label: 개발 환경 준비
---
---
1. <span style="color:Turquoise">**개발 환경 준비**</span> <span style="color:SteelBlue">**1)**</span>
2. Spring IoC [1)](/spring%20boot/Spring-(02)-Spring-IoC/) [2)](/spring%20boot/Spring-(03)-Spring-IoC-2nd/)
3. Spring MVC [1)](/spring%20boot/Spring-(04)-Spring-MVC/) [2)](/spring%20boot/Spring-(05)-Spring-MVC-2nd/) [3)](/spring%20boot/Spring-(06)-Spring-MVC-3rd/)
4. ~~Database 활용~~
5. View Template [1)](/spring%20boot/Spring-(07)-View-Template/) [2)](/spring%20boot/Spring-(08)-View-Template-2nd/) [3)](/spring%20boot/Spring-(09)-View-Template-3rd/) [4)](/spring%20boot/Spring-(10)-View-Template-4th/) [5)](/spring%20boot/Spring-(11)-View-Template-5th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# IDE 설치 및 설정
* VSCode(Visual Studio Code) 사용

---
## VSCode Extensions

![img]({{ '/assets/images/2022/10/17/img1.PNG' | relative_url }}){: .center-image }*Extensions*

* Spring Boot Dashboard
* Spring Boot Extension Pack
* Spring Boot Tools
* Spring Initializr Java Support
* Lombok Annotations Supprot for VS Code
* Extension Pack for Java
* Test Runner for Java
* Maven for Java
* Project Manager for Java
* Language Support for Java

## 자주 사용하는 VSCode 단축키
### 편집관련
* 모든 파일 저장 : `Ctrl`{:.key} + `K`{:.key} 손을 땐 후 `S`{:.key}

* 주석 : `Ctrl`{:.key} + `/`{:.key}

* 소스코드 정리 : `Alt`{:.key} + `Shift`{:.key} + `F`{:.key}

* 한줄 삭제 : `Ctrl`{:.key} + `Shift`{:.key} + `K`{:.key}

* 한줄 복사 : `Alt`{:.key} + `Shift`{:.key} + `↑`{:.key}/`↓`{:.key}

* 블록 들여쓰기 : 블록선택 + `Tab`{:.key}

* 블록 내어쓰기 : 블록선택 + `Shift`{:.key} + `Tab`{:.key}

* 커서 아랫줄에 빈 행 삽입 : `Ctrl`{:.key} + `Enter`{:.key}

* 커서 윗줄에 빈 행 삽입 : `Ctrl`{:.key} + `Shift`{:.key} + `Enter`{:.key}

* 한줄씩 라인 이동 : `Alt`{:.key} + `↑`{:.key}`↓`{:.key}

* 한줄씩 복사하며 이동 : `Alt`{:.key} + `Shift`{:.key} + `↑`{:.key}/`↓`{:.key}

* 커서 왼쪽 문자블럭 삭제 : `Ctrl`{:.key} + `Backspace`{:.key}

* 커서 오른쪽 문자블럭 삭제 : `Ctrl`{:.key} + `Del`{:.key}

### 이동관련
* 마지막 편집 위치로 이동 : `Ctrl`{:.key} + `K`{:.key} + `Q`{:.key}

* 단어 단위로 이동 : `Ctrl`{:.key} + `←`{:.key}/`→`{:.key}

* 커서 고정, 위 아래 스크롤 : `Ctrl`{:.key} + `↑`{:.key}/`↓`{:.key}

* 파일의 맨 앞/맨 뒤로 이동 : `Ctrl`{:.key} + `Home`{:.key}/`End`{:.key}

---
# Spring과 Spring Boot
## Spring
자바 플랫폼을 위한 **오픈 소스 어플리케이션 프레임워크**을 말한다.
**동적인 웹 사이트 개발**하기 위한 여러가지 서비스 제공한다.
대한민국 공공기관의 웹 서비스 개발 시 사용을 권장하고 있는 **전자정부 표준 프레임워크**의 기반 기술이다.

### Spring의 특징
* 경량 컨테이너 - 어플리케이션 개발에 필요한 기반을 제공해서 개발자가 비지니스 로직 구현에만 집중할 수 있다.
* 특정 인터페이스/클래스를 상속 받을 필요 없음(**POJO**;Plain Old Java Object;특정 '기술'에 종속되어 동작하는 것이 아닌 순수한 자바 객체)
    즉, Java가 가진 객체지향 설계의 장점을 그대로 유지하면서 여러 엔터프라이즈 기술들을 사용한다. (PSA;Portable Service Abstraction)
* **IoC**(Inversion of Control;제어 역전)/**DI**(Dependency Injection;의존성 주입), **AOP**(Aspect-Oriented Programming;관점 지향 프로그래밍)을 지원한다.
* 확장성이 높다.(**유지보수↑**)

### Spring Framework
IoC(Inversion of Control) 제어의 역전
: 사용할 객체의 생명주기 관리를 외부(스프링 컨테이너;Spring Container 또는 IoC Container) 에 위임한다.
제어의 역전을 통해 DI, AOP 등이 가능해진다.
이로 인해 개발자는 비지니스 로직을 작성하는데 더 집중할 수 있다.

DI(Dependency Injection) 의존성 주입
: 제어 역전의 방법 중 하나로 사용할 객체를 직접 생성하지 않고 외부 컨테이너가 생성한 객체를 주입받아 사용하는 방식을 의미한다.
스프링에서는 `@Autowired` 어노테이션(annotation)을 통해 의존성을 주입할 수 있다.
1. **생성자**를 통한 의존성 주입 (스프링 공식 문서에서 권장하는 방식)
    (레퍼런스 객체 없이는 객체를 초기화할 수 없게 설계할 수 있기 때문에)
2. **필드 객체 선언**을 통한 의존성 주입
3. **setter 메서드**를 통한 의존성 주입

> 의존성(Dependency) 이란?
: 프로그래밍에서 의존성은 다른 객체를 얼마만큼 구체적으로 아는지를 말한다.

> 의존성을 주입하는 이유는?
: 생성과 사용에 대한 관심을 분리하게 되면 생성에 대한 책임을 다른 누군가에 위임할 수 있고 필요에 따라 객체 생성 방식을 선택할 수 있기 때문이다.
최종적으로는 객체들이 가지는 강한 결합을 느슨하게 만들 수 있고 이는 설계의 유연성을 부여한다.

> 어노테이션(Annotation) 이란?
: 자바에서 주석이란 의미를 가진다.
1. 컴파일러에게 코드 작성시 **문법 에러를 체크**하도록 정보를 제공한다.
2. 소프트웨어 개발툴이 빌드나 배치 시 **코드를 자동으로 생성**할 수 있도록 정보를 제공한다.
3. 실행(런타임) 시 **특정 기능을 실행**하도록 정보를 제공한다.

AOP(Aspect-Oriented Programming) 관점 지향 프로그래밍
: 관점을 기준으로 묶어 개발하는 방식이다.
OOP(Object-Oriented Programming)를 더욱 잘 사용하도록 돕는 개념.
기능 구현에 핵심기능과 부가기능으로 구분해 각각을 하나의 관점으로 보는 것을 의미한다.
비지니스 로직에서 반복되는 부가기능을 하나의 공통로직으로 처리하도록 모듈화해 삽입하는 방식이다.

Bean Factory
: bean을 이용해 객체를 등록한다.
Spring에서 객체는 포괄적: 메서드, 변수, 클래스 등을 포괄적으로 bean으로 표현한다.

---
## Spring Boot
Spring에서 필요한 모듈들을 추가하다 보면 설정이 복잡해지는 문제를 해결하기 위해 등장했다.
별도의 복잡한 설정을 하지 않아도 쉽게 개발할 수 있다.

### Spring Boot의 특징
* 의존성 관리
* 자동 설정

> 프로젝트 생성 시 main 메서드가 존재하는 클래스에 붙는 `@SpringBootApplicaiton` 어노테이션은 크게 `@SpringBootConfiguration`과 `@EnalbeAutoConfiguration`, `@ComponentScan` 어노테이션이 합쳐친 구성이다.
`@ComponentScan`은 `@Component`시리즈 어노테이션이 붙은 클래스를 발견해 빈(bean)을 등록한다.
`@Component`시리즈의 대표적인 예로 `@Controller`, `@RestController`, `@Service`, `@Repository`, `@Configuration`등이 있다.

* 내장 WAS(Web Application Server)
* 모니터링

---
# Spring Boot 프로젝트 생성

`Ctrl`{:.key}+`Shift`{:.key}+`P`{:.key} => `Spring Initializr: Create Maven` =>

`v2.6.12` => `Java` => `com.example` => `demo` => `Jar` => `v11`

## Dependencies (8개)
### 프로젝트 설정 (Web/Template/데이터베이스 제어)
* Spring Web
* Thymeleaf
* Spring Data JPA

### DB 설정 (데이터베이스 종류)
* MariaDB Driver
* H2 Database
* MySqL Driver

### 개발환경 설정 (서버 자동 재시작/코드 자동 완성)
* Spring Boot DevTools
* Lombok

---
# Spring Boot 프로젝트 실행
## Controller 설정
> `file`\src\main\java\com\example\demo\controller\
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos%}
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

// @Controller: 다음에 오는 클래스를 컨트롤러 객체(bean)로 생성해서 등록
@Controller
public class HomeController {
    // @RequestMapping: 요청 url에 따라 해당 메서드를 실행
    @RequestMapping("/") // '/'는 최상위 폴더(주소)
                        // 즉, 최상위 url을 요청하면 아래 메서드 실행
    public String home(){
        return "home"; // Spring Boot에서는 문자열을 넘겨주면 문자열.html을 찾아 보여줌
    }
}
{% endhighlight java %}
컨트롤러에서 리턴 값으로 문자를 반환하면 뷰 리졸버(ViewResolver)가 화면을 찾아서 처리한다.
Spring Boot 템플릿엔진 기본 ViewName 매핑은 다음과 같다.

    resources:template/ + {ViewName} + .html

## 메인페이지 html 작성
> `file`\src\main\resources\templates\home.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">
<!--html주석-->
<!--html 태그가 있아야 함
xmlns:th 템플릿 사용 옵션-->

<head> <!-- 해당 페이지의 메타정보가 포함됨
    jave의 import처럼 참조할 css위치 등을 표시 -->
</head>

<body> <!-- 실제 화면에 출력될 요소를 포함-->
    <h1>Home : 홈페이지 입니다.</h1>
    <!--h1: head 문자열을 표현할 때 사용하는 태그
        숫자는 글자 크기-->
    <h2>h2</h2>
    <h3>h3</h3>
    <h4>h4</h4>
    <h5>h5</h5>
    <h6>h6</h6>
    <p>p: 기본 문자열을 나타내는 태그</p>
</body>

<footer> <!-- body 태그 안에 들어가는 경우도 있음. head, body 만 있어도 됨-->
    <h1>footer 입니다.</h1>
</footer>

</html>
{% endhighlight html %}

## `pom.xml` 수정
* Project Object Model
* 메이븐(Maven)의 기능을 사용하기 위해 작성하는 파일
* 프로젝트, 의존성 라이브러리, 빌드 등의 정보 및 해당 프로젝트를 관리하는 데 필요한 내용이 들어있다.
* DB연결관련 에러 발생 시 Spring Data JPA 주석 처리

> `file`\pom.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
{% raw %}
...

<dependencies>
		<!-- <dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-jpa</artifactId>
		</dependency> -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-thymeleaf</artifactId>
		</dependency>

...
{% endraw %}
{% endhighlight xml %}

---
## Slf4j - Log 사용
* `Lombok`: Spring에서 사용하는 여러가지 번거로운 작업을 어노테이션화 해서 제공하는 라이브러리
대표적으로 로그출력을 위한 `@Slf4j`, `@Getter`, `@Setter` 등이 있다.
* `Slf4j`: log를 남길 때 세부적으로 나눠 출력 가능

> `file`\src\main\java\com\example\demo\controller\
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos%}
package com.example.demo.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

import lombok.extern.slf4j.Slf4j;

@Controller
@Slf4j
public class HomeController {
    @RequestMapping("/")
    public String home(){
        log.trace("trace!");
        log.debug("debug!");
        log.info("info!");
        log.warn("warn!");
        log.error("error!");
        return "home";
    }
}
{% endhighlight %}

> `file`\application.properties
{: style="text-align: right"}
>Spring Boot Properties
{:.filename}
{% highlight bash %}
# log level
logging.level.com.example.demo=trace
{% endhighlight bash %}

### log 종류
* trace : debug보다 좀 더 상세한 정보를 나타낸다
* debug : 프로그램을 디버깅하기 위한 정보를 지정한다
* info : 상태변경과 같은 정보성 메시지를 나타낸다
* warn : 처리 가능한 문제, 향후 시스템 에러의 원인이 될 수 있는 경고성 메시지를 나타낸다
* error : 요청을 처리하는 중 문제가 발생한 경우 출력한다
        
>일반적으로 많은 개발자들이 System.out.println()을 사용하고 있다.
관공서 프로젝트에서 보안상 문제로 log를 출력, 남기면 안되는 경우도 있기 때문이다.

---
## Spring에서 제공하는 어노테이션
(Spring을 사용하기 위해 필수인 어노테이션)
* `@Controller` : 요청과 응답을 처리하는 클래스
요청에 대해 구체적인 처리는 Service에서 처리한다.
* `@Service` : 비지니스 로직을 구현하기 위해 사용하는 클래스

> `file`\src\main\java\com\example\demo\controller\
{: style="text-align: right"}
>Java
{:.filename}
{% highlight Java linenos %}
@RequestMapping("/login")
public String lonin(String id, String pw){
    id = datebaseIdValue // 로 작성하면 안됨. 정석은 Service 클래스에서 작성

    // 응답과 요청에 대한 결과만 나타내주는게 정석
    if(/*서비스에서 비지니스 로직이 끝난 결과값*/){
        // 로그인 성공 했을 경우 보여줄 페이지
    }else{
        // 로그인 실패 했을 경우 보여줄 페이지
    }
    return "login";
}
{% endhighlight %}

* `@Repository` : 서비스에서 요청을 받아 DB를 연결 + 결과값을 서비스에 반환하는 클래스

* `@Component` : 위 3가지의 어노테이션의 상위(부모) 객체로 클래스, 메서드, 변수 등을 포함하는 bean 자체로 등록한다. 

---
## MVC (Model View Controller) 패턴
(디자인 패턴의 일종)

    User - html(프론트엔드 혹은 화면) - Controller - Service - Repository - DB

모델이라는 클래스를 파라미터로 받아 정보를 모델에 담아 html로 보내주면 모델 객체안의 정보를 화면에 출력한다.
DB나 레파지토리에서 정보를 담을 그릇을 VO(Value Object), DTO(Data Transfer Object) 클래스라고 하고 DB의 테이블과 똑같이 만들어진다.

### 모델(Model)
* 데이터를 가진 객체
* 데이터는 내부에 상태 정보를 가질 수도 있고, 모델을 표현하는 이름을 속성으로 가질 수 있다.

1. 사용자가 편집하길 원하는 모든 데이터를 가지고 있어야만 한다.
2. 뷰(View)나 컨트롤러(Controller)에 대해서 어떤 정보도 알 수 없어야 한다.
3. 변경이 일어나면, 변경 통지에 대한 처리 방법을 구현해야만 한다.

### 뷰(View)
* 사용자 인터페이스(UI)의 요소
* 데이터 및 객체의 입력, 보여주는 출력을 담당한다.

1. 모델이 가지고 있는 정보를 따로 저장해서는 안된다.
2. 모델이나 컨트롤러과 같이 다른 구성 요소를 몰라야 된다.
3. 변경이 일어나면, 변경 통지에 대한 처리 방법을 구현해야만 한다.

### 컨트롤러(Controller)
* 데이터와 UI요소들을 잇는 다리 역할
* User가 데이터를 클릭하고 수정하는 것에 대한 이벤트들을 처리하는 부분이다.

1. 모델이나 뷰에 대해서 알고 있어야 한다.
2. 모델이나 뷰의 변경을 모니터링 해야 한다.

---
## Database 설정
DB는 일단 패스

---
# Reference

* 이 포스트는 SeSAC 인공지능 SW 개발자 양성 과정 - 김영식 강사님의 강의내용을 정리한 것입니다.
* siyoon210: [POJO - (Plain Old Java Object)란 뭘까?](https://siyoon210.tistory.com/120)
* 프로그래머 YD: [Java - 의존성 주입(Dependency Injection)이란?](https://7942yongdae.tistory.com/177)
* bangu4: [[Java] Annotation 어노테이션 - 총정리 ](https://bangu4.tistory.com/199)
* 쪽파코딩: [spring-boot View 환경 설정](https://tang-co.tistory.com/127)
* snippet: [모델-뷰-컨트롤러(Model-View-Controller MVC)](https://bsnippet.tistory.com/13)

