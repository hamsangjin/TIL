# Filter와 ThreadLocal 이해
![](https://github.com/hamsangjin/TIL/assets/103736614/73d0c0d9-dd4a-4dbb-87e5-f60dd3cade7f)

Spring Boot Web Application에서는 하나의 요청을 하나의 쓰레드가 처리한다.

## 쓰레드 모델
1. `요청 처리`: Spring Boot는 기본적으로 하나의 요청을 하나의 쓰레드가 처리하는 구조이며, 클라이언트로부터 HTTP 요청이 들어오면, 서블릿 컨테이너는 쓰레드 풀에서 쓰레드를 할당하여 요청을 처리한다.
2. `비동기 처리`: Spring Boot는 비동기 요청 처리를 지원하며, 비동기 처리는 별도의 비동기 작업 쓰레드 풀을 사용한다.

<br>

## 기본 쓰레드 수
- `maxThreads`: 최대 쓰레드 수는 200이며, 이는 동시 요청을 처리할 수 있는 최대 쓰레드 수를 의미한다.
- `minSpareThreads`: 최소 여유 쓰레드 수는 10이며, 이는 초기화 시 생성되는 기본 쓰레드 수다.
- `acceptCount`: 최대 대기 요청 수는 100이며, 쓰레드가 부족할 때 대기할 수 있는 최대 요청 수다.

### 설정 예시
```yaml
server:
  tomcat:
    max-threads: 250         # 최대 250개의 쓰레드를 사용하여 동시 요청을 처리
    min-spare-threads: 20    # 최소 20개의 여유 쓰레드를 유지
```

<br>

---

<br>

# ThreadLocal

## ThreadLocal의 기본 개념
- `ThreadLocal 클래스`: ThreadLocal 클래스는 각 쓰레드가 고유한 값을 가질 수 있도록 해주며, 쓰레드가 해당 변수를 읽거나 쓸 때, ThreadLocal은 그 쓰레드만을 위한 고유한 인스턴스를 반환한다.
- `동작 원리`: 각 쓰레드는 ThreadLocal 객체에 대해 독립적인 값을 가지므로, 한 쓰레드가 ThreadLocal에 값을 저장하면, 다른 쓰레드는 그 값을 볼 수 없다.

<br>

## 사용 예시

### ThreadLocal 선언 및 사용
```java
public class UserContext {
    // 초기화
    private static final ThreadLocal<User> userThreadLocal = ThreadLocal.withInitial(() -> null);

    // 현재 쓰레드에 사용자 설정
    public static void setUser(User user) {
        userThreadLocal.set(user);
    }

    // 현재 쓰레드에 저장된 사용자 정보 반환
    public static User getUser() {
        return userThreadLocal.get();
    }

    // 현재 쓰레드에서 ThreadLocal 변수 제거
    public static void clear() {
        userThreadLocal.remove();
    }
}
```

### 필터를 사용한 설정
```java
import jakarta.servlet.*;
import java.io.IOException;

public class UserFilter implements Filter {
    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        // 사용자가 요청하면서 보낸 값이 있다면 추출해서 UserContext에 저장
        try {
            // 사용자 정보 설정
            User user = new User();
            user.setUsername("sangjin");
            user.setPassword("1234");

            UserContext.setUser(user);

            chain.doFilter(request, response);
        } finally {
            UserContext.clear();
        }
    }
}
```

### Config를 이용한 설정
```java
@Configuration
public class FilterConfig {
    @Bean
    public FilterRegistrationBean<UserFilter> UserFilter() {
        FilterRegistrationBean<UserFilter> registrationBean = new FilterRegistrationBean<>();
        UserFilter UserFilter = new UserFilter();

        registrationBean.setFilter(userFilter);
        registrationBean.addUrlPatterns("/*");
        registrationBean.setOrder(1);

        return registrationBean;
    }
}
```

<br>

## 장점
- `쓰레드 안전성`: 각 쓰레드가 독립적으로 ThreadLocal 변수를 갖기 때문에, 동시성 문제를 피할 수 있다.
- `간편한 상태 유지`: 요청-응답 주기 동안 상태를 쉽게 유지할 수 있고 예를 들어, 인증된 사용자 정보를 요청의 처음부터 끝까지 유지할 수 있다.
- `코드 간결성`: 전역 변수를 사용하지 않고도 요청 스코프 데이터를 저장할 수 있어 코드가 깔끔해진다.
