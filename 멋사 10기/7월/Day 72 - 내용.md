# JWT 인증/인가 필터

## 예외 상황 관련 Enum 클래스
- `JwtExceptionCode.java`
```java
package hello.jwtexam.jwt.exception;

import lombok.Getter;

public enum JwtExceptionCode {
    UNKNOWN_ERROR("UNKNOWN_ERROR", "UNKNOWN_ERROR"),
    NOT_FOUND_TOKEN("NOT_FOUND_TOKEN", "Headers에 토큰 형식의 값 찾을 수 없음"),
    INVALID_TOKEN("INVALID_TOKEN", "유효하지 않은 토큰"),
    EXPIRED_TOKEN("EXPIRED_TOKEN", "기간이 만료된 토큰"),
    UNSUPPORTED_TOKEN("UNSUPPORTED_TOKEN", "지원하지 않는 토큰");

    @Getter
    private String code;

    @Getter
    private String message;

    JwtExceptionCode(String code, String message) {
        this.code = code;
        this.message = message;
    }
}
```

<br>

## 커스텀 인증 토큰 클래스
- `JwtAuthenticationToken.java`
```java
package hello.jwtexam.jwt.token;

import org.springframework.security.authentication.AbstractAuthenticationToken;
import org.springframework.security.core.GrantedAuthority;
import java.util.Collection;

public class JwtAuthenticationToken extends AbstractAuthenticationToken {

    private String token;
    private Object principal;
    private Object credentials;

    // 인증이 완료된 경우
    public JwtAuthenticationToken(Collection<? extends GrantedAuthority> authorities,
                                  Object principal, Object credentials) {
        super(authorities);
        this.principal = principal;
        this.credentials = credentials;
        setAuthenticated(true);
    }

    // 인증이 완료되지 않은 경우
    public JwtAuthenticationToken(String token){
        super(null);
        this.token = token;
        this.setAuthenticated(false);
    }

    @Override
    public Object getCredentials() {
        return this.credentials;
    }

    @Override
    public Object getPrincipal() {
        return this.principal;
    }
}
```

<br>

## 사용자 인증 및 권한 관리를 위한 사용자 세부 정보 정의 클래스
- `CustomUserDetails.java`
```java
package hello.jwtexam.security;

import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.UserDetails;
import java.util.*;
import java.util.stream.Collectors;

public class CustomUserDetails implements UserDetails {
    private final String username;
    private final String password;
    private final String name;
    private final List<GrantedAuthority> authorities;

    public CustomUserDetails(String username, String password, String name, List<String> roles){
        this.username = username;
        this.password = password;
        this.name = name;
        this.authorities = roles.stream().map(SimpleGrantedAuthority::new).collect(Collectors.toList());
    }

    @Override
    public Collection<? extends GrantedAuthority> getAuthorities() {
        return authorities;
    }

    @Override
    public String getPassword() {
        return password;
    }

    @Override
    public String getUsername() {
        return username;
    }

    @Override
    public boolean isAccountNonExpired() {
        return true;
    }

    @Override
    public boolean isAccountNonLocked() {
        return true;
    }

    @Override
    public boolean isCredentialsNonExpired() {
        return true;
    }

    @Override
    public boolean isEnabled() {
        return true;
    }
}
```

<br>

## JWT를 이용한 인증 필터 구현 클래스
- `JwtAuthenticationFilter.java`
```java
package hello.jwtexam.jwt.filter;

import hello.jwtexam.jwt.exception.JwtExceptionCode;
import hello.jwtexam.jwt.token.JwtAuthenticationToken;
import hello.jwtexam.jwt.util.JwtTokenizer;
import hello.jwtexam.security.CustomUserDetails;
import io.jsonwebtoken.*;
import jakarta.servlet.*;
import jakarta.servlet.http.*;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.security.authentication.BadCredentialsException;
import org.springframework.security.core.*;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.util.StringUtils;
import org.springframework.web.filter.OncePerRequestFilter;
import java.io.IOException;
import java.util.*;
import java.util.stream.Collectors;

@RequiredArgsConstructor
@Slf4j
public class JwtAuthenticationFilter extends OncePerRequestFilter {
    private final JwtTokenizer jwtTokenizer;

    // 1. HTTP 요청에서 JWT 토큰 추출하기
    @Override
    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        String token = getToken(request); // accessToken 얻어냄.

        // 2. 추출된 JWT 토큰 검증 및 처리
        if(StringUtils.hasText(token)){
            try{
                // 3. 인증 객체 생성
                getAuthentication(token);
            }catch (ExpiredJwtException e){             // ExpiredJwtException: 토큰이 만료된 경우.
                request.setAttribute("exception", JwtExceptionCode.EXPIRED_TOKEN.getCode());
                log.error("Expired Token : {}",token,e);
                throw new BadCredentialsException("Expired token exception", e);
            }catch (UnsupportedJwtException e){         // UnsupportedJwtException: 지원하지 않는 형식의 토큰인 경우.
                request.setAttribute("exception", JwtExceptionCode.UNSUPPORTED_TOKEN.getCode());
                log.error("Unsupported Token: {}", token, e);
                throw new BadCredentialsException("Unsupported token exception", e);
            } catch (MalformedJwtException e) {         // MalformedJwtException: 잘못된 형식의 토큰인 경우.
                request.setAttribute("exception", JwtExceptionCode.INVALID_TOKEN.getCode());
                log.error("Invalid Token: {}", token, e);
                throw new BadCredentialsException("Invalid token exception", e);
            } catch (IllegalArgumentException e) {      // IllegalArgumentException: 토큰을 찾을 수 없는 경우.
                request.setAttribute("exception", JwtExceptionCode.NOT_FOUND_TOKEN.getCode());
                log.error("Token not found: {}", token, e);
                throw new BadCredentialsException("Token not found exception", e);
            } catch (Exception e) {                     // Exception: 기타 예외 발생 시 내부 오류로 처리합니다.
                log.error("JWT Filter - Internal Error: {}", token, e);
                throw new BadCredentialsException("JWT filter internal exception", e);
            }
        }
        // 처리된 요청의 흐름 제어(정상 처리의 경우)
        // 예외가 발생한 경우 위 catch문에서 예외 메시지 반환
        filterChain.doFilter(request, response);
    }

    // 1. HTTP 요청에서 JWT 토큰 추출하기
    private String getToken(HttpServletRequest request) {
        String authorization = request.getHeader("Authorization");
        if (StringUtils.hasText(authorization) && authorization.startsWith("Bearer ")) {
            return authorization.substring(7);
        }

        Cookie[] cookies = request.getCookies();
        if (cookies != null) {
            for (Cookie cookie : cookies) {
                if ("accessToken".equals(cookie.getName())) {
                    return cookie.getValue();
                }
            }
        }
        return null;
    }

    // 3. 인증 객체 생성
    private void getAuthentication(String token){
        Claims claims = jwtTokenizer.parseAccessToken(token);
        String email = claims.getSubject();
        Long userId = claims.get("userId", Long.class);
        String name = claims.get("name", String.class);
        String username = claims.get("username", String.class);
        List<GrantedAuthority> authorities = getGrantedAuthorities(claims);

        CustomUserDetails userDetails = new CustomUserDetails(username, "", name, authorities.stream().map(GrantedAuthority::getAuthority).collect(Collectors.toList()));

        Authentication authentication = new JwtAuthenticationToken(authorities, userDetails,null);
        SecurityContextHolder.getContext().setAuthentication(authentication);
    }


    private List<GrantedAuthority> getGrantedAuthorities(Claims claims){
        List<String> roles = (List<String>)claims.get("roles");     // 클레임에서 사용자 권한 목록을 추출

        // role들을 GrantedAuthority 객체로 변환
        List<GrantedAuthority> authorities = new ArrayList<>();
        for (String role : roles){
            authorities.add(() -> role);
        }

        // 변환된 GrantedAuthority 리스트 반환
        return authorities;
    }
}
```

<br>

## 인증되지 않은 사용자의 예외를 처리하는 클래스
- `CustomAuthenticationEntryPoint.java`
```java
package hello.jwtexam.jwt.exception;

import com.google.gson.Gson;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.*;
import lombok.extern.slf4j.Slf4j;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.web.AuthenticationEntryPoint;
import org.springframework.stereotype.Component;
import java.io.IOException;
import java.util.HashMap;

@Component
@Slf4j
public class CustomAuthenticationEntryPoint implements AuthenticationEntryPoint {
    // 인증되지 않은 사용자가 보호된 리소스에 접근하려 할 때 동작
    @Override
    public void commence(HttpServletRequest request, HttpServletResponse response, AuthenticationException authException) throws IOException, ServletException {
        String exception = (String)request.getAttribute("exception");

        //RESTful 요청인지, 페이지 요청인지 구분
        if(isRestRequest(request)){
            handleRestResponse(request,response,exception);      // REST 요청 처리
        }else{
            handlePageResponse(request,response,exception);      // 페이지 요청 처리
        }
    }

    // 요청이 RESTful 요청인지 판별
    private boolean isRestRequest(HttpServletRequest request) {
        // XMLHttpRequest 헤더 확인 또는 URI가 "/api/"로 시작하는지 확인
        String requestedWithHeader = request.getHeader("X-Requested-With");
        return "XMLHttpRequest".equals(requestedWithHeader) || request.getRequestURI().startsWith("/api/");
    }

    // HTTP 응답을 설정하는 메소드
    // 오류 코드와 메시지를 포함한 JSON 형식의 응답 생성 및 반환
    private void setResponse(HttpServletResponse response, JwtExceptionCode exceptionCode) throws IOException {
        response.setContentType("application/json;charset=UTF-8");
        response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);

        HashMap<String, Object> errorInfo = new HashMap<>();
        errorInfo.put("message", exceptionCode.getMessage());
        errorInfo.put("code", exceptionCode.getCode());
        Gson gson = new Gson();
        String responseJson = gson.toJson(errorInfo);
        response.getWriter().print(responseJson);
    }

    // REST 요청을 처리하는 메소드
    // 발생한 예외에 따라 적절한 오류 메시지를 JSON 형식으로 반환
    private void handleRestResponse(HttpServletRequest request, HttpServletResponse response, String exception) throws IOException {
        log.error("Rest Request - Commence Get Exception : {}", exception);

        if (exception != null) {
            if (exception.equals(JwtExceptionCode.INVALID_TOKEN.getCode())) {
                log.error("entry point >> invalid token");
                setResponse(response, JwtExceptionCode.INVALID_TOKEN);
            } else if (exception.equals(JwtExceptionCode.EXPIRED_TOKEN.getCode())) {
                log.error("entry point >> expired token");
                setResponse(response, JwtExceptionCode.EXPIRED_TOKEN);
            } else if (exception.equals(JwtExceptionCode.UNSUPPORTED_TOKEN.getCode())) {
                log.error("entry point >> unsupported token");
                setResponse(response, JwtExceptionCode.UNSUPPORTED_TOKEN);
            } else if (exception.equals(JwtExceptionCode.NOT_FOUND_TOKEN.getCode())) {
                log.error("entry point >> not found token");
                setResponse(response, JwtExceptionCode.NOT_FOUND_TOKEN);
            } else {
                setResponse(response, JwtExceptionCode.UNKNOWN_ERROR);
            }
        } else {
            setResponse(response, JwtExceptionCode.UNKNOWN_ERROR);
        }
    }

    // 페이지 요청을 처리하는 메소드
    // 인증되지 않은 사용자라면 "/loginform"으로 리디렉션
    private void handlePageResponse(HttpServletRequest request, HttpServletResponse response, String exception) throws IOException {
        log.error("Page Request - Commence Get Exception : {}", exception);

        if (exception != null) {
            // 추가적인 페이지 요청 예외 처리 로직 추가 가능
        }

        response.sendRedirect("/loginform");
    }
}
```

<br>

## Spring Security 설정
- `SecurityConfig.java`
```java
package hello.jwtexam.config;

import hello.jwtexam.jwt.exception.CustomAuthenticationEntryPoint;
import hello.jwtexam.jwt.filter.JwtAuthenticationFilter;
import hello.jwtexam.jwt.util.JwtTokenizer;
import hello.jwtexam.security.CustomUserDetailsService;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.*;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.http.SessionCreationPolicy;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter;
import org.springframework.web.cors.*;
import java.util.List;

@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {

    private final CustomUserDetailsService customUserDetailsService;
    private final JwtTokenizer jwtTokenizer;
    private final CustomAuthenticationEntryPoint customAuthenticationEntryPoint;

    // Spring Security 필터 체인을 정의하는 메소드
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                // 요청 권한 설정
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/userregform","/userreg","/","/login","/refreshToken","/loginform").permitAll() // 특정 경로에 대한 접근 허용
                        .anyRequest().authenticated() // 그 외의 모든 요청은 인증 필요
                )
                // 커스텀 JWT 인증 필터 추가
                .addFilterBefore(new JwtAuthenticationFilter(jwtTokenizer), UsernamePasswordAuthenticationFilter.class)
                // 기본 폼 로그인 비활성화
                .formLogin(form -> form.disable())
                // 세션 정책을 STATELESS로 설정 (JWT 인증 사용 시 필요)
                .sessionManagement(sessionManagement -> sessionManagement
                        .sessionCreationPolicy(SessionCreationPolicy.STATELESS))
                // CSRF 보호 비활성화
                .csrf(csrf -> csrf.disable())
                // HTTP Basic 인증 비활성화
                .httpBasic(httpBasic -> httpBasic.disable())
                // CORS 설정 적용
                .cors(cors -> cors.configurationSource(configurationSource()))
                // 커스텀 인증 엔트리 포인트 설정 (인증 예외 처리)
                .exceptionHandling(exception -> exception
                        .authenticationEntryPoint(customAuthenticationEntryPoint));

        return http.build();
    }

    // CORS 설정을 정의하는 메소드
    public CorsConfigurationSource configurationSource() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration config = new CorsConfiguration();
        config.addAllowedOrigin("*"); // 모든 도메인 허용
        config.addAllowedHeader("*"); // 모든 헤더 허용
        config.addAllowedMethod("*"); // 모든 HTTP 메소드 허용
        config.setAllowedMethods(List.of("GET","POST","DELETE")); // GET, POST, DELETE 메소드 허용
        source.registerCorsConfiguration("/**", config); // 모든 경로에 대해 CORS 설정 적용
        return source;
    }

    // 비밀번호 인코더 빈 정의 (BCryptPasswordEncoder 사용)
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

<br>

## HTTP 테스트
```http
GET http://localhost:8080/authtest
Cookie: accessToken=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJoYW1Ac2FuZy5qaW4iLCJ1c2VybmFtZSI6InNhbmdqaW4iLCJuYW1lIjoi7ZWo7IOB7KeEIiwidXNlcklkIjoxLCJyb2xlcyI6WyJST0xFX1VTRVIiXSwiZXhwaXJlIjoxODAwMDAwLCJzZWNyZXRLZXkiOiJNVEl6TkRVMk56ZzVNREV5TXpRMU5qYzRPVEF4TWpNME5UWTNPRGt3TVRJPSIsImlhdCI6MTcxOTg4ODY0NywiZXhwIjoxNzE5ODkwNDQ3fQ.NJrTld0B8F96UvslB-EJcdLUdf4qRSKryNBch6v8LtQ
```

<img width="647" alt="스크린샷 2024-07-02 20 20 06" src="https://github.com/hamsangjin/TIL/assets/103736614/0a8b7f03-4c16-406d-bae5-635330d4c8ce">

<br>

---

<br>

# 리플래시토큰으로 엑세스 토큰 발급받기
## 리프레시 토큰을 통한 액세스 토큰 갱신 - 서비스 및 컨트롤러
- `UserService.java`
```java
package hello.jwtexam.service;

import hello.jwtexam.domain.Role;
import hello.jwtexam.domain.User;
import hello.jwtexam.repository.RoleRepository;
import hello.jwtexam.repository.UserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.Collections;
import java.util.Optional;

@Service
@RequiredArgsConstructor
public class UserService {
    private final UserRepository userRepository;
    private final RoleRepository roleRepository;
    private final PasswordEncoder passwordEncoder;

    ...

    // Id를 통한 유저 조회
    @Transactional(readOnly = true)
    public Optional<User> getUser(Long id) {
        return userRepository.findById(id);
    }
}
```

<br>

- `UserApiController.java`
```java
package hello.jwtexam.controller;

import hello.jwtexam.dto.UserLoginResponseDto;
import hello.jwtexam.jwt.util.JwtTokenizer;
import hello.jwtexam.security.dto.UserLoginDto;
import hello.jwtexam.service.*;
import hello.jwtexam.domain.*;
import io.jsonwebtoken.Claims;
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

    ...

    // 리프레시 토큰을 통한 액세스 토큰 갱신 엔드포인트
    @PostMapping("/refreshToken")
    public ResponseEntity refreshToken(HttpServletRequest request, HttpServletResponse response){
        String refreshToken = null;

        // 쿠키에서 리프레시 토큰 추출
        Cookie[] cookies = request.getCookies();
        if(cookies != null){
            for(Cookie cookie : cookies){
                if("refreshToken".equals(cookie.getName())){
                    refreshToken = cookie.getValue();
                    break;
                }
            }
        }

        // 리프레시 토큰이 없는 경우
        if(refreshToken == null)        return new ResponseEntity(HttpStatus.BAD_REQUEST);

        // 리프레시 토큰을 파싱해 클레임 추출
        Claims claims = jwtTokenizer.parseRefreshToken(refreshToken);
        Long userId = Long.valueOf ((Integer)claims.get("userId"));

        // 사용자 정보 조회
        User user = userService.getUser(userId).orElseThrow(() -> new IllegalArgumentException("사용자를 찾지 못했습니다."));

        // 클레임에서 역할 목록 추출
        List roles = (List)claims.get("roles");

        // 새로운 액세스 토큰 생성
        String accessToken = jwtTokenizer.createAccessToken(userId, user.getEmail(),
                user.getName(), user.getUsername(), roles);

        // 새로운 액세스 토큰을 쿠키에 설정
        Cookie accessTokenCookie = new Cookie("accessToken", accessToken);
        accessTokenCookie.setHttpOnly(true);
        accessTokenCookie.setPath("/");
        accessTokenCookie.setMaxAge(Math.toIntExact( jwtTokenizer.ACCESS_TOKEN_EXPIRATION_TIME / 1000));

        response.addCookie(accessTokenCookie);

        // 응답 DTO 생성
        UserLoginResponseDto responseDto = UserLoginResponseDto.builder()
                .accessToken(accessToken)
                .refreshToken(refreshToken)
                .name(user.getName())
                .userId(user.getId())
                .build();

        // 응답 반환
        return new ResponseEntity(responseDto, HttpStatus.OK);
    }
}
```

<br>

## HTTP 테스트
```http
POST http://localhost:8080/refreshToken
Cookie: refreshToken=eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJoYW1Ac2FuZy5qaW4iLCJ1c2VybmFtZSI6InNhbmdqaW4iLCJuYW1lIjoi7ZWo7IOB7KeEIiwidXNlcklkIjoxLCJyb2xlcyI6WyJST0xFX1VTRVIiXSwiZXhwaXJlIjo2MDQ4MDAwMDAsInNlY3JldEtleSI6Ik1USXpORFUyTnpnNU1ERXlNelExTmpjNE9UQXhNak0wTlRZM09Ea3dNVEk9IiwiaWF0IjoxNzE5OTIwNTc3LCJleHAiOjE3MjA1MjUzNzd9.UJKXZ4Z8yHXw-1LUH6ckAA4756OgTr8rCofbkhRHQFM
```

![](https://github.com/hamsangjin/TIL/assets/103736614/b8870595-6877-4b56-a907-acc2e4a92c6f)


<br>

---

<br>

# OAuth2
OAuth2 로그인은 사용자가 신뢰할 수 있는 외부 인증 제공자를 통해 애플리케이션에 로그인할 수 있도록 하는 인증 메커니즘이다.

## 구성
1. `Resource Owner(리소스 소유자)`
    - 설명: 사용자를 의미하며, 자신의 리소스에 접근할 권한을 가지고 있는 주체다.
    - 역할: 애플리케이션에 로그인하고 자신의 리소스에 접근하는 역할을 한다.

2. `Client(클라이언트)`
    - 설명: 사용자가 접근하려는 애플리케이을 의미하며, OAuth2 프로토콜에서 리소스 소유자의 권한을 요청하는 주체다.
    - 역할: 리소스 소유자의 동의를 얻어 OAuth2 제공자로부터 액세스 토큰을 받고, 이를 사용해 리소스 서버에 요청을 보낸다.

3. `Authorization Server(인증 서버)`
    - 설명: OAuth2 제공자를 의미하며, 사용자를 인증하고 권한을 부여하며 액세스 토큰을 발급하는 서버다.
    - 역할: 리소스 소유자를 인증하고, 클라이언트에게 액세스 토큰을 발급한다.

4. `Resource Server(리소스 서버)`
    - 설명: 보호된 리소스를 호스팅하는 서버를 의미하며, 액세스 토큰을 사용해 보호된 리소스에 대한 요청을 처리한다.
    - 역할: 액세스 토큰을 검증하고, 클라이언트가 요청하는 보호된 리소스에 대한 접근을 허용한다.

5. `Access Token(액세스 토큰)`
    - 설명: 인증 서버가 클라이언트에게 발급하는 토큰으로, 리소스 서버에 접근할 때 사용된다.
    - 역할: 리소스 서버에 보호된 리소스를 요청할 때 클라이언트가 사용하는 자격 증명의 역할을 한다.

6. `Refresh Token(리프레시 토큰)`
    - 설명: 액세스 토큰이 만료되었을 때, 새로운 액세스 토큰을 얻기 위해 사용하는 토큰이다.
    - 역할: 사용자가 다시 인증하지 않고 새로운 액세스 토큰을 얻을 수 있도록 한다.

7. `Authorization Grant(권한 부여)`
    - 설명: 클라이언트가 액세스 토큰을 얻기 위해 인증 서버에 제출하는 자격 증명을 말한다.
    - 유형:
      - `Authorization Code(권한 코드)`: 사용자가 클라이언트를 통해 인증 서버에 로그인하여 얻은 일회성 코드
      - `Implicit(암시적)`: 클라이언트가 직접 액세스 토큰을 얻는 방식
      - `Resource Owner Password Credentials(리소스 소유자 비밀번호 자격 증명)`: 사용자가 자신의 자격 증명(아이디, 비밀번호)을 클라이언트에 제공하는 방식
      - `Client Credentials(클라이언트 자격 증명)`: 클라이언트 자신이 리소스에 접근하는 방식

<br>

## 로그인 흐름
1. `사용자 인증 요청`: 클라이언트는 사용자를 인증하기 위해 인증 서버로 리다이렉션한다.
2. `사용자 인증`: 사용자는 인증 서버에서 로그인하고, 인증 서버는 권한 코드를 클라이언트에게 리다이렉션한다.
3. `권한 코드 교환`: 클라이언트는 권한 코드를 액세스 토큰으로 교환하기 위해 인증 서버에 요청을 보낸다.
4. `액세스 토큰 수신`: 인증 서버는 클라이언트에게 액세스 토큰(필요 시 리프레시 토큰)을 반환한다.
5. `리소스 요청`: 클라이언트는 액세스 토큰을 사용해 리소스 서버에 보호된 리소스를 요청한다.
6. `리소스 접근 허용`: 리소스 서버는 액세스 토큰을 검증하고, 유효하면 보호된 리소스를 클라이언트에게 반환한다.

<br>

![](https://github.com/hamsangjin/TIL/assets/103736614/67ea15ad-201b-4aa0-9b15-1d7e74c573b9)

1. 클라이언트 애플리케이션 요청(1단계)
    - 사용자가 클라이언트 애플리케이션에서 `Sign up` 또는 `Log in with GitHub` 버튼을 클릭한다.
    - 클라이언트 애플리케이션은 `GitHub OAuth2 인증 서버`로 인증 요청을 보내며, 이 요청에는 클라이언트 ID와 리디렉션 URI가 포함된다.

2. 인증 서버로 리디렉션(2단계)
    - 사용자는 GitHub 인증 서버로 리디렉션된다.
    - 사용자는 GitHub 로그인 페이지에서 자신의 GitHub 자격 증명(아이디, 비밀번호)을 입력하고 로그인한다.

3. 권한 부여 요청 (3단계)
    - 사용자가 로그인에 성공하면 GitHub 인증 서버는 사용자의 권한을 요청하며, 사용자는 애플리케이션이 자신의 GitHub 계정 정보에 접근하도록 허용할지 여부를 확인한다.
    - 사용자가 권한을 허용하면 인증 서버는 클라이언트 애플리케이션에 권한 코드를 반환한다.

4. 액세스 토큰 요청 (4단계)
    - 클라이언트 애플리케이션은 GitHub 인증 서버에 권한 코드를 액세스 토큰으로 교환하기 위한 요청을 보내며, 이 요청에는 클라이언트 ID, 클라이언트 시크릿, 권한 코드, 리디렉션 URI가 포함된다.
    - 인증 서버는 클라이언트 애플리케이션에 액세스 토큰을 반환한다.

5. 리소스 서버에 요청 (5단계)
    - 클라이언트 애플리케이션은 획득한 액세스 토큰을 사용하여 GitHub 리소스 서버에 보호된 리소스를 요청한다.
    - 리소스 서버는 액세스 토큰을 검증하고, 유효한 토큰일 경우 요청된 리소스를 클라이언트 애플리케이션에 반환한다.

<br>

---

<br>

# OAuth2 실습

## Github App 생성
- `Settings` > `Developer settings`

![](https://github.com/hamsangjin/TIL/assets/103736614/985bfd43-d9f8-486a-8e5e-16de2a8ef51e) | ![](https://github.com/hamsangjin/TIL/assets/103736614/6cdb967a-3faa-4b4a-b401-d79825e7c408)
-- | -- |

- `New Github App` > `입력 후 Create Github App(Webhook 해제)` > `Generate a new client secret` > `Client ID, Secret 복사`

![](https://github.com/hamsangjin/TIL/assets/103736614/e3e1fe80-fb2e-4314-b279-d58ff011401f) | ![](https://github.com/hamsangjin/TIL/assets/103736614/2ed4c00d-7deb-4bd6-b368-9513314271ee) | ![](https://github.com/hamsangjin/TIL/assets/103736614/90a738a5-4178-401b-bfa0-c11e556293bb)
-- | -- | -- |

<br>

## 테이블 생성 쿼리
```sql
CREATE TABLE social_user (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    social_id VARCHAR(255) NOT NULL,
    provider VARCHAR(255) NOT NULL,
    username VARCHAR(255),
    email VARCHAR(255),
    avatar_url VARCHAR(255)
);

CREATE TABLE social_login_info (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    provider VARCHAR(255) NOT NULL,
    social_Id VARCHAR(255) NOT NULL,
    created_At TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    uuid VARCHAR(255) NOT NULL
);

ALTER TABLE users
ADD COLUMN social_id VARCHAR(255),
ADD COLUMN provider VARCHAR(50);

```

<br>

## Entity
- `SocialLoginInfo.java`
```java
package hello.oauthexam.domain;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDateTime;
import java.util.UUID;

@Entity
@Table(name = "social_login_info")
@Getter @Setter
public class SocialLoginInfo {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String provider;
    private String socialId;
    private LocalDateTime createdAt;
    private String uuid;

    public SocialLoginInfo(){
        //소셜 로그인한 시간, uuid 생성
        //소셜 로그인 이후의 특정한 시간까지만 추가작업을 할 수 있도록
        this.createdAt=LocalDateTime.now();
        this.uuid= UUID.randomUUID().toString();
    }
}
```

<br>

- `SocialUser.java`
```java
package hello.oauthexam.domain;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Data
public class SocialUser {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String provider;
    private String socialId;
    private String username;
    private String email;
    private String avatarUrl;
}
```

<br>

- `User.java`
```java
package hello.oauthexam.domain;

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

    @Column(name = "social_id")
    private String socialId;

    private String provider;

    @ManyToMany(fetch = FetchType.EAGER)
    @JoinTable(
            name = "user_roles",
            joinColumns = @JoinColumn(name = "user_id"),
            inverseJoinColumns = @JoinColumn(name = "role_id")
    )
    private Set<Role> roles;
}
```

<br>

## Repository
- `SocialLoginInfoRepository.java`
```java
package hello.oauthexam.repository;

import hello.oauthexam.domain.SocialLoginInfo;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository
public interface SocialLoginInfoRepository extends JpaRepository<SocialLoginInfo, Long> {
    Optional<SocialLoginInfo> findByProviderAndUuidAndSocialId(String provider, String uuid, String socialId);
}
```

<br>

- `SocialUserRepository.java`
```java
package hello.oauthexam.repository;

import hello.oauthexam.domain.SocialUser;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository
public interface SocialUserRepository extends JpaRepository<SocialUser, Long> {
    Optional<SocialUser> findBySocialIdAndProvider(String socialId, String provider);
}
```

<br>

- `UserRepository.java`
```java
package hello.oauthexam.repository;

import hello.oauthexam.domain.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
    Optional<User> findByProviderAndSocialId(String provider, String socialId);
}
```

<br>

## Service
- `SocialLoginInfoService.java`
```java
package hello.oauthexam.service;

import hello.oauthexam.domain.SocialLoginInfo;
import hello.oauthexam.repository.SocialLoginInfoRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.Optional;

@Service
@RequiredArgsConstructor
public class SocialLoginInfoService {
    private final SocialLoginInfoRepository socialLoginInfoRepository;

    /**
     * SocialLoginInfo를 저장하는 메소드
     * @param provider 제공자 (구글, 페이스북 등)
     * @param socialId 소셜 ID
     * @return 저장된 SocialLoginInfo 엔티티
     */
    @Transactional
    public SocialLoginInfo saveSocialLoginInfo(String provider, String socialId) {
        SocialLoginInfo socialLoginInfo = new SocialLoginInfo();
        socialLoginInfo.setProvider(provider);
        socialLoginInfo.setSocialId(socialId);
        return socialLoginInfoRepository.save(socialLoginInfo);
    }

    /**
     * provider, uuid, socialId를 기준으로 SocialLoginInfo를 조회하는 메소드
     * @param provider 제공자 (구글, 페이스북 등)
     * @param uuid 사용자 UUID
     * @param socialId 소셜 ID
     * @return 조회된 SocialLoginInfo 엔티티(Optional)
     */
    @Transactional(readOnly = true)
    public Optional<SocialLoginInfo> findByProviderAndUuidAndSocialId(String provider, String uuid, String socialId) {
        return socialLoginInfoRepository.findByProviderAndUuidAndSocialId(provider, uuid, socialId);
    }
}
```

<br>

- `SocialUserService.java`
```java
package hello.oauthexam.service;

import hello.oauthexam.domain.SocialUser;
import hello.oauthexam.repository.SocialUserRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import java.util.Optional;

@Service
@RequiredArgsConstructor
public class SocialUserService {
    private final SocialUserRepository socialUserRepository;

    /**
     * SocialUser를 저장 또는 업데이트하는 메소드
     * @param socialId 소셜 ID
     * @param provider 제공자 (구글, 페이스북 등)
     * @param username 사용자 이름
     * @param email 사용자 이메일
     * @param avatarUrl 사용자 프로필 사진 URL
     * @return 저장 또는 업데이트된 SocialUser 엔티티
     */
    @Transactional
    public SocialUser saveOrUpdateUser(String socialId, String provider, String username, String email, String avatarUrl) {
        Optional<SocialUser> existingUser = socialUserRepository.findBySocialIdAndProvider(socialId, provider);
        SocialUser socialUser;
        if (existingUser.isPresent()) {
            socialUser = existingUser.get();
            socialUser.setUsername(username);
            socialUser.setEmail(email);
            socialUser.setAvatarUrl(avatarUrl);
        } else {
            socialUser = new SocialUser();
            socialUser.setSocialId(socialId);
            socialUser.setUsername(username);
            socialUser.setEmail(email);
            socialUser.setAvatarUrl(avatarUrl);
            socialUser.setProvider(provider);
        }
        return socialUserRepository.save(socialUser);
    }
}
```

<br>

## Config
- `SecurityConfig.java`
```java
package hello.oauthexam.config;

import hello.oauthexam.security.CustomOAuth2AuthenticationSuccessHandler;
import hello.oauthexam.service.SocialUserService;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.*;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.oauth2.client.userinfo.DefaultOAuth2UserService;
import org.springframework.security.oauth2.client.userinfo.OAuth2UserService;
import org.springframework.security.oauth2.client.userinfo.OAuth2UserRequest;
import org.springframework.security.oauth2.core.user.OAuth2User;
import org.springframework.security.web.SecurityFilterChain;
import org.springframework.web.cors.*;

import java.util.List;

@Configuration
@EnableWebSecurity
@RequiredArgsConstructor
public class SecurityConfig {
    private final CustomOAuth2AuthenticationSuccessHandler customOAuth2AuthenticationSuccessHandler;
    private final SocialUserService socialUserService;

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
                // 요청 권한 설정
                .authorizeHttpRequests(authorize -> authorize
                        .requestMatchers("/userregform", "/userreg", "/", "/loginform", "/").permitAll()                                // 특정 경로에 대한 접근 허용
                        .requestMatchers("/oauth2/**", "/login/oauth2/code/github", "/registerSocialUser", "/saveSocialUser").permitAll() // OAuth2 관련 경로에 대한 접근 허용
                        .anyRequest().authenticated() // 그 외의 모든 요청은 인증 필요
                )
                // CSRF 보호 비활성화
                .csrf(csrf -> csrf.disable())
                // 기본 폼 로그인 비활성화
                .formLogin(form -> form.disable())
                // CORS 설정 적용
                .cors(cors -> cors.configurationSource(configurationSource()))
                // HTTP Basic 인증 비활성화
                .httpBasic(httpBasic -> httpBasic.disable())
                // OAuth2 로그인 설정
                .oauth2Login(oauth2 -> oauth2
                        .loginPage("/loginform") // 로그인 페이지 설정
                        .failureUrl("/loginFailure") // 로그인 실패 시 처리할 URL 설정
                        .userInfoEndpoint(userInfo -> userInfo
                                .userService(this.oauth2UserService()) // OAuth2 유저 정보 엔드포인트 설정
                        )
                        .successHandler(customOAuth2AuthenticationSuccessHandler) // 성공적으로 로그인한 경우 처리할 핸들러 설정
                );
        return http.build();
    }

    // OAuth2 사용자 정보 서비스 빈 설정
    @Bean
    public OAuth2UserService<OAuth2UserRequest, OAuth2User> oauth2UserService() {
        DefaultOAuth2UserService delegate = new DefaultOAuth2UserService();
        return oauth2UserRequest -> {
            OAuth2User oauth2User = delegate.loadUser(oauth2UserRequest);

            // OAuth2User에서 필요한 정보 추출 및 처리
            String token = oauth2UserRequest.getAccessToken().getTokenValue();
            String provider = oauth2UserRequest.getClientRegistration().getRegistrationId();
            String socialId = String.valueOf(oauth2User.getAttributes().get("id"));
            String username = (String) oauth2User.getAttributes().get("login");
            String email = (String) oauth2User.getAttributes().get("email");
            String avatarUrl = (String) oauth2User.getAttributes().get("avatar_url");

            // SocialUserService를 사용하여 사용자 정보 저장 또는 업데이트
            socialUserService.saveOrUpdateUser(socialId, provider, username, email, avatarUrl);

            return oauth2User;
        };
    }

    // CORS 설정을 정의하는 메소드
    public CorsConfigurationSource configurationSource() {
        UrlBasedCorsConfigurationSource source = new UrlBasedCorsConfigurationSource();
        CorsConfiguration config = new CorsConfiguration();
        config.addAllowedOrigin("*"); // 모든 도메인 허용
        config.addAllowedHeader("*"); // 모든 헤더 허용
        config.addAllowedMethod("*"); // 모든 HTTP 메소드 허용
        config.setAllowedMethods(List.of("GET", "POST", "DELETE")); // GET, POST, DELETE 메소드 허용
        source.registerCorsConfiguration("/**", config); // 모든 경로에 대해 CORS 설정 적용
        return source;
    }

    // 비밀번호 인코더 빈 설정 (BCryptPasswordEncoder 사용)
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

<br>

## Security
- `CustomOAuth2AuthenticationSuccessHandler.java`
```java
package hello.oauthexam.security;

import hello.oauthexam.domain.SocialLoginInfo;
import hello.oauthexam.service.*;
import hello.oauthexam.domain.*;
import jakarta.servlet.ServletException;
import jakarta.servlet.http.*;
import lombok.RequiredArgsConstructor;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.oauth2.core.user.DefaultOAuth2User;
import org.springframework.security.web.authentication.AuthenticationSuccessHandler;
import org.springframework.stereotype.Component;
import java.io.IOException;
import java.util.Optional;
import java.util.stream.Collectors;

@RequiredArgsConstructor
@Component
public class CustomOAuth2AuthenticationSuccessHandler implements AuthenticationSuccessHandler {
    private final SocialLoginInfoService socialLoginInfoService;
    private final UserService userService;

    @Override
    public void onAuthenticationSuccess(HttpServletRequest request, HttpServletResponse response, Authentication authentication) throws IOException, ServletException {
        // 요청 경로에서 provider 추출
        String requestUri = request.getRequestURI();
        String provider = extractProviderFromUri(requestUri);

        // provider가 없는 경우 처리
        if(provider == null){
            response.sendRedirect("/");
            return;
        }

        // 인증 객체에서 OAuth2User 정보 추출
        Authentication auth = SecurityContextHolder.getContext().getAuthentication();
        DefaultOAuth2User defaultOAuth2User = (DefaultOAuth2User) auth.getPrincipal();

        // OAuth2User에서 소셜 ID와 이름 추출
        int socialId = (int)defaultOAuth2User.getAttributes().get("id");
        String name = (String)defaultOAuth2User.getAttributes().get("name");

        // provider와 socialId로 사용자 조회
        Optional<User> userOptional = userService.findByProviderAndSocialId(provider, String.valueOf(socialId));

        if (userOptional.isPresent()) {
            // 이미 등록된 회원이면 로그인 처리
            User user = userOptional.get();

            // CustomUserDetails 생성
            CustomUserDetails customUserDetails = new CustomUserDetails(user.getUsername(), user.getPassword(), user.getName(), user.getRoles().stream().map(Role::getName).collect(Collectors.toList()));

            // Authentication 객체 생성 및 SecurityContext에 설정
            Authentication newAuth = new UsernamePasswordAuthenticationToken(customUserDetails, null, customUserDetails.getAuthorities());
            SecurityContextHolder.getContext().setAuthentication(newAuth);

            // 홈 화면으로 리다이렉트
            response.sendRedirect("/welcome");

        } else { // 소셜로 회원가입이 되어있지 않은 경우
            // 소셜 로그인 정보 저장
            SocialLoginInfo socialLoginInfo = socialLoginInfoService.saveSocialLoginInfo(provider, String.valueOf(socialId));

            // 회원 가입 페이지로 리다이렉트
            response.sendRedirect("/registerSocialUser?provider=" + provider + "&socialId=" + socialId + "&name=" + name + "&uuid=" + socialLoginInfo.getUuid());
        }
    }

    // URI에서 provider 추출하는 메소드
    private String extractProviderFromUri(String uri) {
        if(uri == null || uri.isBlank()) {
            return null;
        }

        if(!uri.startsWith("/login/oauth2/code/")){
            return null;
        }

        // 예: /login/oauth2/code/github -> github
        String[] segments = uri.split("/");
        return segments[segments.length - 1];
    }
}
```

<br>

## Controller
- `UserController.java`
```java
package hello.jwtexam.controller;

import hello.jwtexam.domain.User;
import hello.jwtexam.service.UserService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PostMapping;

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

<br>

## yaml 파일 수정
- `application.yml`
```yaml
spring:
  application:
    name: oauthexam

  security:
    oauth2:
      client:
        registration:
          github:
            client-id: 비밀
            client-secret: 비밀
            scope:
              - email
              - profile
            redirect-uri: "{baseUrl}/login/oauth2/code/{registrationId}"
            client-name: GitHub
        provider:
          github:
            authorization-uri: https://github.com/login/oauth/authorize
            token-uri: https://github.com/login/oauth/access_token
            user-info-uri: https://api.github.com/user
            user-name-attribute: id

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

jwt:
  secretKey: 12345678901234567890123456789012
  refreshKey: 12345678901234567890123456789012
```

<br>

## View
- `loginform.html`
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>login</title>
    </head>
    <body>
        <h2>라이언 로그인</h2>
        <form action="/login" method="post">
            <label for="username">아이디 : </label>
            <input type="text" id="username" name="username" required><br><br>
        
            <label for="password">비밀 번호 : </label>
            <input type="text" id="password" name="password" required><br><br>
        
            <button type="submit">로그인</button>
        
            <a th:href="@{/oauth2/authorization/github}" class="btn btn-primary">Login with GitHub</a>
        </form>
    </body>
</html>
```

<br>

- `registerSocialUser.java`
```html
<!DOCTYPE html>
<html lang="ko" xmlns:th="http://www.thymeleaf.org">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>회원 정보 등록</title>
    </head>
    <body>
        <div class="container">
            <h1>환영합니다!</h1>
            <p>기본 회원 정보를 등록해주세요.</p>
            <form th:action="@{/saveSocialUser}" method="post">
                <label for="name">이름</label>
                <input type="text" id="name" name="name" th:value="${name}"> <br>
        
                <label for="username">사용자 ID</label>
                <input type="text" id="username" name="username"> <br>
        
                <label for="email">email</label>
                <input type="text" id="email" name="email">  <br>
        
                <input type="hidden" name="socialId" th:value="${socialId}">
                <input type="hidden" name="provider" th:value="${provider}">
                <input type="hidden" name="uuid" th:value="${uuid}">
                <div>
                    <input type="checkbox" id="terms" name="terms" required>
                    <label for="terms">이용약관과 개인정보취급방침에 동의합니다.</label>
                </div>
        
                <div class="buttons">
                    <button type="button" onclick="window.location.href='/'">취소</button>
                    <button type="submit">가입</button>
                </div>
            </form>
        </div>
    </body>
</html>
```

<br>

## 실행화면
![](https://github.com/hamsangjin/TIL/assets/103736614/ae207f36-6a0f-4e19-920b-1c45b4244f27) | ![](https://github.com/hamsangjin/TIL/assets/103736614/293345b3-dd0d-4b34-9dc5-999292d4b49c)
-- | -- |

![](https://github.com/hamsangjin/TIL/assets/103736614/61881aea-9fae-4e27-8567-8df022aa7e64) | ![](https://github.com/hamsangjin/TIL/assets/103736614/fd23fda8-0fa9-4271-aa28-14c08d87bfab) | ![](https://github.com/hamsangjin/TIL/assets/103736614/062f7318-7591-48af-936b-43afbffd1f89)
-- | -- | -- |
