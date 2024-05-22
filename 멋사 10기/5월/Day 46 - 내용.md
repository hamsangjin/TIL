# 세션

## HttpSession
- `정의`: 서버 측에서 사용자와 관련된 데이터를 저장하는 방법을 제공해주는 것으로, 세션은 서버에 생성되며, 세션 ID를 통해 각 클라이언트를 식별하면서, 이 세션 ID는 보통 쿠키를 사용하여 클라이언트에 저장되고, 각 요청마다 서버로 전송된다.
- `특징`: 객체 저장, 사용자 로그인 상태 유지 등 복잡한 데이터 관리에 적합하고, 서버 메모리에 데이터를 저장하기 때문에 쿠키보다 보안이 강화되지만, 많은 사용자가 접속하는 경우 서버 자원 사용이 증가할 수 있다.

### HttpSession 사용 시나리오
<img src="https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/e9bdfeaf-6eb5-42cd-bcf6-8a2784df0cdb/Untitled.png?id=09c5c35e-f02f-433a-ab1a-b2484916b0d0&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1716451200000&signature=yBtTnTJCa2YosqQJNECh7Pul1JFY6gG0FRlP3XUjTTQ&downloadName=Untitled.png">

- 첫 번째 요청(로그인 요청 포함)
    1. `로그인 요청`: 사용자가 로그인을 요청하고, 이 요청에는 사용자 이름과 비밀번호가 포함될 수 있다.
    2. `HTTP 요청 수신 및 처리`: 서버가 로그인 요청을 받고 사용자의 자격 증명을 검증한다.
    3. `HttpSession 생성`: 사용자의 자격 증명이 확인되면, 서버는 새로운 HttpSession을 생성하고 세션 ID를 발급한다.
    4. `세션 ID를 쿠키로 설정하고 응답`: 서버는 세션 ID를 쿠키로 설정하여 브라우저로 전송하고, 로그인 성공 페이지를 응답한다.
    5. `로그인 페이지 표시`: 브라우저는 로그인 성공 페이지를 사용자에게 표시한다.
- 두 번째 요청(보안 페이지 요청)
    1. `보안 페이지 요청`: 사용자가 보안이 필요한 페이지에 접근을 시도한다.
    2. `HTTP 요청 및 세션 ID 쿠키 전송`: 브라우저는 세션 ID를 포함한 쿠키와 함께 서버에 요청을 보낸다.
    3. `세션 검증`: 서버는 쿠키에서 받은 세션 ID를 사용하여 해당 사용자의 세션을 검색하고, 사용자의 로그인 상태 및 권한을 확인한다.
    4. `보안 컨텐츠 제공`: 세션 정보가 유효하다면, 서버는 보안 페이지를 포함하여 응답을 보낸다.
    5. `보안 페이지 표시`: 브라우저는 보안 페이지를 사용자에게 표시한다.
 
## HttpSession 예제

- `SessionController.java`
```java
import jakarta.servlet.http.HttpSession;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.bind.support.SessionStatus;

@Controller
// 2. 어노테이션을 이용(세션에 저장할거라고 선언)
@SessionAttributes("visitCount2")
public class SessionController {

    // if (visitCount == null) visitCount = 0; 코드와 같은 초기화 역할을 하는 메소드
    // 메소드 레벨에 @ModelAttribute 사용할 시 해당 컨트롤러의 다른 모든 요청 핸들러 메소드가 호출되기 전에 실행됨
    @ModelAttribute("visitCount2")
    public Integer initVisitCount2() {
        return 0;
    }

    @GetMapping("/visit2")
    // @ModelAttribute("visitCount2") Integer visitCount2의 역할: Integer visitCount = (Integer) session.getAttribute("visitCount");
    public String trackVisit(@ModelAttribute("visitCount2") Integer visitCount2, Model model) {     // 세션에서 visitCount2 찾아서 값을 저장시켜줌
        model.addAttribute("visitCount2", ++visitCount2);                               // 세션에 값을 저장시켜줌
        return "visit2";
    }

    // 1. HttpSession을 직접 이용한 예
//    @GetMapping("/visit2")
//    public String trackVisit(HttpSession session, Model model) {
//        // 세션 사용: 값이 점점 늘어남
//        Integer visitCount = (Integer) session.getAttribute("visitCount");   // visitCount 받아오기
//        if (visitCount == null) visitCount = 0;                              // null이면 초기 상태를 의미하므로 0으로 설정
//        session.setAttribute("visitCount", ++visitCount);                    // 방문 횟수 +1로 설정
//
//        // 모델 사용 -> 값이 점점 늘어나지 않고 1로 고정됨
//        // Integer visitCount = (Integer) model.getAttribute("visitCount");
//        // if (visitCount == null) visitCount = 0;
//        // model.addAttribute("visitCount", ++visitCount);
//
//        return "visit2";
//    }

    // 세션 객체 삭제 메소드
    // SessionStatus를 사용하여 세션 속성을 완료 상태로 표시할 수 있고, 이 방법을 사용하면 세션 속성이 올바르게 제거될 수 있다.
//    @GetMapping("resetVisit")
//    public String resetVisit(SessionStatus status, Model model) {
//        status.setComplete();           // 삭제
//        return "redirect:/visit2";      // visit2로 다시 요청
//    }

    // 세션 객체의 특정 속성 삭제 메소드
    // 어노테이션을 이용한 세션관리의 차이점을 잘 봐야함 -> 세션을 직접 이용한 코드랑 동작함
    @GetMapping("resetVisit")
    public String resetVisit(HttpSession session) {
        session.removeAttribute("visitCount2");         // 특정 속성만 삭제
        // session.invalidate();                        // 전체 삭제
        return "redirect:/visit2";                      // visit2로 다시 요청
    }
}
```

- `visit2.html`
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
  <head>
    <meta charset="UTF-8">
    <title>Title</title>
  </head>
  <body>
    <h1> 세션을 이용한 방문 횟수 </h1>
    <!--  1. HttpSession을 직접 이용한 예  -->
<!--    <p> 당신은 <span th:text="${session.visitCount}"></span>번째 방문자입니다.</p>-->

    <!--  2. 어노테이션을 이용한 예  -->
    <p> 당신은 <span th:text="${visitCount2}"></span>번째 방문자입니다.</p>

    <!--  방문횟수 초기화 링크 추가  -->
    <a th:href="@{/resetVisit}"> 방문횟수 초기화 </a>
  </body>
</html>
```

<br>

---

<br>

# 뷰 리졸버(View Resolver)
`뷰 리졸버`는 컨트롤러에서 반환된 뷰 이름을 기반으로 실제 뷰를 찾아내고, 렌더링을 위한 뷰 객체를 반환하는 역할을 한다.

## 사용자 정의 뷰
- `MyCustomView.java`
```java
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.web.servlet.View;

import java.io.PrintWriter;
import java.util.Map;

public class MyCustomView implements View {

    @Override
    public String getContentType() {
        return "text/html";     /// text로 된 html 파일을 보낸다
    }

    @Override
    public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response) throws Exception {
        response.setContentType(getContentType());
        PrintWriter out = response.getWriter();         // 응답을 쓸 수 있는 통로
        out.println("<html><body>");
        out.println("<h1> Custom View Content </h1>");
        out.println("<p> my custom view !! </p>");
        out.println("</body></html>");
        out.close();
    }
}
```

<br>

## 뷰 리졸버 커스터마이징
- `MyCustomViewResolver.java`
```java
import org.example.springmvc.view.MyCustomView;
import org.springframework.core.Ordered;
import org.springframework.web.servlet.*;

import java.util.Locale;

public class MyCustomViewResolver implements ViewResolver, Ordered {
    private int order;

    @Override
    public int getOrder() {
        return this.order;
    }

    public void setOrder(int order) {
        this.order = order;
    }

    @Override
    public View resolveViewName(String viewName, Locale locale) throws Exception {
        if (viewName.startsWith("my-prefix")) return new MyCustomView();        //

        return null;        // 다음 뷰 리졸버가 처리하게 함
    }
}
```

<br>

## 뷰 리졸버 설정
- `WebConfig.java`
```java
import org.springframework.context.annotation.*;
import org.springframework.web.servlet.config.annotation.EnableWebMvc;

@Configuration
@EnableWebMvc
public class WebConfig {

    @Bean
    public MyCustomViewResolver myCustomViewResolver() {
        MyCustomViewResolver resolver = new MyCustomViewResolver();
        resolver.setOrder(0);
        return resolver;
    }
}
```

<br>

## 컨트롤러 설정
- `MyViewController.java`
```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class MyViewController {
    @GetMapping("/custom")
    public String customView() {
        return "my-prefix-custom";  // my-prefix로 시작하는 뷰 이름
    }
}
```

<br>

## 실행 결과
<img width="341" alt="스크린샷 2024-05-22 13 56 04" src="https://github.com/hamsangjin/TIL/assets/103736614/6129a9ea-df3b-4b93-b4d1-a6abe571e37a">

<br>

---

<br>

# 에러 페이지 처리

## 자동 에러 페이지 설정 비활성화
- `application.yml`
```yml
server:
  ...
  error:
    whitelabel:
      enabled: false
```

<br>

## Error Page
- `error.html`
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1> 공습경보 !! 공습경보 !!</h1>
        <p th:text="${errorMsg}"> Error Page </p>
    </body>
</html>
```

<br>

## ControllerAdvice 설정
```java
import org.slf4j.*;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@ControllerAdvice
public class GlobalExceptionHandler {
    private static final Logger logger = LoggerFactory.getLogger(GlobalExceptionHandler.class);

    // 특정 예외 처리로, 지정된 예외가 발생했을 때 실행될 메서드를 정의한다. 여기서는 Exception.class를 지정하였기 때문에, 모든 종류의 예외를 처리할 수 있다.
    @ExceptionHandler(value = Exception.class)
    public String handlerException(Exception e, Model model) {
        logger.error("Server Error: ", e);
        model.addAttribute("errorMsg", e.getMessage());
        return "error";
    }
}
```
