---
layout: post
title: Spring (9) - View Template 3rd
date:   2022-10-24
description: Thymeleaf와 Controller에서 조건문 연습
toc: true
comments: true
tags:
 - spring boot
---
---
1. 개발 환경 준비 [1)](/2022/10/Spring-(1)-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%A4%80%EB%B9%84/)
2. Spring IoC [1)](/2022/10/Spring-(2)-Spring-IoC/) [2)](/2022/10/Spring-(3)-Spring-IoC-2nd/)
3. Spring MVC [1)](/2022/10/Spring-(4)-Spring-MVC/) [2)](/2022/10/Spring-(5)-Spring-MVC-2nd/) [3)](/2022/10/Spring-(6)-Spring-MVC-3rd/)
4. <del>Database 활용</del>
5. <span style="color:Turquoise">**View Template**</span> [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) <span style="color:SteelBlue">**3)**</span>
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Thymeleaf
## Iteration, Conditional Evaluation
* ${#numbers.sequence(from, to, [step])}
: Thymeleaf의 Number Format 클래스 중 유틸리티 메서드인 sequence
from에서 to까지 step(default=1)까지 정수의 시퀀스 생성한다.

* #~
: Thymeleaf에서 제공하는 기본 객체(자바의 클래스)이다.

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("pagination")
public String pagination(Model model, @RequestParam(defaultValue = "1") int page) {
    int startPage = (page - 1) / 10 * 10 + 1;
    int endPage = startPage + 9;
    model.addAttribute("startPage", startPage);
    model.addAttribute("endPage", endPage);
    model.addAttribute("page", page);
    return "pagination";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\pagination.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <th:block th:each="pageNumber : ${#numbers.sequence(startPage, endPage)}">
        <!-- style 속성 : 해당 요소를 꾸며주기 위해 사용 
                여러 속성값을 사용할 때에는 ;로 구분 -->
        <span th:if="${page} == ${pageNumber}" th:text="${pageNumber}"
            style="font-weight: bold; color: blue;"></span>
        <span th:unless="${page} == ${pageNumber}" th:text="${pageNumber}"
            style="color: crimson;"></span>
    </th:block>
</body>

</html>
{% endhighlight html %}
> http://localhost:8080/pagination?page=12
![img]({{ '/assets/images/2022-10-24/img1.PNG' | relative_url }}){: .left-image }

---
## Link Url Expression - @{ ... }
* th:href 사용 : @{주소(파라미터=값)}

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("linkUrl")
public String linkUrl(Model model, @RequestParam(defaultValue = "1") int page) {
    int startPage = 1;
    if (page > 4) {
        startPage = page - 4;
    }
    int endPage = startPage + 8;
    model.addAttribute("startPage", startPage);
    model.addAttribute("endPage", endPage);
    model.addAttribute("page", page);
    return "linkUrl";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\linkUrl.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <!-- a태그 : html에서 특정 주소 값으로 이동 시켜주는 버튼
            href속성 : a태그에서 사용하는 문법, 이동시킬 주소를 명시해준다. -->
    <!-- 기본 a태그 -->
    <a href="/">홈으로 이동</a><br />
    <th:block th:each="pageNumber : ${#numbers.sequence(startPage, endPage)}">
        <!-- 기본 a태그의 href속성에 thymeleaf 구문을 사용하면 인식이 안된다-->
        <!-- <a href="/linkUrl?page=$(pageNumber)">[[${pageNumber}]]</a> -->
        <!-- thymeleaf의 href사용 -->
        <a th:if="${page} == ${pageNumber}" th:text="${pageNumber}"
            style="font-weight: bold; color: blue;"></a>
        <a th:unless="${page} == ${pageNumber}" th:href="@{/linkUrl(page=${pageNumber})}"
            th:text="${pageNumber}"
            style="color: crimson;"></a>
    </th:block>
</body>

</html>
{% endhighlight html %}
> http://localhost:8080/linkUrl?page=23
![img]({{ '/assets/images/경로/2022-10-24/img2.PNG' | relative_url }}){: .left-image }

---
# Session



















> `file`\path\page.html
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...
Java code
...
{% endhighlight java %}

> `file`\path\page.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
HTML code
{% endhighlight html %}
> http://localhost:8080/접속주소
![img]({{ '/assets/images/2022-10-24/img.PNG' | relative_url }}){: .left-image }