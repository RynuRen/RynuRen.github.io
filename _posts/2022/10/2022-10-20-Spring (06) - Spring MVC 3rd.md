---
title: Spring (6) - Spring MVC 3rd
published: true
date:   2022-10-20
description: PathVariable과 ModeAttribute를 이용한 요청처리
comments: true
categories:
 - spring boot
tags:
 - spring boot
 - java
toc: true
toc_sticky: true
toc_label: Spring MVC 3rd
---
---
1. 개발 환경 준비 [1)](/spring%20boot/Spring-(01)-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%A4%80%EB%B9%84/)
2. Spring IoC [1)](/spring%20boot/Spring-(02)-Spring-IoC/) [2)](/spring%20boot/Spring-(03)-Spring-IoC-2nd/)
3. <span style="color:Turquoise">**Spring MVC**</span> [1)](/spring%20boot/Spring-(04)-Spring-MVC/) [2)](/2022/10/Spring-(5)-Spring-MVC-2nd/) <span style="color:SteelBlue">**3)**</span> 
4. ~~Database 활용~~
5. View Template [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) [3)](/2022/10/Spring-(9)-View-Template-3rd/) [4)](/2022/10/Spring-(10)-View-Template-4th/) [5)](/2022/10/Spring-(11)-View-Template-5th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# 요청 처리
## 요청 처리 시 사용하는 클래스와 어노테이션
### `@PathVariable`
* 주소창에 입력된 주소에서 값을 동적으로 받아온다.
* 요청을 처리하는 URL에 { 주소값 } 형식으로 지정한다.
* `@PathVariable`("주소값") String 변수명 => 주소값이 변수명으로 들어간다.

> `file`\src\main\java\com\example\basic\controller\RequustController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...
@RestController
public class RequustController {
    @GetMapping("req/path/{path1}/{path2}")
    public String path(
            @PathVariable("path1") String path1,
            @PathVariable("path2") String path2) {
        return path1 + ", " + path2;
    }

...
{% endhighlight %}
> http://localhost:8080/req/path/1st_path/2nd_path
![img]({{ '/assets/images/2022/10/20/img1.PNG' | relative_url }}){: .left-image }

---
### `@ModelAttribute`
* DTO의 변수들이 파라미터가 되어 전송된다.
* Model 클래스인 Member 클래스 안에 있는 변수명으로 파라미터를 받아와 지정한 html에 member라는 이름으로 데이터가 전송
* html에서는 전송받은 member라는 이름으로 데이터를 사용할 수 있다. (thymleaf 사용)

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@Controller
public class HtmlController {
    @GetMapping("/signup")
    public String signUp(@ModelAttribute Member member) {
        return "signUp";
    }

...
{% endhighlight %}

> `file`\src\main\resources\templates\signUp.html
{: style="text-align: right"}
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
> http://localhost:8080/signup?name=val1&userId=val2&userPassword=val3
![img]({{ '/assets/images/2022/10/20/img2.PNG' | relative_url }}){: .left-image }

---
### 연습
* 클라이언트의 요청 파라미터에 따라 응답이 달라지도록 작성해보자

> `file`\src\main\java\com\example\basic\controller\RequustController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...
@RestController
public class RequustController {
    @GetMapping("req/data")
    public Map<String, Object> dataArea(@RequestParam Map<String, Object> map) {
        return map;
    }

...
{% endhighlight %}
> http://localhost:8080/req/data?area=제주도&score=100
![img]({{ '/assets/images/2022/10/20/img3.PNG' | relative_url }}){: .left-image }

---
# Reference

* 이 포스트는 SeSAC 인공지능 SW 개발자 양성 과정 - 김영식 강사님의 강의내용을 정리한 것입니다.