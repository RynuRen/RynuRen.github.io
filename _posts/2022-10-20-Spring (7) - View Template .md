---
layout: post
title: Spring (7) - View Template
date:   2022-10-20
description: Spring (7) - View Template
toc: true
tags:
 - spring boot
---
---
1. 개발 환경 준비
2. Spring IoC
3. Spring MVC
4. Database 활용
5. **_View Template_**
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Thymeleaf
- 스프링 부트에서 권장하는 View Template
- HTML5 문법을 사용하는 HTML 태그 및 속성 기반의 Template Engine
- 텍스트, HTML, XML, JavaScript, CSS 등 사용 가능
- Controller에서 View로 넘겨준 Model을 이용하여 내용 출력

>HTML
{:.filename}
{% highlight html %}
<h3>[[${list}]]</h3>
{% endhighlight %}

* \src\main\java\com\example\basic\controller\HtmlController.java

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

* \src\main\resources\templates\welcome.html

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

---
## Variable Expression : ${...}

* \src\main\java\com\example\basic\controller\ThymeleafController.java

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

* \src\main\resources\templates\user.html

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

* 문법의 차이에 따라 3가지 방법이 있다
1. 단순 문자열 데이터 표현 방법
2. 열리는 태그 안에 속성값을 넣는 방식, 기능을 나타내는 문법(text표현)

---
## Iteration - th:each
* 반복작업을 표현하는 방법 (반복문)

* \src\main\java\com\example\basic\controller\ThymeleafController.java

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

* \src\main\resources\templates\userList.html

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

---
## Conditional Evaluaiton - th:if, th:unless, th:switch

* Thymeleaf의 if, else, switch 조건문

* \src\main\java\com\example\basic\controller\ThymeleafController.java

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

* \src\main\resources\templates\mode.html

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

---
## 연습

* Thymeleaf 조건문 활용 연습

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

* 파라미터 userId 값에 따라 해당하는 userId 값과 동일한 유저정보만 출력
* 예) http://localhost:8080/member?userId=1 접속 시 홍길동의 정보만 출력

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