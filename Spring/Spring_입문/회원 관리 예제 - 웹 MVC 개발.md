# Spring 입문 - 회원 관리 예제(웹 MVC 개발)

> 인프런의 "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술" 강의를 듣고 정리한 내용입니다.

## 목차
- [회원 웹 기능 - 홈 화면 추가](#회원-웹-기능---홈-화면-추가)
- [회원 웹 기능 - 등록](#회원-웹-기능---등록)
<!--
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

- `실행결과 - http://localhost:8080/`
<p align = center><img width="719" alt="스크린샷 2023-12-06 15 32 45" src="https://github.com/hamsangjin/TIL/assets/103736614/9d602e7b-744c-47da-aeb9-4759abbf9f34"></p>

</br>
</br>

- `회원가입 화면(구현 전) - http://localhost:8080/members/new`
<p align = center><img width="734" alt="스크린샷 2023-12-06 15 33 27" src="https://github.com/hamsangjin/TIL/assets/103736614/38b7d14e-409e-4da6-87e2-bf328f877a97"></p>


</br>

> - 페이지가 존재하지 않아도 index.html로 이동하지 않는 이유
>   - 스프링 컨테이너에서 관련 컨트롤러가 있는 찾고, 없으면 static파일을 찾는 것이므로,
>   - 지금은 HomeController가 있기 때문에 정적 리소스는 무시된다.
>   - 따라서 컨트롤러가 정적파일보다 우선순위가 높다고 생각하면 된다.

</br>

---


## 회원 웹 기능 - 등록
이제 회원가입으로 이동할 수 있게 `MemberController.java`에 아래 코드를 추가해준다.

```java
@GetMapping(value = "/members/new")
        public String createForm() {
            return "members/createMemberForm";
        }
```

</br>

이제 회원가입으로 이동했을 때 보여줄 html 파일을 작성해보자
- main/resources/templates/members/createMemberForm.html
```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">

<body>

<div class="container">

    <form action="/members/new" method="post">
        <div class="form-group">
            <label for="name">이름</label>
            <input type="text" id="name" name="name" placeholder="이름을 입력하세요"> </div>
        <button type="submit">등록</button>
    </form>

</div> <!-- /container -->

</body>
</html>
```

</br>

- `회원가입 화면(구현 후) - http://localhost:8080/members/new`
<p align = center><img width="713" alt="스크린샷 2023-12-06 15 43 43" src="https://github.com/hamsangjin/TIL/assets/103736614/6b9d5477-f6bd-43ae-bd43-af86a2df787e"></p>

</br>

그 후, 회원가입 페이지에서 회원가입을 하면 전달받은 폼 객체를 생성한다.
- main/java/hello/hellospring/controller/MemberForm.java
```java
package hello.hellospring.controller;

public class MemberForm {

    private String name;

    public String getName(){
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

}
```

</br>

그렇게 이제 `MemberController.java`에서 회원을 실제로 등록하는 메소드를 만들어보자

```java
@PostMapping(value = "/members/new")
public String create(MemberForm form) {

    Member member = new Member();
    member.setName(form.getName());

    memberService.join(member);

    return "redirect:/";
}
```

</br>

> `@PostMapping` : `html` 파일 `form` 태그에서 `action url`로 `post` 방식으로 넘어왔을 때 처리해준다는 의미의 어노테이션으로, `@RequestMapping`의 형식 중 하나이다.

</br>

**작동 원리**
- url `http://localhost:8080/members/new`에서 등록 버튼을 통해 입력된 `name`이 `action url`로 `post`방식으로 넘어온다.
- **MemberController**의 `@PostMapping`이 되어있는 `create 메소드`가 호출된다.
- 그렇게 `MemberForm`의 `html input`에 입력된 `name` 값이 들어온다.

</br>

**간단한 테스트**

실제로 회원가입할 때 입력된 `name`이 넘어오는지 테스트해보자

먼저 `name`에 `test`를 입력하고 등록버튼을 통해 넘겨보자
<p align=center><img width="717" alt="스크린샷 2023-12-06 15 56 08" src="https://github.com/hamsangjin/TIL/assets/103736614/bf703132-78d9-4e40-ac19-0957e0790aac"></p>

</br>

그리고 아래 코드를 통해 터미널에 `name`을 출력해보자
```java
@PostMapping("/members/new")
public String create(MemberForm form) {
    Member member = new Member();
    member.setName(form.getName());

    System.out.println("member = " + member.getName());        // 해당 코드

    memberService.join(member);

    return "redirect:/";
}
```

</br>

<p align=center><img width="1517" alt="스크린샷 2023-12-06 15 57 31" src="https://github.com/hamsangjin/TIL/assets/103736614/a5205bd6-2291-4548-b554-f3f5a11f29da"></p>

</br>

이 사진을 보면 `member`에 `test`라는 `name`이 잘 저장된 것을 알 수 있다.

---

<!--
## 회원 웹 기능 - 조회
-->
