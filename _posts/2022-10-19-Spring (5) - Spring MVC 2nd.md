---
layout: post
title: Spring (5) - Spring MVC 2nd
date:   2022-10-19
description: GetMapping과 ResponseBody를 이용한 응답처리, HttpServletRequest와 RequestParam을 이용한 요청처리
toc: true
comments: true
tags:
 - spring boot
 - java
---
---
1. 개발 환경 준비
2. Spring IoC
3. **_Spring MVC_**
4. Database 활용
5. View Template
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# 응답처리

>HTML 경로 표기법
* / : 웹사이트의 루트 폴더 (절대 경로)
* . : 현재 웹페이지가 소속된 폴더 (상대 경로)
* .. : 현재 웹페이지의 부모 폴더 (상대 경로)
* 자식폴더명 : 현재 소속된 폴더의 자식 폴더 (상대 경로)
출처 [https://sas-study.tistory.com/m/127](https://sas-study.tistory.com/m/127)

---
## `@GetMapping(value="")`
* `@RequestMapping(value="", method = RequestMethod.GET)` 와 같은 의미
* Get요청을 받았을 때 사용


### String
* 반환값에 보여줄 경로의 페이지 명시

* \src\main\java\com\example\basic\controller\HtmlController.java

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("/html/string")
public String string() {
    return "/html/string";
}

...
{% endhighlight %}
* 화면 출력 : /html/string.html


### void
* 요청에 명시된 페이지 표시

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("html/void")
public void htmlVoid() {

}

...
{% endhighlight %}
* 화면 출력 : html/void.html


### Map<키, 값>

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("html/map")
public Map<String, Object> htmlMap(Map<String, Object> map) {
    Map<String, Object> map2 = new HashMap<String, Object>();
    return map2;
}

...
{% endhighlight %}
* 화면 출력 : html/map.html


### Model

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("html/model")
public Model htmlModel(Model model) {
    return model;
}

...
{% endhighlight %}
* 화면 출력 : html/model.html


### ModelAndView
* setViewName 메소드로 출력할 화면을 지정

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("html/modelAndView")
public ModelAndView htmlModelAndView() {
    ModelAndView mav = new ModelAndView();
    mav.setViewName("html/model_and_view");
    return mav;
}

...
{% endhighlight %}
* 화면 출력 : html/model_and_view.html


### Object (DTO : Date transter object)

* Member 클래스 작성

* \src\main\java\com\example\basic\model\Member.java

>Java
{:.filename}
{% highlight java linenos %}
package com.example.basic.model;

import lombok.Data;

@Data // @Getter, @Setter, @toString 등등 annotation들을 종합해 놓은 annotation
public class Member {
    private String name;
    private String userId;
    private String userPassword;
}
{% endhighlight %}

>HTML
{:.filename}
{% highlight html linenos %}
<html xmlns:th="http://www.thymeleaf.org">

<head>
</head>

<body>
    <h1>Html Object</h1>
    [[${member}]]
    <hr>
    [[${member.name}]]
</body>

</html>
{% endhighlight %}

* `[[${}]]` : Thymeleaf 템플릿: 데이터를 주고 받을때 기본이 되는 형식

* \src\main\java\com\example\basic\controller\HtmlController.java

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("html/object")
public Member htmlObject() {
    Member member = new Member();
    member.setName("kim");
    return member; // member가 html의 member에 전송됨
}

...
{% endhighlight %}
* 화면 출력 : html/object.html

---
## `@ResponseBody`
* 별도의 html 페이지 없이 반환할 데이터를 전송


### String
* 리턴 문자열을 데이터만 body에 담아 전송

* \src\main\java\com\example\basic\controller\Json1Controller1.java

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("/json/string")
@ResponseBody
public String jsonString() {
    return "반환할 데이터";
}

...
{% endhighlight %}


### Map
* json의 형태로 응답

>리턴 타입이 map이나 model클래스 일 경우 json형태로 응답한다
{: .note}

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("json/map")
@ResponseBody
public Map<String, Object> jsonMap(Map<String, Object> map) {
    Map<String, Object> map2 = new HashMap<String, Object>();
    map2.put("key1", "value");
    map2.put("key2", 2324);
    map2.put("key3", true);
    return map2;
}

...
{% endhighlight %}

>json의 형태
* 키값-밸류값 으로 이루어진 자바스크립트의 파일 형식
{% highlight bash %}
{
    "userId":1,
    "name":"이름",
    "userPw":"password"
}
{% endhighlight %}


### Object (DTO : Date transter object)
* json의 형태로 응답

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("json/object")
@ResponseBody
public Member jsonObject() {
    Member member = new Member();
    member.setName("홍길동");
    member.setUserId("user01");
    member.setUserPassword("password");
    return member;
}

...
{% endhighlight %}


### List
* 배열의 형태로 응답

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("json/list")
@ResponseBody
public List<String> jsonList() {
    List<String> list = new ArrayList<>();
    list.add("list1");
    list.add("list2");
    list.add("list3");
    return list;
}

...
{% endhighlight %}

---
## 연습
1) http://localhost:8080/html/exam 접속 시 exam.html 띄우기

* \src\main\java\com\example\basic\controller\HtmlController.java

>Java
{:.filename}
{% highlight java %}
...

@GetMapping("html/exam")
public void exam(){

}

...
{% endhighlight %}

2) http://localhost:8080/json/exam 접속 시 json 데이터로 응답하기

* \src\main\java\com\example\basic\controller\Json1Controller.java

>Java
{:.filename}
{% highlight java %}
...

@GetMapping("json/exam")
@ResponseBody
public Map<String, Object> jsonExam() {
    Map<String, Object> map = new HashMap<>();
    List<Member> memList = new ArrayList<>();
    Member mem1 = new Member(), mem2 = new Member();

    mem1.setName("가");
    mem1.setUserId("A");
    mem1.setUserPassword("1");
        
    mem2.setName("나");
    mem2.setUserId("B");
    mem2.setUserPassword("2");
        
    memList.add(mem1);
    memList.add(mem2);

    map.put("count", 2);
    map.put("list", memList);

    return map;
}

...
{% endhighlight %}

---
# 요청 처리
* GET - 데이터를 가져오기
* POST – 데이터 저장
* PUT – 데이터 수정
* DELETE – 데이터 삭제

![img]({{ '/assets/images/2022-10-19/img1.PNG' | relative_url }}){: .center-image }*상태 코드*

---
## `@GetMapping`
* 일반적으로 주소를 입력해서 접속하면 무조건 GET요청

* \src\main\java\com\example\basic\controller\MethodController.java

>Java
{:.filename}
{% highlight java linenos %}
package com.example.basic.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MethodController {
    @GetMapping("req/get")
    // @RequestMapping(value = "req/get", method = RequestMethod.GET) 같은 의미
    public String get(){
        return "GET";
    }
}
{% endhighlight %}

---
## `@PostMapping`
* 데이터베이스 등의 저장소에 리소스를 저장할 때

>Java
{:.filename}
{% highlight java linenos %}
...

@PostMapping("req/post")
public String post(){
    return"POST";
}

...
{% endhighlight %}

---
## 요청 처리 시 사용하는 클래스와 어노테이션
* HttpServletRequest - 가장 전통적으로 사용되는 방식
* **`@RequestParam` (편리함)**
    파라미터 명칭에 맞게 변수 사용
    파라미터 종류 및 개수 상관없이 사용
* **`@PathVariable` (깔끔함)** - 요청 주소의 경로명 활용
* **`@ModelAttribute` (명확함)**
    Model / DTO / VO 등 객체와 연계하여 활용
    JPA, MyBatis 등 ORM 프레임워크 활용
* `@RequestBody` (AJAX 요청 시 주로 사용)
    보편적인 요청 파라미터 형식을 사용하지 않고 JSON 형태의 파라미터 사용
(Query String Parameter, Form Data, Payload)
    사용 시 메소드 방식을 POST로 지정

---
### `HttpServletRequest`
* 모든 파라미터는 문자열로 전달 (필요 시 숫자로 변환하여 사용)

* \src\main\java\com\example\basic\controller\RequustController.java

>Java
{:.filename}
{% highlight java linenos %}
package com.example.basic.controller;

import javax.servlet.http.HttpServletRequest;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class RequustController {
    @GetMapping("req/http")
    public String http(HttpServletRequest request) {
        String name = request.getParameter("name");
        String pageNum = request.getParameter("pageNum");
        return name + ", " + pageNum;
    }
}
{% endhighlight %}
요청
>http://localhost:8080/req/http?name=abc&pageNum=123

url 주소에서 ? 뒤에 오는 항목들이 모두 파라미터(매개변수)
파라미터의 형태는 '변수명=값', 파라미터간 구분은 &

응답
>abc, 123

---
### `@RequestParam`
* 지정된 파라미터가 없으면 400 오류 발생
* defaultValue 또는 required 옵션으로 오류 제어 가능
* `@RequestParam("key1")` 과 같이 name을 지정하지 않으면 변수명을 name으로 사용
* int 등 String이 아닌 형태로도 사용 가능

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("req/param1")
public String param1(@RequestParam("key1") String key, @RequestParam int key2) {
    return key + ", " + key2;
}

...
{% endhighlight %}
요청
>http://localhost:8080/req/param1?key1=abcd&key2=1234

응답
>abcd, 1234


* Map을 활용하면 파라미터를 정하지 않고 전달된 모든 파라미터를 동적으로 사용가능

>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("req/param2")
public String param2(@RequestParam Map<String, Object> map) {
    return map.toString();
}

...
{% endhighlight %}
요청
>http://localhost:8080/req/param2?name=abcde&pageNum=12&address=seoul

응답
>{name=abcde, pageNum=12, address=seoul}