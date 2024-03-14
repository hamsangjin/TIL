# 자바란

> 미국 Sun Microsystems사에서 개발한 객체지향 프로그래밍 언어로, C++문법을 기본으로 개발한 언어이다.

<br>

# 자바 프로그램 작성과 실행

## 코드 작성

```java
public class Hello{
    public static void main(String[] args){
        System.out.println("Hello");
    }
}
```

## 코드 컴파일

터미널을 이용해 파일이 생성된 폴더로 이동해 자바 컴파일 명령어인 `javac`를 이용해 컴파일하고, 컴파일된 클래스 파일을 실행해보자.

![Untitled](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/15790d62-e992-4ea6-8536-ed1b8b86ba6d/Untitled.png?id=7c50bf51-42c2-42e8-a26d-d16a1a48839a&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710468000000&signature=ajc9mqQzTeFqXuS1_2KcQvTybzpSwTwGNj9RHDF4RJA&downloadName=Untitled.png)

<br>

# **Hello.java 파일 분석하기**

```java
public class Hello{
    public static void main(String[] args){
		    System.out.println("Hello");
    }
}
```

## 1. 클래스 선언

```java
public class Hello{ ... }
```

- public class로 정의된 `Hello` 클래스
- public class의 클래스 이름(Hello)과 파일 이름(Hello.java)은 같아야 한다.

## 2. 메소드 선언

```java
public static void main(String[] args){ ... }
```

- 클래스는 필드(Field)와 메소드(Method)를 가질 수 있다.
- 프로그램이 실행하려면 반드시 가져야 하는 `main 메소드`
- Java로 만든 프로그램이 실행되려면 main 코드를 가지고 있어야 한다.

## 3. System.out.println(”Hello”);

```java
System.out.println(”Hello”);
```

- `System`클래스의 `out.out`의 `println()` 메소드
