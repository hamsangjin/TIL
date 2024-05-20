# Spring MVC

## Spring MVC와 Spring Boot의 통합

### 자동 구성
- `자동 설정`: Spring Boot는 클래스패스 상의 라이브러리를 기반으로 실행 가능한 Spring 애플리케이션을 자동으로 설정한다. 예를 들어, Spring Web MVC가 클래스패스에 있다면, Spring Boot는 자동으로 DispatcherServlet, Web MVC 설정 등을 구성한다.
- `자동 의존성 관리`: Spring Boot의 스타터 패키지를 사용하면 필요한 의존성을 쉽게 관리할 수 있습니다. spring-boot-starter-web은 Spring MVC, Tomcat 서버, Jackson 라이브러리 등 웹 애플리케이션 개발에 필요한 의존성을 모두 포함하고 있다.

### 간소화된 설정
- `application.properties/yml 파일`: Spring Boot에서는 application.properties 또는 application.yml 파일을 통해 애플리케이션의 다양한 설정을 쉽게 변경할 수 있다. 예를 들어, 서버 포트, 컨텍스트 패스, 로깅 레벨 등을 간단히 설정할 수 있다.
- `내장 서버`: Spring Boot는 내장 Tomcat, Jetty 또는 Undertow 서버를 사용하여 별도의 서버 설치 없이 애플리케이션을 실행할 수 있게 하며, 이는 개발 및 테스트 과정을 간편하게 만들어 준다.

### 개발 및 배포 용이성
- `개발자 도구`: Spring Boot는 개발자 생산성을 향상시키기 위한 도구들을 제공한다. 예를 들어, Spring Boot DevTools는 코드가 변경될 때마다 자동으로 애플리케이션을 재시작하고, 브라우저를 리프레시하는 기능을 지원한다.
- `운영 준비성`: 애플리케이션의 건강 상태, 메트릭스 등을 모니터링할 수 있는 액추에이터 엔드포인트를 제공하며, 이는 배포 후 애플리케이션의 상태를 실시간으로 모니터링하고 관리하는 데 도움을 준다.

### 통합의 장점
`Spring MVC`와 `Spring Boot`를 통합 사용함으로써, 개발자는 복잡한 XML 구성 없이 애노테이션과 간단한 설정만으로도 강력하고 유연한 웹 애플리케이션을 빠르게 개발할 수 있고 또한, Spring Boot의 자동 구성 기능은 일반적인 개발 패턴과 모범 사례를 기반으로 하므로, 개발자가 보다 집중하여 품질 높은 코드를 작성할 수 있는 환경을 제공한다.

<br>

---

<br>

# HTTP 프로토콜
HTTP(HyperText Transfer Protocol)는 웹에서 데이터를 교환하는 기본 프로토콜이며, 클라이언트와 서버 간의 통신을 가능하게 하며, 요청과 응답의 형태로 정보가 교환된다. 

또한, HTTP는 상태를 유지하지 않는 프로토콜로서 이전 통신 내역을 기억하지 않으며, 각 요청은 독립적이며 이는 HTTP의 요청이나 응답이 이전 또는 미래의 요청과 응답과 연결되지 않음을 의미한다.

## GET과 POST
- `HTTP Request Message`
<img width="485" alt="스크린샷 2024-05-20 10 22 01" src="https://github.com/hamsangjin/TIL/assets/103736614/ffe62854-162f-4395-8dbe-5690b5ed4705">

GET방식은 body부분을 전송하지 않는다.

- `HTTP Response Message`
<img width="487" alt="스크린샷 2024-05-20 10 22 45" src="https://github.com/hamsangjin/TIL/assets/103736614/326dd134-beae-40e9-bef0-2176bb9704f0">

- `curl -v http://www.naver.com`
<img width="438" alt="스크린샷 2024-05-20 10 24 26" src="https://github.com/hamsangjin/TIL/assets/103736614/ffe8ee20-7387-49bf-96e9-d5bff423256a">

<br>
<br>

---

<br>

# 컨트롤러와 라우팅
`컨트롤러`는 Spring MVC 아키텍처에서 중심적인 역할을 하는 컴포넌트로, 웹 애플리케이션에서 사용자의 요청을 받아 처리하고 그 결과를 뷰에 전달하는 역할을 한다.

## 기능과 책임
- `요청 매핑`: 컨트롤러는 클라이언트로부터 들어오는 요청(URL, HTTP 메소드)을 적절한 처리 로직과 매핑하기 위해 `@RequestMapping` 과 같은 애노테이션을 사용한다.
- `데이터 처리`: 요청에서 데이터를 추출하고, 필요한 처리를 수행한 후, 결과 데이터를 모델 객체에 담아 뷰로 전달하며, 이 과정에서 `@RequestParam`, `@PathVariable`, `@ModelAttribute` 등의 애노테이션을 활용할 수 있다.
- `뷰 선택`: 처리 결과에 따라 사용자에게 보여줄 뷰를 선택하고, 이를 위해 `ModelAndView` 객체를 반환하거나 뷰 이름을 문자열로 지정한다.

<br>

## 컨트롤러의 종류
Spring MVC에서는 주로 두 가지 유형의 컨트롤러를 사용한다. 
1. `@Controller`
    - 전통적인 웹 페이지 기반의 컨트롤러로서, 주로 JSP나 Thymeleaf 같은 뷰 템플릿을 사용하여 HTML을 생성한다.
    - 주로 웹 어플리케이션에서 데이터를 뷰에 전달하여 사용자가 볼 수 있는 페이지를 동적으로 생성하는 데 사용된다.
2. `@RestController`
    - RESTful 웹 서비스를 개발할 때 사용되는 컨트롤러로, `@Controller`와 `@ResponseBody`를 합친 것과 같다.
    - 이 컨트롤러는 데이터를 JSON이나 XML 형태로 클라이언트에게 바로 반환하며, 주로 데이터 API 서버를 구축하는 데 사용된다.

<br>

## 예제

- `MyController.java`
```java
import jakarta.servlet.http.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.ModelAndView;

@Controller
public class MyController {

//    @GetMapping("greeting")
//    public String greeting(String name, int age, HttpServletRequest req){
//        // http://localhost/greeting?name=sangjin
//        // ?name=sangjin&age=20: 쿼리 문자열
//        System.out.println(name);
//        System.out.println(age);
//
//        System.out.println("req: " + req.getParameter("name"));
//        System.out.println("req: " + req.getParameter("age"));
//        return "greeting";
//    }

//    @GetMapping("greeting")
//    public String greeting(
//            @RequestParam(name="name", required = false, defaultValue = "Ham") String name,
//            @RequestParam(name="age", required = false, defaultValue = "0") int age,
//            Model model){
//        System.out.println(name);
//        System.out.println(age);
//
//        model.addAttribute("name", name);
//        model.addAttribute("age", age);
//        return "greeting";
//    }

    @GetMapping("greeting")
    public ModelAndView greeting(@RequestParam String name, ModelAndView modelAndView) {
        System.out.println(name);
        modelAndView.addObject("name", name);
        modelAndView.setViewName("greeting");

        return modelAndView;
    }
}
```

- `greeting.html`
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org/">    // 타임리프 사용 선언
<head>
    <meta charset="UTF-8">
    <title>Greeting</title>
</head>
<body>
<h1 th:text="'Hello, ' + ${name} + '!!'"> Hello  </h1>    // 타임리프 text 출력
</body>
</html>
```

<br>

---

<br>

# 뷰와 뷰 리졸버
`뷰 템플릿(View Template)`은 웹 애플리케이션에서 사용자 인터페이스를 구성하는 도구를 말하며, 뷰 템플릿을 사용하면 서버에서 데이터를 동적으로 HTML 페이지에 통합하여 웹 브라우저에서 렌더링할 수 있고, 이를 통해 동적인 웹 콘텐츠를 생성하고, 반복되는 HTML 구조를 효율적으로 관리할 수 있다.


## 뷰 템플릿의 역할
1. `데이터 표시`: 뷰 템플릿은 컨트롤러로부터 전달받은 모델 데이터를 사용하여 사용자에게 정보를 제공하는 HTML을 생성한다.
2. `코드 재사용`: 공통 레이아웃, 헤더, 푸터, 네비게이션 바 등을 템플릿 파일에 정의하여 여러 페이지에 걸쳐 재사용할 수 있다.
3. `유지 관리`: 디자인 변경이 필요할 때 HTML 코드를 한 곳에서 수정함으로써 전체 사이트의 레이아웃을 쉽게 업데이트할 수 있다.

<br>

## 뷰 템플릿 엔진
- `Thymeleaf`: Spring MVC와 자연스럽게 통합되어 자바 개발자에게 친숙하며, HTML을 서버와 클라이언트 양쪽에서 동일하게 사용할 수 있도록 설계되었다.
- `JSP (JavaServer Pages)`: 오랜 기간 동안 사용되어온 Java 기반의 뷰 기술로, 동적 웹 페이지 생성에 사용되지만, 최신 개발 환경에서는 `JSP`보다 더 현대적이고 유연한 `Thymeleaf`나 `FreeMarker` 같은 다른 템플릿 엔진을 선호한다.
- `FreeMarker`: 유연성이 높고 강력한 템플릿 언어로, 주로 웹 애플리케이션 개발에 사용된다.

<br>

## Thymeleaf
`Thymeleaf`는 현대적인 서버 사이드 자바 템플릿 엔진으로, HTML, XML, JavaScript, CSS 등을 생성하는 데 사용된다. 

주로 웹 및 독립형 환경에서 사용되며, `Spring Framework`와의 통합을 위해 특별히 설계된 `Spring Edition`을 제공한다.

### 기본 문법 및 사용법
1. 표현식
    - `변수 표현식( ${...} )`: 모델 속성을 HTML에 삽입할 때 사용된다.
    - `선택 변수 표현식( *{...} )`: 선택된 객체에 대해 평가되며, `th:object`와 함께 사용된다.
    - `메시지 표현식( #{...} )`: 국제화(i18n) 메시지를 템플릿에 삽입할 때 사용된다.
    - `링크 URL 표현식( @{...} )`: URL을 동적으로 생성할 때 사용된다.
    - `리터럴 값( 'one text', +, * 등)`: 문자열, 숫자, 불린 값을 표현하고 연산할 때 사용된다.
2. 표준 속성 설정자
    - `th:text:` 태그의 텍스트 콘텐츠를 설정하며, `<span th:text="'Hello, ' + ${user.name} + '!'"></span>`와 같이 사용할 수 있다.
    - `th:each`: 반복자 역할을 하며, 리스트나 배열 등을 순회할 때 사용되며, `<tr th:each="user : ${users}">`와 같이 사용할 수 있다.
    - `th:if, th:unless`: 조건부 표시를 위해 사용되며, `<div th:if="${user.isAdmin()}">Admin Features</div>`와 같이 사용할 수 있다.
    - `th:include, th:replace`: 한 템플릿의 특정 부분을 다른 템플릿 조각으로 포함하거나 대체할 때 사용된다.
3. URL 처리
    - `th:href`: 링크 생성에 사용되며, `<a th:href="@{/login}">Login</a>`와 같이 사용할 수 있다.

<br>

## Thymeleaf 템플릿과 표현식 예제
- `ExamController.java`
```java
import org.example.springmvc.domain.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.*;

@Controller
public class ExamController {
    @GetMapping("/exam")
    public String showExample(Model model) {
        model.addAttribute("username", "JohnDoe");
        model.addAttribute("isAdmin", true);
        model.addAttribute("languages", new String[]{"English", "Spanish", "German"});
        return "exam";
    }
}
```

- `exam.html`
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>Thymeleaf Example</title>
    </head>
    <body>
        <h1>Thymeleaf Example</h1>
    
        <!-- 변수 표현식 -->
        <p>Username: <span th:text="${username}"></span>
        </p>
    
        <!-- 선택 변수 표현식 -->
        <div th:object="${languages}">
            Languages:
            <span th:text="*{[0]}"></span>,
            <span th:text="*{[1]}"></span>,
            <span th:text="*{[2]}"></span>
        </div>
    
        <!-- 조건에 따라 다르게 표시 -->
        <div th:if="${isAdmin}">
            <p th:text="${username} + '는 관리자입니다 !'"></p>
        </div>
        <div th:unless="${isAdmin}">
            <p th:text="${username} + '는 관리자가 아닙니다 !'"></p>
        </div>
    
        <!-- 링크 URL 표현식 -->
        <a th:href="@{/home}">home</a>
    
        <!-- 리터럴 값 -->
        <p th:text="'Welcome, ' + ${username} + '!'">Welcome message</p>
    
        <!-- 메시지 표현식 -->
        <!-- messages(application.yml에서 정의된 messages의 basename).properties에 정의된 내용이 출력됨 -->
        <!-- messages.properties 파일을 다양한 언어로 정의해서 국제화처리를 할 수 있음 -->
        <p th:text="#{welcome.message}">Welcome message</p>
    </body>
</html>
```

- `application.yml`
```yml
spring:
  application:
    name: springmvc
  messages:
    basename: messages
    encoding: utf-8
    cache-duration: 0

server:
  port: 80
```

- `messages.properties`
```properties
welcome.message = welcome
product.name = pen
```

- `messages_en.properties`
```properties
welcome.message = welcome
product.name = pen
```

- `messages_ko.properties`
```properties
welcome.message = 환영합니다.
product.name = 펜
```

이때, 인텔리제이에서 이대로 실행하면 깨져서 출력이 되니 아래와 같이 설정을 해준다.
- `설정 - 에디터 - 파일인코딩`
<img width="970" alt="스크린샷 2024-05-20 15 31 35" src="https://github.com/hamsangjin/TIL/assets/103736614/f9bfdd70-2a77-497f-93b6-a15b42bd6254">

<br>

## Thymeleaf 템플릿과 표준 속성 설정자 예제
- `User.java`
```java
import lombok.*;

@Getter
@AllArgsConstructor
public class User {
    private String name;
    private boolean admin;
}
```

- `ExamController.java`
```java
import org.example.springmvc.domain.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.*;

@Controller
public class ExamController {
    @GetMapping("/users")
    public String users(Model model) {
        model.addAttribute("msg", "User MSG !!");

        List<User> users = new ArrayList<>(Arrays.asList(
                new User("홍길동", true),
                new User("아무개", false),
                new User("삽살개", false)
        ));
        model.addAttribute("users", users);

        return "users";
    }
}
```

- `users.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>Users</title>
  </head>
  <body>
    <h1>User List</h1>
    <table>
      <tr th:each="user : ${users}">
        <td th:text="${user.name}">Username</td>
        <td th:if="${user.admin}" th:text="'Admin'">Admin</td>
        <td th:unless="${user.admin}" th:text="'User'">User</td>
      </tr>
    </table>
    <div th:replace="fragments/footer :: footer"></div>
  </body>
</html>
```

- `footer.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>Fragment Example</title>
  </head>
  <body>
    <div th:fragment="footer">
      <footer>
        <p>&copy; 2024 Company, Inc.</p>
      </footer>
    </div>
  </body>
</html>
```

<br>

## 뷰 템플릿과 데이터 모델의 통합
뷰 템플릿과 데이터 모델의 통합을 통해 컨트롤러가 처리한 데이터를 사용자에게 표시할 수 있다.

### 데이터 모델
데이터 모델은 컨트롤러와 뷰 사이에서 데이터를 전달하는 컨테이너 역할을 하며, Spring MVC에서는 주로 `Model`, `ModelMap`, 또는 `ModelAndView` 객체를 사용하여 뷰에 데이터를 전달한다.
- `Model`: 간단한 인터페이스로, 뷰에 전달될 데이터를 추가하는 메소드를 제공한다.
- `ModelMap`: Map 에 기반한 구체적인 구현으로, 속성을 이름-값 쌍으로 저장한다.
- `ModelAndView`: 모델 데이터와 뷰의 이름을 동시에 포함하는 객체로, 컨트롤러가 뷰와 모델을 함께 반환할 때 사용된다.

### 컨트롤러와 Thymeleaf 템플릿의 통합 예제
- `Item.java`
```java
import lombok.*;

@Getter
@AllArgsConstructor
public class Item {
    private String name;
    private int price;
}
```

- `Product.java`
```java
import lombok.*;

@Getter
@AllArgsConstructor
public class Product {
    private int id;
    private String name;
    private double price;
}
```

- `ExamController.java`
```java
import org.example.springmvc.domain.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.*;

@Controller
public class ExamController {
    @GetMapping("/welcome")
    public String welcome(Model model) {
        model.addAttribute("msg", "Welcome to Spring MVC!");

        List<Item> items = Arrays.asList(
                new Item("pen", 3000),
                new Item("notebook", 5000),
                new Item("eraser", 1500)
        );

        model.addAttribute("items", items);

        return "welcome";
    }

    @GetMapping("/products")
    public String products(Model model) {
        model.addAttribute("msg", "Product MSG !!");

        List<Product> products = new ArrayList<>(Arrays.asList(
                new Product(1, "Apple", 1.20),
                new Product(2, "Banan", 0.75),
                new Product(3, "Cherry", 2.05)
        ));

        model.addAttribute("products", products);

        return "products";
    }
}
```

- `welcome.html`
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org/">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}"></h1>

<ul>
    <li th:each=" item : ${items}" th:text="${item.name}"></li>
</ul>

<div th:include="common/footer :: footer"></div>

</body>
</html>
```

- `products.html`
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org/">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1 th:text="${msg}"></h1>

<table>
  <tr>
    <th>상품 아이디</th>
    <th>상품명</th>
    <th>상품 가격</th>
  </tr>

  <tr th:each="product: ${products}">
    <td th:text="${product.id}"></td>
    <td th:text="${product.name}"></td>
    <td th:text="${product.price}"></td>
  </tr>
</table>

<hr>

<ul>
<!--  여기서 필드들은 클래스의 Getter 메소드를 통해 호출한다. -->
  <li th:each="product : ${products}" th:text="'제품 번호 ' + ${product.id} + ' - ' + ${product.name} + ' 가격: ' + ${product.price} + '$'"></li>
</ul>

<hr>

<div th:include="common/footer :: footer"></div>

</body>
</html>
```

- `footer.html`
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org/">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<!-- Fragment 정의 -->
<div th:fragment="footer">
    <footer>
        <p>&copy; 2024 My Website.</p>
    </footer>
</div>

</body>
</html>
```
