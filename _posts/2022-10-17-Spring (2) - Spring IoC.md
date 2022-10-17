---
layout: post
title: Spring (2) - Spring IoC
date:   2022-10-17
description: Spring (2) - Spring IoC
toc: true
tags:
 - spring boot
---

1. 개발 환경 준비
2. **_Spring IoC_**
3. Spring MVC
4. Database 활용
5. View Template
6. AOP / Filter / Interceptor
7. File Upload / Download


# Bean 생성 및 주입
## 1) @Congiguration 파일의 메소드에서 객체 반환
* 메소드 자체를 bean으로 등록

### @configuration 작성
* `@Configuration`: 해당 클래스가 Spring 설정에 사용되는 클래스라는 것을 의미
클래스 이름은 자유롭게 지정 가능

* `@Bean`: 해당 클래스가 Bean을 등록하는 메소드라는 것을 의미
메소드 명: Bean의 이름으로 지정
메소드의 매개변수는 Bean에 등록된 객체가 있다면 자동으로 전달

* \src\main\java\com\example\demo\config\BeanConfig.java

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


### main method 수정
* \src\main\java\com\example\demo\DemoApplication.java

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
		// 변수명.메소드()  ... 와 결과적으로 같은 형태
		ApplicationContext context = new AnnotationConfigApplicationContext(BeanConfig.class);
		String s = (String) context.getBean("bean1");
		System.out.println("bean 생성1 : " + s);

		SpringApplication.run(DemoApplication.class, args);
	}
}
{% endhighlight %}

## 2) XML 파일 Element 작성

### xml 파일 작성
* \src\main\resources\bean2.xml

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

### main method 수정
* \src\main\java\com\example\demo\DemoApplication.java

>Java
{:.filename}
{% highlight java linenos%}
...

import org.springframework.context.support.ClassPathXmlApplicationContext;

...

        ApplicationContext context2 = new ClassPathXmlApplicationContext("classpath:bean2.xml");
        int i = (int) context2.getBean("bean2");
        System.out.println("bean 생성2 : " + i);
{% endhighlight %}

## 3) @ComponentScan
* `@ComponentScan`: Component를 스캔해준다
`basePackage`: 스캔을 시작할 루트(최상위) package를 정해줌
`basePackageClasses`: 설정된 클래스의 패키지로부터 모든 하위의 패키지에 대해 컴포넌트를 스캔

### @Configuration 수정
* \src\main\java\com\example\demo\config\BeanConfig.java

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


### Bean 작성
* \src\main\java\com\example\demo\config\Bean3.java

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

### main method 수정
* \src\main\java\com\example\demo\DemoApplication.java

>Java
{:.filename}
{% highlight java linenos%}
...

ApplicationContext context3 = new AnnotationConfigApplicationContext(BeanConfig.class);
Bean3 b = (Bean3) context3.getBean("bean3"); // 클래스가 컴포넌트화 되면서 첫문자가 소문자가 됨
System.out.println(b.run());

...
{% endhighlight %}

# Stereo Type Annotation
* 스프링에서 기본적으로 제공
* 특수한 기능을 가지고 있음
* 자주 사용되는 대표적인 4종류
1. `@Controller`: 요청과 응답을 처리하는 클래스에 사용
2. `@Service`: 비지니스 로직을 구현한 클래스에 사용
3. `@Repository`: Persistence 역할을 하는 클래스에 사용
4. `@Component`: 위 3가지 어노테이션의 상위(부모) 객체

# 실습
1. 프로젝트 생성(이름: basic)
2. 2번째 pdf 파일의 bean 생성 실습 적용
