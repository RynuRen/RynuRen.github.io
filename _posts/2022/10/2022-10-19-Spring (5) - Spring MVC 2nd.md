---
title: Spring (5) - Spring MVC 2nd
published: true
date:   2022-10-19
description: GetMapping과 ResponseBody를 이용한 응답처리, HttpServletRequest와 RequestParam을 이용한 요청처리
comments: true
categories:
 - spring boot
tags:
 - spring boot
 - java
toc: true
toc_sticky: true
toc_label: Spring MVC 2nd
---
---
1. 개발 환경 준비 [1)](/2022/10/Spring-(1)-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%A4%80%EB%B9%84/)
2. Spring IoC [1)](/2022/10/Spring-(2)-Spring-IoC/) [2)](/2022/10/Spring-(3)-Spring-IoC-2nd/)
3. <span style="color:Turquoise">**Spring MVC**</span> [1)](/2022/10/Spring-(4)-Spring-MVC/) <span style="color:SteelBlue">**2)**</span> [3)](/2022/10/Spring-(6)-Spring-MVC-3rd/)
4. ~~Database 활용~~
5. View Template [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/) [3)](/2022/10/Spring-(9)-View-Template-3rd/) [4)](/2022/10/Spring-(10)-View-Template-4th/) [5)](/2022/10/Spring-(11)-View-Template-5th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# 응답처리

> HTML 경로 표기법
* / : 웹사이트의 루트 폴더 (절대 경로)
* . : 현재 웹페이지가 소속된 폴더 (상대 경로)
* .. : 현재 웹페이지의 부모 폴더 (상대 경로)
* 자식폴더명 : 현재 소속된 폴더의 자식 폴더 (상대 경로)

## `@GetMapping`
* `@GetMapping(value="")`는 `@RequestMapping(value="", method = RequestMethod.GET)`와 같은 의미이다.
* Get 요청을 받았을 때 사용한다.

---
### return String
* 반환값에 보여줄 경로의 페이지를 명시한다.

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
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
> /html/string.html

---
### void
* 요청에 명시된 페이지를 표시한다.

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

@GetMapping("html/void")
public void htmlVoid() {

}

...
{% endhighlight %}
> html/void.html

---
### return Map<키, 값>

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
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
> html/map.html

---
### return Model

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
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
> html/model.html

---
### return ModelAndView
* `setViewName()` 메서드로 출력할 화면을 지정한다.

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
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
> html/model_and_view.html

---
### return Object (DTO : Date transter object)

* Member 클래스 작성

> `file`\src\main\java\com\example\basic\model\Member.java
{: style="text-align: right"}
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

* html 페이지 작성

> `file`\src\main\resources\templates\html\object.html
{: style="text-align: right"}
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

> `[[${변수}]]`
: Thymeleaf 템플릿: 데이터를 주고 받을때 기본이 되는 형식이다.
{: .note}

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
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
> http://localhost:8080/html/object
![img]({{ '/assets/images/2022/10/19/img2.PNG' | relative_url }}){: .left-image }

---
## `@ResponseBody`
* 별도의 html 페이지 없이 반환할 데이터를 전송한다.

---
### return String
* 리턴 문자열을 데이터만 body에 담아 전송한다.

> `file`\src\main\java\com\example\basic\controller\Json1Controller1.java
{: style="text-align: right"}
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

---
### return Map<키, 값>
* json의 형태로 응답한다.

> 리턴 타입이 map이나 model클래스 일 경우 json형태로 응답한다.
{: .note}

> `file`\src\main\java\com\example\basic\controller\Json1Controller1.java
{: style="text-align: right"}
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
> http://localhost:8080/json/map
![img]({{ '/assets/images/2022/10/19/img3.PNG' | relative_url }}){: .left-image }

> json의 형태
* 키값-밸류값 으로 이루어진 자바스크립트의 파일 형식이다.
{% highlight bash %}
{
    "userId":1,
    "name":"이름",
    "userPw":"password"
}
{% endhighlight %}

---
### return Object (DTO : Date transter object)
* json의 형태로 응답한다.

> `file`\src\main\java\com\example\basic\controller\Json1Controller1.java
{: style="text-align: right"}
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
> http://localhost:8080/json/object
![img]({{ '/assets/images/2022/10/19/img4.PNG' | relative_url }}){: .left-image }

---
### return List
* 배열의 형태로 응답한다.

> `file`\src\main\java\com\example\basic\controller\Json1Controller1.java
{: style="text-align: right"}
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
> http://localhost:8080/json/object
![img]({{ '/assets/images/2022/10/19/img5.PNG' | relative_url }}){: .left-image }

---
## 연습
1) http://localhost:8080/html/exam 접속 시 exam.html 띄워보자

> `file`\src\main\java\com\example\basic\controller\HtmlController.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java %}
...

@GetMapping("html/exam")
public void exam(){

}

...
{% endhighlight %}

> `file`\src\main\resources\templates\html\exam.html
{: style="text-align: right"}
>HTML
{:.filename}
{% highlight html linenos %}
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.1.2/dist/css/bootstrap.min.css" rel="stylesheet">
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.1.2/dist/js/bootstrap.bundle.min.js"></script>
<div class="p-5 bg-primary text-white text-center">
    <h1>My First Bootstrap 5 Page</h1>
    <p>Resize this responsive page to see the effect!</p>
</div>
<nav class="navbar navbar-expand-sm bg-dark navbar-dark">
    <div class="container-fluid">
        <ul class="navbar-nav">
            <li class="nav-item"><a class="nav-link active" href="#">Active</a></li>
            <li class="nav-item"><a class="nav-link" href="#">Link</a></li>
        </ul>
    </div>
</nav>
<div class="container mt-5">
    <div class="row">
        <div class="col-sm-12">
            <h2 class="mt-5">TITLE HEADING</h2>
            <h5>Title description, Sep 2, 2020</h5>
            <p>Sunt in culpa qui officia deserunt mollit</p>
        </div>
    </div>
</div>
{% endhighlight %}
> http://localhost:8080/html/exam
![img]({{ '/assets/images/2022/10/19/img6.PNG' | relative_url }}){: .left-image }


2) http://localhost:8080/json/exam 접속 시 json 데이터로 응답해보자

> `file`\src\main\java\com\example\basic\controller\Json1Controller.java
{: style="text-align: right"}
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
> http://localhost:8080/json/exam
![img]({{ '/assets/images/2022/10/19/img7.PNG' | relative_url }}){: .left-image }

---
# 요청 처리
* GET - 데이터를 가져오기
* POST – 데이터 저장
* PUT – 데이터 수정
* DELETE – 데이터 삭제

![img]({{ '/assets/images/2022/10/19/img1.PNG' | relative_url }}){: .center-image }*상태 코드*

---
## `@GetMapping`
* 일반적으로 주소를 입력해서 접속하면 무조건 GET요청이다.

> `file`\src\main\java\com\example\basic\controller\MethodController.java
{: style="text-align: right"}
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
* 데이터베이스 등의 저장소에 리소스를 저장할 때 사용한다.

> `file`\src\main\java\com\example\basic\controller\MethodController.java
{: style="text-align: right"}
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
* `HttpServletRequest` 클래스 - 가장 전통적으로 사용되는 방식이다.
* `@RequestParam` **(편리함)**
    파라미터 명칭에 맞게 변수를 사용한다.
    파라미터 종류 및 개수 상관없이 사용한다.
* `@PathVariable` **(깔끔함)** - 요청 주소의 경로명을 활용한다.
* `@ModelAttribute` **(명확함)**
    Model / DTO / VO 등 객체와 연계하여 활용한다.
    JPA, MyBatis 등 ORM 프레임워크를 활용한다.
* `@RequestBody` (AJAX 요청 시 주로 사용)
    보편적인 요청 파라미터 형식을 사용하지 않고 JSON 형태의 파라미터를 사용한다. (Query String Parameter, Form Data, Payload)
    사용 시 메소드 방식을 POST로 지정한다.

---
### `HttpServletRequest`
* 모든 파라미터는 문자열로 전달한다. (필요 시 숫자로 변환하여 사용)

> `file`\src\main\java\com\example\basic\controller\RequustController.java
{: style="text-align: right"}
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
> http://localhost:8080/req/http?name=abc&pageNum=123
![img]({{ '/assets/images/2022/10/19/img8.PNG' | relative_url }}){: .left-image }

> url 주소에서 ? 뒤에 오는 항목들이 모두 파라미터(매개변수)이다.
파라미터의 형태는 '변수명=값', 파라미터간 구분은 &로 한다.
{: .note}

---
### `@RequestParam`
* 지정된 파라미터가 없으면 400 오류가 발생한다.
* defaultValue 또는 required 옵션으로 오류 제어가 가능하다.
* `@RequestParam("key1")` 과 같이 name을 지정하지 않으면 변수명을 name으로 사용한다.
* int 등 String이 아닌 형태로도 사용 가능하다.

> `file`\src\main\java\com\example\basic\controller\RequustController.java
{: style="text-align: right"}
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
> http://localhost:8080/req/param1?key1=abcd&key2=1234
![img]({{ '/assets/images/2022/10/19/img9.PNG' | relative_url }}){: .left-image }

* Map을 활용하면 파라미터를 정하지 않고 전달된 모든 파라미터를 동적으로 사용가능하다.

> `file`\src\main\java\com\example\basic\controller\RequustController.java
{: style="text-align: right"}
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
> http://localhost:8080/req/param2?name=abcde&pageNum=12&address=seoul
![img]({{ '/assets/images/2022/10/19/img10.PNG' | relative_url }}){: .left-image }

---
# Reference
{: style="text-align: right"}
코딩하는흑구: [[HTML] 경로표기법(절대경로, 상대경로)](https://sas-study.tistory.com/m/127)
{: style="text-align: right"}