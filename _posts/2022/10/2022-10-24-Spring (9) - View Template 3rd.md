---
layout: post
title: Spring (9) - View Template 3rd
date:   2022-10-24
description: Thymeleaf 반복문과 Url링크
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
5. <span style="color:Turquoise">**View Template**</span> [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) <span style="color:SteelBlue">**3)**</span> [4)](/2022/10/Spring-(10)-View-Template-4th/) [5)](/2022/10/Spring-(11)-View-Template-5th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Thymeleaf
## Iteration, Conditional Evaluation
* ${#numbers.sequence(from, to, [step])}
: Thymeleaf의 Number Format 클래스 중 유틸리티 메서드인 sequence
from에서 to까지 step(default=1)차이만큼 정수의 시퀀스 생성한다.

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
        <th:block th:if="${page} == ${pageNumber}">
            <span th:text="${pageNumber}" style="font-weight: bold; color: blue;"></span>
        </th:block>
        <th:block th:unless="${page} == ${pageNumber}">
            <a th:href="@{/linkUrl(page=${pageNumber})}" th:text="${pageNumber}"
                style="color: crimson;"></a>
        </th:block>
    </th:block>
</body>

</html>
{% endhighlight html %}
> http://localhost:8080/linkUrl?page=23
![img]({{ '/assets/images/2022-10-24/img2.PNG' | relative_url }}){: .left-image }

---
## 연습1
* 구구단 출력페이지를 thymelaef a태그를 이용해 생성해보자

> `file`\src\main\resources\templates\home.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>Home 화면 입니다.</h1>
    <th:block th:each="danNum : ${#numbers.sequence(2, 9)}">
        <a th:href="@{/gugudan(dan=${danNum})}">구구단 자동생성 2단 ~ [[${danNum}]]단</a><br />
    </th:block>
</body>

</html>
{% endhighlight html %}
> http://localhost:8080/
![img]({{ '/assets/images/2022-10-24/img3.PNG' | relative_url }}){: .left-image }

---
## 연습2
* 홈화면에 userlist로 가는 항목을 추가해보자

> `file`\src\main\resources\templates\home.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<a href="userlist">사용자 목록</a>

...
{% endhighlight html %}

* 유저목록에서 이름을 누르면 해당 유저의 페이지로 가도록 해보자

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
            <td>
                <a th:href="@{member(userId=${member.userId})}">[[${member.name}]]</a>
            </td>
            <td th:text="${member.userPassword}"></td>
        </tr>
    </table>
</body>

</html>
{% endhighlight html %}

---
## 연습3
* 게시판 글 상세보기가 가능하도록 작성해보자

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("board")
public String board(Model model, @RequestParam(defaultValue = "1") int page) {
    int startPage = (page - 1) / 10 * 10 + 1;
    int endPage = startPage + 9;
    model.addAttribute("startPage", startPage);
    model.addAttribute("endPage", endPage);
    model.addAttribute("page", page);

    List<Board> boardList = new ArrayList<>();
    Board board = new Board();
    board.setBNo(1);
    board.setTitle("첫번째 글입니다.");
    board.setPublisher("kim");
    boardList.add(board);

    board = new Board();
    board.setBNo(2);
    board.setTitle("두번째 글입니다.");
    board.setPublisher("lee");
    boardList.add(board);

    board = new Board();
    board.setBNo(3);
    board.setTitle("세번째 글입니다.");
    board.setPublisher("park");
    boardList.add(board);

    model.addAttribute("boardList", boardList);
    return "board";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\board.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<table border="1">
    <tr>
        <td>번호</td>
        <td align="center">게시글 제목</td>
        <td>작성자</td>
    </tr>
    <tr th:each="board : ${boardList}">
        <td th:text="${board.bNo}"></a></td>
        <td>
            <a th:href="@{/boardDetail(bNo=${board.bNo})}">[[${board.title}]]</a>
        </td>
        <td th:text="${board.publisher}"></td>
    </tr>
</table>
<hr>
<th:block th:each="pageNumber : ${#numbers.sequence(startPage, endPage)}">
    <a th:if="${page}==${pageNumber}" th:text="${pageNumber}" style="font-weight:bold"></a>
    <a th:unless="${page}==${pageNumber}" th:href="@{/board(page=${pageNumber})}" th:text="${pageNumber}"></a>
</th:block>

...
{% endhighlight html %}
> http://localhost:8080/board
![img]({{ '/assets/images/2022-10-24/img4.PNG' | relative_url }}){: .left-image }

* 상세 페이지

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("boardDetail")
public String boardDetail(Model model, @RequestParam int bNo) {
    List<Board> boardList = new ArrayList<>();
    Board board = new Board();
    board.setBNo(1);
    board.setTitle("첫번째 글입니다.");
    board.setContent("첫번째 글 테스트 내용입니다");
    board.setPublisher("kim");
    boardList.add(board);

    board = new Board();
    board.setBNo(2);
    board.setTitle("두번째 글입니다.");
    board.setContent("두번째 글 테스트 내용입니다");
    board.setPublisher("lee");
    boardList.add(board);

    board = new Board();
    board.setBNo(3);
    board.setTitle("세번째 글입니다.");
    board.setContent("세번째 글 테스트 내용입니다");
    board.setPublisher("park");
    boardList.add(board);
    
    // 파라미터 bNo와 boardList 안에 있는 글의 글번호가 일치하면
    Board result = new Board();
    for (Board b : boardList) {
        if (b.getBNo() == bNo) {
            result = b;
        }
    }
    model.addAttribute("result", result);

    return "boardDetail";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\boardDetail.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<table border="1">
    <tr>
        <td>번호</td>
        <td>게시글 제목</td>
        <td>작성자</td>
    </tr>
    <tr>
        <td th:text="${result.bNo}"></td>
        <td th:text="${result.title}"></td>
        <td th:text="${result.publisher}"></td>
    </tr>
</table><hr>
<table border="1">
    <tr>
        <td>게시글 내용</td>
    </tr>
    <tr>
        <td th:text="${result.content}"></td>
    </tr>
</table>


...
{% endhighlight html %}
> 두번째 글입니다. `Click`{:.key}
![img]({{ '/assets/images/2022-10-24/img5.PNG' | relative_url }}){: .left-image }

---
## 연습4
* 로그인 직접 입력하기 (post요청)

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("login3")
public String login3() {
    return "login3";
}

@PostMapping("login3")
public String login3(Model model, @RequestParam("id") String id, @RequestParam("pw") String pw) {
    System.out.println("아이디 확인 : " + id);
    System.out.println("비밀번호 확인 : " + pw);
    model.addAttribute("id", id);
    model.addAttribute("pw", pw);
    return "loginResult";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\login3.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h1>로그인 post 메소드 테스트</h1>
<hr/>
<form action="/login3" method="post">
    <input type="text" placeholder="아이디" name="id" />
    <input type="password" placeholder="비밀번호" name="pw" />
    <input type="submit" value="로그인" /> <!-- submit : form타입에 요청 -->
</form>

...
{% endhighlight html %}

> `file`\src\main\resources\templates\loginResult.html1
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h1>로그인 post 메소드 테스트</h1>
<hr/>
<h3>[[${id}]] 님 환영합니다.</h3>
<h3>비밀번호 앞 두자리는 [[${#strings.substring(pw, 0, 2)}]] 입니다.</h3>

...
{% endhighlight html %}
> http://localhost:8080/login3
![img]({{ '/assets/images/2022-10-24/img6.PNG' | relative_url }}){: .left-image }

> 아이디: abc, 비밀번호: 12345 => `로그인`{:.key}
![img]({{ '/assets/images/2022-10-24/img7.PNG' | relative_url }}){: .left-image }

---
## 연습5 - Get, Post 매핑 활용
* 다음 조건을 만족시켜보자
1. Get요청으로 들어왔을 때는 adminLogin.html로 이동한다.
2. adminLogin.html에서는 form태그와 input태그를 활용해서 로그인 기능 적용한다.
3. adminLogin.html에서 로그인 시도하게 되면 adminLogin의 post매핑으로 연결한다.
4. 이때 파라미터는 id와 pw라고 한다.
5. 관리자 계정 정보는 id는 admin, pw는 1234이며 로그인 성공시 adminPage.html로 이동하고 로그인 실패시 loginFail.html로 이동한다.

> `file`\src\main\java\com\example\basic\controller\ThymeleafController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("adminLogin")
public String adminLogin() {
    return "adminLogin";
}

@PostMapping("adminLogin")
public String adminLogin(@RequestParam Map<String, Object> map) {
    String adminId = "admin";
    String adminPw = "1234";
    if (map.get("id").equals(adminId) && map.get("pw").equals(adminPw)) {
        return "adminPage";
    } else {
        return "loginFail";
    }
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\adminLogin.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h1>관리자 로그인 페이지</h1>
<hr/>
<form action="/adminLogin" method="post">
    <input type="text" placeholder="아이디" name="id" />
    <input type="password" placeholder="비밀번호" name="pw" />
    <input type="submit" value="로그인" />
</form>

...
{% endhighlight html %}
> http://localhost:8080/adminLogin
![img]({{ '/assets/images/2022-10-24/img8.PNG' | relative_url }}){: .left-image }

> 아이디: admin, 비밀번호: 1234 => `로그인`{:.key}
![img]({{ '/assets/images/2022-10-24/img9.PNG' | relative_url }}){: .left-image }

> 아이디: admin, 비밀번호: 123 => `로그인`{:.key}
![img]({{ '/assets/images/2022-10-24/img10.PNG' | relative_url }}){: .left-image }