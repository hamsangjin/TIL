# 오토와이어링
자바 기반 설정 방식에서 `@Bean` 메소드를 사용하거나 XML기반 설정 방식에서 `<bean>` 요소를 사용하는 것처럼, 명시적으로 빈을 정의하지 않고도 `DI 컨테이너`에 빈을 자동으로 주입하는 방식이다.

<br>

## 타입을 사용한 방식
`@Autowired` 어노테이션을 이용한 방식으로, 모든 주입방식에서 사용 가능하다.

기본적으로 의존성 주입이 반드시 성공한다고 가정할 수 있고, 주입할 타입에 해당하는 빈을 찾지 못하면 `NoSuchBeanDefinitionException`이 발생한다. 필수 조건을 사용하고 싶지 않을 경우 `@Autowired(required = false)`로 설정한다.

인젝션을 할 수 있는 여러개의 빈을 발견하게 될 경우에는 `NoUniqueBeanDefinitionException`이 발생한다.
- `@Qualifier`: 빈이 여러 개일 경우 `@Qualifier` 어노테이션을 이용해 빈 이름을 지정할 수 있다.
- `@Primary`: `@Qualifier`를 사용하지 않았을 때 우선적으로 선택할 빈을 지정할 수 있다.

<br>

## 이름으로 오토와이어링하기
빈의 이름이 필드명이나 프로퍼티명과 일치할 경우에 JSR-250사양을 지원하는 `@Resource` 애노테이션을 이용해 **빈 이름으로 필드 인젝션**을 할 수 있다.
- `@Resource`
  - `필드 주입`: 필드이름과 같은 이름의 빈이 선택
  - `Settter 주입`: 프로 퍼티 이름과 같은 이름의 빈이 선택
  - `생성자 주입`: 사용 불가

<br>

## JSR-250
`JSR-250`는 자바 커뮤니티 프로세스(JCP)를 통해 정의된 자바 표준화 요청 중 하나이며, 주요 어노테이션은 아래와 같다.

- `@PostConstruct`: 객체의 생성과 의존성 주입이 완료된 후, 초기화 목적으로 실행할 메서드에 사용되며, 해당 메소드는 객체가 생성된 후 단 한 번만 호출되며, 초기화 작업에 주로 사용된다.
- `@PreDestroy`: 컨테이너에 의해 빈이 제거되기 전에 호출될 메서드에 사용되며, 리소스의 해제, 정리 작업을 수행하는 데 사용할 수 있다.
- `@Resource`: 리소스나 서비스에 대한 참조를 주입받기 위해 사용되며, 이름, 타입 등을 기반으로 의존성을 주입하는데 사용되고, 예를 들어 데이터 소스, 세션, 기타 환경 자원 등을 주입받는 데 활용된다.

<br>

---

<br>

# 컴포넌트 스캔
Java Config파일에 `@Bean`으로 일일이 등록하는 것이 아니라, 빈이 될 수 있는 클래스들을 찾아서 자동으로 등록하게 하는 방법이다.

또, `클래스 로더`를 스캔하면서 특정 클래스를 찾은 다음 `DI 컨테이너`에 등록하는 방법을 말한다.

별도의 설정의 없는 기본 설정에서는 다음과 같은 애노테이션이 붙은 클래스가 탐색 대상이 되고 탐색된 컴포넌트는 DI컨테이너에 등록된다.
- `@Component`, `@Controller`, `@Service`, `@Repository`, `@RestController` `@Configuration`
- `@ControllerAdvice`
- `@ManagedBean(java.annotation.ManagedBean)`, `@Named(javax.inject.Named)`

다음과 같은 방법으로 특정 패키지 이하를 스캔한다. 
- `@ComponentScan(basePackages = "examples.di")`

기본 스캔 대상 외에도 추가로 다른 컴포넌트를 포함하고 싶을 경우 필터를 적용한 컴포넌트 스캔할 수 있다. 스프링 프레임워크는 다음과 같은 필터를 제공한다.
- `애노테이션을 활용할 필터(ANNOTATION)`
- `할당 가능한 타입을 활용한 필터(ASSIGNABLE_TYPE)` 
- `정규 표현식 패턴을 활용한 필터(REGEX)`
- `AspectJ패턴을 활용한 필터(ASPECTJ)`

<br>

---

<br>

# 빈의 생명주기
DI 컨테이너에서 관리되는 빈의 생명주기는 크게 다음의 세가지 단계로 구분할 수 있다.
- `빈 초기화 단계(initialization)`
- `빈 사용 단계(activation)`
- `빈 종료 단계(destruction)`

<br>

## 생명주기
<img width="536" alt="스크린샷 2024-05-13 18 32 57" src="https://github.com/hamsangjin/TIL/assets/103736614/919263ea-285d-4b54-9bbc-947a30956e2d">

1. 스프링이 빈을 `인스턴스화` 한다.
2. 스프링이 값과 빈의 레퍼런스를 빈의 프로퍼티로 주입한다.
3. 빈이 `BeanNameAware`를 구현하면 스프링이 `빈의 ID`를 `setBeanName()`메소드에 넘긴다.
4. 빈이 `BeanFactoryAware`를 구현하면 `setBeanFactory()`메소드를 호출하여 빈 팩토리 전체를 넘긴다.
5. 빈이 `ApplicationContextAware`를 구현하면 스프링이 `setApplicationContext()`메소드를 호출하고 둘러싼 애플리케이션 컨텍스트에 대한 참조를 넘긴다.
6. 빈이 `Bean PostProcessor` 인터페이스를 구현하면 스프링은 `postProcessBeforeInitialization()` 메소드를 호출한다.
7. 빈이 `InitializingBean`인터페이스를 구현하면 스프링은 `afterPropertiesSet()` 메소드를 호출한다. 마찬가지로 빈이 `init-method`와 함께 선언됐으면 지정한 초기화 메소드가 호출된다.
8. 빈이 `BeanPostProcessor`를 구현하면 스프링은 `postProcessAfterInitialization()` 메소드를 호출한다.
9. 빈은 애플리케이션에서 사용할 준비가 된 것이며, 애플리케이션 컨텍스트가 소멸될 때까지 애플리케이션 컨텍스트에 남아있게 된다.
10. 빈이 `DisposableBean` 인터페이스를 구현하면 스프링은 `destroy()`메소드를 호출한다. 마찬가지로 빈이 `destory-method`와 함께 선언됐으면 지정된 메소드가 호출된다.

<br>

## 생명주기 관련 애노테이션
- `@PostConstruct`: JSR-250 스펙으로, JSR-250을 구현하고 있는 다른 프레임워크에서도 사용가능하고, 인스턴스 생성 후에 호출된다.
- `@Bean(initMethod)`: `@Bean(initMethod="init")` 과 같은 형태로 사용함으로써 초기화 메소드를 사용할 수 있다.
- `@PreDestroy`: JSR-250 스펙에서 정의되어 있고, 종료될 때 사용할 메소드 위에 사용하면 된다.
- `@Bean(destroyMethod)`: `@Bean(destroyMethod="destroy")`와 같은 형태로 사용함으로써 종료될 때 호출되도록 할 수 있다.

<br>

---

<br>

# 빈 설정 분할
DI 컨테이너에서 관리하는 빈이 많아지면 많아질수록 설정(config) 내용도 많아져서 관리하기가 어려워진다. 

이럴 때는 빈 설정 범위를 명확히 하고 가독성도 높이기 위해 목적에 맞게 분할하는 것이 좋다.

- 자바 기반 설정의 분할: `@Import` 애노테이션을 사용한다.
- XML 기반 설정의 분할: `<import>` 요소를 사용한다.

<br>

---

<br>

# Profile별 설정 구성
스프링 프레임워크에서는 설정 파일을 특정 환경이나 목적에 맞게 선택적으로 사용할 수 있도록 그룹화할 수 있으며, 이 기능을 `Profile`이라 한다.
- 자바 기반 설정 방식: `@Profile(“dev”)`나 `@Profile(“dev”, “real”)`와 같은 방식으로 `@Profile` 애노테이션을 사용한다.
- XML기반 설정 방식: `<beans>` 요소의 `profile`속성을 활용한다.

<br>

## Profile 선택

- `Dspring.profiles.active=real`: 자바 명령행 옵션으로 프로파일을 지정하는 방법
- `export SPRING_PROFILES_ACTIVE=real`: 환경 변수로 프로파일을 지정하는 방법

> `spring.profiles.active`를 지정하지 않으면 기본값으로 `spring.profiles.default`에 지정
된 프로파일을 사용한다.

<br>

---

<br>

# Spring JDBC

## JDBC와 Spring Data JDBC의 차이점
- `JDBC(Java Database Connectivity)`: Java에서 데이터베이스에 접근할 수 있게 해주는 API
    SQL 쿼리 실행, 데이터베이스 연결 관리 등의 기능을 제공하며, JDBC는 데이터베이스와의 직접적인 상호작용을 처리하지만, 이를 사용하기 위해서는 많은 보일러플레이트 코드가 필요하다.
- `Spring Data JDBC`: Spring 프레임워크의 일부로, JDBC의 복잡성을 추상화하고 간소화한다.
    Spring JDBC는 템플릿 디자인 패턴을 사용하여 코드 중복을 줄이고, 개발자가 SQL 쿼리에 집중할 수 있도록 돕고, 예외 처리를 일관된 방식으로 처리할 수 있게 하여 데이터베이스 작업을 더욱 효율적이고 안정적으로 만든다.

<br>

## Spring Data JDBC의 장점
- `간소화된 에러 처리`: JDBC의 복잡하고 다양한 예외 처리를 스프링의 `DataAccessException`으로 일관되게 처리할 수 있다.
- `리소스 관리`: 데이터베이스 연결과 같은 리소스를 자동으로 관리하며, 사용 후에는 적절하게 클로즈하여 리소스 누수를 방지한다.
- `간결한 코드`: `JdbcTemplate`과 같은 템플릿 클래스를 사용하여 반복적인 코드 작성을 줄일 수 있다.
- `풍부한 기능 제공`: 데이터 액세스를 위한 다양한 편의 기능을 제공하며, SQL 쿼리 실행, 업데이트, 스토어드 프로시저 호출, 트랜잭션 관리 등을 간편하게 수행할 수 있다.

<br>

## Spring Data JDBC 구조
- `DataSource`: 모든 Spring JDBC 작업의 기반으로, 데이터베이스 연결을 관리하고, Spring Boot에서는 `application.properties` 또는 `application.yml` 파일을 통해 DataSource를 쉽게 설정할 수 있다.
- `JdbcTemplate`: SQL 쿼리 실행을 단순화하는 주요 클래스로, JDBC 작업에 필요한 많은 세부 사항을 처리한다.
- `NamedParameterJdbcTemplate`: JdbcTemplate의 확장으로, 쿼리에서 이름 기반의 파라미터를 사용하여 쿼리 가독성과 유지보수성을 높일 수 있다.
- `SimpleJdbcInsert와 SimpleJdbcCall`: 데이터 삽입과 스토어드 프로시저 호출을 위한 편리한 방법을 제공한다.

<br>

## Spring Data JDBC Template
`JdbcTemplate` 클래스는 Spring의 데이터 접근 전략에서 중심적인 역할을 하며, JDBC의 복잡함을 추상화하고 일반적인 데이터베이스 작업을 단순화하고, `query`, `update`, `execute` 등 `JdbcTemplate`의 주요 메서드들이 있다.
