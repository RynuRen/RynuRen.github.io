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
5. <span style="color:Turquoise">**View Template**</span> [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) [3)](/2022/10/Spring-(9)-View-Template-3rd/) <span style="color:SteelBlue">**4)**</span>
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

## 연습 세션만으로 새 게시판 프로젝트 작성
* p태그 : 가로줄 전체를 범위로 잡는다.

* 홈화면 설정

> `file`\src\main\java\com\example\sesac\first\controller\HomeController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
package com.example.sesac.first.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {
    @GetMapping("/")
    public String home() {
        return "home";
    }
}
{% endhighlight java %}

> `file`\path\page.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>Home 화면</h1>
    <a href="/user/join">회원가입</a> | 
    <th:block th:if="${session.user} == null">
        <a href="/user/login">로그인</a>
    </th:block>
    <th:block th:unless="${session.user} == null">
        <a href="/user/logout">로그아웃</a> | <span>[[${session.user.userName}]] 님 환영합니다.</span>
        <a href="/board/boardList">게시판</a>
    </th:block>
</body>

</html>
{% endhighlight html %}
> http://localhost:8080/
![img]({{ '/assets/images/2022-10-25/img0.PNG' | relative_url }}){: .left-image }

* 사용자 가입

> `file`\src\main\java\com\example\sesac\first\controller\UserController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@Controller
@RequestMapping("user") // http://localhost:8080/user
public class UserController {
    @GetMapping("join") // http://localhost:8080/user/join
    public String join() {
        return "user/join";
    }

    @PostMapping("join") // http://localhost:8080/user/join
    public String join(User user, HttpSession session) {
        List<User> userlist = new ArrayList<>();
        if (session.getAttribute("userlist") != null) {
            userlist = (List<User>) session.getAttribute("userlist");
        }
        userlist.add(user);
        session.setAttribute("userlist", userlist);
        // session.setAttribute("joinUser", user);
        return "redirect:/";
    }

...
{% endhighlight java %}

> `file`\path\page.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h2>회원가입</h2>
<form action="/user/join" method="post">
    <input type="text" placeholder="아이디" name="userId"/><br />
    <input type="text" placeholder="비밀번호" name="userPw"/><br />
    <input type="text" placeholder="이름" name="userName"/><br />
    <input type="text" placeholder="주소" name="userAddr"/><br />
    <input type="submit" value="회원가입"/>
</form>

...
{% endhighlight html %}
> 홈화면의 `회원가입`{:.key}
![img]({{ '/assets/images/2022-10-25/img4.PNG' | relative_url }}){: .left-image }

* 로그인

> `file`\src\main\java\com\example\sesac\first\controller\UserController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("login")
public String login() {
    return "user/login";
}

@PostMapping("login")
public String login(User user, HttpSession session) {
    String id = user.getUserId();
    String pw = user.getUserPw();
    List<User> userlist = new ArrayList<>();
    userlist = (List<User>) session.getAttribute("userlist");
    for (User listedUser : userlist) {
        if (listedUser.getUserId().equals(id) && listedUser.getUserPw().equals(pw)) {
            session.setAttribute("user", listedUser);
            break;
        } else {
            session.setAttribute("user", null);
        }
    }

    return "redirect:/";
}

...
{% endhighlight java %}

> `file`\path\page.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h2>로그인</h2>
<form action="/user/login" method="post">
    <input type="text" placeholder="아이디" name="userId"/><br />
    <input type="text" placeholder="비밀번호" name="userPw"/><br />
    <input type="submit" value="로그인"/>
</form>

...
{% endhighlight html %}
> 홈화면의 `로그인`{:.key}
![img]({{ '/assets/images/2022-10-25/img5.PNG' | relative_url }}){: .left-image }

> 아이디: 1, 비밀번호: 1234, 이름: 홍길동, 주소: 서울 => `로그인`{:.key}
![img]({{ '/assets/images/2022-10-25/img6.PNG' | relative_url }}){: .left-image }

* 로그아웃

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("logout")
public String logout(HttpSession session) {
    session.removeAttribute("user");
    return "redirect:/";
}

...
{% endhighlight java %}

* 게시판 구현

> `file`\path\page.html
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@Controller
@RequestMapping("board")
public class BoardController {
    // 게시글 목록
    @GetMapping("boardList")
    public String boardList(Model model, Board board, HttpSession session) {
        if (session.getAttribute("boardList") != null){
            List<Board> boardList = (List<Board>) session.getAttribute("boardList");
            model.addAttribute("boardList", boardList);
        } else {
            model.addAttribute("boardList", null);
        }
        return "board/boardList";
    }
    
...
{% endhighlight java %}

> `file`\src\main\resources\templates\board\boardList.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h2>- 게시글 목록 -</h2>
<table border="1">
    <tr>
        <td>번호</td>
        <td align="center">게시글 제목</td>
        <td>작성자</td>
    </tr>
    <th:block th:if="${boardList} != null">
        <tr th:each="board : ${boardList}">
            <td th:text="${board.boardNo}"></a></td>
            <td>
                <a th:href="@{/board/boardDetail(boardNo=${board.boardNo})}">[[${board.boardTitle}]]</a>
            </td>
            <td th:text="${board.boardWriter}"></td>
        </tr>
    </th:block>
    <th:block th:unless="${boardList} != null">
        <tr colspan="3">게시글이 존재하지 않습니다.</tr>
        <!-- colspan : 셀 병합 -->
    </th:block>
</table>
<a href="/">홈으로</a> | <a href="/board/boardCreate">글쓰기</a>

...
{% endhighlight html %}
> 로그인 세션 상태에서 홈화면의 `게시판`{:.key}
![img]({{ '/assets/images/2022-10-25/img7.PNG' | relative_url }}){: .left-image }

* 게시글 작성

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

// 게시글 작성 페이지
@GetMapping("boardCreate")
public String boardCreate() {
    return "board/boardCreate";
}
//게시글 작성 요청
@PostMapping("boardCreate")
public String boardCreate(Board board, HttpSession session) {
    List<Board> boardList = new ArrayList<>();
    if (session.getAttribute("boardList") != null) {
        boardList = (List<Board>) session.getAttribute("boardList");
        int preBoardNo = Integer.parseInt(boardList.get(boardList.size() - 1).getBoardNo());
        board.setBoardNo(Integer.toString(preBoardNo + 1));
    } else {
        board.setBoardNo("1");
    }
    User user = (User) session.getAttribute("user");
    board.setBoardWriter(user.getUserName());
    boardList.add(board);
    session.setAttribute("boardList", boardList);
    return "redirect:/board/boardList";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\board\boardCreate.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h2>- 게시글 작성 -</h2>
<form action="/board/boardCreate" method="post">
    <h3>작성자 : [[${session.user.userName}]]</h3>
    <h3>글 제목</h3>
    <textarea name="boardTitle" style="width:100%;"></textarea>
    <h3>글 내용</h3>
    <textarea name="boardContent" style="width:100%; height:200px"></textarea>
    <input type="submit" value="게시글 작성">
</form>
<a href="/board/boardList">취소</a>

...
{% endhighlight html %}
> `게시글 작성`{:.key}
![img]({{ '/assets/images/2022-10-25/img8.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-25/img9.PNG' | relative_url }}){: .left-image }

* 게시글 상세보기

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

// 게시글 상세보기
@GetMapping("boardDetail")
public String boardDetail(Model model, HttpSession session, @RequestParam String boardNo) {
    List<Board> boardList = (List<Board>) session.getAttribute("boardList");
    for (Board board : boardList) {
        if (board.getBoardNo().equals(boardNo)) {
            model.addAttribute("board", board);
            break;
        }
    }
    return "board/boardDetail";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\board\boardDetail.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h2>- 게시글 [[${board.boardNo}]] -</h2>
<h3>작성자 : [[${board.boardWriter}]]</h3>
<h3>글 제목</h3>
<textarea name="boardTitle" style="width:100%;" readonly>[[${board.boardTitle}]]</textarea>
<h3>글 내용</h3>
<textarea name="boardContent" style="width:100%; height:200px" readonly>[[${board.boardContent}]]</textarea>
<a href="/board/boardList">목록으로</a>
<th:block th:if="${session.user.userName} == ${board.boardWriter}">
    | <a th:href="@{/board/boardUpdate(boardNo=${board.boardNo})}">수정</a>
</th:block>

...
{% endhighlight html %}
> 작성한 게시글 제목 클릭
![img]({{ '/assets/images/2022-10-25/img10.PNG' | relative_url }}){: .left-image }

* 게시글 수정

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

// 게시글 수정 페이지
@GetMapping("boardUpdate")
public String boardUpdate(Model model, HttpSession session, @RequestParam String boardNo) {
    // 수정하고자 하는 페이지의 글정보를 가져와 페이지에 표시
    List<Board> boardList = (List<Board>) session.getAttribute("boardList");
    User user = (User) session.getAttribute("user");
    for (Board board : boardList) {
        if (board.getBoardNo().equals(boardNo) && board.getBoardWriter().equals(user.getUserName())) {
            model.addAttribute("board", board);
            return "board/boardUpdate";
        }
    }
    return "board/boardError";
}
// 게시글 수정 요청
@PostMapping("boardUpdate")
public String boardUpdate(Board board, HttpSession session) {
    List<Board> boardList = (List<Board>) session.getAttribute("boardList");
    User user = (User) session.getAttribute("user");
    board.setBoardWriter(user.getUserName());
    for (int i = 0; i < boardList.size(); i++) {
        if (boardList.get(i).getBoardNo().equals(board.getBoardNo())) {
            boardList.set(i, board);
            session.setAttribute("boardList", boardList);
        }
    }
    return "redirect:/board/boardList";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\board\boardUpdate.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<h2>- 게시글 [[${board.boardNo}]] 수정 -</h2>
<form action="/board/boardUpdate" method="post">
    <input type="hidden" name="boardNo" th:value="${board.boardNo}" />
    <h3>작성자 : [[${session.user.userName}]]</h3>
    <h3>글 제목</h3>
    <textarea name="boardTitle" style="width:100%;">[[${board.boardTitle}]]</textarea>
    <h3>글 내용</h3>
    <textarea name="boardContent" style="width:100%; height:200px">[[${board.boardContent}]]</textarea>
    <input type="submit" value="게시글 수정">
</form>
<a href="/board/boardList">취소</a>

...
{% endhighlight html %}
> 자세히보기에서 수정
![img]({{ '/assets/images/2022-10-25/img11.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-25/img12.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-25/img13.PNG' | relative_url }}){: .left-image }
