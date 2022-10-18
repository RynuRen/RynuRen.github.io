---
layout: post
title: Spring (4) - Spring MVC
date:   2022-10-18
description: Spring (4) - Spring MVC
toc: true
tags:
 - spring boot
---

1. 개발 환경 준비
2. Spring IoC
3. **_Spring MVC_**
4. Database 활용
5. View Template
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Spring Web MVC 구조
* Model-View-Controller
* Presentation 과 Business 를 분리시키기 위해 사용
* MVC 아키텍처의 일반적인 흐름

![img]({{ '\assets\images\2022-10-18\img1.PNG' | relative_url }}){: .center-image }*MVC*

---
## DB에서 수행하는 작업 (CRUD)
웹 개발의 지표

* 삽입 create
* 조회 read
* 수정 update
* 삭제 delete

---
# Controller 에서 주로 사용되는 Annotation

![img]({{ '\assets\images\2022-10-18\img2.PNG' | relative_url }}){: .center-image }*Controller Annotation*

* `@RestController`: `@Controller` + `@ResponseBody`
* `@RequestMapping`: 특정 주소로 입력됐을 때 실행할 메소드와 매핑
* `@GetMapping`: HTTP Get 요청을 특정 handler 메소드와 매핑
* `@RequestParam`: 메소드 파라미터(매개변수)가 web 요청 파라미터와 결합
url 뒤에 붙는 파라미터값을 가져올 때 사용

>http://www.주소.com/longin?id=아이디&pw=패스워드

url 주소의 ? 뒤가 파라미터(매개변수), = 기준으로 Lv는 변수명, Rv는 값

`@ModelAttribute`: web view에 노출할 model attribute에 반환할 메소드나 메소드 파라미터를 묶어줌
`@ResponseBody`: REST 통신을 하는 메소드
대표적 REST통신: AJAX기술 (새로고침 없이 페이지를 업데이트)

---
# 응답 처리

## @RequestMapping(value="")
* web 요청을 메소드로 매핑
* /second 주소가 들어왔을 때 (즉 localhost:8080/second 혹은 127.0.0.1:8080/second)
* \src\main\java\com\example\demo\controller\HomeController.java

>Java
{:.filename}
{% highlight java linenos %}
@RequestMapping("/second")
public String second() {
    return "second";
}
{% endhighlight %}