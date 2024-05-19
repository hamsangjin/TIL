# AOP(Aspect Oriented Programming)
관점지향 프로그래밍이라고 하며, 자체적인 언어라기보다는 기존의 OOP언어를 보완하는 확장 형태로 사용되고 있다.

<img width="729" alt="스크린샷 2024-05-19 23 07 06" src="https://github.com/hamsangjin/TIL/assets/103736614/40293228-f093-466c-8dcc-bd8877a7b0d3">

<br>
<br>

## AOP 목적 및 장점
<img width="610" alt="스크린샷 2024-05-19 23 09 12" src="https://github.com/hamsangjin/TIL/assets/103736614/186a7757-f362-4de2-ba4b-7b119a091706">

- 중복을 줄여서 적은 코드 수정으로 전체 변경을 할 수 있게하자라는 목적에서 출발
- AOP의 필요성을 이해하는 가장 기초가 되는 개념은 `관심의 분리`
- `핵심관점(업무로직)` + `횡단관점(트랜잭션/로그/보안/인증 처리 등)`으로 관심의 분리를 실현
- 중복되는 코드 제거, 효율적인 유지보수, 높은 생산성, 재활용성 극대화, 변화 수용이 용이 등의 장점

<br>

## AOP 용어
- `Joinpoint`: 메소드를 호출하는 시점, 예외가 발생하는 시점과 같이 애플리케이션을 실행 할 때 특정 작업이 실행되는 시점을 의미한다.
- `Advice`: Joinpoint에서 실행되어야 하는 코드, `횡단 관점`에 해당한다.
- `Target`: 실질적인 비지니스 로직을 구현하고 있는 코드 `핵심 관점`에 해당한다.
- `Pointcut`: `Target` 클래스와 `Advice`가 결합될 때 둘 사이의 결합규칙을 정의하는 것이며, 예를 들면 `Advice`가 실행된 `Target`의 특정 메소드등을 지정하는 것을 말한다.
- `Aspect`: `Advice`와 `Pointcut`을 합쳐서 하나의 `Aspect`라고 한다. 즉, 일정한 패턴을 가지는 클래스에 `Advice`를 적용하도록 지원할 수 있는 것을 `Aspect`라고 한다.
- `Weaving`: AOP에서 `Joinpoint`들을 `Advice`로 감싸는 과정을 `Weaving`이라고 하고, `Weaving`하는 작업을 도와주는 것이 AOP 툴이 하는 역할이다.

<br>

## Spring AOP
스프링은 `Aspect`의 적용 대상이 되는 객체에 대한 `Proxy`를 만들어 제공하며, 대상객체를 사용하는 코드는 대상객체를 `Proxy`를 통해서 간접적으로 접근한다.
- `Proxy`는 공통기능을 실행한 뒤 대상객체의 실제 메서드를 호출하거나 또는 대상객체의 실제 메소드가 호출된 뒤 공통기능을 실행한다.

<br>

## 스프링 프레임워크에서 지원하는 어드바이스 유형
- `Before`: 조인 포인트 전에 실행되며, 예외가 발생하는 경우만 제외하고 항상 실행된다.
- `After Returning`: 조인 포인트가 정상적으로 종료한 후에 실행되며, 예외가 발생하면 실행되지 않는다.
- `After Throwing`: 조인 포인트에서 예외가 발생했을 때 실행되며, 예외가 발생하지 않고 정상적으로 종료하면 실행되지 않는다.
- `After`: 조인 포인트에서 처리가 완료된 후 실행되며, 예외 발생이나 정상 종료 여부와 상관없이 항상 실행된다.
- `Around`: 조인 포인트 전후에 실행된다.

<br>

## 포인트컷 표현식의 주요 구성 요소
1. `execution`: 가장 많이 사용되는 포인트컷 지시자로, 메서드 실행을 기준으로 매칭하고, 메서드 실행 시 조인 포인트를 찾는 데 사용된다.
2. `within`: 특정 타입에 속한 조인 포인트를 매칭하는 데 사용되며, 이 지시자는 주어진 타입 내의 모든 조인 포인트를 매칭한다.
3. `this`: 빈 참조가 주어진 타입의 인스턴스인 조인 포인트를 매칭한다.
4. `target`: 대상 객체가 주어진 타입의 인스턴스인 조인 포인트를 매칭한다.
5. `args`: 메서드가 주어진 타입의 인수를 갖는 조인 포인트를 매칭한다.
6. `@annotation`: 특정 어노테이션이 적용된 메서드를 조인 포인트로 매칭한다.
7. `@within`: 지정된 어노테이션이 타입 수준에서 선언된 객체의 모든 조인 포인트를 매칭한다.
8. `@target`: 지정된 어노테이션이 대상 객체에 적용된 조인 포인트를 매칭한다.
9. `@args`: 전달된 인수에 어노테이션이 있는 메서드를 조인 포인트로 매칭한다.

<br>

## 포인트컷 표현식의 구문
- `execution(접근제어자 반환타입 클래스이름패턴.메소드이름패턴(메소드파라미터패턴) 예외패턴)`
  - `execution(* set*(..))`: set으로 시작하는 모든 메서드
  - `execution(* com.example.demo.service.*.*(..))`: com.example.demo.service 패키지 안의 모든 클래스의 모든 메서드
  - `execution(public * *(..))`: 모든 public 메서드
  - `execution(* *.*(String, ..))`: 첫 번째 파라미터가 String인 모든 메서드
 
<br>

## 자바 기반 설정 방식에서의 어드바이스 정의
- `@Aspect`
  - 스프링 빈에 `@Aspect`를 명시하면 해당 빈이 Aspect로 작동한다.
  - 클래스 레벨에 `@Order`를 명시하여 `@Aspect` Bean 간의 작동 순서를 정할 수 있고, int 타입의 정수로 순서를 정할 수 있는데 값이 낮을수록 우선순위가 높으며, 기본값은 가장 낮은 우선순위를 가지는 `Ordered.LOWEST_PRECEDENCE`이다.
  - `@Aspect`가 명시된 빈에는 어드바이스라 불리는 메소드를 작성할 수 있다.
  - 대상 스프링 빈의 메소드의 호출에 끼어드는 시점과 방법에 따라 `@Before`, `@After`, `@AfterReturning`, `@AfterThrowing`, `@Around` 등을 명시할 수 있다.
- `@Before`: 대상 객체의 메서드 호출 전에 공통 기능을 실행한다.
- `@After`: 어드바이스를 명시하면 대상 메소드의 실행 후에 끼어 들어 원하는 작업을 할 수 있지만, 대상 메소드의 제어나 가공은 불가능하다.
- `@AfterReturning`: `@Aspect` 클래스의 메소드 레벨에 `@AfterReturning`을 명시하면 해당 메소드의 실행이 종료되어 값을 리턴할 때 끼어 들 수 있지만, 리턴 값을 확인할 수 있을 뿐 대상 메소드의 제어나 가공은 불가능하다.
- `@Around`: `@Around` 어드바이스는 앞서 설명한 어드바이스의 기능을 모두 포괄하는 개념이며, 대상 메소드를 감싸는 느낌으로 실행 전후 시점에 원하는 작업을 할 수 있고, 대상 메소드의 실행 제어 및 리턴 값 가공도 가능하다.
- `JoinPoint`: 대상 객체 및 호출되는 메서드에 대한 정보나 전달되는 파라미터에 대한 정보가 필요한 경우 `org.aspectj.lang.JoinPoint`를 파라미터로 추가하면 된다.

<br>

## 자바 기반 설정 방식에서의 어드바이스 정의 예제

- `ServiceAspect.java`
```java
import org.aspectj.lang.JoinPoint;
import org.aspectj.lang.annotation.*;
import org.springframework.stereotype.Component;

// Aspect: PointCut과 Advice가 합쳐진 것
@Aspect
@Component
public class ServiceAspect {

    @Pointcut("execution(* org.example.aopexam.*.*(..))")
    public void pc1() {}

    @Pointcut("execution(* hello())")
    public void pc2() {}

    @Pointcut("execution(* org.example.aopexam.SimpleService.*(..))")
    public void pc3() {}

    // pointcut 표현식: 해당 경로의 모든 클래스들의 모든 메소드의 모든 파라미터를 의미
    @Before("pc1()")
    public void before(JoinPoint joinPoint) {
        System.out.println("Before : " + joinPoint.getSignature().getName());
    }

    // 해당 경로의 SimpleService 클래스들의 모든 메소드의 모든 파라미터를 의미
    @After("pc3()")
    public void after() {
        System.out.println("After!!!!");
    }

    @AfterReturning(pointcut = "execution(* org.example.aopexam.*.*(..))", returning = "result")
    public void afterReturningServiceMethod(JoinPoint joinPoint, Object result){
        System.out.println("After method " + joinPoint.getSignature().getName() + ", return value: " + result);
    }

    @Before("execution(* setName(..))")
    public void beforeName(JoinPoint joinPoint){}
}
```

- `SimpleService.java`
```java
import org.springframework.stereotype.Service;

@Service
public class SimpleService {
    public String doSomething() {
        // 핵심 관점(target)
        System.out.println("SimpleService do something");

        return "do something";
    }

    public void hello() {
        System.out.println("Hello !!");
    }
}
```

- `AopExamApplication01.java`
```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class AopExamApplication01 {
    public static void main(String[] args) {
        SpringApplication.run(AopExamApplication01.class, args);
    }

    @Bean
    public CommandLineRunner run(SimpleService simpleService) {
        return args -> {
            System.out.println(simpleService.doSomething());
            simpleService.hello();
        };
    }
}
```

<br>

---

<br>

# Spring MVC(Model-View-Controller)
`Spring MVC`는 `모델-뷰-컨트롤러` 패턴을 기반으로 하는 웹 애플리케이션 프레임워크이며, Spring 프레임워크의 일부로, 개발자가 더 쉽고, 유연하며, 효과적으로 웹 애플리케이션을 개발할 수 있게 지원한다.

## 주요 특징
1. `분리된 구성 요소`: MVC 패턴의 적용을 통해, 애플리케이션의 각 부분을 독립적으로 개발하고 관리할 수 있습니다.
    - `모델(Model)`: 데이터와 비즈니스 로직 처리
    - `뷰(View)`: 사용자 인터페이스 담당
    - `컨트롤러(Controller)`: 사용자 입력과 모델의 상호 작용 관리
2. `유연성`: `다양한 뷰 기술(JSP, Thymeleaf, FreeMarker 등)`을 지원하며, 개발자는 자신의 요구 사항에 맞는 뷰를 선택할 수 있고 또한 JSON, XML 등 다양한 형식으로 데이터를 반환할 수 있어 `RESTful API` 구현에 적합하다.
3. `강력한 구성 기능`: Spring의 의존성 주입과 AOP를 통해 애플리케이션 구성 요소를 모듈화하고 관리할 수 있고, 이를 통해 애플리 케이션의 유지 보수성과 확장성이 향상된다.
4. `양방향 데이터 바인딩`: Spring MVC는 form 태그 라이브러리를 사용하여 모델 객체와 HTML 폼 요소 간의 데이터를 자동으로 바인딩할 수 있으며, 유효성 검사를 손쉽게 적용할 수 있다.
5. `종합적인 에러 처리`: 컨트롤러나 서비스 계층에서 발생하는 예외를 효과적으로 관리할 수 있는 메커니즘을 제공하고, 이를 통해 개발자는 에러 페이지를 사용자 친화적으로 만들 수 있으며, 애플리케이션의 견고성을 높일 수 있다.

<br>

## MVC 디자인 패턴의 실행 흐름
<img width="438" alt="스크린샷 2024-05-19 23 36 27" src="https://github.com/hamsangjin/TIL/assets/103736614/b0f4a4a5-2a0f-40c6-a087-943481e66d28">

1. `Incoming request`: 모든 웹 요청은 `Front Controller`로 들어오고, 이 예시에서는 `Servlet 엔진(ex: Tomcat)`이 이를 처리한다.
2. `Front Controller`: 이는 일반적으로 `DispatcherServlet`이며, 모든 요청을 적절한 핸들러나 컨트롤러로 라우팅하는 중앙 집중식 컨트롤러이다.
3. `Controller`: 요청을 받은 컨트롤러는 필요한 데이터를 처리하고, 비즈니스 로직을 실행한 다음, 결과 데이터를 `Model` 객체에 저장하고, 이 처리 과정은 `Handle request`와 `Create model`로 나타난다.
4. `Model`: `Controller`가 생성한 `Model`은 데이터를 포함하고 있으며, 이 데이터는 뷰에서 사용자에게 표시된다.
5. `Delegate request of response`: `Front Controller`는 `Controller`가 준비한 `Model` 데이터를 바탕으로, 응답을 렌더링할 적절한 뷰를 찾기 위해 `View template`으로 요청을 위임한다.
6. `View template`: `뷰 템플릿(예: JSP, Thymeleaf)`은 Model 데이터를 사용하여 최종 사용자에게 보여줄 HTML 페이지 등의 응답을 생성합니다.
7. `Render response`: 데이터가 통합된 뷰는 사용자에게 보여지기 위해 렌더링된다.
8. `Return control and Return response`: 최종적으로 렌더링된 페이지는 사용자에게 반환되고, 요청 처리가 완료된다.

<br>

## Spring MVC 프레임워크에서 요청처리

<img width="470" alt="스크린샷 2024-05-19 23 51 49" src="https://github.com/hamsangjin/TIL/assets/103736614/301a2f2e-5230-4b95-9e24-b7c103cde408">

1. `Request`: 모든 클라이언트 요청은 `Dispatcher Servlet`으로 들어온다.
2. `Handler Mapping`: `Dispatcher Servlet`은 `Handler Mapping`을 사용하여 해당 요청을 처리할 적합한 컨트롤러를 찾는다.
3. `Handler Adapter`: 선택된 컨트롤러를 실행할 수 있게 하는 `Adapter`이며, 실제 컨트롤러가 실행되도록 중간에서 조정하는 역할을 한다.
4. `Controller`: 실제 비즈니스 로직을 처리하는 컨트롤러가 요청을 받아 처리하며, 이 과정에서 데이터베이스 접근이 필요한 작업은 `Repository`를 통해 수행되며, 이러한 데이터 접근은 `Service 계층`을 통해서도 이루어질 수 있다.
5. `Model`: 컨트롤러는 처리 결과를 `Model 객체`에 설정하며, 데이터와 함께 뷰에 전달되어 사용자에게 표시될 데이터를 포함한다.
6. `View Resolver`: 컨트롤러에서 반환된 뷰 이름을 바탕으로 실제 뷰 객체를 찾아내는 역할을 한다. 즉, 뷰 이름을 통해 실제 렌더링될 뷰 객체를 결정한다.
7. `View`: `View` 객체는 `Model` 데이터를 사용하여 사용자에게 보여질 최종적인 응답을 생성하며, 이 과정에서 `HTML`, `JSON` 등의 형태로 변환될 수 있다.
8. `Response`: 최종적으로 생성된 뷰가 클라이언트에게 응답으로 반환된다.

위 그림으로 이해가 안 가면 아래 그림으로 이해하면 편하다 !
![image](https://github.com/hamsangjin/TIL/assets/103736614/42b0893a-d2a0-40a4-90d3-d708e12ca343)

## Spring MVC 간단한 예시 코드

- `application.yml`
```yml
spring:
  application:
    name: springmvc

server:
  port: 80
```

- `springmvc/controller/MyController.java`
```java
package org.example.springmvc.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@Controller              // 리턴값 : view
//@RestController        // 리턴값: 데이터라 리턴한 "home"을 띄워줌
public class MyController {
    @GetMapping("/home")
    public String home() {
        return "home";
    }
}
```

- `resources/templates/home.html`
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Home</title>
</head>
<body>
<h1>spring mvc home !!</h1>

</body>
</html>
```

- `springmvc/SpringmvcApplication.java`
```java
package org.example.springmvc;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class SpringmvcApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringmvcApplication.class, args);
    }
}
```
