# Spring Boot란
## 스프링 프레임워크(Spring Framework)
프레임워크가 내가 만든 것들을 동작시켜주기 때문에 프레임워크가 어떻게 동작하는지를 이해해야 한다.

### 프레임워크의 구성요소
- `프레임워크 코어`
- `확장 포인트`
- `확장 모듈`
- `메타데이터`

### 프레임워크의 장점
- 빠른 구현 시간
- 관리의 용이성 증가
- 개발자들의 역량 획일화
- 검증된 아키텍처의 재사용과 일관성 유지

<br>

## Spring Boot
`Spring Boot`는 Spring 프레임워크를 기반으로 만들어진 도구로, 복잡한 Spring 구성을 단순화하고, 더 빠르고 광범위한 접근성을 제공하기 위해 개발되었다.

### Spring Boot의 주요 장점
1. `자동 구성(Auto-configuration)`: Spring Boot는 클래스패스 세팅, 다른 빈, 다양한 프로퍼티 설정을 기반으로 어플리케이션을 자동으로 구성해줌으로써 개발자가 일반적인 어플리케이션 설정에 필요한 시간을 대폭 줄여준다.
2. `독립 실행 가능한 어플리케이션`: `내장 서버 지원(Tomcat, Jetty, Undertow 등)`으로 인해 별도의 웹 서버를 설치할 필요 없이 어플리케이션을 실행할 수 있다.
3. `의존성 관리`: `starter` 종속성을 통해 필요한 의존성을 쉽게 관리할 수 있다.
4. `운영 준비 상태`: 각종 상태 체크, 건강 상태, 메트릭스 등을 제공하는 `액추에이터(Actuator)`로 프로덕션 준비를 보다 수월하게 할 수 있다.
5. `뛰어난 커뮤니티 및 기술 지원`: Spring Boot는 활발한 커뮤니티와 지속적인 업데이트를 통해 최신 Java 기술 트렌드에 발맞춰 나가고 있다.


### Spring Boot의 단점 
1. `과도한 자동 구성`: 자동 구성이 매우 편리하긴 하지만, 때때로 너무 많은 자동화가 이루어져 어플리케이션의 시작 시간이 길어지거나 예상치 못한 동작을 할 때가 있다.
2. `리소스 사용량`: 내장 서버와 자동 구성 기능 때문에 가벼운 마이크로서비스를 위한 다른 솔루션들에 비해 상대적으로 많은 메모리와 리소스를 사용할 수 있다.
3. `학습 곡선`: Spring Boot 자체는 단순화된 접근을 제공하지만, 내부에서는 여전히 복잡한 Spring 생태계와 관련된 지식이 필요하며, Spring Boot의 자동 구성 이면의 작동 방식을 이해하기 위해선 Spring 프레임워크에 대한 깊은 이해가 요구된다.

<br>

---

<br>

# Spring Boot 프로젝트 생성

## Spring Initializr
[Spring Initializr](https://start.spring.io/) 웹사이트에 접속하여, 프로젝트 정보를 입력한 후, `GENERATE` 버튼을 클릭해 생성해준다.

<img width="1583" alt="스크린샷 2024-05-09 15 57 02" src="https://github.com/hamsangjin/TIL/assets/103736614/a54bb60e-0253-4d4c-920b-dba3df1564d0">

<br>
<br>
<br>

저장된 파일의 압축을 해제한다.
<img width="894" alt="스크린샷 2024-05-09 15 59 37" src="https://github.com/hamsangjin/TIL/assets/103736614/1c6a5f25-2f4f-455c-8b5b-0049e9943a27">

<br>
<br>
<br>

이제 IntelliJ에서 프로젝트를 열어보자
`IntelliJ 실행` -> `프로젝트 열기` -> `build.gradle 파일 선택` -> `Open`

<img width="1060" alt="스크린샷 2024-05-09 16 01 46" src="https://github.com/hamsangjin/TIL/assets/103736614/6490c91d-8add-4d46-b27b-0133c2c883a8">

<br>
<br>

---

<br>

# build.gradle 파일 분석
```java
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.2.5'
    id 'io.spring.dependency-management' version '1.1.4'
}

group = 'org.example'
version = '0.0.1-SNAPSHOT'

java {
    sourceCompatibility = '21'
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

## org.springframework.boot 플러그인
`org.springframework.boot 플러그인`은 Spring Boot 애플리케이션을 빌드하고 패키징하는 데 사용되는 Gradle 플러그인이다.

이 플러그인을 사용하면 `Spring Boot` 애플리케이션을 빌드하면, 편리하게 패키징할 수 있고, `jar` 파일이나 `Docker` 이미지를 생성할 수 있으며, 이를 통해 애플리케이션을 쉽게 배포하고 실행할 수 있습니다.

<br>

## io.spring.dependency-management 플러그인
`io.spring.dependency-management 플러그인`은 `Gradle` 프로젝트에서 의존성 관리를 위해 사용되는 플러그인이다.

이 플러그인을 사용하면, 버전 관리를 자동으로 수행하여 의존성 관리를 더욱 효율적으로 할 수 있다.
- `dependencyUpdates`: 프로젝트의 의존성에 대해 업데이트 가능한 버전을 확인하고, 출력한다.
- `dependencyManagement`: 의존성 관리를 위해 사용되는 `Maven BOM(Bill of Materials)` 파일을 생성한다.
  - `Maven BOM(Bill of Materials)`: 의존성 관리를 위한 파일로, `Maven Repository`에 업로드되는 특수한 종류의 `POM` 파일이며, `BOM` 파일은 프로젝트에서 사용하는 의존성의 버전을 관리할 수 있도록 도와준다.

<br>

## gradle의 group, version, sourceCompatibility 속성
Gradle에서 `group, `version`, `sourceCompatibility`는 프로젝트 설정을 지정하는 속성이다.
- `group`: 프로젝트의 `그룹명`을 설정하는 속성으로, `Maven Repository`에 업로드될 때 그룹명으로 사용되며, 보통 프로젝트의 패키지명과 동일하게 지정된다.
- `version`: 프로젝트의 `버전`을 설정하는 속성으로, 버전은 프로젝트의 변경사항에 따라 업데이트되며, `Maven Repository`에서는 버전별로 구분된다.
- `sourceCompatibility`: 프로젝트의 `소스 코드 버전`을 설정하는 속성으로, 이 속성을 설정하면 컴파일러가 해당 버전으로 코드를 컴파일한다.

<br>

## repositories{ mavenCentral() }
Gradle 빌드 시스템에서 `repositories 블록`은 **사용할 라이브러리들이 저장**되어 있는 `Maven Repository의 위치`를 지정한다.

위의 코드에서 `mavenCentral()`은 Gradle에서 제공하는 기본적인 라이브러리 저장소인 `Maven Central Repository`를 사용한다는 것을 의미한다.

<br>

## 의존성(dependencies) 설정하기 
`org.springframework.boot:spring-boot-starter-web`은 Spring Boot 애플리케이션을 개발할 때 웹 개발에 필요한 의존성을 관리해주는 `Starter` 프로젝트이다.

이를 통해, 불필요한 의존성을 추가하지 않고도 웹 애플리케이션을 개발할 수 있으며, Spring Boot의 자동 설정 기능을 이용하여 빠르고 쉽게 웹 애플리케이션을 구성할 수 있다.

<br>

---

<br>

# Spring Boot 실행 클래스
```java
@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```
## SpringApplication.run()
- `스프링 부트(프레임워크)`를 실행한다.
- 이때 `DemoApplication.class` 설정파일(메타파일)을 첫번째 파라미터로 받는다.
- `DemoApplication` 클래스가 설정파일일 수 있는 이유는 `@SpringBootApplication`이 붙어 있기 때문이다.
- `@SpringBootApplication`는 내부에 `@SpringBootConfiguration`를 포함하고 있고, `@SpringBootConfiguration`는 내부에 `@Configuration`을 포함하고 있다. 스프링 프레임워크는 `@Configuration`이 붙은 클래스를 설정 클래스로 여긴다.

<br>

## @SpringBootApplication 애노테이션
`@SpringBootApplication` 애노테이션은 스프링 부트에서 메인 클래스에 추가되는 애노테이션으로, 스프링 부트 애플리케이션의 기본적인 설정을 자동으로 구성한다.

클래스 레벨에서 사용하기 때문에 스프링 부트 애플리케이션의 메인 클래스에 `@SpringBootApplication` 애노테이션을 추가해야 하며, 다음을 포함한다.
1. `@Configuration`: 해당 클래스가 스프링 설정 클래스임을 나타내는 애노테이션
2. `@EnableAutoConfiguration`: 스프링 부트 자동 구성을 활성화하는 애노테이션
3. `@ComponentScan`: 해당 애노테이션에 지정된 패키지에서 컴포넌트를 스캔하여 스프링 빈으로 등록하는 애노테이션
  - `@Component 애노테이션`: 스프링에서 컴포넌트를 스캔하고 빈으로 등록할 때 사용되는 가장 기본적인 애노테이션

<br>

---

<br>

# DI
DI를 알아보기 전 먼저 컨테이너와 IoC에 대한 개념을 정리하자

## 컨테이너(Container)
`컨테이너`는 인스턴스의 생명주기를 관리하며, 생성된 인스턴스들에게 추가적인 기능을 제공한다.

<br>

## IoC(Inversion of Control)
`IoC`란 보통 IoC를 제어의 역전이라고 번역하고, 개발자가 프로그램의 흐름을 제어하는 코드를 작성하지만 이 흐름의 제어를 개발자가 하는 것이 아니라 다른 프로그램이 그 흐름을 제어하는 것을 의미한다.

<br>

## DI(Dependency Injection)
`DI`는 의존성 주입이란 뜻을 가지고 있으며, 클래스 사이의 의존 관계를 `빈(Bean)설정 정보`를 바탕으로 컨테이너가 자동으로 연결해주는 것을 말한다.

<br>

## DI 컨테이너
스프링 공식문서에서는 `IoC 컨테이너`라고 기재하고 있으며, 개발자들은 보통 `DI 컨테이너`라고 말한다.
스프링 프레임워크 외의 DI컨테이너 
- `CDI(Contexts & Dependency Injection)`
- `Google Guice`
- `Dagger`

<br>

## DI 컨테이너에서 인스턴스를 관리할 때의 장점
- 인스턴스의 스코프를 제어할 수 있다.
- 인스턴스의 생명 주기를 제어할 수 있다.
- AOP방식으로 공통 기능을 집어넣을 수 있다.
- 의존하는 컴포넌트 간의 결합도를 낮춰서 단위 테스트를 쉽게 만든다.

<br>

## DI, DI 컨테이너와 관련된 용어
1. `의존 객체(Dependency)`: 객체 간의 관계 중 하나로, 객체 A가 객체 B를 사용하는 경우, 객체 A가 객체 B에 의존하고 있다고 표현한다.
2. `의존 주입(Dependency Injection)`: 객체 간의 의존 관계를 설정하는 방법 중 하나로, 외부에서 의존 객체를 `생성자`, `setter`, `필드` 등을 통해 주입하는 방법이다.
3. `Bean`: 스프링에서 DI를 사용하기 위해 생성되는 객체를 의미한다.
4. `BeanFactory`: 스프링에서 Bean을 생성하고 관리하는 컨테이너를 의미한다.
5. `ApplicationContext`: `BeanFactory`를 상속한 스프링 컨테이너로, 더 다양한 기능을 제공한다.
6. `Component`: 스프링에서 Bean을 생성하기 위한 애노테이션 중 하나로, 해당 클래스를 `Bean`으로 등록한다.
7. `Qualifier`: **같은 타입의 Bean이 여러 개** 있을 경우, **어떤 Bean을 사용할 지 결정**하는 용도로 사용된다.
8. `Autowiring`: 자동으로 Bean을 주입하는 기능으로, `@Autowired` 애노테이션을 통해 사용된다.
9. `Scope`: 빈의 생성 주기와 관련된 범위(Scope)를 설정할 수 있으며, 대표적인 빈 스코프로는 `Singleton`, `Prototype`, `Request`, `Session` 등이 있다.
10. `Proxy`: DI 컨테이너는 빈을 가져올 때, 해당 빈을 감싸는 `Proxy` 객체를 생성할 수도 있으며, `Proxy` 객체는 빈의 메서드 호출을 가로채서 `보안`, `로깅`, `트랜잭션` 등의 작업을 수행할 수 있다.
