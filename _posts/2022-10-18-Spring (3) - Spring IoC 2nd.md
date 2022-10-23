---
layout: post
title: Spring (3) - Spring IoC 2nd
date:   2022-10-18
description: Bean 생성 연습과 Maven
toc: true
comments: true
tags:
 - spring boot
---
---
1. 개발 환경 준비 [1)](/2022/10/Spring-(1)-%EA%B0%9C%EB%B0%9C-%ED%99%98%EA%B2%BD-%EC%A4%80%EB%B9%84/)
2. <span style="color:Turquoise">**Spring IoC**</span> [1)](/2022/10/Spring-(2)-Spring-IoC/) <span style="color:SteelBlue">**2)**</span>
3. Spring MVC [1)](/2022/10/Spring-(4)-Spring-MVC/) [2)](/2022/10/Spring-(5)-Spring-MVC-2nd/) [3)](/2022/10/Spring-(6)-Spring-MVC-3rd/)
4. <del>Database 활용</del>
5. View Template [1)](/2022/10/Spring-(7)-View-Template/) [2)](/2022/10/Spring-(8)-View-Template-2nd/)
6. AOP / Filter / Interceptor
7. File Upload / Download

---
# 연습
* ImageUtil 클래스를 bean으로 생성하고 save 메서드를 실행하여 URL의 이미지 다운로드할 수 있게 작성해보자

> `file`\src\main\java\com\example\demo\config\ImageUtil.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
package com.example.demo.config;

import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.net.URL;

import javax.imageio.ImageIO;

import org.springframework.stereotype.Component;

@Component
public class ImageUtil {
    public void save(String path) throws IOException {
        URL url = null;
        url = new URL(path);

        String fileName = path.substring(path.lastIndexOf("/") + 1);
        String fileExt = path.substring(path.lastIndexOf(".") + 1);

        BufferedImage img = ImageIO.read(url);
        // 루트 경로에 download 폴더가 존재해야 함
        ImageIO.write(img, fileExt, new File("download\\" + fileName));
    }
}
{% endhighlight %}

> `file`\src\main\java\com\example\demo\DemoApplication.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
...

import java.io.IOException;
import com.example.demo.config.ImageUtil;

...

public class DemoApplication {
	public static void main(String[] args) throws IOException {
        ApplicationContext context = new AnnotationConfigApplicationContext(BeanConfig.class);
        ImageUtil imageUtil = (ImageUtil) context.getBean("imageUtil");
		imageUtil.save("http://ggoreb.com/images/luffy.jpg");

...
{% endhighlight %}

---
# Maven
* 자바 프로젝트 관리 도구 (Build Tool)
* 컴파일/빌드/수행/테스트/배포 + **라이브러리 의존성 관리**
* Apache의 중앙 저장소 또는 별도의 자체 중앙 저장소 구축 가능
* 가장 많이 사용되는 빌드 툴 중 하나이다.

## 장점
* 빌드부터 배포까지의 작업 자동화
* 라이브러리의 버전 및 의존성 관리 편리하다.

## 단점
* 특정 플러그인의 설정에 문제가 생기면 작업 수행 불가능하다.
* 네트워크 상태가 원활하지 못한 경우 프로젝트 오류가 발생한다.

## Maven 프로젝트의 기본 구조
1. src 하위에 main 과 test 2가지로 구분된다.
2. main 에는 프로젝트의 소스코드, test 에는 main 소스코드의 test코드가 작성된다.
3. resources 에는 프로젝트에 필요한 자원 파일 저장한다.
4. pom.xml 에 정의된 의존성에 따라 Maven Dependencies 구성된다.
5. pom.xml 에 프로젝트 버전 등의 정보, 저장소, 라이브러리 의존, 플러그인 등의 설정이 저장된다.

## 라이브러리 관리
* dependency에 정의된 내용에 따라 중앙 저장소로부터 JAR 파일을 다운로드하여 로컬 저장소에 저장하고 프로젝트에 등록한다.
* 사용하려는 JAR가 로컬 저장소에 존재하면 그대로 사용하고 존재하지 않으면 중앙 저장소에서 다운로드한다.