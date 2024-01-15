# Spring 입문 - AOP

> 인프런의 "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술" 강의를 듣고 정리한 내용입니다.

## 목차
- [AOP가 필요한 상황](#AOP가-필요한-상황)

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

