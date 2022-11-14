---
title: Spring (4) - Spring MVC
published: true
date:   2022-10-18
description: MVC 개념과 Controller에 대해, RequestMapping을 이용한 응답처리
comments: true
categories:
 - spring boot
tags:
 - spring boot
 - java
toc: true
toc_sticky: true
toc_label: Spring MVC
---
---
1. 개발 환경 준비 [1)](/2022/10/Spring-(1)-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%A4%80%EB%B9%84/)
2. Spring IoC [1)](/2022/10/Spring-(2)-Spring-IoC/) [2)](/2022/10/Spring-(3)-Spring-IoC-2nd/)
3. <span style="color:Turquoise">**Spring MVC**</span> <span style="color:SteelBlue">**1)**</span> [2)](/2022/10/Spring-(5)-Spring-MVC-2nd/) [3)](/2022/10/Spring-(6)-Spring-MVC-3rd/)
4. ~~Database 활용~~
5. View Template [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) [3)](/2022/10/Spring-(9)-View-Template-3rd/) [4)](/2022/10/Spring-(10)-View-Template-4th/) [5)](/2022/10/Spring-(11)-View-Template-5th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Spring Web MVC 구조
* Model-View-Controller
* Presentation과 Business를 분리시키기 위해 사용한다.
* MVC 아키텍처 (FrontControler / Command 패턴) 흐름

![img]({{ '/assets/images/2022/10/18/img1.PNG' | relative_url }}){: .center-image }*MVC*

* DispatcherServlet이 FrontController 역할을 담당
    요청(request)에 따라 해당 Controller로 매핑
    응답(response)으로 보여줄 View 처리


---
## DB에서 수행하는 작업 (CRUD)
웹 개발의 지표

* 삽입 **C**reate
* 조회 **R**ead
* 수정 **U**pdate
* 삭제 **D**elete

---
# Controller 에서 주로 사용되는 Annotation

![img]({{ '/assets/images/2022/10/18/img2.PNG' | relative_url }}){: .center-image }*Controller Annotation*

* `@RestController` : `@Controller` + `@ResponseBody`
* `@RequestMapping` : 특정 주소로 입력됐을 때 실행할 메소드와 매핑
* `@GetMapping` : HTTP Get 요청을 특정 handler 메소드와 매핑
* `@RequestParam` : 메소드 파라미터(매개변수)를 web 요청 파라미터와 결합시킨다.
    url 뒤에 붙는 파라미터값을 가져올 때 사용한다.

>http://www.주소.com/longin?id=아이디&pw=패스워드
url 주소의 ? 뒤가 파라미터(매개변수), = 기준으로 Lv는 변수명, Rv는 값

* `@ModelAttribute`: web view에 노출할 model attribute에 반환할 메소드나 메소드 파라미터를 묶어준다.
* `@RequstBody`: REST 통신을 하는 메소드
    대표적 REST통신: AJAX기술 (새로고침 없이 페이지를 업데이트)

---
# 응답 처리

## `@RequestMapping`
web 요청을 메소드로 매핑

* /second 주소가 들어왔을 때 (localhost:8080/second 혹은 127.0.0.1:8080/second) 작동하는 메소드 작성해보자

> `file`\src\main\java\com\example\demo\controller\HomeController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@RequestMapping("/second")
public String second() {
    return "second";
}

...
{% endhighlight %}

---
# Reference

* 이 포스트는 SeSAC 인공지능 SW 개발자 양성 과정 - 김영식 강사님의 강의내용을 정리한 것입니다.