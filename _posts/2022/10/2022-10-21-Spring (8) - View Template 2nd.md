---
layout: post
title: Spring (8) - View Template 2nd
date:   2022-10-21
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
4. ~~Database 활용~~
5. <span style="color:Turquoise">**View Template**</span> [1)](/2022/10/Spring-(7)-View-Template/) <span style="color:SteelBlue">**2)**</span> [3)](/2022/10/Spring-(9)-View-Template-3rd/) [4)](/2022/10/Spring-(10)-View-Template-4th/) [5)](/2022/10/Spring-(11)-View-Template-5th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Thymeleaf
> 문자열을 조합하는 방법

>HTML
{:.filename}
{% highlight html linenos %}
<span>문자열 [[${변수}]] 문자열</span>

<span th:text="'문자열 ' + ${변수} + ' 문자열'"></span>

<span th:text="|문자열 ${변수} 문자열|"></span>
{% endhighlight %}

---
## 조건문 연습1

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("admin")
public String admin(Model model, @RequestParam String userId){
    model.addAttribute("userId", userId);
    return "admin";
}

...
{% endhighlight %}

> `file`\src\main\resources\templates\admin.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <!-- th:black thymeleaf 문법을 사용하기 위한 전용 블록으로 화면에 출력되지 않음 -->
    <!-- if문 사용 -->
    <th:block th:if="${userId} == admin">
        <h1>관리자님 환영합니다.</h1>
    </th:block>
    <th:block th:unless="${userId} == admin">
        <h1>[[${userId}]] 님 안녕하세요.</h1>
    </th:block>
    <!-- <h1 th:if="${userId} == admin" th:text="'관리자님 환영합니다.'"></h1>
    <h1 th:unless="${userId} == admin" th:text="${userId} + ' 님 안녕하세요.'"></h1>
    <h1 th:unless="${userId} == admin" th:text="|${userId} 님 안녕하세요.|"></h1> -->
    <hr>

    <!-- switch문 사용 -->
    <th:block th:switch="${userId}">
        <h1 th:case="admin" th:text="'관리자님 환영합니다.'"></h1>
        <h1 th:case="*" th:text="|${userId} 님 안녕하세요.|"></h1>
    </th:block>
    <hr />

    <!-- 삼항연산자 사용 -->
    <h1 th:text="${userId} != admin ? |${userId} 님 안녕하세요.| : '관리자님 환영합니다.'"></h1>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/admin?userId=admin
![img]({{ '/assets/images/2022-10-21/img1.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/admin?userId=RynuRen
![img]({{ '/assets/images/2022-10-21/img2.PNG' | relative_url }}){: .left-image }

---
## 조건문 연습2 - Controller

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("admin2")
public String admin2(Model model, @RequestParam String userId) {
    String result = "";
    if (userId.equals("admin")) {
        result = "관리자님 어서오세요.";
    } else {
        result = userId + " 님 환영합니다.";
    }
    model.addAttribute("result", result);
    return "admin2";
}

...
{% endhighlight %}

> `file`\src\main\resources\templates\admin2.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>[[${result}]]</h1>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/admin2?userId=admin
![img]({{ '/assets/images/2022-10-21/img3.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/admin2?userId=RynuRen
![img]({{ '/assets/images/2022-10-21/img4.PNG' | relative_url }}){: .left-image }

---
## 조건문 연습3
* 구구단 자동생성 페이지
* localhost:8080/gugudan?dan=5 -> 2단부터 5단까지 출력하는 페이지 구현해보자

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("gugudan")
public String gugudan(Model model, @RequestParam int dan) {
    List<String[]> gugu = new ArrayList<>();
    for (int i = 2; i <= dan; i++) {
        String tmp = "";
        String[] arr = new String[2];
        arr[0] = Integer.toString(i);
        for (int j = 1; j < 10; j++) {
            tmp += i + " × " + j + " ＝ " + i * j + "<br />";
        }
        tmp += "<br />";
        arr[1] = tmp;
        gugu.add(arr);
    }
    model.addAttribute("gugu", gugu);
    return "gugudan";
}

...
{% endhighlight %}

> `file`\src\main\resources\templates\gugudan.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>구구단 출력 페이지</h1>
    <th:block th:each="dan : ${gugu}">
        <h2 th:text="|--- ${dan[0]}단 ---|"></h2>
        <span th:utext="${dan[1]}"></span>
    </th:block>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/gugudan?dan=3
![img]({{ '/assets/images/2022-10-21/img5.PNG' | relative_url }}){: .left-image }

---
## 조건문 연습4
* 로그인 기능 구현
* localhost:8080/login?id=admin&pw=1234 -> 성공적으로 로그인 되었습니다.
* 아이디가 틀렸을 경우 -> 아이디를 확인 해주세요.
* 아이디를 정상입력, 비밀번호가 틀렸을 경우 -> 비밀번호를 확인 해주세요.

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("login")
public String login(Model model, @RequestParam String id, @RequestParam String pw) {
    String result = "";
    if (id.equals("admin") && pw.equals("1234")) {
        result = "성공적으로 로그인 되었습니다.";
    } else if (!id.equals("admin")) {
        result = "아이디를 확인 해주세요.";
    } else {
        result = "비밀번호를 확인 해주세요.";
    }
    model.addAttribute("result", result);
    return "login";
}

...
{% endhighlight %}

> `file`\src\main\resources\templates\login.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>로그인 페이지</h1>
    [[${result}]]
</body>

</html>
{% endhighlight %}
> http://localhost:8080/login?id=admin&pw=1234
![img]({{ '/assets/images/2022-10-21/img6.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/login?id=admi&pw=123
![img]({{ '/assets/images/2022-10-21/img7.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/login?id=admin&pw=123
![img]({{ '/assets/images/2022-10-21/img8.PNG' | relative_url }}){: .left-image }

---
## 조건문 연습5
* 로그인 기능 구현 고도화
* localhost:8080/login2 -> home.html 이동
* localhost:8080/login2?id=admin&pw=1234 -> admin.html 이동
* localhost:8080/login2?id=user&pw=5678 -> userPage.html 이동
* 로그인 실패 시 -> loginFail.html 이동
* 로그인 기능의 아이디, 비밀번호 체크는 포함되어야 한다.

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
@GetMapping("login2")
public String login2(Model model, @RequestParam(defaultValue = "") String id,
        @RequestParam(defaultValue = "") String pw) {
    if (id.isEmpty()) {
        return "home";
    } else if (id.equals("admin") && pw.equals("1234")) {
        model.addAttribute("userId", id);
        return "admin";
    } else if (id.equals("user") && pw.equals("5678")) {
        model.addAttribute("userId", id);
        return "userPage";
    } else {
        return "loginFail";
    }
}
{% endhighlight %}

* Map을 이용한 풀이

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
@GetMapping("login2")
public String login2(Model model, @RequestParam Map<String, Object> map) {
    if (map.isEmpty()) {
        return "home";
    } else if (map.get("id").equals("admin") && map.get("pw").equals("1234")) {
        model.addAttribute("userId", "admin");
        return "admin";
    } else if (map.get("id").equals("user") && map.get("pw").equals("5678")) {
        model.addAttribute("userId", "user");
        return "userPage";
    } else {
        return "loginFail";
    }
}
{% endhighlight %}
> http://localhost:8080/login2
![img]({{ '/assets/images/2022-10-21/img9.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/login2?id=admin&pw=1234
![img]({{ '/assets/images/2022-10-21/img1.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/login2?id=user&pw=5678
![img]({{ '/assets/images/2022-10-21/img10.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/login2?id=user&pw=567
![img]({{ '/assets/images/2022-10-21/img11.PNG' | relative_url }}){: .left-image }