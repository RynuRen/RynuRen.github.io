---
layout: post
title: Spring (10) - View Template 4th
date:   2022-10-25
description: Session
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
5. <span style="color:Turquoise">**View Template**</span> [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) [3)](/2022/10/Spring-/Spring-(9)-View-Template-3rd/) <span style="color:SteelBlue">**4)**</span>
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Session
- 클라이언트에 대한 정보를 서버에 저장할 수 있는 공간이다.
- 접속하는 클라이언트 당 하나의 세션을 생성한다.
- 사물함과 같은 형식으로 저장이 되며 사물함의 번호를 클라이언트로 전송한다.
- 번호를 분실하는 경우 새로운 세션을 생성하고 다시 클라이언트로 전송한다.
- 설문조사와 같이 여러단계로 입력된 정보나 로그인 정보 등 클라이언트가 접속되어 있는 동안 내용을 기억해야 하는 경우에 활용한다.

## `HttpSession`
* redirect: 특정 주소와 매핑 (return String일 때는 .html와 매핑)
* ${ ... }: EL(Expression Lagnuage)문법

> `file`\src\main\java\com\example\basic\controller\SessionController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@Controller
public class SessionController {
    // sessionLogin 페이지로 이동
    @GetMapping("sessionLogin")
    public String sessionLogin() {
        return "sessionLogin";
    }
    // sessionLogin 페이지에서 로그인 시도
    @PostMapping("sessionLogin")
    // session을 이용하기 위한 매개변수 선언
    public String sessionLogin(User user, HttpSession session) {
        System.out.println("userId : " + user.getUserId());
        System.out.println("userPw : " + user.getUserPw());
        session.setAttribute("user", user); // session에 user데이터를 user라는 이름으로 저장
        return "redirect:/main"; // http://localhost:8080/main 으로 요청
    }

    @GetMapping("main")
    public String main() {
        return "main";
    }

    @GetMapping("logout")
    public String logout(HttpSession session) {
        // session.removeAttribute("user"); // 특정 세션의 값을 삭제
        session.invalidate(); // 세션의 정보를 모두 제거
        return "redirect:/"; // http://localhost:8080 으로 요청
    }

}

...
{% endhighlight java %}

> `file`\src\main\java\com\example\basic\model\User.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
package com.example.basic.model;

import lombok.Data;

@Data
public class User {
    private String userId;
    private String userPw;
}
{% endhighlight java %}

> `file`\src\main\resources\templates\sessionLogin.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h1>세션 활용 로그인</h1>
<form action="/sessionLogin" method="post">
    <!-- input의 name값으로 파라미터를 줄수도 있지만,
        자바소스에서 model클래스 자체를 매개변수로 하고있다면,
        model클래스의 필드멤버와 name값을 일치 시켜준다. -->
    ID : <input type="text" placeholder="아이디" name="userId"/><br>
    PW : <input type="password" placeholder="비밀번호" name="userPw"/><br>
    <input type="submit" value="로그인"/>
</form>

...
{% endhighlight html %}
> http://localhost:8080/sessionLogin
![img]({{ '/assets/images/2022-10-25/img1.PNG' | relative_url }}){: .left-image }
> id에 abc, pw에 1234 => `Click`{:.key}
![img]({{ '/assets/images/2022-10-25/img2.PNG' | relative_url }}){: .left-image }

> `file`\src\main\resources\templates\home.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<th:block th:if="${session.user} == null">
    <a href="/sessionLogin">세션 로그인 하기</a><hr>
</th:block>
<th:block th:unless="${session.user} == null">
    <a href="/logout">로그아웃</a><hr>
</th:block>
{% endhighlight html %}

> `file`\src\main\resources\templates\main.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<h1>메인 페이지</h1>
<th:block th:if="${session.user} != null">
    <p>[[${session.user.userId}]] 님이 로그인 하셨습니다.</p>
    <form action="/gugudan" method="get">
        <input type="text" placeholder="2단부터 출력할 단수" name="dan" />
        <input type="submit" value="생성" />
    </form>
</th:block>
<th:block th:unless="${session.user} != null">
    <p>로그인 정보가 존재하지 않습니다.</p>
</th:block>
<hr>
<a href="/">홈으로</a>
{% endhighlight html %}
![img]({{ '/assets/images/2022-10-25/img3.PNG' | relative_url }}){: .left-image }
















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
![img]({{ '/assets/images/2022-10-25/img1.PNG' | relative_url }}){: .left-image }
