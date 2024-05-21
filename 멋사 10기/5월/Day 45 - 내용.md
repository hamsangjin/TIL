# 뷰(View)와 뷰 리졸버(ViewResolver)

## Thymeleaf 템플릿에서 특정 요소들만 출력하기
- `ExamController.java`
```java
import org.example.springmvc.domain.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.util.*;

@Controller
public class ExamController {
    @GetMapping("/list")
    public String showList(Model model) {
        List<String> items = Arrays.asList("Item 1", "Item 2", "Item 3", "Item 4", "Item 5",
                "Item 6", "Item 7", "Item 8", "Item 9", "Item 10");
        model.addAttribute("items", items);
        return "list";
    }
}
```

- `list.html`
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org">
    <head>
        <title>List Example</title>
    </head>
    <body>
        <h1>List Example</h1>
        <ul>
            <!-- th:each의 status 변수 사용, index 범위 조정 -->
            <li
                th:each="item, stat : ${items}"
                th:if="${stat.index >= 2 and stat.index < 7}"
                th:text="${item}"
            >
            </li>
        </ul>
    </body>
</html>
```

- `th:each`: 리스트 `items`를 반복 처리하며, `item`은 현재 항목을, `stat`은 상태 변수를 나타낸다.
- `th:if`: 이 조건은 3번째 아이템(index=2)부터 시작하여 7번째 아이템(index=6)까지만 출력하도록 제한하며, Thymeleaf에서의 index도 0부터 시작한다.
- `th:text`: 각 `li` 태그의 텍스트 컨텐츠로 `item` 값을 설정한다.

<br>

## Thymeleaf 템플릿에서 데이터 변환하기
- `ExamController.java`
```java
import org.example.springmvc.domain.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

import java.time.*;
import java.util.*;

@Controller
public class ExamController {
    @GetMapping("/datetime")
    public String showDateTime(Model model) {
        // 날짜 및 시간 데이터
        LocalDate date = LocalDate.now();
        LocalDateTime dateTime = LocalDateTime.now();
        LocalTime time = LocalTime.now();
        ZonedDateTime zonedDateTime = ZonedDateTime.now(ZoneId.of("Asia/Seoul"));
        
        // 모델에 데이터 추가
        model.addAttribute("currentDate", date); 
        model.addAttribute("currentDateTime", dateTime); 
        model.addAttribute("currentTime", time); 
        model.addAttribute("currentZonedDateTime", zonedDateTime);
        
        return "datetime"; // Thymeleaf 템플릿 이름 
    }
}
```

- `datetime.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org">
  <head>
    <title>Date and Time Demo</title>
  </head>
  <body>
    <h1>Date and Time Examples</h1>
    <p>
      Current Date:
      <span th:text="${#temporals.format(currentDate, 'yyyy-MM-dd')}"></span>
    </p>

    <p>
      Current DateTime:
      <span
              th:text="${#temporals.format(currentDateTime, 'yyyy-MM-dd HH:mm:ss')}"
      ></span>
    </p>

    <p>
      Current Time:
      <span th:text="${#temporals.format(currentTime, 'HH:mm:ss')}"></span>
    </p>

    <p>
      Current Zoned DateTime:
      <span
              th:text="${#temporals.format(currentZonedDateTime, 'yyyy-MM-dd HH:mm:ss zzzz')}"
      ></span>
    </p>
  
  </body>
</html>
```
- `#temporals.format(currentDate, 'yyyy-MM-dd')`: LocalDate 객체를 'yyyy-MM-dd' 형식으로 변환한다.
- `#temporals.format(currentDateTime, 'yyyy-MM-dd HH:mm:ss')`: LocalDateTime 객체를 'yyyy-MM-dd HH:mm:ss' 형식으로 변환한다.
- `#temporals.format(currentTime, 'HH:mm:ss')`: LocalTime 객체를 'HH:mm:ss' 형식으로 변환한다.
- `#temporals.format(currentZonedDateTime, 'yyyy-MM-dd HH:mm:ss zzzz')`: ZonedDateTime 객체를 'yyyy-MM-dd HH:mm:ss zzzz(시간대를 전체 이름으로 표시)' 형식으로 변환한다.

<br>

---

<br>

# 폼 데이터와 데이터 검증

## 폼의 생성과 처리

- `build.gradle에 내용 추가`
```gradle
implementation 'org.springframework.boot:spring-boot-starter-validation'
```

- `UserForm.java`
```java
package org.example.springmvc.domain;

import jakarta.validation.constraints.*;
import lombok.*;

@Getter
@Setter
public class UserForm {
    @NotEmpty(message = "username은 공백을 허용하지 않습니다.")
    @Size(min = 4, max = 20, message = "username은 4-20자 까지만 허용합니다.")
    private String username;

    @NotEmpty(message = "비밀번호는 공백을 허용하지 않습니다.")
    @Size(min = 6, message = "비밀번호는 최소 6자 이상 입력해야 합니다.")
    private String password;
}
```

- `FormController.java`
```java
import jakarta.validation.Valid;
import org.example.springmvc.domain.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;

@Controller
public class FormController {
    @GetMapping("/form")
    public String showForm(Model model){
        model.addAttribute("userForm", new UserForm());

        return "form";
    }

    @PostMapping("/submitForm")
    public String submitForm(@Valid @ModelAttribute("userForm") UserForm userForm,
                             BindingResult result){

        // 에러발견 됐으니 다시 form으로
        if(result.hasErrors())  return "form";

        return "result";
    }
}
```
- `@ModelAttribute`: 폼 데이터가 자동으로 해당 객체의 필드에 바인딩되어 컨트롤러에서 쉽게 사용할 수 있다.
- `@Valid 어노테이션`과 `Validator 인터페이스`를 사용하여 폼 데이터의 유효성을 검증할 수 있다.
- 유효성 검사를 통과하지 못한 데이터는 `BindingResult` 객체에 오류로 등록되며, 이를 통해 오류 메시지를 사용자에게 표시할 수 있다.

- `form.html`
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Form</title>
    </head>
    <body>
        <h1>폼 검증 예제입니다.</h1>

        <form method="post" th:action="@{/submitForm}" th:object="${userForm}">
            <label for="username"> Username </label>
            <input type="text" th:field="*{username}" id="username" />
            <div th:if="${#fields.hasErrors('username')}" th:errors="*{username}"> </div>

            <br>

            <label for="password"> Password </label>
            <input type="text" th:field="*{password}" id="password"/>
            <div th:if="${#fields.hasErrors('password')}" th:errors="*{password}"> </div>

            <br>

            <button type="submit"> submit </button>
        </form>
    </body>
</html>
```

- `result.html`
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Result</title>
    </head>
    <body>
        <h1>성공적으로 입력했어요^^</h1>
    </body>
</html>
```

<br>

## @ModelAttribute의 사용
`@ModelAttribute`는 Spring MVC에서 모델 데이터를 메소드 파라미터나 메소드 레벨에 바인딩하는 데 사용되는 어노테이션이다.

### 메소드 파라미터로 사용
폼이나 요청 파라미터를 객체에 자동으로 바인딩하기 위해 컨트롤러 메소드의 파라미터에 사용되며, 이는 폼 데이터를 받아 객체에 매핑하고, **해당 객체를 컨트롤러 로직 내에서 사용할 때** 매우 효과적이다.

- `예제`
```java
@PostMapping("/register")
public String registerUser(@ModelAttribute User user) {
    // user 객체는 폼 데이터에 자동으로 바인딩됩니다.
    userService.save(user);
    return "redirect:/users";
}
```

### 모델에 데이터 추가하기 위해 메소드 레벨에 사용
컨트롤러 내에서 `@ModelAttribute` 어노테이션이 적용된 메소드는 해당 컨트롤러의 다른 모든 요청 핸들러 메소드가 호출되기 전에 실행되며, 이 방법은 모든 요청에 **공통적인 모델 데이터를 초기화**할 때 사용된다.
- `예제`
```java
@ModelAttribute
public void addAttributes(Model model) {
    model.addAttribute("msg", "Welcome to the Application!");
}
```

<br>

## 데이터 검증을 위한 Spring Validation, @Valid, BindingResult

### @Valid
`@Valid` 어노테이션은 `Java Bean Validation API`와 통합하여 *모델 객체에 선언된 제약조건을 기반*으로 데이터 검증을 수행한다.

- `예제`
```java
@PostMapping("/register")
public String registerUser(@Valid User user, BindingResult result) {
    if (result.hasErrors()) {
        return "registerForm";
    }
    userService.save(user);
    return "redirect:/users";
}
```

### BindingResult
`BindingResult`는 `@Valid` 어노테이션 또는 `@Validated` 어노테이션을 사용하여 데이터 바인딩과 검증 후 발생하는 오류를 포착하고 관리하는 데 사용된다.

- `예제`
```java
if (result.hasErrors()) {
    model.addAttribute("user", user);
    return "registerForm";
}
```

### Bean Validation 사용
Spring MVC와 함께 `Bean Validation API`를 사용하려면, `@Valid` 어노테이션과 함께 검증 규칙을 클래스 필드에 어노테이션으로 명시해야 한다.

- `예제`
```java
public class User {
    @NotNull(message = "Name cannot be null")
    private String name;

    @Email(message = "Invalid email format")
    private String email;
}
```

<br>

## 주요 제약 조건 어노테이션
`주요 제약 조건 어노테이션`은 `Bean Validation API`의 일부로 제공되는 제약 조건 어노테이션이다.

1. `@NotEmpty`
    - 필드가 null이 아니고 길이가 0이 아닌 문자열을 가져야 함을 나타낸다.
    - 예제: `@NotEmpty(message = "The field must not be empty")`

2. `@Size`
    - 문자열, 컬렉션, 배열 필드의 크기가 지정된 범위 내에 있어야 함을 나타낸다.
    - 예제: `@Size(min = 2, max = 14, message = "The size must be between 2 and 14")`

3. `@NotNull`
    - 필드가 null이 아니어야 함을 나타낸다.
    - 예제: `@NotNull(message = "The field cannot be null")`
    
4. `@Email`
    - 문자열이 이메일 형식을 따라야 함을 나타낸다.
    - 예제: `@Email(message = "Invalid email format")`

5. `@Min` 및 `@Max`
    - 숫자 필드가 지정된 최소값 또는 최대값을 초과하거나 미달하지 않아야 함을 나타냅니다.
    - Min 예제: `@Min(value = 18, message = "Age must be at least 18")`
    - Max 예제: `@Max(value = 65, message = "Age must be no more than 65")`

6. `@Positive` 및 `@PositiveOrZero`
    - 숫자가 양수(0 포함 또는 미포함)이어야 함을 나타낸다.
    - Positive 예제: `@Positive(message = "The value must be positive")`
    - PositiveOrZero 예제: `@PositiveOrZero(message = "The value must be positive or zero")`

7. `@Negative` 및 `@NegativeOrZero`
    - 숫자가 음수(0 포함 또는 미포함)이어야 함을 나타낸다.
    - Negative 예제: `@Negative(message = "The value must be negative")`
    - NegativeOrZero 예제: `@NegativeOrZero(message = "The value must be negative or zero")`

<br>

---

<br>

# Forward와 Redirect

## Forward와 Redirect의 개념 

### 1. Forward(서버 내 포워딩)
- `정의`: 클라이언트로부터 받은 요청을 서버 내의 다른 자원(서블릿, JSP 등)으로 전달하는 방법이며 이때, 요청의 URL이 변경되지 않는다.
- `특징`: 요청과 응답 객체가 보존되어 전달되면서 URL은 변하지 않으며, 클라이언트는 포워딩이 일어났는지 알 수 없고, 서버 내부에서만 처리되기 때문에 추가적인 네트워크 비용이 없다.

### 2. Redirect (클라이언트 리다이렉션)
- `정의`: 서버가 클라이언트에게 새로운 위치로 요청을 재지시하는 방법으로, 클라이언트는 새로운 위치로 요청을 다시 보낸다.
- `특징`: 이 과정에서 브라우저의 URL이 변경되며, 서버는 `HTTP 상태 코드 302(임시 이동)` 또는 `301(영구 이동)`과 함께 새로운 URL을 클라이언트에게 전달하고, 클라이언트는 이 URL로 새로운 요청을 보내며, 이 때 요청과 응답 객체가 새로 생성된다.

<br>

## forward와 redirect 예제코드
- `RoutingController.java`
```java
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class RoutingController {
    @GetMapping("/start")
    public String start(Model model) {
        model.addAttribute("forwardTest", "Hello Forward");
        return "forward:/forward";  // forward 경로로 요청보냄
    }

    @GetMapping("/forward")
    public String forward() {
        return "forwardPage";       // forwardPage 뷰 불러오기
    }

    @GetMapping("/redirect")
    public String redirect(Model model) {
        model.addAttribute("redirectTest", "Hello Redirect");
        // 새로운 요청(URL도 바뀜)이기 때문에 모델 정보도 날라감
        return "redirect:/finalDestination";
    }

    @GetMapping("/finalDestination")
    public String finalDestination(Model model) {
        return "redirectPage";
    }
}
```
- `/start`: 이 경로로 요청이 오면, `start()` 메소드가 실행되어 요청이 `/forward` 경로로 포워드되고, 이는 사용자가 URL 변경을 인지하지 못하는 상태에서 내부적으로만 처리가 이동한다.
- `/redirect`: 이 경로로 요청이 오면, `redirect()` 메소드가 실행되어 요청이 `/finalDestination` 경로로 리다이렉트됩니다. 이 경우, 클라이언트의 브라우저는 URL을 `/finalDestination`으로 업데이트한다.

- `forwardPage.html`
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1> 나는 포워드라네 ~~ </h1>
        <p th:text="${forwardTest}"></p>
    </body>
</html>
```

- `redirectPage.html`
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h1> 나는 리다이렉트라네 ~~ </h1>
<p th:text="${redirectTest}"></p>
</body>
</html>
```

- `localhost/start 요청`: url이 바뀌지 않으며, 모델의 정보를 출력하는 걸 볼 수 있다.
<img width="377" alt="스크린샷 2024-05-21 14 36 09" src="https://github.com/hamsangjin/TIL/assets/103736614/d5afc947-6c80-40fe-9387-c72022ccf476">

<br>

- `localhost/redirect 요청`: 새로운 요청이기 때문에 url이 바뀌고, 모델의 정보 또한 불러오지 못하는 걸 볼 수 있다.
<img width="359" alt="스크린샷 2024-05-21 14 38 18" src="https://github.com/hamsangjin/TIL/assets/103736614/046ee37e-b7c0-407b-95f4-db2604e6922a">

<br>
<br>

---

<br>

# 쿠키와 세션
## Cookie
- `정의`: 서버가 사용자의 웹 브라우저에 저장하는 작은 데이터 조각을 의미하며, 클라이언트가 서버에 요청을 할 때마다 이 데이터가 요청과 함께 서버로 전송되고, 이를 통해 서버는 사용자의 상태 정보를 유지할 수 있다.
- `특징`: 일반적으로 사용자의 선호, 세션 토큰, 기타 사용자 식별 정보를 저장하는 데 사용되며, 사용자가 사이트에 대한 설정을 저장하는 데 도움이 되고, 브라우저 간 세션 유지에 유용하지만, 보안에 취약할 수 있으므로 중요한 정보는 저장하지 않아야 한다.

### Cookie 사용 시나리오
<img width="287" alt="스크린샷 2024-05-21 15 22 04" src="https://github.com/hamsangjin/TIL/assets/103736614/a4c81ac4-2e20-4572-8f60-7c4851629f4f">

- 첫 번째 요청
    1. `사용자 요청`: 사용자가 웹 페이지에 액세스를 요청하고, 이때는 아직 쿠키가 설정 되지 않았기 때문에 쿠키 정보는 없다.
    2. `HTTP 요청 수신`: 서버는 요청을 받고 사용자에 대한 어떠한 정보도 가지고 있지 않으므로, 기본 설정이나 일반 페이지를 제공한다.
    3. `HTTP 응답 전송`: 서버는 사용자의 브라우저로 응답을 보내며, 이 응답에는 사용자 설정에 따라 쿠키를 설정할 수 있다(ex: 세션 ID, 선호 언어)
    4. `페이지 표시`: 브라우저는 서버로부터 받은 응답을 사용자에게 표시한다.
- 두 번째 요청
    1. `사용자 요청`: 사용자가 다시 페이지를 요청하거나 다른 페이지로 이동하고, 이번에는 브라우저가 이전 요청에서 설정된 쿠키를 함께 전송한다.
    2. `HTTP 요청 및 쿠키 전송`: 서버는 쿠키를 포함한 HTTP 요청을 받고 쿠키를 검증하여 사용자의 선호나 설정을 확인한다.
    3. `쿠키 검증 및 페이지 맞춤 설정`: 쿠키 검증 후, 서버는 사용자 맞춤 정보를 제공할 수 있다.
    4. `맞춤형 HTTP 응답 전송`: 서버는 맞춤형 컨텐츠를 포함하여 응답을 보낸다.
    5. `맞춤형 페이지 표시`: 브라우저는 맞춤형 페이지를 사용자에게 표시한다.

<br>

## Cookie 예제 코드
- `VisitController.java`
```java
import jakarta.servlet.http.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

@Controller
public class VisitController {
    @GetMapping("/visit")
    public String showVisit(
            @CookieValue(name = "lastVisit", defaultValue = "N/A") String lastVisit,
            HttpServletResponse response,
            Model model) {

        Cookie cookie = new Cookie("lastVisit", "sangjin");
        cookie.setMaxAge(3600);
        response.addCookie(cookie);

        model.addAttribute("lastVisit", lastVisit);

        return "visit";
    }
}
```

- `visit.html`
```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        <h1> 쿠키 예제 </h1>
        <p th:text="${lastVisit}"></p>
    </body>
</html>
```

- 실행 결과(전/후)

![image.jpg1](https://github.com/hamsangjin/TIL/assets/103736614/375451d3-3559-492b-b5e3-7397f1cd973f) | ![image.jpg2](https://github.com/hamsangjin/TIL/assets/103736614/0c35ecea-952c-4989-9f36-2bf3b0aed659")
--- | --- | 

## HttpSession
- `정의`: 서버 측에서 사용자와 관련된 데이터를 저장하는 방법을 제공해주는 것으로, 세션은 서버에 생성되며, 세션 ID를 통해 각 클라이언트를 식별하면서, 이 세션 ID는 보통 쿠키를 사용하여 클라이언트에 저장되고, 각 요청마다 서버로 전송된다.
- `특징`: 객체 저장, 사용자 로그인 상태 유지 등 복잡한 데이터 관리에 적합하고, 서버 메모리에 데이터를 저장하기 때문에 쿠키보다 보안이 강화되지만, 많은 사용자가 접속하는 경우 서버 자원 사용이 증가할 수 있다.
