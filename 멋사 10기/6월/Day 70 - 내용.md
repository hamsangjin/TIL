# 사용자 이름 / 비밀번호 인증

## 예제 코드
- `SecurityConfig.java`
```java
package hello.securityexam3;

import lombok.extern.slf4j.Slf4j;
import org.springframework.context.annotation.*;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.*;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
@Slf4j
public class SecurityConfig {
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        // 인증 설정(기본 설정과 같음)
        http
                .authorizeRequests(authroizeRequest -> authroizeRequest
                        .requestMatchers("/shop/**", "/test").permitAll()               // 이떄 지정한 페이지는 누구나 접근 가능
                        .requestMatchers("/user/mypage").hasRole("USER")                // 해당 페이지는 USER만
                        .requestMatchers("/admin/abc").hasRole("ADMIN")                 // 해당 페이지는 ADMIN만
                        .requestMatchers("/admin/**").hasAnyRole("ADMIN", "SUPERUSER")  // 해당 페이지는 ADMIN과 SUPERUSER만
                        .anyRequest()       // 모든 요청에 대해서
                        .authenticated()    // 인증을 요구
                )
                .formLogin(Customizer.withDefaults());

        return http.build();
    }

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        // user
        UserDetails user = User.withUsername("user")
                .password(passwordEncoder().encode("1234"))
                .roles("USER")
                .build();
        // admin
        UserDetails admin = User.withUsername("admin")
                .password(passwordEncoder().encode("1234"))
                .roles("ADMIN")
                .build();
        // superuser
        UserDetails superuser = User.withUsername("superuser")
                .password(passwordEncoder().encode("1234"))
                .roles("SUPERUSER")
                .build();
        // sangjin
        UserDetails sangjin = User.withUsername("sangjin")
                .password(passwordEncoder().encode("1234"))
                .roles("ADMIN, USER")
                .build();

        // 사용자 정보를 제공: 사용자 정보를 인메모리에 저장
        return new InMemoryUserDetailsManager(user, admin, superuser, sangjin);
    }
}
```
`requestMatchers` 메소드의 경로에 **라고 작성하면 모든 경로를 뜻하지만, 그보다 위에 **이 아닌 구체적인 경로가 작성되었다면 그 구체적인 경로는 따로 적용된다.

- `AdminController.java`
```java
package hello.securityexam3;

import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/admin")
public class AdminController {
    // ADMIN
    @GetMapping("/abc")
    public String abc() {
        return "abc";
    }

    // ADMIN, SUPERUSER
    @GetMapping("/def")
    public String def() {
        return "def";
    }

    // ADMIN, SUPERUSER
    @GetMapping("/list")
    public String list() {
        return "list";
    }

    // ADMIN, SUPERUSER
    @GetMapping("/add")
    public String add() {
        return "add";
    }
}
```

- `HomeController.java`
```java
package hello.securityexam3;

import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HomeController {
    @GetMapping("/test")
    public String test(){
        // ContextHolder 이용, 현재 스레드의 보안 컨텍스트에서 인증 정보를 가져옴
        Authentication authentication = SecurityContextHolder.getContext().getAuthentication();
        System.out.println(authentication.isAuthenticated());

        // 인증 정보가 없거나, 인증되지 않았거나, 인증 주체가 문자열(즉, 익명 사용자)인 경우
        if (authentication == null || !authentication.isAuthenticated() || authentication.getPrincipal() instanceof String) {
            return "익명 사용자입니다 !";
        }

        // 로그인 인증 후, 사용자 정보 가져오기
        UserDetails userDetails = (UserDetails) authentication.getPrincipal();
        return "username :: " + userDetails.getUsername();
    }
}
```

### Thymeleaf에서 사용자 인증 정보 사용
- `build.gradle`
```
...
implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6:3.1.2.RELEASE'
```

- `mypage.html`
```html
mypage.html

<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" xmlns:sec="https://www.thymeleaf.org/extras/spring-security">
    <head>
        <meta charset="UTF-8">
        <title>mypage</title>
    </head>
    <body>
        <h1>My page</h1>
        <div sec:authorize="isAuthenticated()">
            <p>안녕하세요. <span sec:authentication="name"></span>님! </p>
            <p>당신의 롤은 : <span sec:authentication="principal.authorities"></span> </p>
            
            <p>상세정보</p>
            <ul>
                <li>사용자 이름: <span sec:authentication="principal.username"></span></li>
                <li>계정 만료 여부: <span sec:authentication="principal.accountNonExpired"></span></li>
                <li>계정 잠김 여부: <span sec:authentication="principal.accountNonLocked"></span></li>
                <li>자격 증명 여부: <span sec:authentication="principal.credentialsNonExpired"></span></li>
                <li>활성부 여부: <span sec:authentication="principal.enabled"></span></li>
            </ul>
        </div>
        
        <div sec:authorize="isAuthenticated()">
            <p>로그인되지 않았습니다. </p> <a th:href="@{login}">로그인 </a> 해주세요!
        </div>
    </body>
</html>
```
`sec` 속성을 이용해서 인증 정보들을 사용하는 것을 볼 수 있다.
![image](https://github.com/hamsangjin/TIL/assets/103736614/b757315c-1841-4503-9896-89dcf36ae6e1)

<br>

---

<br>

# 실습 예제코드

## 기본 설정
- `build.grale`
```
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.3.1'
    id 'io.spring.dependency-management' version '1.1.5'
}

group = 'hello'
version = '0.0.1-SNAPSHOT'

java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-security'
    implementation 'org.thymeleaf.extras:thymeleaf-extras-springsecurity6:3.1.2.RELEASE'
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
    runtimeOnly 'com.mysql:mysql-connector-j'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

- `application.yml`
```yaml
spring:
  application:
    name: securityexam

  security:
    user:
      name: user
      password: 1234

  datasource:
    url: jdbc:mysql://localhost:3306/liondb
    username: sangjin
    password: sangjin
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
```

<br>

## Config
- `SecurityConfig.java`
```java
package hello.zizonsecurity.config;

import hello.zizonsecurity.security.CustomUserDetailsService;
import lombok.*;
import org.springframework.context.annotation.*;
import org.springframework.security.config.Customizer;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {
    private final CustomUserDetailsService customUserDetailsService;

    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/userregform", "/userreg", "/").permitAll()
                        .anyRequest().authenticated()
                )
                .formLogin(form -> form
                        .loginPage("/loginform")
                        .loginProcessingUrl("/login")     // 로그인 폼의 action URL
                        .defaultSuccessUrl("/welcome")    // 로그인 성공 시 리다이렉트 경로 
                        .permitAll()                      // 로그인 페이지에는 다 접근 가능
                )
                .logout(logout -> logout
                        .logoutUrl("/logout")
                        .logoutSuccessUrl("/")
                )
                .sessionManagement(sessionManagement -> sessionManagement
                        .maximumSessions(1)                 // 동시 접속 허용 개수(-1은 무제한)
                        // 동시 로그인을 차단할 때, 현재 사용자 인증 실패 처리(true)를 할거냐, 이전 사용자의 세션 만료 처리(false)를 할거냐
                        .maxSessionsPreventsLogin(true)
                )

                .userDetailsService(customUserDetailsService)
                .csrf(csrf -> csrf.disable());

        return http.build();
    }
}
```
- login 작동 방식
    - 사용자가 로그인 폼에서 입력한 사용자 이름과 비밀번호를 제출할 때, 이 `URL("/login")`로 POST 요청이 전송된다.
    - Spring Security는 이 URL로 들어오는 요청을 가로채고, 사용자 인증을 처리한다.
    - 이로써, `controller`에 `@PostMapping` 을 하지 않아도 됨!

<br>

## 도메인
- `User.java`
```java
package hello.zizonsecurity.domain;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;
import java.util.Set;

@Entity
@Table(name = "users")
@Getter @Setter
@NoArgsConstructor
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true, length = 50)
    private String username;

    @Column(nullable = false, length = 100)
    private String password;

    @Column(nullable = false, length = 50)
    private String name;

    @Column(nullable = false, length = 100)
    private String email;

    @Column(name = "registration_date", updatable = false, nullable = false)
    private LocalDateTime registrationDate = LocalDateTime.now();

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
            name = "user_roles",
            joinColumns = @JoinColumn(name = "user_id"),
            inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;
}
```

- `Role.java`
```java
package hello.zizonsecurity.domain;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "roles")
@Getter @Setter
@NoArgsConstructor
public class Role {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false, unique = true, length = 50)
    private String name;
}
```

<br>

## 리포지토리
- `UserRepository.java`
```java
package hello.zizonsecurity.repository;

import hello.zizonsecurity.domain.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}
```

- `RoleRepository.java`
```java
package hello.zizonsecurity.repository;

import hello.zizonsecurity.domain.Role;
import org.springframework.data.jpa.repository.JpaRepository;

public interface RoleRepository extends JpaRepository<Role, Long> {
    Role findByName(String name);
}
```

<br>

## 시큐리티
- `CustomUserDetailsService.java`
```java
package hello.zizonsecurity.security;

import hello.zizonsecurity.domain.User;
import hello.zizonsecurity.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.core.userdetails.*;
import org.springframework.stereotype.Service;
import org.springframework.security.core.userdetails.User.*;

@Service
@RequiredArgsConstructor
public class CustomUserDetailsService implements UserDetailsService {
    private final UserRepository userRepository;

    // 시큐리티는 이 메소드를 통해서 유저 정보를 얻어낼 수 있음
    @Override
    public UserDetails loadUserByUsername(String username) throws UsernameNotFoundException {
        User user = userRepository.findByUsername(username);
        if (user == null)       throw new UsernameNotFoundException(username);

        UserBuilder userBuilder = org.springframework.security.core.userdetails.User.withUsername(username);
        userBuilder.password(user.getPassword());
        userBuilder.roles(user.getRoles().stream().map(role -> role.getName()).toArray(String[]::new));

        return userBuilder.build();
    }
}
```
앞에서 했던 예제의 `SecurityConfig`와 같은 역할을 하며, 다른 점은 메모리 저장 방식에서 실제 DB에 저장하는 방식으로 바뀌었다.

<br>

## 서비스
- `UserService.java`
```java
package hello.zizonsecurity.service;

import hello.zizonsecurity.domain.*;
import hello.zizonsecurity.repository.*;
import lombok.*;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.Collections;

@Service
@RequiredArgsConstructor
public class UserService {
    private final UserRepository userRepository;
    private final RoleRepository roleRepository;
    private final PasswordEncoder passwordEncoder;

    // 회원가입
    @Transactional
    public User registerUser(User user) {
        // Role 추가
        Role userRole = roleRepository.findByName("USER");
        user.setRoles(Collections.singleton(userRole));

        // password 인코딩
        user.setPassword(passwordEncoder.encode(user.getPassword()));
        return userRepository.save(user);
    }

    @Transactional
    public User findByUsername(String username) {
        return userRepository.findByUsername(username);
    }
}
```

<br>

## 컨트롤러
- `UserController.java`
```java
package hello.zizonsecurity.controller;

import hello.zizonsecurity.domain.User;
import hello.zizonsecurity.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;

@Controller
@RequiredArgsConstructor
public class UserController {
    private final UserService userService;

    @GetMapping("/userregform")
    public String userregform() {
        return "users/userregform";
    }

    @PostMapping("/userreg")
    public String userreg(@ModelAttribute("user") User user,
                          BindingResult result) {
        if (result.hasErrors())     return "users/userregform";

        User byUsername = userService.findByUsername(user.getUsername());
        // 중복된 사용자 에러 처리
        if (byUsername != null) {
            result.rejectValue("username", null, "이미 사용중인 아이디입니다.");
            return "users/userregerror";
        }

        userService.registerUser(user);
        return "redirect:/welcome";
    }

    @GetMapping("/welcome")
    public String welcome() {
        return "users/welcome";
    }

    @GetMapping("/loginform")
    public String loginform() {
        return "users/loginform";
    }

    @GetMapping("/")
    public String home() {
        return "home";
    }
}
```
