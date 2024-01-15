# Spring 입문 - AOP

> 인프런의 "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술" 강의를 듣고 정리한 내용입니다.

## 목차
- [AOP가 필요한 상황](#AOP가-필요한-상황)
- [AOP 적용](#AOP-적용)
</br>

---

</br>

# AOP

> `AOP(Aspect Oriented Programming)` : 관점 지향 프로그래밍이라고 불리며, 관점 지향은 쉽게 말해 어떤 로직을 기준으로 핵심 관심 사항,
> 공통 관심 사항으로 나누어서 보고 그 관점을 기준으로 각각 모듈화하겠다는 것


## AOP가 필요한 상황

### 모든 메소드의 호출 시간을 측정하고 싶은 경우

- /main/java/hello/hellospring/service/MemberService.java
```java

// @Service
@Transactional
public class MemberService {

    ...

    public Long join(Member member){

        long start = System.currentTimeMillis();

        try {
            validateDuplicateMember(member);

            memberRepository.save(member);
            return member.getId();
        } finally {
            long finish = System.currentTimeMillis();
            System.out.println("join = " + (finish - start) + "Ms");
        }
    }

    public List<Member> findMembers(){
        long start = System.currentTimeMillis();

        try {
            return memberRepository.findAll();
        } finally {
            long finish = System.currentTimeMillis();
            System.out.println("findMembers " + (finish - start) + "ms");
        }

    }

    ...

```

- 이렇게 **시작 시간**과 **끝나는 시간**을 측정해 차이를 출력하는 방법으로 호출 시간을 확인할 수 있다.
- 하지만 회원 가입 및 조회에 시간을 측정하는 기능은 `핵심 관심 사항`이 아니고 `공통 관심 사항`이다.
- 이렇게 되면, **시간을 측정하는 로직(공통)**과 **핵심 비즈니스의 로직**이 섞여서 유지보수가 어려워진다.
- 그러나, 시간을 측정하는 로직을 별도의 공통 로직으로 만들기 어려우며, 변경할 때도 모든 로직을 찾아가면서 변경해야하는 번거로움(예를 들어 1,000개의 로직)이 생긴다.

> 이때, AOP를 적용해보자

</br>

---

</br>

## AOP 적용

> 공통 관심 사항와 핵심 관심 사항을 분리하는 것이 핵심

<p align=center>
<img width="608" alt="스크린샷 2024-01-15 14 48 57" src="https://github.com/hamsangjin/TIL/assets/103736614/43f9339e-b968-44d4-85f0-27eb0f0f42e5">
</p>

- `TimeTraceAop`를 이용해 `helloController`나 `memberService`같이 원하는 곳에 공통 관심 사항을 적용한다.

</br>

### 시간 측정 AOP 등록

- main/java/hello/hellospring/aop/TimeTraceAop.java
```java
package hello.hellospring.aop;

import org.aspectj.lang.ProceedingJoinPoint;
import org.aspectj.lang.annotation.Around;
import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;


@Aspect
@Component
public class TimeTraceAop {

    @Around("execution(* hello.hellospring..*(..))")
    public Object execute(ProceedingJoinPoint joinPoint) throws Throwable{

        long start = System.currentTimeMillis();
        System.out.println("START : " + joinPoint.toString());

        try {
            return joinPoint.proceed();

        } finally {
            long finish = System.currentTimeMillis();
            System.out.println("END : " + joinPoint.toString() + " " + (finish - start) + "ms");
        }

    }
}
```

- `@Aspect` : aop로 사용하겠다는 의미
- `@Component` : 스프링 빈에 등록
- `@Around()` : 공통 관심사항을 타겟팅해줄 수 있고, 실행하려는 패키지명과 클래스를 설정 가능하다. 
  - 현재는 `hello.hellospring 하위 패키지`는 다 설정하게 함
- `joinPoint.proceed()` : 대상 객체의 메소드를 실행하므로, 이 코드 전후로 공통 기능을 위한 코드를 위치시킴

</br>

### 스프링의 AOP 동작 방식 설명

- 간단하게 `memberController`와 `memberService`의 의존관계 차이를 알아보자

#### AOP 적용 전 의존 관계
<p align=center>
<img width="607" alt="스크린샷 2024-01-15 15 36 45" src="https://github.com/hamsangjin/TIL/assets/103736614/2776b203-1138-458d-bf13-366683ae7b29">
</p>

- AOP 적용 전엔 `memberController`가 `memberService`를 의존하고 있고, `memberController`에서 메소드를 호출하면 `memberService`의 메소드를 호출했을 것이다.

#### AOP 적용 후 의존 관계
<p align=center>
<img width="605" alt="스크린샷 2024-01-15 15 37 11" src="https://github.com/hamsangjin/TIL/assets/103736614/92467412-5d31-4ab2-b314-cb41c3eddd1c">
</p>

- AOP가 적용된 후엔 `memberController`에서 `memberService`를 호출하면, 실제 `memberService`가 아닌 가짜 `memberService` 즉, **proxy**를 만들어내 **proxy**를 스프링 빈으로 등록한다.
- 그리고 `jointPoint.proceed()`했을 때 실제 `memberService`를 호출해준다.

</br>

- 이렇게 **핵심 관심 사항**과 **공통 관심 사항**을 분리했고, 시간을 측정하는 로직을 별도의 공통 로직으로 만들었다.
- 따라서, 핵심 관심 사항을 깔끔하게 유지할 수 있고, 변경할 때 이 로직만 변경하면 돼서 편리하며, 원하는 적용 대상을 선택할 수 있게 됐다.

</br>

실제로 **Proxy**가 주입되는지 콘솔에 출력해 확인해보자
- main/java/hello/hellospring/controller/MemberController.java

```java
public class MemberController {

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
        System.out.println("memberService = " + memberService.getClass());
    }

    ...
```

- 결과 확인
<p align=center>
<img width="803" alt="스크린샷 2024-01-15 16 03 19" src="https://github.com/hamsangjin/TIL/assets/103736614/6657ef4c-687f-45f9-90b1-4fd992384df9">
</p>

- `$$SpringCGLIB$$0` : `memberService`를 가지고 복제해서 코드를 조작하는 기술
