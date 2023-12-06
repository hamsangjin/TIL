# Spring 입문 - 회원 관리 예제(웹 MVC 개발)

> 인프런의 "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술" 강의를 듣고 정리한 내용입니다.

## 목차
- [회원 웹 기능 - 홈 화면 추가](#회원-웹-기능---홈-화면-추가)
<!--
- [회원 웹 기능 - 등록](#회원-웹-기능---등록)
- [회원 웹 기능 - 조회](#회원-웹-기능---조회)
-->

</br>

## 회원 웹 기능 - 홈 화면 추가

먼저 home.html 파일을 불러와줄 홈 컨트롤러를 추가해준다.

- main/java/hello/hellospring/controller/HomeController.java
```java
package hello.hellospring.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class HomeController {

    @GetMapping("/")
    public String home() {
        return "home";
    }

}

```

</br>

이제 `@GetMapping`으로 매핑되어 있는 `"/"`에 화면을 구성할 home.html을 만들어보자

- main/resources/templates/home.html
```html
<!DOCTYPE HTML>

<html xmlns:th="http://www.thymeleaf.org">
<body>
<div class="container">
    <div>
        <h1>Hello Spring</h1> <p>회원 기능</p>
        <p>
            <a href="/members/new">회원 가입</a>
            <a href="/members">회원 목록</a>
        </p>
    </div>
</div> <!-- /container -->

</body>
</html>

```

</br>

> - 페이지가 존재하지 않아도 index.html로 이동하지 않는 이유
>   - 스프링 컨테이너에서 관련 컨트롤러가 있는 찾고, 없으면 static파일을 찾는 것이므로,
>   - 지금은 HomeController가 있기 때문에 정적 리소스는 무시된다.
>   - 따라서 컨트롤러가 정적파일보다 우선순위가 높다고 생각하면 된다.

</br>

<!--
## 회원 웹 기능 - 등록

## 회원 웹 기능 - 조회
-->
