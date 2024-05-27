# 스프링 부트와 로깅

`로깅`은 애플리케이션의 동작 상태를 이해하고, 문제가 발생했을 때 원인을 분석하기 위해 필수적인 부분이다. 

스프링 부트는 기본적으로 `Apache Commons Logging`을 내부 로깅 API로 사용하지만, 실제 로깅은 `SLF4J(간단 로깅 파사드)`와 `Logback`을 기본으로 사용하여 처리한다.

## 로깅 라이브러리

1. `SLF4J(Simple Logging Facade for Java)`: 로깅을 위한 추상 레이어(facade) 제공으로, 다양한 로깅 프레임워크(JUL, Log4j, Logback 등)를 일관된 방식으로 사용할 수 있다.
2. `Logback`: `SLF4J`의 기본 구현체로, 강력하고 유연한 로깅 기능을 제공합니다. `XML` 또는 `Groovy` 기반의 구성 파일을 통해 로거, 어펜더, 필터 등을 세밀하게 설정할 수 있다.
3. `Log4j2`: `Logback`과 유사하며, 성능 및 기능 측면에서 일부 개선된 점을 가지고 있으며, 스프링 부트와 함께 사용하기 위해서는 `SLF4J`와 함께 `Log4j2`를 구성해야 합니다.

<br>

## 스프링 부트의 로깅 설정
- `logging.file.name`: 로그 파일의 이름과 경로를 지정하여, 지정된 경로에 로그 파일을 생성한다.
- `logging.file.path`: 로그 파일을 저장할 디렉터리를 지정한다.
- `logging.level.*`: 패키지 레벨로 로깅 레벨을 설정하여, `INFO`, `DEBUG`, `WARN`, `ERROR` 등의 로깅 레벨을 조정할 수 있다.

위 기본 설정으로 충분하지 않은 경우, `Logback`의 `logback-spring.xml` 파일이나 `Log4j2`의 `log4j2-spring.xml` 파일을 프로젝트에 추가하여 보다 상세한 로깅 구성을 할 수 있다.

### logback-spring.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <!-- 콘솔 Appender -->
    <appender name="CONSOLE" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 파일 롤링 Appender -->
    <appender name="FILE" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>logs/application.log</file>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- 롤링 파일 이름 및 롤링 정책 -->
            <fileNamePattern>logs/archived/application.%d{yyyy-MM-dd}.log</fileNamePattern>
            <maxHistory>30</maxHistory> <!-- 30일 동안 로그 보관 -->
        </rollingPolicy>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!-- 특정 패키지 로그 설정 -->
    <logger name="com.example.log.specific" level="DEBUG" additivity="false">
        <appender-ref ref="FILE"/>
    </logger>

    <!-- 기본 로그 설정 -->
    <root level="INFO">
        <appender-ref ref="CONSOLE"/>
    </root>
</configuration>
```

- `Console Appender`: 콘솔에 로그를 출력하는 설정이며, 로그 패턴을 `%d{yyyy-MM- dd HH:mm:ss} - %msg%n`로 설정하여 로그 메시지 앞에 날짜와 시간을 표시한다.
- `Rolling File Appender`: 파일에 로그를 저장하고, 시간 기반으로 파일을 롤링하는 설정이며, `logs/application.log` 파일에 로그를 저장하며, 하루가 지날 때마다 `logs/archived/` 디렉토리에 날짜를 포함한 새 파일명으로 롤링한다.
- `특정 패키지 로그 설정`: `com.example.log.specific` 패키지 및 그 하위 패키지의 로그를 `DEBUG` 레벨로 설정하고, 파일에만 기록하도록 `FILE` 애펜더를 지정하며 또, `additivity="false"`는 해당 로거 설정이 상위 로거의 애펜더 설정을 상속하지 않도록 한다.
- `Root 로그 설정`: 모든 로그의 기본 설정으로, `INFO` 레벨의 로그를 콘솔에 출력한다.

<br>

# JPA
`JPA(Java Persistence API)`는 Java 플랫폼에 대한 `ORM(Object-Relational Mapping) 표준`을 제공하는 API다. 

이 API는 객체 지향 모델을 관계형 데이터베이스의 테이블에 매핑하여, 개발자가 데이터베이스 작업을 더 직관적이고 객체지향적인 방식으로 수행할 수 있도록 돕는다.

## 주요 구성 요소
<img width="291" alt="스크린샷 2024-05-27 11 11 41" src="https://github.com/hamsangjin/TIL/assets/103736614/c5ff63a7-eab6-4523-a201-a8c0f3070bdc">

- `엔티티 매니저(EntityManager)`: JPA를 통해 데이터베이스 작업을 수행하는 데 중심역할을 하는 클래스로 엔티티의 생명 주기를 관리한다.
- `엔티티`: 데이터베이스의 테이블에 해당하는 클래스로, JPA 어노테이션을 사용하여 데이터베이스 테이블과 매핑된다.
- `영속성 컨텍스트(Persistence Context)`: 엔티티 인스턴스의 생명 주기를 관리하는 환경으로, 트랜잭션 범위 내에서 엔티티를 저장하고 관리한다.
- `트랜잭션`: 데이터베이스 작업을 묶어주는 방법으로, 작업들이 모두 성공하거나 실패하게 보장한다.

<br>

## 예제 코드

### build.gradle
```gradle
plugins {
    id 'groovy'
}

group = 'org.example'
version = '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    // Java 어플리케이션에서 관계형 데이터베이스를 객체 지향적으로 사용할 수 있도록 해주는 ORM 프레임워크
    implementation 'org.hibernate:hibernate-core:6.4.4.Final'

    implementation 'com.mysql:mysql-connector-j:8.3.0'

    // 자바 표준 인터페이스를 제공하여 애플리케이션에서 데이터베이스 엔티티를 관리할 수 있는 방법을 정의
    implementation 'jakarta.persistence:jakarta.persistence-api:3.1.0'

    // Java의 로깅을 위한 추상화 레이어(facade)를 제공
    implementation 'org.slf4j:slf4j-simple:2.0.13'

    compileOnly group: 'org.projectlombok', name: 'lombok', version: '1.18.32'
}

test {
    useJUnitPlatform()
}
```

<br>

### resources/META-INF/persistence.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- JPA 설정의 루트 엘리먼트으로, 모든 persistence-unit 설정을 포함 -->
<persistence xmlns="http://java.sun.com/xml/ns/persistence"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://java.sun.com/xml/ns/persistence http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"
             version="2.0">

    <!-- 데이터베이스 연결과 엔티티 관리를 위한 설정 그룹을 정의 -->
    <persistence-unit name="UserPU" transaction-type="RESOURCE_LOCAL">
        <!-- JPA 프로바이더로, Hibernate가 JPA 구현체로 사용되고 있음을 나타내고 있다. -->
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
<!--        <exclude-unlisted-classes>false</exclude-unlisted-classes>-->
        <class>com.example.jpa.User</class>  <!-- Add this line -->
        <!-- persistence-unit 내에서 관리할 엔티티 클래스를 나열 -->
        <properties>
            <property name="jakarta.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="jakarta.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/examplesdb"/>
            <property name="jakarta.persistence.jdbc.user" value="sangjin"/>
            <property name="jakarta.persistence.jdbc.password" value="sangjin"/>

            <!-- update가 아닌 create로 하면 존재하더라도 drop하고 새로 생성함(create, update, create-drop, validate -->
            <property name="hibernate.hbm2ddl.auto" value="update"/>

            <!--Dialect: 데이터베이스와의 상호 작용을 위해 SQL을 동적으로 생성(MySQLDialect, Oracle12cDialect 등)-->
            <property name="hibernate.dialect" value="org.hibernate.dialect.MySQLDialect"/>
            <property name="hibernate.show_sql" value="true"/>
            <property name="hibernate.format_sql" value="true"/>
        </properties>
    </persistence-unit>
</persistence>
```

<br>

### Entity 클래스 - User.java
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "jpa_user")       // 클래스명과 테이블명이 다르면 table 어노테이션을 통해 테이블명을 정해줄 수 있다.
@Getter
@Setter
@NoArgsConstructor
public class User {
    @Id // 기본키
    @GeneratedValue(strategy = GenerationType.IDENTITY) // 값 자동으로 생성
    private Long id;

    // @Column(name = "user_name"): 테이블 컬럼명을 정할 수 있음
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
}
```

<br>

### DAO - UserDao.java
```java
package com.example.jpa;

import jakarta.persistence.*;

public class UserDao {
    // 엔티티 매니저를 생성하는 팩토리 역할(비용이 많이 들어 하나만 사용함)
    private EntityManagerFactory emf;

    public UserDao() {
        // EntityManagerFactory 초기화
        // "UserPU" 는 persistence.xml 파일에 정의된 persistence unit의 이름
        emf = Persistence.createEntityManagerFactory("UserPU");
    }

    public void createUser(User user){
        EntityManager em = emf.createEntityManager();
        try {
            em.getTransaction().begin();    // 트랜잭션 시작
            em.persist(user);               // User 엔티티를 영속성 컨텍스트에 저장
            em.getTransaction().commit();   // 데이터베이스에 반영
        } finally {
            em.close();
        }
    }

    // 애플리케이션이 종료될 때 EntityManagerFactory 닫기
    public void close(){
        if(emf != null) emf.close();
    }
}
```

<br>

### DAO 리팩토링 - JPAUtil.java
```java
package com.example.jpa;

import jakarta.persistence.*;

public class JPAUtil {
    // EntityManagerFactory 초기화
    private static final EntityManagerFactory emfInstance = Persistence.createEntityManagerFactory("UserPU");

    // Java 어플리케이션이 종료될 때 자동으로 close() 메소드 호출
    static {
        Runtime.getRuntime().addShutdownHook(new Thread(() -> {     // addShutdownHook: JVM 종료 시 특정 작업을 실행
            if (emfInstance != null) {
                System.out.println("---- emf close ---");
                emfInstance.close();
            }
        }));
    }

    // 인스턴스 생성을 외부에서 할 수 없도록 제한
    private JPAUtil(){}

    // 다른 클래스에서 EntityManagerFactory에 접근할 수 있는 메소드 -> 싱글톤
    public static EntityManagerFactory getEntityManagerFactory() {
        return emfInstance;
    }
}
```

<br>

### DAO 리팩토링 - UserDao.java
```java
package com.example.jpa;

import jakarta.persistence.*;

public class UserDao {
    // DAO 리팩토링
    public void createUser(User user) {
        // EntityManager 인스턴스 생성
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        try {
            em.getTransaction().begin();    // 트랜잭션 시작
            em.persist(user);               // 엔티티 영속화
            em.getTransaction().commit();   // 트랜잭션 커밋
        } finally {
            em.close();                     // 리소스 정리
        }
    }
}
```

<br>

### Main - logback.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>

<!--Logback 설정 파일의 루트 요소-->
<configuration>
    <!--로그 메시지를 어디에 어떻게 출력할지 정의(ConsoleAppender)-->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!--로그 메시지의 형식을 지정-->
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger{36} - %msg%n</pattern>
        </encoder>
    </appender>

    <!--애플리케이션의 모든 로깅에 적용되는 기본 로그 레벨을 설정-->
    <root level="debug">
        <appender-ref ref="STDOUT" />
    </root>

</configuration>
```

<br>

### Main - UserMain.java
```java
package com.example.jpa;

import org.slf4j.*;

public class UserMain {
    // UserMain 클래스 전용의 Logger 인스턴스를 생성해, 어플리케이션의 실행 상태를 기록
    private static final Logger logger = LoggerFactory.getLogger(UserMain.class);
    public static void main(String[] args) {
        UserDao userDao = new UserDao();

        User newUser = new User("sangjin", "ham@sang.jin");
        userDao.createUser(newUser);    // 데이터베이스에 새로 생성된 newUser 저장

        // 로그 메시지를 출력
        logger.info("Created user: {}", newUser.getName());    // 사용자의 이름
        logger.info("Created user: {}", newUser.getEmail());   // 이메일
        logger.info("Created user: {}", newUser.getId());      // ID
    }
}
```
- 실행결과
<img width="224" alt="스크린샷 2024-05-27 21 14 36" src="https://github.com/hamsangjin/TIL/assets/103736614/6e8afee9-dd5c-412e-adaf-913c3371636e">

<br>

### 조회 메소드 추가 - UserDao.java
```java
package com.example.jpa;

import jakarta.persistence.*;

public class UserDao {
    public void createUser(User user) {
        ...
    }

    public User findUser(Long userId) {
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        try {
            User user = em.find(User.class, userId);
            em.close();
            return user;
        } finally {
            em.close();
        }
    }
}
```

<br>

### 조회 메소드 사용 - UserMain.java
```java
package com.example.jpa;

import org.slf4j.*;

public class UserMain {
    private static final Logger logger = LoggerFactory.getLogger(UserMain.class);
    public static void main(String[] args) {

        // 특정 유저 검색
        logger.info("------------ find a user -------------");
        User foundUser = userDao.findUser(1L);
        logger.info("Found user: " + foundUser.getName());
    }
}
```
- 실행결과
<img width="213" alt="스크린샷 2024-05-27 21 23 15" src="https://github.com/hamsangjin/TIL/assets/103736614/f51f702b-f787-4a5f-b0d0-da4f4ace7d3c">
