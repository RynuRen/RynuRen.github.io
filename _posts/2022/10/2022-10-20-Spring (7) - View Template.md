---
layout: post
title: Spring (7) - View Template
date:   2022-10-20
description: View Template 중 하나인 Thymeleaf의 사용법, 반복문, 조건문
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
5. <span style="color:Turquoise">**View Template**</span> <span style="color:SteelBlue">**1)**</span> [2)](/2022/10/Spring-(8)-View-Template 2nd/) [3)](/2022/10/Spring-(9)-View-Template-3rd/) [4)](/2022/10/Spring-(10)-View-Template-4th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Thymeleaf
- Spring Boot 에서 권장하는 View Template
- HTML5 문법을 사용하는 HTML 태그 및 속성 기반의 Template Engine
- 텍스트, HTML, XML, JavaScript, CSS 등에 사용 가능하다.
- Controller에서 View로 넘겨준 Model을 이용하여 내용을 출력한다.

>HTML
{:.filename}
{% highlight html %}
<h3>[[${list}]]</h3>
{% endhighlight %}

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("welcome")
public String welcome(Model model){
    ArrayList<String> list = new ArrayList<>();
    list.add("list1");
    list.add("list2");
    list.add("list3");
    model.addAttribute("list", list);

    Map<String, Object> map = new HashMap<>();
    map.put("key1", "value1");
    map.put("key2", 22);
    model.addAttribute("map", map);

    Member member = new Member();
    member.setName("이름");
    member.setUserId("아이디");
    member.setUserPassword("비밀번호");
    model.addAttribute("member", member);

    model.addAttribute("message", "타임리프");
    return "welcome";
}

...
{% endhighlight %}

> `file`\src\main\resources\templates\welcome.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>List 정보 확인</h1>
    <h3>[[${list}]]</h3>
    <h3>[[${list[0]}]]</h3>

    <h1>Map 정보 확인</h1>
    <h3>[[${map}]]</h3>
    <h3>[[${map.key1}]]</h3>
    <h3>[[${map['key2']}]]</h3>

    <h1>Member 정보 확인</h1>
    <h3>[[${member}]]</h3>
    <h3>[[${member['userId']}]]</h3>
    <h3>[[${member.userPassword}]]</h3>

    <h1>Message</h1>
    <h3>[[${message}]]</h3>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/welcome
![img]({{ '/assets/images/2022-10-20/img4.PNG' | relative_url }}){: .left-image }

---
## Variable Expression : ${...}

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
package com.example.basic.controller;

import java.util.HashMap;
import java.util.Map;

import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class ThymeleafController {
    @GetMapping("user")
    public String user(Model model) {
        Map<String, Object> user =  new HashMap<>();
        user.put("userId", "z");
        user.put("userName", "zoo");
        user.put("userAge", 25);
        model.addAttribute("user", user);
        return "user";
    }
}
{% endhighlight %}

> `file`\src\main\resources\templates\user.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <!-- thymeleaf 표현법 -->
    <!-- 1. 열리는 tag와 닫히는 tag사이에 표현 -->
    <!-- span 태그 : 별도의 영역이 없는 태그,
            데이터의 크기만큼 영역이 잡힘 -->
    아 이 디:<span>[[${user.userId}]]</span><br>
    이    름:<span>[[${user.userName}]]</span><br>
    비밀번호:<span>[[${user.userPw}]]</span><br>
    <hr>
    <!-- 2. 열리는 tag에 th:속성명="값" 으로 표현 -->
    아 이 디:<span th:text="${user.userId}"></span><br>
    이    름:<span th:text="${user.userName}"></span><br>
    비밀번호:<span th:text="${user.userPw}"></span><br>
    <hr>
    <!-- 3. 열리는 tag에 data-th-속성명="값" 으로 표현 -->
    아 이 디:<span data-th-text="${user.userId}"></span><br>
    이    름:<span data-th-text="${user.userName}"></span><br>
    비밀번호:<span data-th-text="${user.userPw}"></span><br>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/user
![img]({{ '/assets/images/2022-10-20/img5.PNG' | relative_url }}){: .left-image }

* 문법의 차이에 따라 여러 방법이 있다.
1. 단순 문자열 데이터 표현 방법
2. 열리는 태그 안에 속성값을 넣는 방식, 기능을 나타내는 문법(text표현)

---
## Iteration - th:each
* 반복작업을 표현하는 방법 (반복문)

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("userlist")
public String userList(Model model) {
    List<Member> userList = new ArrayList<>();
    Member member = new Member();
    member.setName("홍길동");
    member.setUserId("hong");
    member.setUserPassword("1");
    userList.add(member);

    member = new Member();
    member.setName("이순신");
    member.setUserId("lee");
    member.setUserPassword("2");
    userList.add(member);

    member = new Member();
    member.setName("임꺽정");
    member.setUserId("lim");
    member.setUserPassword("3");
    userList.add(member);
    
    model.addAttribute("userList", userList);
    return "userList";
}

...
{% endhighlight %}

> `file`\src\main\resources\templates\userList.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <table border="1">
        <tr>
            <td>아이디</td>
            <td>이름</td>
            <td>비밀번호</td>
        </tr>
        <tr th:each="member : ${memberList}">
            <td th:text="${member.userId}"></td>
            <td th:text="${member.Name}"></td>
            <td th:text="${member.userPassword}"></td>
        </tr>
    </table>
    <hr>
    <th:block th:each="pageNumber : ${#numbers.sequence(1, 10)}">
        <span th:text="${pageNumber}"></span>
    </th:block>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/userlist
![img]({{ '/assets/images/2022-10-20/img6.PNG' | relative_url }}){: .left-image }

---
## Conditional Evaluaiton - th:if, th:unless, th:switch

* Thymeleaf의 if, else, switch 조건문

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("mode")
public String mode(Model model, @RequestParam Map<String, Object> map) {
    // 동적 파라미터의 데이터가 잘 들어오는지 체크
    // for (String data : map.keySet()) {
    //     System.out.println(map.get(data));
    // }
    model.addAttribute("name", map.get("name"));
    model.addAttribute("auth", map.get("auth"));
    model.addAttribute("category", map.get("category"));
    return "mode";
}

...
{% endhighlight %}

> `file`\src\main\resources\templates\mode.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>타임리프 조건문 활용</h1>
    관리자 이름 :
    <!-- 조건식이 참일 경우 -->
    <span th:if="${name} != null" th:text="${name}"></span>
    <!-- 조건식이 거짓일 경우 -->
    <span th:unless="${name} != null" th:text="이름없음"></span>
    <br>
    권한 :
    <!-- 삼항 연산자 -->
    <span th:text="${auth} != null ? ${auth} : '권한없음'"></span>
    <br>
    담당 카테고리 :
    <!-- switch문 -->
    <span th:switch="${category}">
        <span th:case="1">커뮤니티</span>
        <span th:case="2">장터</span>
        <span th:case="3">갤러리</span>
    </span>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/mode
![img]({{ '/assets/images/2022-10-20/img7.PNG' | relative_url }}){: .left-image }

> http://localhost:8080/mode?name=사용자&auth=456&category=3
![img]({{ '/assets/images/2022-10-20/img8.PNG' | relative_url }}){: .left-image }

---
## 연습

* Thymeleaf 조건문 활용 연습

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("member")
public String gerUser(Model model, @RequestParam String userId){
    List<Member> memberList = new ArrayList<>();
    Member member = new Member();
    member.setName("홍길동");
    member.setUserId("1");
    member.setUserPassword("123");
    memberList.add(member);

    member = new Member();
    member.setName("이순신");
    member.setUserId("2");
    member.setUserPassword("456");
    memberList.add(member);

    member = new Member();
    member.setName("임꺽정");
    member.setUserId("3");
    member.setUserPassword("789");
    memberList.add(member);

    model.addAttribute("memberList", memberList);
    model.addAttribute("userId", userId);
    return "member";
}

...
{% endhighlight %}

* 파라미터 userId 값에 따라 해당하는 userId 값과 동일한 유저정보만 출력되게 작성해보자
* 예) http://localhost:8080/member?userId=1 접속 시 홍길동의 정보만 출력

* each반복문과 if문 사용

> `file`\src\main\resources\templates\member.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <table border="1">
        <tr>
            <td>아이디</td>
            <td>이름</td>
            <td>비밀번호</td>
        </tr>
        <tr th:each="member : ${memberList}">
            <span th:if="${userId} == ${member.userId}">
                <td th:text="${member.userId}"></td>
                <td th:text="${member.name}"></td>
                <td th:text="${member.userPassword}"></td>
            </span>
        </tr>
</body>

</html>
{% endhighlight %}

* switch문 활용

>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <table border="1">
        <tr>
            <td>아이디</td>
            <td>이름</td>
            <td>비밀번호</td>
        </tr>
        <span th:switch="${userId}">
            <tr th:case="1">
                <td th:text="${memberList[0].userId}"></td>
                <td th:text="${memberList[0].name}"></td>
                <td th:text="${memberList[0].userPassword}"></td>
            </tr>
            <tr th:case="2">
                <td th:text="${memberList[1].userId}"></td>
                <td th:text="${memberList[1].name}"></td>
                <td th:text="${memberList[1].userPassword}"></td>
            </tr>
            <tr th:case="3">
                <td th:text="${memberList[2].userId}"></td>
                <td th:text="${memberList[2].name}"></td>
                <td th:text="${memberList[2].userPassword}"></td>
            </tr>
        </span>
</body>

</html>
{% endhighlight %}
> http://localhost:8080/member?userId=2
![img]({{ '/assets/images/2022-10-20/img9.PNG' | relative_url }}){: .left-image }