---
layout: post
title: Spring Boot Project (3)
published: true
date:   2022-10-28
description: 프로젝트 구현 (2/2)
toc: true
comments: true
tags:
 - spring boot
---
---
# 3) 게시글 관련
## 게시글 작성
게시글 작성 버튼은 유저 세션이 존재할 경우에만 표시되게 한다. 정보수정에서 별명을 바꿀 수 있게 디자인 했기에 나중에 게시글 수정이나 삭제 시 작성자를 구별할 작성자의 id도 게시글 데이터에 추가로 받기로 했다. 하지만 게시글과 리스트에는 작성자의 별명으로만 노출되게 구현했다.

> `file`\src\main\java\kr\ac\sesac\springboot\webproject\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@PostMapping("create")
public String create(Board board, HttpSession session) {
    User user = (User) session.getAttribute("user");
    board.setBoardWriter(user.getUserNick());
    board.setBoardWriterId(user.getUserId());
    boardMapper.putBoard(board);
    return "redirect:/board/list";
}

...
{% endhighlight java %}

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<insert id="putBoard" parameterType="kr.ac.sesac.springboot.webproject.model.Board">
    INSERT INTO board VALUES(NULL, #{boardTitle}, #{boardContent}, #{boardWriter}, #{boardWriterId}, now(), NULL, 0, 0, 0)
</insert>

...
{% endhighlight xml %}

---
## 게시글 조회
상세보기 페이지 호출시 해당 게시판 id를 통해 boardViews를 1씩 더해 업데이트 하는 쿼리를 실행하는 방식으로 조회수를 구현했다. 하지만 상세 페이지에서 새로고침을 할때마다 조회수가 올라가 수정이 필요해 보인다.

> `file`\src\main\java\kr\ac\sesac\springboot\webproject\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("detail")
public String detail(Model model, HttpSession session, @RequestParam int boardId) {
    Board board = boardMapper.selectBoard(boardId);
    boardMapper.updateViews(boardId);
    model.addAttribute("board", board);
    return "board/detail";
}

...
{% endhighlight java %}

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<update id="updateViews" parameterType="kr.ac.sesac.springboot.webproject.model.Board">
    UPDATE board SET boardViews=boardViews+1
    WHERE boardId = #{boardId}
</update>

...
{% endhighlight xml %}

게시글 수정일은 수정일에 데이터가 있을 때만 표시되도록 했다.
게시글의 수정과 삭제는 작성자와 세션의 유저가 같을 경우에만 표시되게 한다.

> `file`\src\main\resources\templates\board\detail.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<th:block th:unless="${board.boardUpdateDate} == null">
    <tr>
        <td style="padding-left: 5px;">수정일</td>
        <td colspan="3" style="padding-right: 5px; text-align: right;">
            [[${#dates.format(board.boardUpdateDate, 'yyyy-MM-dd HH:mm:ss')}]]
        </td>
    </tr>
</th:block>

...

<th:block th:unless="${session.user} == null">
    <th:block th:if="${session.user.userId} == ${board.boardWriterId}">
        <tr>
            <td colspan="4" style="text-align: right; padding-right: 5px;">
                <input type="button" value="게시글 수정"
                    th:onclick="|location.href='@{edit(boardId=${board.boardId})}'|">
                <input type="button" value="게시글 삭제"
                    th:onclick="|location.href='@{delete(boardId=${board.boardId})}'|">
            </td>
        </tr>
    </th:block>
</th:block>

...
{% endhighlight html %}

---
## 게시글 수정
수정 요청을 쿼리 할 때와 수정 후 리다이렉트 할 때 게시글을 구별할 용도로 필요한 boardId는 hidden속성으로 view에 준 뒤 submit시 받아왔다.

> `file`\src\main\resources\templates\board\detail.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
...

<form action="/board/edit" method="post">
    <input type="hidden" name="boardId" th:value="${board.boardId}">
    <tr>
        <td width="80px" style="padding-left: 5px;">작성자</td>
        <td>[[${board.boardWriter}]]</td>
    </tr>
    <tr>
        <td style="padding-left: 5px;">제목</td>
        <td style="padding-right:5px;">
            <textarea name="boardTitle" style="width:100%; height:21px;">[[${board.boardTitle}]]</textarea>
        </td>
    </tr>
    <tr>
        <td colspan="2" style="padding: 5px;">
            <textarea name="boardContent" style="width:100%; height:300px">[[${board.boardContent}]]</textarea>
        </td>
    </tr>
    <tr>
        <td></td>
        <td style="text-align: right; padding-right: 5px;"><input type="submit" value="수정 완료"></td>
    </tr>
</form>

...
{% endhighlight html %}

수정할 수 있는 항목은 제목과 내용이며 추가로 수정일에 데이터를 추가해줬다.

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<update id="updateBoard" parameterType="kr.ac.sesac.springboot.webproject.model.Board">
    UPDATE board SET boardTitle=#{boardTitle}, boardContent=#{boardContent}, boardUpdateDate=now()
    WHERE boardId = #{boardId}
</update>

...
{% endhighlight xml %}

---
## 게시글 삭제
삭제는 간단하게 boardId를 가져와 해당 항목을 삭제하는 방식으로 했다.

> `file`\src\main\java\kr\ac\sesac\springboot\webproject\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("delete")
public String delete(@RequestParam int boardId) {
    boardMapper.deleteBoard(boardId);
    return "redirect:/board/list";
}

...
{% endhighlight java %}

---
# 구현 화면
메인페이지
![img]({{ '/assets/images/2022-10-28/img1.PNG' | relative_url }}){: .left-image }

계정생성
![img]({{ '/assets/images/2022-10-28/img2.PNG' | relative_url }}){: .left-image }

비밀번호 길이 제한
![img]({{ '/assets/images/2022-10-28/img3.PNG' | relative_url }}){: .left-image }

이메일 형식 제한
![img]({{ '/assets/images/2022-10-28/img4.PNG' | relative_url }}){: .left-image }

로그인
![img]({{ '/assets/images/2022-10-28/img5.PNG' | relative_url }}){: .left-image }

로그인 성공 시 메인페이지
![img]({{ '/assets/images/2022-10-28/img6.PNG' | relative_url }}){: .left-image }

로그인 세션이 없을 때 게시판 화면
![img]({{ '/assets/images/2022-10-28/img7.PNG' | relative_url }}){: .left-image }

게시글 리스트 설정 변경시 페이지네이션과 보이는 리스트

> `file`\src\main\java\kr\ac\sesac\springboot\webproject\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

////수정할 것////
int countPage = 3; // 한 화면에 출력될 페이지 수
int countPost = 3; // 한 페이지에 출력할 게시글 수
////수정할 것////

...
{% endhighlight java %}
![img]({{ '/assets/images/2022-10-28/img8.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-28/img9.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-28/img10.PNG' | relative_url }}){: .left-image }

로그인 세션이 존재할 경우
![img]({{ '/assets/images/2022-10-28/img11.PNG' | relative_url }}){: .left-image }

게시글 작성
![img]({{ '/assets/images/2022-10-28/img12.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-28/img13.PNG' | relative_url }}){: .left-image }

게시글 상세보기
![img]({{ '/assets/images/2022-10-28/img14.PNG' | relative_url }}){: .left-image }

로그인 세션이 없을 시
![img]({{ '/assets/images/2022-10-28/img15.PNG' | relative_url }}){: .left-image }

게시글 수정
![img]({{ '/assets/images/2022-10-28/img16.PNG' | relative_url }}){: .left-image }
![img]({{ '/assets/images/2022-10-28/img17.PNG' | relative_url }}){: .left-image }

게시글 삭제
![img]({{ '/assets/images/2022-10-28/img18.PNG' | relative_url }}){: .left-image }

---
# 개선점
1. 회원가입 시 primary값인 아이디를 중복으로 쿼리문으로 넣으면 에러페이지를 출력하게 되는데 미리 아이디 중복체크 기능을 넣거나 쿼리문을 넣기 전에 if문으로 감싸 에러페이지를 방지 했어야 했다.
2. 게시글 페이지네이션도 '한 화면에 출력될 페이지 수'와 '한 페이지에 출력할 게시글 수'를 유저가 선택해서 변경이 가능하도록 커스터마이징 옵션을 주면 더 좋았을 것이다.
3. 게시글 상세페이지를 새로고침 할 때마다 조회수가 증가하는 문제가 있다. 사용자 쿠키를 읽어 이미 해당 게시글을 방문한 사용자라면 조회수 증가 쿼리를 실행하지 않는 방법이 있었을 것이다.
4. DB에 항목만 만들어 놓은 추천과 비추천을 구현하지 못했다. 해당 기능으로 추천과 비추천 수의 계산으로 일정이상의 점수를 받은 게시글을 상단이나 메인페이지에 노출시키는 것도 괜찮았을 것이다.
5. 덧글 기능은 게시글마다 comment DB항목을 달아 게시글 내용 아래쪽에 작성과 수정, 삭제가 가능하게 만들어야 했다.
6. 게시글 상세 페이지 하단에 게시글 목록을 출력해 게시글 페이지 이동을 자유롭게 만들어야 했다.
7. 관리자 계정을 설정해 모든 게시글에 삭제 권한을 주고 공지사항 작성 기능도 구현해 리스트를 호출 할 때 상단에 노출시키는 기능도 구현하고 싶었다.
8. 게시글을 삭제할 때 DB상에서 완전히 없애는게 아니라 삭제 컬럼을 하나 추가해 tinyint형으로 리스트를 불러올 때나 게시글 상세보기 시 이 컬럼이 0이면 불러오지 않도록 구현하면 게시글 복구도 가능하지 않을까 생각한다.

---
# 마무리
자바 스프링 부트의 기능을 넣는 것에 집중해서 웹페이지상의 디자인이 너무 심플했다. 보기만이라도 좋게 하려고 HTML로 이리저리 찾아봤지만 마음대로 되지 않아 시간을 많이 잡아 먹은 듯 하다.
DB와 연동해 필요한 것만 화면에 출력하는 게 생각보다 재미있었다. SQL에 대해 좀더 배우고 프로젝트를 시작했으면 더 즐거웠을 것 같다.
구현하고 싶은 기능들을 넣기위해 방법을 찾거나 테스트를 하는데 시간이 걸려 최초에 목표했던 기능들을 넣지 못한게 아쉽다. 그래도 구현한 기능들이 에러없이 출력되는 것을 보니 성취감은 있었다. 

---
# Reference
밈아: [[Thymeleaf] location.href에 변수 넣기 (th:onclick, GET)](https://mimah.tistory.com/entry/Thymeleaf-locationhref%EC%97%90-%EB%B3%80%EC%88%98-%EB%84%A3%EA%B8%B0-thonclick-GET)
{: style="text-align: right"}