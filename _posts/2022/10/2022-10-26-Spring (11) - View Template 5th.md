---
title: Spring (11) - View Template 5th
published: true
date:   2022-10-26
description: 데이터베이스 연동
comments: true
categories:
 - spring boot
tags:
 - spring boot
 - java
toc: true
toc_sticky: true
toc_label: View Template 5th
---
---
1. 개발 환경 준비 [1)](/spring%20boot/Spring-(01)-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%A4%80%EB%B9%84/)
2. Spring IoC [1)](/spring%20boot/Spring-(02)-Spring-IoC/) [2)](/spring%20boot/Spring-(03)-Spring-IoC-2nd/)
3. Spring MVC [1)](/spring%20boot/Spring-(04)-Spring-MVC/) [2)](/spring%20boot/Spring-(05)-Spring-MVC-2nd/) [3)](/spring%20boot/Spring-(06)-Spring-MVC-3rd/)
4. ~~Database 활용~~
5. <span style="color:Turquoise">**View Template**</span> [1)](/spring%20boot/Spring-(07)-View-Template/) [2)](/spring%20boot/Spring-(08)-View-Template-2nd/) [3)](/spring%20boot/Spring-(09)-View-Template-3rd/) [4)](/spring%20boot/Spring-(10)-View-Template-4th/) <span style="color:SteelBlue">**5)**</span>
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# MariaDB
* 오픈 소스의 관계형 데이터베이스 관리 시스템(RDBMS)이다.
* MySQL과 동일한 소스 코드를 기반으로 하며, GPL v2 라이선스를 따른다.
* 오라클 소유의 현재 불확실한 MySQL의 라이선스 상태에 반발하여 만들어졌으며, MariaDB는 MySQL과 소스코드를 같이 하므로 사용방법과 구조가 MySQL과 동일하다.

* MyBatis 설정 추가하기

> `file`\pom.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight java linenos %}
...

<!-- mybatis설정-->
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.2.0</version>
</dependency>

...
{% endhighlight xml %}

>`file`\src\main\resources\application.properties
{: style="text-align: right"}
>properties
{:.filename}
{% highlight bash %}
{% raw %}
# MyBatis
mybatis.mapper-locations=classpath:mapper/**/*.xml

spring.datasource.url=jdbc:mysql://localhost:3306/sesac?serverTimezone=UTC&characterEncoding=UTF-8
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=1234
{% endraw %}
{% endhighlight bash %}

> `file`\src\main\java\com\example\sesac\first\config\MyBatisConfig.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
package com.example.sesac.first.config;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@MapperScan(basePackages = "com.example.sesac.first.mapper")
public class MyBatisConfig {
    
}
{% endhighlight java %}


---
## 회원 관련 기능
### 회원가입
* 세션 활용 회원가입 요청을 수정한다.

* BD와 연결해서 BD에 user정보를 삽입하는 XML을 작성한다.

> `file`\src\main\resources\mapper\userMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.sesac.first.mapper.UserMapper">
    <insert id="join" parameterType="com.example.sesac.first.model.User">
        INSERT INTO user VALUES(#{userId},#{userPw},#{userName},#{userAddr})
    </insert>
</mapper>
{% endhighlight xml %}

* user mapper 인터페이스를 작성한다.

> `file`\src\main\java\com\example\sesac\first\mapper\UserMapper.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
package com.example.sesac.first.mapper;

import org.apache.ibatis.annotations.Mapper;

import com.example.sesac.first.model.User;

@Mapper
public interface UserMapper {
    public void join(User user);
    public String getPw(String id);
    public User selectUser(String id);
}
{% endhighlight java %}

> `file`\src\main\java\com\example\sesac\first\controller\UserController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
@Autowired
UserMapper userMapper;

...

@PostMapping("join")
public String join(User user, HttpSession session) {
    userMapper.join(user);
    return "redirect:/";
}

...
{% endhighlight java %}

---
### 로그인
* 세션 활용 로그인 요청을 수정한다.

> `file`\src\main\java\com\example\sesac\first\controller\UserController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@PostMapping("login")
public String login(User user, HttpSession session) {
    String id = user.getUserId();
    String pw = user.getUserPw();
    // 로그인 시도 id로 DB에서 pw값을 가져온다.
    String getPw = userMapper.getPw(id);
    if (getPw != null) {
        if (getPw.equals(pw)) {
            // DB에서 user정보를 가져와서 userData에 저장
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

* 로그인 요청을 BD와 연동하기

> `file`\src\main\resources\mapper\userMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<select id="getPw" parameterType="String" resultType="String">
    SELECT userPw FROM user WHERE userId = #{userId}
</select>
<select id="selectUser" parameterType="String" resultType="com.example.sesac.first.model.User">
    SELECT * FROM user WHERE userId = #{userId}
</select>

...
{% endhighlight xml %}

---
## 게시판 관련 기능
* board mapper 인터페이스 작성한다.

> `file`\src\main\java\com\example\sesac\first\mapper\BoardMapper.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
package com.example.sesac.first.mapper;

import java.util.ArrayList;

import org.apache.ibatis.annotations.Mapper;

import com.example.sesac.first.model.Board;

@Mapper
public interface BoardMapper {
    public ArrayList<Board> boardList();
    public void boardCreate(Board board);
    public void boardUpdate(Board board);
    public void boardRemove(int boardNo);
}
{% endhighlight java %}

---
### Read
* 게시글 전체 읽어오기

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.example.sesac.first.mapper.BoardMapper">
    <select id="boardList" resultType="com.example.sesac.first.model.Board">
        SELECT * FROM board
    </select>
</mapper>
{% endhighlight xml %}

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
@Autowired
BoardMapper boardMapper;

...

@GetMapping("boardList")
public String boardList(Model model, HttpSession session) {
    // DB에서 게시글 목록을 전부 가져와서 model에 담아준다.
    List<Board> boardList = boardMapper.boardList();
    model.addAttribute("boardList", boardList);
    return "board/boardList";
}

...
{% endhighlight java %}

---
### Create
* 새 게시글 작성하기

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<insert id="boardCreate" parameterType="com.example.sesac.first.model.Board">
    INSERT INTO board VALUES(NULL,#{boardTitle},#{boardContent},#{boardWriter})
</insert>

...
{% endhighlight xml %}

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("boardCreate")
public String boardCreate() {
    return "board/boardCreate";
}

@PostMapping("boardCreate")
public String boardCreate(Board board, HttpSession session) {
    User user = (User) session.getAttribute("user");
    board.setBoardWriter(user.getUserId());
    boardMapper.boardCreate(board);
    return "redirect:/board/boardList";
}

...
{% endhighlight java %}

---
### Update
* 게시글 수정하기

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<update id="boardUpdate" parameterType="com.example.sesac.first.model.Board">
    UPDATE board SET boardTitle=#{boardTitle}, boardContent=#{boardContent}
    WHERE boardNo=#{boardNo}
</update>

...
{% endhighlight xml %}

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("boardUpdate")
public String boardUpdate(Model model, HttpSession session, @RequestParam int boardNo) {
    // 수정하고자 하는 페이지의 글정보를 가져와 페이지에 표시
    List<Board> boardList = boardMapper.boardList();
    User user = (User) session.getAttribute("user");
    for (Board board : boardList) {
        if (board.getBoardNo() == boardNo && board.getBoardWriter().equals(user.getUserId())) {
            model.addAttribute("board", board);
            return "board/boardUpdate";
        }
    }
    return "board/boardError";
}

@PostMapping("boardUpdate")
public String boardUpdate(Board board, HttpSession session) {
    boardMapper.boardUpdate(board);
    return "redirect:/board/boardList";
}

...
{% endhighlight java %}

---
### Delete
* 게시글 삭제하기

> `file`\src\main\resources\mapper\boardMapper.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
...

<delete id="boardRemove" parameterType="int">
    DELETE FROM board WHERE boardNo=#{boardNo}
</delete>

...
{% endhighlight xml %}

> `file`\src\main\java\com\example\sesac\first\controller\BoardController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("boardRemove")
public String boardRemove(@RequestParam int boardNo) {
    boardMapper.boardRemove(boardNo);
    return "redirect:/board/boardList";
}

...
{% endhighlight java %}

---
# Reference

* 이 포스트는 SeSAC 인공지능 SW 개발자 양성 과정 - 김영식 강사님의 강의내용을 정리한 것입니다.
* 아기상어 뚜루루뚜루: [MariaDB | 윈도우 MariaDB 설치 및 접속하기](https://kitty-geno.tistory.com/55)
