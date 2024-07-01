# Spring Boot와 Spring Security 폼로그인 시 세션에 저장되는 값

Spring Boot 및 Security를 사용해 개발하면 기본적으로 세션에 `사용자 인증 정보`, `권한 정보`, `세션 관리 정보`, `기타 필요한 사용자 정보` 등이 저장된다.

Spring Security는 다음과 같은 주요 클래스들을 사용하여 세션 관리를 한다.

1.  `SecurityContext`: 현재 인증된 사용자의 정보를 담고 있는 객체로, 주로 Authentication 객체를 포함하며, 필요에 따라 Authentication을 통해 사용자의 권한 정보도 접근할 수 있다.
2.  `SecurityContextHolder`: SecurityContext를 저장하고 제공하는 역할을 하며, 기본적으로 ThreadLocal을 사용하여 현재 실행 중인 스레드에 대한 SecurityContext를 관리한다.
3. `Authentication`: 사용자의 인증 정보를 담고 있는 객체로, 주로 Principal과 GrantedAuthority를 포함하고 있다.

<br>

---

<br>

# 서버가 2대 이상으로 이중화 되어 있을 경우 발생할 수 있는 문제점?

한 대의 서버로 운영하는 서비스가 커짐에 따라 현재 성능으로는 운영이 불가능해졌다고 가정해보자.

- `scale-up 방식`: 서버 자체 성능을 늘려 부하를 견딜수 있게 하는 방식이지만, 여전히 서버 한 대에 모든 트래픽이 집중되므로 만일에 서버 장애가 생길시 서버가 복구될 때까지 서비스를 중단해야 하는 상황이 발생할 수 있는 위험이 있다.
    - `문제점`: 사용하려던 서비스가 중단된다면 엄청난 비즈니스 손실(수익 손실)이 생길 수 있다.
- `scale-out 방식`: 서버를 여러 대로 늘려서 각 서버에 **로드밸런싱***으로 트래픽을 분산하게 한다. 그래서 서버 한 대에 장애가 생겨도 다른 서버는 살아있으니 서비스 문제가 생기지 않는다.
    - `Load Balancer`: 여러 대의 서버를 두고 트래픽을 분산처리하여 서버의 로드율 증가, 부하량, 속도저하 등을 해결해주는 서비스
    - `문제점`: 데이터 정합성, 세션 불일치

> 현재 언급한 서버가 2대 이상으로 이중화되는 경우는`scale-out 방식`에 해당한다.

<br>

---

<br>

# Redis를 이용한 세션 클러스터링 설정하기
![image](https://github.com/hamsangjin/TIL/assets/103736614/3c4ba7ad-c3da-42ef-9395-1baa89d5ff34)

`세션 클러스터링`은 세션의 불일치 해결 방법 중 하나로, 서버들을 하나의 클러스터로 묶어 관리하고,클러스터 내의 서버들이 세션을 공유할 수 있도록 하는 방식이다.

예를 들어 서버1에서 login session이 저장되었다면, 서버2와 서버3에도 서버1에 저장되어있는 세션을 전파(복사)하는 것을 말한다.

~~실제 Redis 사용은 생략~~

<br>

---

<br>

# 세션을 공유할 때의 단점

1. `네트워크 지연과 부하`: 세션 데이터를 Redis와 같은 외부 저장소에 저장하고 가져오는 과정에서 네트워크 지연이 발생할 수 있다.
2. `운영 복잡성`: Redis나 기타 외부 저장소를 사용하기 위해서는 추가적인 운영 관리가 필요하다.
3. `보안 문제`: 외부 저장소에 세션 데이터를 저장할 때 보안 문제가 발생할 수 있다.
4. `성능 문제`: 매우 빠른 인메모리 세션 스토어에 비해 외부 저장소를 사용하면 성능이 느릴 수 있다.
5. `비용`: 외부 저장소를 사용하는 경우 추가적인 비용이 발생할 수 있다.

<br>

---

<br>

# JWT(JSON Web Token)

`JWT`는 정보를 안전하게 전송하기 위한 표준 방법 중 하나로, 주로 인증된 사용자가 데이터를 안전하게 전송하고, 정보를 검증할 때 사용된다.

JWT는 JSON 객체를 사용하여 정보를 저장하고, 디지털 서명을 통해 검증된다.

## 구성요소
1. `Header`: JWT의 타입과 해싱 알고리즘 정보가 담겨 있으며, 예를 들어, `{"alg": "HS256", "typ": "JWT"}` 와 같은 JSON 객체를 말한다.
2. `Payload`: 실제로 전송할 데이터가 담긴 부분으로, 클레임이라 불리는 정보들이 포함되고, 클레임은 세 가지 유형으로 나뉜다.
    - `Registered claims`: 특정한 정보를 제공하기 위해 사전에 정의된 클레임들
    - `Public claims`: 사용자 정의 클레임으로 충돌을 피하기 위해 URI 형식으로 작성됨
    - `Private claims`: 애플리케이션에서 사용할 수 있는 클레임
3. `Signature`: Header와 Payload의 내용을 인코딩하고, 비밀 키(secret key)를 사용하여
서명하며, 이 서명은 메시지가 변경되지 않았음을 확인하는 데 사용된다.

<br>

## JWT 작동 방식
1. `생성`: 사용자가 로그인하면 서버는 JWT를 생성하여 사용자에게 반환하고, 이 JWT는 클라이언트의 저장소에 저장된다.
2. `전송`: 클라이언트는 JWT를 HTTP 요청의 헤더나 URL 매개변수로 전송하여 서버에 전달한다.
3. `검증`: 서버는 JWT의 서명을 통해 해당 토큰이 유효한지 검증하고, 클라이언트가 요청한 작업을 수행하며, 서명이 일치하지 않거나 JWT가 만료된 경우 서버는 요청을 거부할 수 있다.

<br>

---

<br>
  
# JWT를 자바에서 이용하기

- `build.gradle`
```java
...

dependencies {
    // jwt & json
    // jwts
    implementation group: 'io.jsonwebtoken', name: 'jjwt-api', version: '0.11.5'
    runtimeOnly group: 'io.jsonwebtoken', name: 'jjwt-impl', version: '0.11.5'
    runtimeOnly group: 'io.jsonwebtoken', name: 'jjwt-jackson', version: '0.11.5'

    //gson - json 메시지를 다루기 위한 라이브러리
    implementation 'com.google.code.gson:gson'

    ...
}
...
```

<br>

- `application.yml`
```yaml
...

jwt:
  secretKey: 12345678901234567890123456789012
  refreshKey: 12345678901234567890123456789012
```
- `Access Token`: 접근에 관여하는 토큰
- `Refresh Token`: Access Token의 인증이 만료되었을 때, 재발급에 관여하는 토큰

<br>

- `JwtTokenizer.java`
```java
package hello.jwtexam.jwt.util;

import io.jsonwebtoken.*;
import io.jsonwebtoken.security.Keys;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;
import java.nio.charset.StandardCharsets;
import java.security.Key;
import java.util.*;

@Component
@Slf4j
public class JwtTokenizer {
    private final byte[] accessSecret;
    private final byte[] refreshSecret;

    // 토큰 만료 시간 설정 -> ms라서 x1000
    public static Long ACCESS_TOKEN_EXPIRATION_TIME = 30 * 60 * 1000L;              // 30분
    public static Long REFRESH_TOKEN_EXPIRATION_TIME = 7 * 24 * 60 * 60 * 1000L;    // 7일

    /*
        생성자(JwtTokenizer)
        생성자에서 @Value 어노테이션을 통해 주입받은 jwt.secretKey와 jwt.refreshKey를
        UTF-8 인코딩된 바이트 배열로 변환하여 accessSecret 와 refreshSecret 필드에 저장한다.
    */
    public JwtTokenizer(@Value("${jwt.secretKey}") String accessSecret,
                        @Value("${jwt.refreshKey}") String refreshSecret) {
        this.accessSecret = accessSecret.getBytes(StandardCharsets.UTF_8);
        this.refreshSecret = refreshSecret.getBytes(StandardCharsets.UTF_8);
    }
    
    /*
        서명 키 생성 메소드
        getSigningKey 메서드는 주어진 바이트 배열을 기반으로 HMAC-SHA 알고리즘에 사용할 SecretKey를 생성하여 반환한다.
    */
    public static Key getSigningKey(byte[] secretKey) {
        return Keys.hmacShaKeyFor(secretKey);
    }

    /*
        JWT 생성(createToken 메서드)
        createToken 메서드는 사용자 ID, 이메일, 이름, 사용자명, 역할 등을 기반으로 JWT를 생성한다.
        JWT의 Payload(클레임)에는 이 정보들이 포함되며, 토큰은 발급 시각과 만료 시각이 지정된다.
        signWith 메서드를 통해 HMAC-SHA 알고리즘을 사용하여 서명되며, getSigningKey 메서드를 호출하여 해당 알고리즘에 필요한 SecretKey를 생성한다.
    */
    private String createToken(Long id, String email, String name, String username,
                               List<String> roles, Long expire, byte[] secretKey) {

        Claims claims = Jwts.claims().setSubject(email);
        claims.put("username", username);
        claims.put("name", name);
        claims.put("userId", id);
        claims.put("roles", roles);
        claims.put("expire", expire);
        claims.put("secretKey", secretKey);

        return Jwts.builder()
                .setClaims(claims)
                .setIssuedAt(new Date())
                .setExpiration(new Date(new Date().getTime() + expire))
                .signWith(getSigningKey(secretKey))
                .compact();
    }

    /*
        AccessToken 및 RefreshToken 생성 메서드
        createAccessToken 메서드와 createRefreshToken 메서드는 각각 AccessToken과 RefreshToken을 생성한다.
        createToken 메서드를 호출하여 JWT를 생성하며, 각 토큰의 만료 시간과 사용할 보안 키를 지정한다.
    */
    // ACCESS Token 생성
    public String createAccessToken(Long id, String email, String name, String username, List<String> roles) {
        return createToken(id, email, name, username, roles, ACCESS_TOKEN_EXPIRATION_TIME, accessSecret);
    }

    // Refresh Token 생성
    public String createRefreshToken(Long id, String email, String name, String username, List<String> roles) {
        return createToken(id, email, name, username, roles, REFRESH_TOKEN_EXPIRATION_TIME, refreshSecret);
    }

    // 토큰에서 유저 아이디 얻기
    public Long getUserIdFromToken(String token){
        String[] tokenArr = token.split(" ");
        token = tokenArr[1];
        Claims claims = parseToken(token, accessSecret);
        return Long.valueOf((Integer)claims.get("userId"));
    }

		/*
        토큰 파싱 및 검증
        parseToken, parseAccessToken, parseRefreshToken 메서드는 각각 AccessToken 및 RefreshToken을 파싱하여 클레임(claims)을 추출한다.
        parseToken 메서드 내에서는 Jwts.parserBuilder()를 통해 JWT 파서를 생성하고, setSigningKey 메서드로 검증에 사용할 SecretKey를 설정한다.
    */
    public Claims parseToken(String token, byte[] secretKey){
        return Jwts.parserBuilder()
                .setSigningKey(getSigningKey(secretKey))
                .build()
                .parseClaimsJws(token)
                .getBody();
    }

    public Claims parseAccessToken(String accessToken) {
        return parseToken(accessToken, accessSecret);
    }

    public Claims parseRefreshToken(String refreshToken) {
        return parseToken(refreshToken, refreshSecret);
    }
}
```

<br>

- `JwtExamApplication.java`
```java
package hello.jwtexam;

import hello.jwtexam.jwt.util.JwtTokenizer;
import io.jsonwebtoken.Claims;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.lang.reflect.Array;
import java.util.Arrays;

@SpringBootApplication
@Slf4j
public class JwtExamAppication {
    public static void main(String[] args) {
        SpringApplication.run(JwtExamAppication.class, args);
    }

		// 토큰 생성을 위한 JwtTokenizer 의존성 주입
    @Autowired
    JwtTokenizer jwtTokenizer;

    @Bean
    public CommandLineRunner run() {
        return args -> {
		        // Access Token 생성
            String accessToken = jwtTokenizer.createAccessToken(1L, "ham@sang.jin", "함상진", "sangjin", Arrays.asList("ROLE_USER"));
            log.info("Access token {}", accessToken);

						// Refresh Token 생성
            String refreshToken = jwtTokenizer.createRefreshToken(1L, "ham@sang.jin", "함상진", "sangjin", Arrays.asList("ROLE_USER"));
            log.info("Refresh token {}", refreshToken);

						// AccessToken 파싱
            Claims claims = jwtTokenizer.parseAccessToken(accessToken);
            log.info("Access Token Claims : {}", claims);
            log.info("userId :: {}", claims.get("userId"));
        };
    }
}
```
![image](https://github.com/hamsangjin/TIL/assets/103736614/c20cfeb4-05e1-4a33-81e6-d244ac8270bb)

<br>

---

<br>

# FORM 로그인이 아닌, API를 이용한 로그인 (JWT이용)

## 장점
- Stateless 성격과 확장성
- Cross-Origin Resource Sharing (CORS)
- 스케일링 및 성능
- 분리된 클라이언트 및 서버 엔드포인트
- 보안 강화

<br>

## 단점
- 토큰 크기와 보안
- 세션 관리의 어려움
- 토큰 만료와 갱신
- 정보 최신성
- 사용자 로그아웃 관리
- 복잡성과 디버깅

<br>

## 실습
- `SecurityConfig.java`
```java
package hello.jwtexam.config;

import hello.jwtexam.security.CustomUserDetailsService;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.*;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.web.cors.*;
import java.util.List;

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
                        .requestMatchers("/userregform", "/userreg", "/", "/login").permitAll()
                        .anyRequest().authenticated()
                )
                // 폼로그인 막기(JWT를 사용한 인증 방식만 허용)
                .formLogin(form -> form.disable()
                )
                // 세션 사용 못하게 막기 -> JWT 사용
                .sessionManagement(sessionManagement -> sessionManagement
                                .sessionCreationPolicy(SessionCreationPolicy.STATELESS)
                )
                // CSRF 공격은 주로 세션 기반 인증 시스템에서 문제가 되므로 비활성화
                .csrf(csrf -> csrf.disable())
                // HTTP 기본 인증을 사용하지 않고 JWT를 사용하므로 비활성화
                .httpBasic(httpBasic -> httpBasic.disable())
                // 다른 도메인에서 서버에 요청을 할 수 있도록 CORS 설정을 추가 -> api 테스트 가능
                .cors(cors -> cors.configurationSource(configurationSource()));
        return http.build();
    }

    public CorsConfigurationSource configurationSource(){
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration config = new CorsConfiguration();
        // 모든 도메인(*), 헤더(*), 메소드(*)에 대해 허용
        config.addAllowedOrigin("*");
        config.addAllowedHeader("*");
        config.addAllowedMethod("*");
        // 특히 GET, POST, DELETE 메소드를 허용하도록 명시
        config.setAllowedMethods(List.of("GET", "POST", "DELETE"));
        // 설정은 모든 경로(/**)에 적용
        source.registerCorsConfiguration("/**", config);
        return source;
    }
}
```

<br>

- `RefreshToken.java`
```java
package hello.jwtexam.domain;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "refresh_token")
@Getter @Setter
public class RefreshToken {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "user_id")
    private Long userId;
    
    private String value;
}
```

<br>

- `RefreshTokenRepository.java`
```java
package hello.jwtexam.repository;

import hello.jwtexam.domain.RefreshToken;
import org.springframework.data.jpa.repository.JpaRepository;
import java.util.*;

public interface RefreshTokenRepository extends JpaRepository<RefreshToken, Long> {
    Optional<RefreshToken> findByValue(String value);
}
```

<br>

- `RefreshTokenService.java`
```java
package hello.jwtexam.service;

import hello.jwtexam.domain.RefreshToken;
import hello.jwtexam.repository.RefreshTokenRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.Optional;

@Service
@RequiredArgsConstructor
public class RefreshTokenService {
    private final RefreshTokenRepository refreshTokenRepository;

    @Transactional
    public RefreshToken addRefreshToken(RefreshToken refreshToken) {
        return refreshTokenRepository.save(refreshToken);
    }

    @Transactional(readOnly = true)
    public Optional<RefreshToken> getRefreshToken(String refreshToken) {
        return refreshTokenRepository.findByValue(refreshToken);
    }

    public void deleteRefreshToken(String refreshToken) {
        refreshTokenRepository.findByValue(refreshToken).ifPresent(refreshTokenRepository::delete);
    }
}
```

<br>

- `UserLoginDto.java`
```java
package hello.jwtexam.security.dto;

import jakarta.validation.constraints.*;
import lombok.*;

@Getter
@Builder
@NoArgsConstructor
@AllArgsConstructor
// dto로 유효성 검사 !
public class UserLoginDto {
    @NotEmpty
    private String username;

    @NotEmpty
    // @Pattern(regexp=  "^(?=.[a-zA-Z])(?=.\d)(?=.*\W).{8,20}$")
    private String password;
}
```

<br>

- `UserLoginResponseDto.java`
```java
package hello.jwtexam.dto;

import lombok.*;

@Getter
@Builder
@NoArgsConstructor
@AllArgsConstructor
// 응답 객체 !
public class UserLoginResponseDto {
    private String accessToken;
    private String refreshToken;
    private Long userId;
    private String name;
}
```

<br>

- `UserApiController.java`
```java
package hello.jwtexam.controller;

import hello.jwtexam.domain.*;
import hello.jwtexam.dto.UserLoginResponseDto;
import hello.jwtexam.jwt.util.JwtTokenizer;
import hello.jwtexam.security.dto.UserLoginDto;
import hello.jwtexam.service.*;
import jakarta.servlet.http.*;
import jakarta.validation.Valid;
import lombok.RequiredArgsConstructor;
import org.springframework.http.*;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.*;
import java.util.List;
import java.util.stream.Collectors;

@RestController
@RequiredArgsConstructor
public class UserApiController {
    private final UserService userService;
    private final PasswordEncoder passwordEncoder;
    private final JwtTokenizer jwtTokenizer;
    private final RefreshTokenService refreshTokenService;

    @PostMapping("/login")
    // Dto의 유효성검사를 진행
    public ResponseEntity login(@RequestBody @Valid UserLoginDto userLoginDto,
                                BindingResult bindingResult, HttpServletResponse response) {
        // Dto의 유효성 검사에 오류가 있는 경우
        if(bindingResult.hasErrors()) {
            return new ResponseEntity(HttpStatus.BAD_REQUEST);
        }

        // Dto의 유효성 검사에 오류가 없는 경우 -> 사용자 이름으로 유저 조회
        User user = userService.findByUsername(userLoginDto.getUsername());

        // 조회한 유저의 비밀번호와 입력한 비밀번호 일치하지 않은 경우
        if(!passwordEncoder.matches(userLoginDto.getPassword(), user.getPassword())) {
            return new ResponseEntity("비밀번호가 올바르지 않습니다.", HttpStatus.UNAUTHORIZED);
        }

        // 여기까지 return 안 된 경우 db와 정보가 일치한 정보를 입력한 것 -> 인증 성공(토큰 발급 진행)
        // Roles 객체 꺼내 롤의 이름만 리스트로 얻어오기
        List<String> roles = user.getRoles().stream().map(Role::getName).collect(Collectors.toList());

        // 토큰 발급
        String accessToken = jwtTokenizer.createAccessToken(user.getId(), user.getEmail(), user.getName(), user.getUsername(), roles);
        String refreshToken = jwtTokenizer.createRefreshToken(user.getId(), user.getEmail(), user.getName(), user.getUsername(), roles);

        // Refresh Token을 DB에 저장
        RefreshToken refreshTokenEntity = new RefreshToken();
        refreshTokenEntity.setValue(refreshToken);
        refreshTokenEntity.setUserId(user.getId());

        refreshTokenService.addRefreshToken(refreshTokenEntity);

        // 응답으로 보낼 값(토큰을 쿠키로 설정)
        UserLoginResponseDto loginResponseDto = UserLoginResponseDto.builder()
                .accessToken(accessToken)
                .refreshToken(refreshToken)
                .userId(user.getId())
                .name(user.getName())
                .build();

        Cookie accessTokenCookie = new Cookie("accessToken", accessToken);
        accessTokenCookie.setHttpOnly(true);    // 쿠키를 HTTP 요청에서만 사용할 수 있게 하여 보안을 강화합니다.
        accessTokenCookie.setPath("/");         // 쿠키의 경로 설정
        accessTokenCookie.setMaxAge(Math.toIntExact(JwtTokenizer.ACCESS_TOKEN_EXPIRATION_TIME / 1000));        // 쿠키의 유지시간의 단위는 초, 토큰의 유지시간의 단위는 밀리초

        Cookie refreshTokenCookie = new Cookie("refreshToken", refreshToken);
        refreshTokenCookie.setHttpOnly(true);
        refreshTokenCookie.setPath("/");
        refreshTokenCookie.setMaxAge(Math.toIntExact(JwtTokenizer.REFRESH_TOKEN_EXPIRATION_TIME / 1000));      // 쿠키의 유지시간의 단위는 초, 토큰의 유지시간의 단위는 밀리초

				// 응답 객체에 쿠키를 추가
        response.addCookie(accessTokenCookie);
        response.addCookie(refreshTokenCookie);

				// 스프링 MVC는 HttpServletResponse 객체(쿠키)와 ResponseEntity 객체를 결합하여 최종 HTTP 응답을 생성하고 클라이언트에게 전송 
        return new ResponseEntity(loginResponseDto, HttpStatus.OK);
    }
}
```
