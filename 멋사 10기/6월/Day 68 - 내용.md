# Spring Security
자바 기반의 애플리케이션에서 인증(Authentication)과 권한 부여(Authorization)를 담당하는 보안 프레임워크

<br>

## 주요 기능
- `인증(Authentication)`: 사용자의 신원을 확인하는 과정으로, 다양한 인증 메커니즘(폼 로그인, HTTP Basic, OAuth 등)을 지원한다.
- `권한 부여(Authorization)`: 인증된 사용자가 애플리케이션 내에서 어떤 자원에 접근 할 수 있는지를 결정하며, 역할 기반 접근 제어(Role-Based Access Control, RBAC)를 포함한 다양한 접근 제어 방식을 지원한다.
- `세션 관리`: 사용자 세션의 생성, 유지, 만료 등을 관리하여 보안성을 높인다.
- `보안 이벤트 로깅`: 로그인 시도, 실패 등의 보안 관련 이벤트를 로깅하여 보안 감사를 돕는다.

<br>

---

<br>

# Spring Security 아키텍처

## 필터 검토
Spring Security의 서블릿 지원은 서블릿 필터를 기반으로 한다.

![](https://github.com/hamsangjin/TIL/assets/103736614/5c7d2df6-0e05-4b8a-b59c-de3f85663d46)

<br>

## 필터 체인
클라이언트가 애플리케이션에 요청을 보내면, 컨테이너는 요청 URI 경로에 따라 HttpServletRequest를 처리해야 하는 필터 인스턴스와 서블릿이 포함된 FilterChain을 생성한다. 

Spring MVC 애플리케이션에서는 DispatcherServlet 인스턴스가 서블릿 역할을 한다. 

하나의 HttpServletRequest와 HttpServletResponse는 최대 하나의 서블릿만 처리할 수 있다.

### 예제 코드
```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
    // 애플리케이션의 나머지 부분을 호출하기 전에 작업 수행
    chain.doFilter(request, response); // 애플리케이션의 나머지 부분 호출
    // 애플리케이션의 나머지 부분을 호출한 후 작업 수행
}
```

<br>

## DelegatingFilterProxy
서블릿 컨테이너의 생명 주기와 Spring의 `ApplicationContext`간의 연결을 가능하게 한다.  

`DelegatingFilterProxy`는 표준 서블릿 컨테이너 메커니즘을 통해 등록할 수 있으며, 필터를 구현하는 Spring 빈에 모든 작업을 위임한다.

![](https://github.com/hamsangjin/TIL/assets/103736614/6804eaef-a06b-4763-9c03-2574dcc24758)

### 예제 코드
```java
public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) {
    Filter delegate = getFilterBean(someBeanName);
    delegate.doFilter(request, response);
}
```

<br>

## FilterChainProxy
Spring Security의 서블릿 지원은 `FilterChainProxy` 내에 포함되어 있다.

`FilterChainProxy`는 Spring Security가 제공하는 특별한 필터로, `SecurityFilterChain`을 통해 여러 필터 인스턴스에 위임할 수 있게 한다.

`FilterChainProxy`는 일반적으로 `DelegatingFilterProxy`로 래핑되며, 현재 요청에 대해 어떤 Spring Security 필터 인스턴스가 호출되어야 하는지를 결정한다.

![](https://github.com/hamsangjin/TIL/assets/103736614/167c8e15-4701-4abb-afa3-1d086a7cc976)

<br>

## SecurityFilterChain
`SecurityFilterChain`은 `FilterChainProxy`가 현재 요청에 대해 어떤 Spring Security 필터 인스턴스를 호출할지를 결정하는 데 사용된다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig {
@Bean
    public SecurityFilterChain filterChain(HttpSecurity http) throws Exception {
        http
            .csrf(Customizer.withDefaults())
            .authorizeHttpRequests(authorize -> authorize
                .anyRequest().authenticated()
            )
            .httpBasic(Customizer.withDefaults())
            .formLogin(Customizer.withDefaults());
        return http.build();
    }
}
```
1. `CsrfFilter`: CSRF 공격으로부터 보호
2. `UsernamePasswordAuthenticationFilter`: 폼 로그인 처리
3. `BasicAuthenticationFilter`: HTTP 기본 인증 처리
4. `AuthorizationFilter`: 요청의 권한 부여 처리

<br>

---

<br>

# 실습 코드
- `build.gradle`
![](https://github.com/hamsangjin/TIL/assets/103736614/4bd2f5bb-73aa-452f-8c9e-000c12c157a9)

- `HomeController.java`
```java
package hello.securityexam;

import org.springframework.web.bind.annotation.*;

@RestController
public class HomeController {
    @GetMapping("/")
    public String home() {
        return "Home";
    }
}
```

<br>

## / 접속
![](https://github.com/hamsangjin/TIL/assets/103736614/b2d433ff-5e1a-4dc9-8f47-f1a6ae333f03)

위와같이 의존성을 추가한 것만으로도, 모든 경로에 login한 경우에만 접근이 가능해지며, login 기능도 제공해준다.

그리고 user라는 id와 함께 pw로 구성된 기본 회원 정보를 제공해준다.

![](https://github.com/hamsangjin/TIL/assets/103736614/c6752962-d685-406c-be16-981dc30345f8)

<br>

## 필터 체인 구성 및 설정 변경
- application.yml
```yaml
spring:
  application:
    name: securityexam

  security:
    user:
      # 제공하는 회원 정보를 변경
      name: user        
      password: 1234
```

- `SecurityConfig.java`
```java
package hello.securityexam;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // 인증 설정(기본 설정과 같음)
        http
                .authorizeRequests(authroizeRequest -> authroizeRequest
                        .anyRequest()       // 모든 요청에 대해서
                        .authenticated()    // 인증을 요구
                )
                .formLogin(Customizer.withDefaults());  // 인증을 하는 방법은 기본으로 제공해주는 formlogin을 사용

        // 아이디 기억 설정
        http
                .rememberMe(remember -> remember
                        .rememberMeParameter("remember")
                        .tokenValiditySeconds(3000) // 토큰 유지 시간
                );

        return http.build();
    }
}
```




