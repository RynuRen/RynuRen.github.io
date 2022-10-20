---
layout: post
title: Spring (6) - Spring MVC 3rd
date:   2022-10-20
description: PathVariable과 ModeAttribute를 이용한 요청처리
toc: true
tags:
 - spring boot
 - java
---
---
1. 개발 환경 준비
2. Spring IoC
3. **_Spring MVC_**
4. Database 활용
5. View Template
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# 요청 처리
## 요청 처리 시 사용하는 클래스와 어노테이션
* `HttpServletRequest` - 가장 전통적으로 사용되는 방식
* **`@RequestParam` (편리함)**
    파라미터 명칭에 맞게 변수 사용
    파라미터 종류 및 개수 상관없이 사용
* **`@PathVariable` (깔끔함)** - 요청 주소의 경로명 활용
* **`@ModelAttribute` (명확함)**
    Model / DTO / VO 등 객체와 연계하여 활용
    JPA, MyBatis 등 ORM 프레임워크 활용
* `@RequestBody` (AJAX 요청 시 주로 사용)
    보편적인 요청 파라미터 형식을 사용하지 않고 JSON 형태의 파라미터 사용
(Query String Parameter, Form Data, Payload)
    사용 시 메소드 방식을 POST로 지정

---
### `@PathVariable`
* 주소창에 입력된 주소에서 값을 동적으로 받아옴

* \src\main\java\com\example\basic\controller\RequustController.java

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("req/path/{path1}/{path2}")
public String path(
        @PathVariable("path1") String path1,
        @PathVariable("path2") String path2) {
    return path1 + ", " + path2;
}

...
{% endhighlight %}
요청
>http://localhost:8080/req/path/1st_path/2nd_path

응답
>1st_path, 2nd_path

* 요청을 처리하는 URL에 { 주소값 } 형식으로 지정
* `@PathVariable`("주소값") String 변수명 => 주소값이 변수명으로 들어감

---
### `@ModelAttribute`
* DTO의 변수들이 파라미터가 되어 전송

* \src\main\java\com\example\basic\controller\HtmlController.java

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("/signup")
public String signUp(@ModelAttribute Member member) {
    return "signUp";
}

...
{% endhighlight %}

* \src\main\resources\templates\signUp.html

>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>Member 정보</h1>
    <hr /><!-- 여는 태그와 닫는 태그를 한번에 표현 -->
    <h2>[[${member}]]</h2>
    <h2>사용자 이름 : [[${member.name}]]</h2><br /><!-- 줄바꿈 -->
    <h2>사용자 아이디 : [[${member.userId}]]</h2><br>
    <h2>사용자 비밀번호 : [[${member.userPassword}]]</h2><br>
</body>

</html>
{% endhighlight %}
요청
>http://localhost:8080/signup?name=val1&userId=val2&userPassword=val3

응답
>
{% highlight bash %}
Member 정보
Member(name=val1, userId=val2, userPassword=val3)
사용자 이름 : val1

사용자 아이디 : val2

사용자 비밀번호 : val3
{% endhighlight %}

* Model 클래스인 Member 클래스 안에 있는 변수명으로 파라미터를 받아와 지정한 html에 member라는 이름으로 데이터가 전송
* html에서는 전송받은 member라는 이름으로 데이터를 사용할 수 있다. (thymleaf 사용)


### 연습
* 클라이언트의 요청 파라미터에 따라 응답이 달라지도록 작성하기

* \src\main\java\com\example\basic\controller\RequustController.java

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("req/data")
public Map<String, Object> dataArea(@RequestParam Map<String, Object> map) {
    return map;
}

...
{% endhighlight %}
요청
>http://localhost:8080/req/data?area=제주도&score=100

응답
>{"area":"제주도","score":"100"}