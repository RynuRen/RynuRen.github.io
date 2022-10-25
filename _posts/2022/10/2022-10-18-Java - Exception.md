---
layout: post
title: Java - Exception
date:   2022-10-18
description: 예외 처리
toc: true
comments: true
tags:
 - java
---
---
# 예외 처리
(기초 자바 강의에서 넘어간...)

**예외 (Exception)**
:사용자의 잘못된 조작, 개발자의 잘못된 코딩으로 인한 오류
예외가 발생되면 프로그램이 바로 종료되지만 예외 처리를 추가하면 정상 실행 상태로 돌아갈 수 있다.
1. 컴파일 오류 (Compile Error)
2. 런타임 오류 (Runtime Error)
3. 논리 오류 (Logical Error) - 흐름상 잘못된 코딩

---
## 예외의 종류

>Java
{:.filename}
{% highlight java %}
// compile error
int a = 1.2;

// runtime exception
// Arithmetic: 산술
System.out.println(4 / 0);
        
// IndexOutOfBounds: 인덱스 범위 벗어남
System.out.println(new String().charAt(1));

// NullPointer
String str = null;
System.out.println(str.equals(""));

// NegativeArraySize 1
int[] arrs = new int[-1];

// NegativeArraySize 2
String s = "Exception";
int count = s.indexOf("a"); // indexOf():값의 인덱스를 확인, 없으면 -1 반환
int[] arrays = new int[count]; // NegativeArraySize
System.out.println(arrays);

// exception
new File("").createNewFile();
{% endhighlight %}

---
## if문을 이용한 예외처리

>Java
{:.filename}
{% highlight java linenos %}
// String numStr = " 123";
// int num = Integer.parseInt(numStr); // NumberFormatException

String numStr = " 123";
Pattern p = Pattern.compile("^[0-9]*$"); // 정규식 표현, pattern 정규식 생성
Matcher m = p.matcher(numStr); // 패턴 매칭
boolean isNumber = m.matches(); // 해당 변수가 정규식 표현과 매치여부 판단
if (isNumber) {
    // 사전 작업없이 그냥 실행시키면 에러가 발생하므로
    // if문을 사용하여 해당 작업이 실행 가능할 때만 실행되도록 작성
    int num = Integer.parseInt(numStr);
}
System.out.println("isNumber : " + isNumber);

// Object obj = new String("a");
// int a = (Integer) obj; // ClassCastException

Object obj = new String("a");
if (obj instanceof Integer) {
    int a = (Integer) obj;
    System.out.println("a1 : " + a);
} else if (obj instanceof String) {
    String a = (String) obj;
    System.out.println("a2 : " + a);
}
{% endhighlight %}

---
## try-catch-finally를 이용한 예외처리

>Java
{:.filename}
{% highlight java linenos %}
String numStr = " 123";
try {
    int num = Integer.parseInt(numStr); // parseInt(): 문자열을 int형으로 변환하는 메소드
    System.out.println("정상실행 : " + num);
} catch (NumberFormatException e) { // Exception으로 받으면 포괄적 처리
    e.printStackTrace(); // e에 대한 자세한 예외를 출력
} finally {
    System.out.println("항상 실행 1");
}
        
Object obj = new String("a");
try {
    int a = (Integer) obj;
    System.out.println("정상실행 : " + a);
} catch (ClassCastException e) {
    System.out.println(e.getMessage());
}finally {
    System.out.println("항상 실행 2");
}
{% endhighlight %}

---
### 다중 예외 처리

>Java
{:.filename}
{% highlight java linenos %}
String text = "";
try {
    BufferedReader reader = new BufferedReader(new FileReader("data/소나기.txt"));
    while (true) {
        String data = reader.readLine();
        if (data == null)
            break;
            text += data + "\n";
        }
    reader.close();
} catch (FileNotFoundException e) {
    System.out.println(e.getMessage());
} catch (IOException e) {
    e.printStackTrace();
} finally {
    System.out.println(text);
}
{% endhighlight %}

---
## 예외 떠넘기기 (throw)
메소드를 호출한 곳으로 떠넘기는 역할

>Java
{:.filename}
{% highlight java linenos %}
public static void main(String[] args) throws NumberFormatException, ClassCastException{
    String numStr = " 123";
    int num = Integer.parseInt(numStr);
    
    Object obj = new String("a");
    int a = (Integer) obj;
    
    System.out.println("정상 출력");
}
{% endhighlight %}

---
### 의도적인 예외 발생

> `file`ThrowExam.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
import java.rmi.AccessException;

public class ThrowExam {
    private String strNum;
    private String num;

    public String getStrNum() {
        return strNum;
    }

    public String getNum() {
        return num;
    }

    public void setStrNum(String strNum) {
        this.strNum = strNum;
    }

    public void setNum(String num) {
        this.num = num;
    }

    public void check() throws AccessException {
        try {
            int num = Integer.parseInt(this.strNum);
            Object obj = new String(this.num);
            int a = (Integer) obj;
        } catch (NumberFormatException e) {
            throw new AccessException("숫자 변환 실패");
        } catch (ClassCastException e) {
            throw new AccessException("형변환 실패");
        }
    }
}
{% endhighlight %}

> `file`ErrorExam03_thorw.java
{: style="text-align: right"}
>Java
{:.filename}
{% highlight java linenos %}
public class ErrorExam03_throws {
    public static void main(String[] args) {
        ThrowExam throwExam = new ThrowExam();
        throwExam.setNum("123");
        throwExam.setStrNum("abc");
        try {
            throwExam.check();
        } catch (AccessException e) {
            e.printStackTrace();
        }
    }
}
{% endhighlight %}