---
title: Spring Boot Project (2)
published: true
date:   2022-10-27
description: 프로젝트 구현 (1/2)
comments: true
categories:
 - spring boot
tags:
 - spring boot
 - java
toc: true
toc_sticky: true
toc_label: 프로젝트 구현 (1/2)
---
---
# 1) 레이아웃 구현
thymeleaf-layout을 이용해 페이지를 세 구역을 나눴고 일단 footer에 copyright 문구만 띄우는데는 성공했다.

head fragment에는 페이지에서 타이틀 값을 가져와 타이틀을 설정하는 기능을 넣었다.

## top 페이지
top구역을 네비게이션 구역으로 사용해 게시판과 유저관련 기능을 넣었다. div태그에 float속성을 줘서 3개로 나눴다.

메인과 게시판으로 이동하는 링크들을 좌측에, 유저관련 기능은 우측으로 넣었다. 로그인 세션의 유무에 따라 없으면 '로그인과 계정생성' 있으면 '로그아웃과 정보수정'을 표시했다.

> `file`\src\main\resources\templates\fragments\topNav.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<th:block th:if="${session.user} == null">
    <a href="/user/join">계정생성</a> |
    <a href="/user/login">로그인</a>
</th:block>
<th:block th:unless="${session.user} == null">
    [[${session.user.userNick}]] 님 환영합니다. |
    <a href="/user/logout">로그아웃</a> | <a href="/user/userDetail">정보수정</a>
</th:block>

...
{% endhighlight html %}

## bottom 페이지
일단 담을것이 없어 Copyright만 넣었다.

---
# 2) 계정 관련
계정에 필요한 데이터는 데이터를 식별할 userId를 primary값으로 했고, 추가로 비밀번호, 별명, 이메일, 계정 생성일을 데이터 값으로 정했다.

## 계정 생성
계정 생성 페이지에서 form태그의 input태그에 type속성을 각각 다르게 설정해서 입력받을 값을 지정했다. 비밀번호는 타입을 password로 해서 입력을 가렸고 minlength 속성도 8로 추가해 8자리 이상을 받도록 했다. 이메일은 타입을 email로 해서 입력받을 때 @양쪽으로 문자가 있는지 체크하게 했다.

필수적으로 입력받을 아이디, 비밀번호, 닉네임에는 required 속성을 추가해 반드시 값을 입력받도록 했습니다. 이로써 null 데이터를 db에 등록하는 것을 막을 수 있었다.

> `file`\src\main\resources\templates\user\join.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<form action="/user/join" method="post">
    ID* : <input type="text" placeholder="아이디" name="userId" required /><br /><br />
    Password* : <input type="password" placeholder="비밀번호" name="userPw" minlength=8 required /><br /><br />
    Nickname* : <input type="text" placeholder="별명" name="userNick" required /><br /><br />
    E-mail : <input type="email" placeholder="이메일" name="userEmail" /><br /><br />
    (*필수) <input type="submit" value="생성" />
</form>

...
{% endhighlight html %}

계정생성일을 기록하기 위해 value에 마지막으로 now()를 추가해 줬다.

> `file`\src\main\resources\mapper\userMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<insert id="join" parameterType="kr.ac.sesac.springboot.webproject.model.User">
    INSERT INTO user VALUES(#{userId},#{userPw},#{userNick},#{userEmail},now())
</insert>

...
{% endhighlight xml %}

---
## 로그인
계정 생성과 마찬가지로 아이디와 비밀번호에 required 속성을 추가해 null이면 submit할 수 없게 했다.

비밀번호의 타입도 password로 설정했다.

> `file`\src\main\java\kr\ac\sesac\springboot\webproject\controller\UserController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@PostMapping("login")
public String login(User user, HttpSession session) {
    String id = user.getUserId();
    String pw = user.getUserPw();
    String getPw = userMapper.getPw(id);
    if (getPw != null) {
        if (getPw.equals(pw)) {
            User userData = userMapper.selectUser(id);
            session.setAttribute("user", userData);
            return "redirect:/";
        }
    } else {
        session.setAttribute("user", null);
    }
    return "user/loginFail";
}

...
{% endhighlight java %}

---
## 정보수정
로그인 해서 세션에 유저 정보가 있을 때만 접근할 수 있도록 했다.

아이디는 수정할 수 없게 readonly 속성을 주고 비밀번호를 제외한 닉네임과 이메일은 해당 세션의 유저정보에서 읽어와 보여주고 수정도 가능하게 했다.

수정을 하려면 현재 비밀번호에 맞는 비밀번호를 입력해야 하고 비밀번호를 변경하고자 할때에는 바꿀 비밀번호 칸에 입력하면 DB상에 수정되도록 했다.

가장 아래쪽에는 계정을 생성한 날짜를 보여준다.

> `file`\src\main\resources\templates\user\userDetail.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<form action="/user/userDetail" method="post">
    ID* : <input type="Text" name="userId" th:value="${session.user.userId}" readonly="" /><br /><br />
    Current Password* : <input type="password" placeholder="현재 비밀번호"
        name="userPw" minlength=8 required /><br /><br />
    Password : <input type="password" placeholder="바꿀 비밀번호" name="fixPw" minlength=8 /><br /><br />
    Nickname* : <input type="text" name="userNick" th:value="${session.user.userNick}" required /><br /><br />
    E-mail : <input type="email" name="userEmail" th:value="${session.user.userEmail}" /><br /><br />
    Account creation date : [[${#dates.format(session.user.userCreateDate, 'yyyy-MM-dd')}]]<br /><br />
    (*필수) <input type="submit" value="수정" />
</form>

...
{% endhighlight html %}

> `file`\src\main\java\kr\ac\sesac\springboot\webproject\controller\UserController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@PostMapping("userDetail")
public String userDetail(String fixPw, User user, HttpSession session) {
    User curUser = (User) session.getAttribute("user");
    // 현재 세션 유저의 id로 DB에서 pw를 구해 입력받은 pw와 비교
    String getPw = userMapper.getPw(curUser.getUserId());
    if (getPw.equals(user.getUserPw())) {
        // 수정할 pw가 있다면 DB로 보내기전에 변경
        if (fixPw != null) {
            user.setUserPw(fixPw);
        }
        userMapper.userUpdate(user);
        // 현재 세션에 반영
        session.setAttribute("user", user);
        return "redirect:/";
    }
    return "user/changeFail";
}

...
{% endhighlight java %}

> `file`\src\main\resources\mapper\userMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<update id="userUpdate" parameterType="kr.ac.sesac.springboot.webproject.model.User">
    UPDATE user SET userPw=#{userPw}, userNick=#{userNick}, userEmail=#{userEmail}
    WHERE userId=#{userId} 
</update>

...
{% endhighlight xml %}

---
# 3) 게시글 관련
게시글의 데이터는 데이터를 구별할 고유id와 제목, 내용, 작성자, 작성자를 구별할 작성자id, 작성일, 수정일, 조회수, 추천, 비추천으로 정했다.

추가로 DB에서 불러올 때 매길 row number를 저장할 필드도 선언해 뒀다.

## 게시글 리스트
이곳에서는 해당 게시글의 내용과 수정일은 사용하지 않고 나머지 6개의 데이터만 게시글당 한줄에 나타냈다.

우선 게시글이 하나도 없을 경우 목록의 한 열을 병합해 게시글이 존재하지 않음을 나타냈다.

해당 게시글의 제목에 게시글의 상세페이지로 넘어갈 하이퍼링크를 만들었다.

작성일의 경우 게시글이 오늘 작성됐다면 시:분으로 아니라면 월-일만 표시되도록 if문을 쓰고 thymeleaf의 클래스인 calendars, dates들의 메서드를 활용했다.

> `file`\src\main\resources\templates\board\list.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<th:block th:with="nowDay=${#calendars.format(#calendars.createNow(), 'yyyy-MM-dd')}">
    <!-- 작성일이 오늘이면 시간만 표시 -->
    <th:block th:if="${nowDay} == ${#dates.format(board.boardCreateDate, 'yyyy-MM-dd')}">
        <td th:text="${#dates.format(board.boardCreateDate, 'HH:mm')}" style="text-align: center;">
        </td>
    </th:block>
    <!-- 작성일이 오늘이 아니면 월, 일만 표시 -->
    <th:block th:unless="${nowDay} == ${#dates.format(board.boardCreateDate, 'yyyy-MM-dd')}">
        <td th:text="${#dates.format(board.boardCreateDate, 'MM-dd')}" style="text-align: center;">
        </td>
    </th:block>
</th:block>

...
{% endhighlight html %}

DB에 저장한 시간을 불러 웹에 표시했을때 시간이 맞지 않은 것을 발견하고 설정을 뒤적이다가 application.properties에 답이 있었다.

    spring.datasource.url=jdbc:mysql://localhost:3306/webproject?serverTimezone=UTC&characterEncoding=UTF-8

여기에 serverTimezone이 UTC로 설정되어 있어서 스프링으로 불러오면 KST인 +9가 되어 이상해진 것이었다. 다음으로 바꿔주니 정상적으로 출력되었다.

    spring.datasource.url=jdbc:mysql://localhost:3306/webproject?serverTimezone=Asia/Seoul&characterEncoding=UTF-8

페이지네이션을 구현하기 위해 매퍼에서 쿼리로 불러올때 구문에 LIMIT를 써서 해당 페이지에 표시할 게시글만 불러왔다. 표시하지 않을 내용과 수정일은 포함하지 않았다.

MariaDB에서는 Row_Number를 지원하지 않으므로 변수를 설정하여 가장 오래된 게시글부터 번호를 매겼고, 표시를 시작할 게시글의 전 숫자와 출력할 게시글 수를 LIMIT의 변수로 넘겨 해당 페이지에 표시할 게시글만 가져오게 작성했다.

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<select id="getTotal" resultType="int">
    SELECT count(*) AS totalCount FROM board
</select>
<select id="getList" parameterType="int" resultType="kr.ac.sesac.springboot.webproject.model.Board">
    SELECT @rownum:=@rownum + 1 AS rnum, boardId, boardTitle, boardWriter, boardCreateDate, boardViews, boardThumbUp
    FROM board, (SELECT @rownum:=0) R
    ORDER BY boardCreateDate DESC
    LIMIT #{startPost}, #{countList}
</select>

...
{% endhighlight xml %}

Java 소스 쪽에서도 테이블 아래에 표시할 페이지 번호들을 계산하여 model에 담아 html로 넘겼다. html쪽에서도 이전에 배웠던 thymeleaf 문법을 이용해 구현은 완료했다.

> `file`\src\main\java\kr\ac\sesac\springboot\webproject\controller\UserController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("list")
public String list(Model model, HttpSession session, @RequestParam(defaultValue = "1") int page) {
    ////수정할 것////
    int countPage = 5; // 한 화면에 출력될 페이지 수
    int countPost = 10; // 한 페이지에 출력할 게시글 수
    ////수정할 것////
    int totalCount = boardMapper.getTotal(); // BD에 등록된 총 게시글 수

    int totalPage = totalCount / countPost; // 총 페이지 수
    // 총 게시글을 한 화면에 출력될 게시글로 나눠서 나머지가 있다면 표시할 페이지를 하나 추가
    if (totalCount % countPost > 0) {
        totalPage++;
    }
    // 총 페이지 수보다 접속한 페이지가 크면 마지막 페이지로 보정한다.
    if (totalPage < page) {
        page = totalPage;
    }
    int startPage = (page - 1) / countPage * countPage + 1;
    int endPage = startPage + countPage - 1;
    // 마지막 페이지가 총 페이지 수 보다 크면 마지막 페이지로 보정한다.
    if (endPage > totalPage) {
        endPage = totalPage;
    }
    if (endPage == 0) {
        endPage = 1;
    }
    // 마지막 페이지 리스트 전의 마지막 페이지
    int preLastPage = totalPage - (totalPage%countPage==0?countPage:totalPage%countPage);
    List<Board> list = boardMapper.getList((page - 1) * countPost, countPost);
    model.addAttribute("list", list);
    model.addAttribute("startPage", startPage);
    model.addAttribute("endPage", endPage);
    model.addAttribute("page", page);
    model.addAttribute("preLastPage", preLastPage);
    return "board/list";
}

...
{% endhighlight java %}

> `file`\src\main\resources\templates\board\list.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<th:block th:if="${page} > 1">
    <a th:href="@{/board/list(page=${page} - 1)}">&lt</a>
</th:block>
<th:block th:each="pageNumber : ${#numbers.sequence(startPage, endPage)}">
    <a th:if="${page}==${pageNumber}" th:text="${pageNumber}" style="font-weight:bold"></a>
    <a th:unless="${page}==${pageNumber}" th:href="@{/board/list(page=${pageNumber})}"
        th:text="${pageNumber}"></a>
</th:block>
<th:block th:if="${preLastPage} >= ${page}">
    <a th:href="@{/board/list(page=${page} + 1)}">&gt</a>
</th:block>

...
{% endhighlight html %}

---
# 추가 사항
아이디 중복체크 - 버튼으로 쿼리실행, 결과 획득에 따라 가능/불가능 팝업 메시지를 띄울 수 있으면 좋겠다.

계정 생성시 비밀번호 확인 기능을 넣어 생성 버튼을 눌렀을때 두항목이 다르면 에러 팝업 메시지를 띄울 수 있으면 좋겠다.

게시판 리스트에서 한 페이지에 표시할 글의 수를 유저가 고를 수 있게 콤보박스를 구현해보면 좋겠다.

조회수와 추천, 비추천 시스템도 추가해 보자.

게시글 작성 버튼은 유저 세션이 존재할 경우에만 표시되게 한다.

게시글의 수정과 삭제는 작성자와 세션의 유저가 같을 경우에만 표시되게 한다.

아마 시간이 안될테지만 관리자 계정으로 게시글을 작성할 경우 공지로 등록할 수 있는 옵션을 주고 공지일 경우 게시글 목록의 상단에 고정한다.

추천에 비추천을 뺀 값을 기준으로 상위 몇개의 개시글을 상단에 노출시킬 수 있으면 좋겠다.

---
# Reference
sgoho01: [타임리프 레이아웃 (thymeleaf layout dialect)](https://sgoho01.tistory.com/12)

Jan92: [Spring Boot 타임리프 Thymeleaf layout 적용하는 방법](https://wildeveloperetrain.tistory.com/136)

mdn web docs: [input: 입력 요소](https://developer.mozilla.org/ko/docs/Web/HTML/Element/Input)

zepinos: [페이징에 대한 이해](https://zepinos.tistory.com/29)

간둥이: [[Thymeleaf] 현재 날짜 출력 및 형식 변경](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=hay6308&logNo=220968374336)

로넬: [[Mysql] Default Timezone 설정 방법](https://blog.naver.com/developer501/222492629932)
