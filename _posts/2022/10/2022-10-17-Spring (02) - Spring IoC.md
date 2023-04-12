---
title: Spring (2) - Spring IoC
published: true
date:   2022-10-17
description: Bean 생성
comments: true
categories:
 - spring boot
tags:
 - spring boot
 - java
toc: true
toc_sticky: true
toc_label: Spring IoC
---
---
1. 개발 환경 준비 [1)](/spring%20boot/Spring-(01)-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%A4%80%EB%B9%84/)
2. <span style="color:Turquoise">**Spring IoC**</span> <span style="color:SteelBlue">**1)**</span> [2)](/spring%20boot/Spring-(03)-Spring-IoC-2nd/)
3. Spring MVC [1)](/spring%20boot/Spring-(04)-Spring-MVC/) [2)](/spring%20boot/Spring-(05)-Spring-MVC-2nd/) [3)](/spring%20boot/Spring-(06)-Spring-MVC-3rd/)
4. ~~Database 활용~~
5. View Template [1)](/spring%20boot/Spring-(07)-View-Template/) [2)](/spring%20boot/Spring-(08)-View-Template-2nd/) [3)](/spring%20boot/Spring-(09)-View-Template-3rd/) [4)](/spring%20boot/Spring-(10)-View-Template-4th/) [5)](/spring%20boot/Spring-(11)-View-Template-5th/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# Spring IoC (제어 역전; Inversion of Control)
* 제어권이 framework에게 있어 필요에 따라 Spring framework가 사용자의 코드를 호출

* Spring Container
: bean의 생명주기를 관리한다.

![img]({{ '/assets/images/2022/10/17/img2.PNG' | relative_url }}){: .center-image }*Spring Container*

* `BeanFactory`
: 객체 생성과 검색에 대한 기능을 정의하는 interface이다.
Spring의 IoC를 담당하는 핵심 컨테이너이다.

* `ApplicationContext`
: 위의 `BeanFactory` interface를 상속하는, 기능이 더욱 추가된 interface이다.
자원 처리 추상화나 메시지, 프로필, 환경변수 등을 처리할 수 있는 기능을 추가로 제공하고 있어 `BeanFactory`보다 더 자주 사용된다.

* `AnnotationConfigApplicationContext`
: `ApplicationContext` interface를 구현한 클래스로 `@Configuration` 어노테이션이 붙은 클래스를 이용하여 설정 정보를 읽어와 bean객체를 생성, 관리한다.

* `ClassPathXmlApplicationContext`
: `ApplicationContext` interface를 구현한 클래스로 classpath의 XML파일에서 설정 정보를 읽어온다.

---
# Bean 생성 및 주입
## 1) `@Congiguration` 어노테이션 클래스에서 직접 등록
* 메서드 자체를 bean객체로 등록하는 방법

### - `@Configuration` 작성
* `@Configuration`
: 해당 클래스가 Spring 설정에 사용되는 클래스라는 것을 의미한다.
클래스 이름은 자유롭게 지정 가능하다.

* `@Bean`
: 해당 메서드가 생성한 객체를 Spring이 관리하는 bean객체로 등록한다.
메서드 명: bean객체의 이름으로 지정, 구분할 때 사용한다.
메서드의 매개변수는 bean에 등록된 객체가 있다면 자동으로 전달한다.

> `file`\src\main\java\com\example\demo\config\BeanConfig.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java %}
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class BeanConfig {
    @Bean
    public String bean1(){
        return "첫번째 빈";
    }
}
{% endhighlight %}


### - main method 수정
* `getBean()`
: 생성된 bean객체를 검색할 때 사용된다.

> `file`\src\main\java\com\example\demo\DemoApplication.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java %}
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import com.example.demo.config.BeanConfig;

@SpringBootApplication
public class DemoApplication {
	public static void main(String[] args) {
		// 클래스1 변수명 = new 클래스1();
		// 변수명.메서드()  ... 와 결과적으로 같은 형태
		ApplicationContext context = new AnnotationConfigApplicationContext(BeanConfig.class);
		String s = (String) context.getBean("bean1");
		System.out.println("bean 생성1 : " + s);

		SpringApplication.run(DemoApplication.class, args);
	}
}
{% endhighlight %}

---
## 2) XML 파일에서 정보를 읽어와 등록

### - xml 파일 작성
> `file`\src\main\resources\bean2.xml
{: style="text-align: right"}
>XML
{:.filename}
{% highlight xml linenos %}
{% raw %}
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="bean2" class="java.lang.Integer">
        <constructor-arg value="2324" />
    </bean>
</beans>
{% endraw %}
{% endhighlight xml %}

### - main method 수정
> `file`\src\main\java\com\example\demo\DemoApplication.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos%}
...

import org.springframework.context.support.ClassPathXmlApplicationContext;

...

        ApplicationContext context2 = new ClassPathXmlApplicationContext("classpath:bean2.xml");
        int i = (int) context2.getBean("bean2");
        System.out.println("bean 생성2 : " + i);

...
{% endhighlight %}

---
## 3) `@ComponentScan`으로 `@Component`를 탐색해서 등록
* `@ComponentScan`
: Component를 스캔해준다

> Spring Boot의 main메서드가 포함된 패키지에 붙는 `@SpringBootApplicaiton` 어노테이션을 `Crtl`{:key} + `클릭`으로 들어가보면 `@ComponentScan`도 포함되어 있음을 확인할 수 있다.
따라서 해당 클래스가 포함된 package의 하위 package에 대해 스캔하여 빈으로 등록한다. 이 범위를 벗어난 컴포넌트를 빈으로 등록하기 위해 `@ComponentScan`으로 범위를 지정할 수 있다.

* `basePackage`
: 스캔을 시작할 루트(최상위) package를 정해준다.

* `basePackageClasses`
: 설정된 클래스의 package로부터 모든 하위의 package에 대해 component를 스캔한다.

### - `@Configuration` 수정
> `file`\src\main\java\com\example\demo\config\BeanConfig.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos%}
...

@Configuration
@ComponentScan(basePackages = "com.example.demo.config")
public class BeanConfig {

...
{% endhighlight %}

* class 자체를 검색할 수도 있다

>Java
{:.filename}
{% highlight java linenos%}
...

@ComponentScan(basePackageClasses = Bean3.class) //또는 BeanConfig.class

...
{% endhighlight %}

### - Bean으로 등록할 클래스 작성
> `file`\src\main\java\com\example\demo\config\Bean3.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos%}
package com.example.demo.config;

import org.springframework.stereotype.Component;

@Component
public class Bean3 {
    public String run(){
        return "세번째 빈";
    }
}
{% endhighlight %}


### - main method 수정
> `file`\src\main\java\com\example\demo\DemoApplication.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos%}
...

ApplicationContext context3 = new AnnotationConfigApplicationContext(BeanConfig.class);
Bean3 b = (Bean3) context3.getBean("bean3"); // 클래스가 컴포넌트화 되면서 첫문자가 소문자가 됨
System.out.println(b.run());

...
{% endhighlight %}

---
# Stereo Type Annotation
* Spring에서 기본적으로 제공하는 어노테이션이다.
* 특수한 기능을 가지고 있다.
* 자주 사용되는 대표적인 4종류
1. `@Controller`: 요청과 응답을 처리하는 클래스에 사용한다.
2. `@Service`: 비지니스 로직을 구현한 클래스에 사용한다.
3. `@Repository`: Persistence 역할을 하는 클래스에 사용한다.
4. `@Component`: 위 3가지 어노테이션의 상위(부모) 객체이다.

---
# 실습
1. 프로젝트 생성(이름: basic)
2. 2번째 pdf 파일의 bean 생성 실습 적용

---
# Reference

* 이 포스트는 SeSAC 인공지능 SW 개발자 양성 과정 - 김영식 강사님의 강의내용을 정리한 것입니다.
* ~~젼꾸이자 쪄니: [스프링 기본 핵심원리 (스프링 컨테이너와 스프링 빈)](https://jihyunhillpark.github.io/springframework/spring-fundamental4/)~~
