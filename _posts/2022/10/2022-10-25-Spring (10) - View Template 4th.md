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
5. <span style="color:Turquoise">**View Template**</span> [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) [3)](/2022/10/Spring-(9)-View-Template-3rd/) <span style="color:SteelBlue">**4)**</span> [5)](/2022/10/Spring-(11)-View-Template-5th/)
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
> return redirect:/주소
: return "viewName"과의 차이는 viewName은 해당 페이지를 보여줄 뿐이지만 redirect는 해당 주소로 URL요청을 다시한다.
결과적으로 주소창의 주소가 바뀌고 해당 URL의 controller가 다시 호출된다.

> EL(Expression Lagnuage)문법 - 자세한건 Reference참고
: 자바 bean의 property를 JSP(Java Server Pages)의 표현식 <%= %>이나 액션 태그 <jsp:useBean>를 사용하는것 보다 쉽고 간결하게 꺼낼수 있게 하는 기술이다.
* ${ ... }
    : JSP가 실행될 때 즉시 반영된다. (Immediate evaluation)
    객체 property를 꺼낼때 주로 사용한다.
* #{ ... }
    : 시스템에서 필요하다고 판단될 때 그 값을 사용한다. (Deferred evaluation)
    사용자 입력값을 객체의 property에 담는 용도로 주로 사용한다.

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
        // 확인용 터미널 출력
        System.out.println("userId : " + user.getUserId());
        System.out.println("userPw : " + user.getUserPw());
        session.setAttribute("user", user); // session에 user데이터를 user라는 이름으로 저장
        return "redirect:/main"; // http://localhost:8080/main 으로 요청 (main으로 get요청이 간다)
    }

    @GetMapping("main")
    public String main() {
        return "main";
    }
    // 로그아웃 구현
    @GetMapping("logout")
    public String logout(HttpSession session) {
        // session.removeAttribute("user"); // 특정 세션의 값을 삭제
        session.invalidate(); // 세션의 정보를 모두 제거
        return "redirect:/"; // http://localhost:8080 으로 요청
    }
}

...
{% endhighlight java %}

* 데이터를 담을(DTO) User 클래스 작성

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
        model클래스의 멤버필드와 name값을 일치 시켜준다. -->
    ID : <input type="text" placeholder="아이디" name="userId"/><br>
    PW : <input type="password" placeholder="비밀번호" name="userPw"/><br>
    <input type="submit" value="로그인"/>
</form>

...
{% endhighlight html %}
> http://localhost:8080/sessionLogin
![img]({{ '/assets/images/2022-10-25/img1.PNG' | relative_url }}){: .left-image }
> id: abc, pw: 1234 => `Click`{:.key} => 터미널
![img]({{ '/assets/images/2022-10-25/img2.PNG' | relative_url }}){: .left-image }

* 세션에 user 정보의 유무에 따라 다른 내용 출력하기

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

---
## 연습
* 세션에 유저 정보와 게시글 정보를 담아 게시판을 구현해보자

> * p 태그<p></p>와 div 태그<div></div>
    : 태그 스스로 가진 공간이 있어 내용에 상관없이 공간을 차지한다. Block-level Element
    가로, 세로 사이즈 조절이 가능하다.
    p는 기본 margin이 있고 주로 텍스트의 단락 구분을 위해 사용하지만 div는 전체적인 레이아웃을 나누는데 사용한다.
* span 태그 <span></span>
    : 내용에 따라 공간의 크기가 정해진다. Inline-level Element
    가로, 세로 사이즈 조절이 불가능하다.

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
    public String home(HttpSession session) {
        return "home";
    }
}
{% endhighlight java %}

* 세션에 user 정보가 있을때만 게시판 링크가 보이게 작성한다.

> `file`\src\main\resources\templates\home.html
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

* 회원가입 페이지 만들기 - 세션에 user의 정보를 저장한다.
* 여러 user의 정보를 담기 위해 list를 사용한다.

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
        // DTO인 User 클래스의 객체를 가지는 arraylist를 생성한다.
        List<User> userlist = new ArrayList<>();
        // 세션에 userlist라는 이름의 속성이 존재하면 불러와 생성된 userlist에 주소값을 넣는다.
        if (session.getAttribute("userlist") != null) {
            userlist = (List<User>) session.getAttribute("userlist");
        }
        // post메소드 형식으로 페이지에서 입력된 값은 DTO인 user의 멤버필드에 각각 할당되고
        // 이 user를 userlist에 추가한다.
        userlist.add(user);
        // 세션에 userlist라는 이름으로 userlist를 추가한다.
        session.setAttribute("userlist", userlist);
        // 홈화면으로 리다이렉트 해준다.
        return "redirect:/";
    }

...
{% endhighlight java %}

* submit키를 누르면 form태그 안의 input태그 각각의 name들이 각각의 value와 짝을 이뤄 form의 method형식에 맞춰 요청시 전달된다.
* post로 요청할 때 매개변수로 DTO가 있고 name이 DTO의 멤버필드와 일치하면 해당 value가 멤버필드의 값에 할당된다.

> `file`\src\main\resources\templates\user\join.html
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
<a href="/">홈으로</a>

...
{% endhighlight html %}
> 홈화면의 `회원가입`{:.key}
![img]({{ '/assets/images/2022-10-25/img4.PNG' | relative_url }}){: .left-image }

* 로그인 페이지 만들기 - id와 pw를 입력받는다.
* 입력받은 값과 세션의 userlist와 비교하여 id와 pw가 일치하면 해당 user정보를 세션에 추가한다.

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
    // 입력받은 id와 pw 데이터
    String id = user.getUserId();
    String pw = user.getUserPw();
    // 세션에 userlist가 없는데 로그인 시도 시 그냥 진행하면 nullexception 발생하므로 체크한다.
    if (session.getAttribute("userlist") != null) {
        List<User> userlist = (List<User>) session.getAttribute("userlist");
        for (User listedUser : userlist) {
            if (listedUser.getUserId().equals(id) && listedUser.getUserPw().equals(pw)) {
                ssession.setAttribute("user", listedUser);
                return "redirect:/";
            }
        }
    }
    return "user/loginFail";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\user\login.html
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
<a href="/">홈으로</a>

...
{% endhighlight html %}
> 홈화면의 `로그인`{:.key}
![img]({{ '/assets/images/2022-10-25/img5.PNG' | relative_url }}){: .left-image }

> 아이디: 1, 비밀번호: 1234, 이름: 홍길동, 주소: 서울 => `로그인`{:.key}
![img]({{ '/assets/images/2022-10-25/img6.PNG' | relative_url }}){: .left-image }

* 로그아웃 - 세션에서 user만 삭제한다.

> `file`\src\main\java\com\example\sesac\first\controller\UserController.java
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

* 게시판 리스트 만들기

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
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
        <tr>
            <td colspan="3">게시글이 존재하지 않습니다.</td>
            <!-- colspan : 셀 병합 -->
        </tr>
    </th:block>
</table>
<a href="/">홈으로</a> | <a href="/board/boardCreate">글쓰기</a>

...
{% endhighlight html %}
> 로그인 세션 상태에서 홈화면의 `게시판`{:.key}
![img]({{ '/assets/images/2022-10-25/img7.PNG' | relative_url }}){: .left-image }

* 게시글 작성하기

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
        // 작성할 글번호 = 마지막으로 작성된 글번호 + 1
        int preBoardNo = Integer.parseInt(boardList.get(boardList.size() - 1).getBoardNo());
        board.setBoardNo(Integer.toString(preBoardNo + 1));
    } else {
        // 게시글리스트가 비어있다면 작성할 글번호는 1이다.
        board.setBoardNo("1");
    }
    // 세션의 user에서 name을 가져와 writer로 사용한다.
    User user = (User) session.getAttribute("user");
    board.setBoardWriter(user.getUserName());
    // 최종적으로 post요청시 전달받은 board와
    // 위에서 정한 boardNo, boardWriter를 담은 board를 boardList에 추가한다.
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

* 게시글 상세보기 - 게시글 제목에 게시글 번호를 파라미터로 링크를 넘겨준다.

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
    // 세션의 boardList에서 boardNo가 일치하는 board를 찾아
    // model에 board라는 이름으로 담아 view로 넘긴다.
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

* readonly로 수정이 불가능하게 만든다.
* 게시글 수정 링크는 작성자와 세션의 userName이 같을 경우만 표시한다.

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
> 작성한 게시글 `제목`{:.key}
![img]({{ '/assets/images/2022-10-25/img10.PNG' | relative_url }}){: .left-image }

* 게시글 수정하기

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

// 게시글 수정 페이지
@GetMapping("boardUpdate")
public String boardUpdate(Model model, HttpSession session, @RequestParam String boardNo) {
    // 수정하고자 하는 글의 번호를 세션의 boardList에서 찾아 해당 게시글의 정보를 model에 담아 view로 보낸다.
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

* input의 hidden 타입을 이용해 post요청 시 넘길 boardNo 값을 지정한다.
* title과 content는 수정한 값을 그대로 넘기고 writer는 java에서 세션 userName 값으로 지정한다.

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
> 자세히 보기에서 `수정`{:.key}
![img]({{ '/assets/images/2022-10-25/img11.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-25/img12.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-25/img13.PNG' | relative_url }}){: .left-image }

---
# Reference
{: style="text-align: right"}
PrinceY: [[Spring & Web] return "redirect:/주소" 와 일반 return "view이름"의 차이](https://blog.naver.com/PostView.naver?blogId=sim4858&logNo=221007278858&categoryNo=0&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)
Leica: [JSP - EL 표현식 문법과 사용 방법](https://atoz-develop.tistory.com/entry/JSP-EL-%ED%91%9C%ED%98%84%EC%8B%9D-%EB%AC%B8%EB%B2%95%EA%B3%BC-%EC%82%AC%EC%9A%A9-%EB%B0%A9%EB%B2%95)
하늘호수: [[CSS] CSS 레이아웃 div 태그, p 태그, span 태그의 차이점](https://blog.naver.com/pungwun/220501759403)
{: style="text-align: right"}