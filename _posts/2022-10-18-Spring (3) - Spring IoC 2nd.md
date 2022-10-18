---
layout: post
title: Spring (3) - Spring IoC 2nd
date:   2022-10-18
description: Spring (3) - Spring IoC 이어서...
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

---
# 연습문제
* ImageUtil 클래스를 Bean으로 생성, save 메소드를 실행하여 URL의 이미지 다운로드 하기
* \src\main\java\com\example\demo\config\ImageUtil.java

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

* \src\main\java\com\example\demo\DemoApplication.java

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
* 빌드 툴
* 컴파일/빌드/수행/테스트/배포 + 라이브러리 의존성 관리
* Apache의 중앙 저장소 또는 별도의 자체 중앙 저장소 구축 가능
* 가장 많이 사용되는 빌드 툴 중 하나
* pom.xml 에 프로젝트 버전 등의 정보, 저장소, 라이브러리 의존, 플러그인 등의 설정이 저장됨

## 라이브러리 관리
* dependency에 정의된 내용에 따라 중앙 저장소로부터 JAR 파일을 다운로드하여 로컬 저장소로 저장하고 프로젝트에 등록
* 사용하려는 JAR가 로컬 저장소에 존재하면 그대로 사용하고
존재하지 않으면 중앙 저장소로부터 다운로드