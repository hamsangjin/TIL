# Spring 입문 - 스프링 DB 접근기술

> 인프런의 "스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술" 강의를 듣고 정리한 내용입니다.

## 목차
- [H2 데이터베이스 설치](#h2-데이터베이스-설치)
- [순수 Jdbc](#순수-jdbc)
- [스프링 통합 테스트](#스프링-통합-테스트)
- [스프링 JdbcTemplate](#스프링-jdbctemplate)
- [JPA](#jpa)
- [스프링 데이터 JPA](#스프링-데이터-jpa)

</br>

---

</br>

## H2 데이터베이스 설치

### 설치 및 실행

먼저 [https://www.h2database.com/html/download-archive.html](https://www.h2database.com/html/download-archive.html)에서 1.4.200 버전을 설치해준다.

그 후, terminal에서 h2/bin/h2.sh 파일을 명령어 `chmod 755 h2.sh`을 통해 권한 변경을 하고 명령어 `./h2.sh`를 이용해 실행시킨다.

실행시키게 되면 아래 창이 뜬다.
<p align=center>
<img width="711" alt="스크린샷 2024-01-05 14 22 10" src="https://github.com/hamsangjin/TIL/assets/103736614/736d6dba-b7ac-4d22-b219-b7d835870eac">
</p>

- `JDBC URL` : 파일의 경로

</br>

먼저 아무것도 건드리지 않고 연결을 한다.
<p align=center>
<img width="898" alt="스크린샷 2024-01-05 14 28 17" src="https://github.com/hamsangjin/TIL/assets/103736614/810a2b90-b474-4cbb-8960-d2baf5555e4f">
</p>

위 창이 뜨면 잘 연결된 것이고, 가장 왼쪽 위 버튼을 통해 연결 전 창을 다시 띄운 후, 
</br>
지금처럼 `JDBC URL`을 파일로 설정하게 되면 오류가 날 수 있으므로, `jdbc:h2:tcp://localhost/~/test`로 경로를 변경해 파일에 직접 접근하는 것이 아니라 소켓을 통해 접근할 수 있게 변경해준다.
</br>
그 후 로컬에 test.mv.db가 생성하는지 삭제 후 확인해보고 생성된다면 정상 작동되는 것이다.

### 테이블 생성
기존에 진행했었던 `member` 테이블을 생성해보자.
<p align=center>
<img width="462" alt="스크린샷 2024-01-05 14 45 09" src="https://github.com/hamsangjin/TIL/assets/103736614/d6bc6120-3aaf-4311-b66f-04aa94231808">
</p>

왼쪽을 보면 쿼리문에 따라 `member`라는 테이블이 생성된 것을 볼 수 있다.

- `bigint` : java의 long과 동일
- `generated by default as identity` : 먼저 값을 세팅하지 않고, 데이터가 삽입됐을 때 자동으로 값을 채워줌
- `varchar` : 가변길이 문자열을 의미

</br>


이제 `member` 테이블에 값을 삽입해보고 조회해 확인해보자.
<p align=center>
<img width="273" height="300" alt="스크린샷 2024-01-05 14 53 46" src="https://github.com/hamsangjin/TIL/assets/103736614/c2148147-fe54-4a06-9c98-a03da4205e3e">
<img width="273" height="300" alt="스크린샷 2024-01-05 14 55 53" src="https://github.com/hamsangjin/TIL/assets/103736614/562adcb0-6523-4ecc-81fe-090d1290da2b">
</p>

정상적으로 삽입되며, id값이 자동으로 추가된 걸 확인할 수 있다.

</br>


이제 이러한 sql문도 같이 관리하기 위해 `hello-spring/sql` 폴더에 `ddl.sql`이라는 파일을 생성해 아까 작성한 쿼리문을 저장해 관리한다.
<p align=center>
<img width="786" alt="스크린샷 2024-01-05 15 04 53" src="https://github.com/hamsangjin/TIL/assets/103736614/68d948b2-ca8d-4077-b588-af6d02b93284">
</p>

</br>

---

</br>

## 순수 Jdbc

### Jdbc 환경설정
> 지금 진행하는 것은 오래된 방법이라, 이런 식으로 사용했었구나 ~ 라는 식으로 가볍게 듣는 것이 좋다.


먼저 `build.gradle` 파일에 `jdbc`, `h2 데이터베이스` 관련 라이브러리를 추가해주자
<p align=center>
<img width="964" alt="스크린샷 2024-01-05 15 13 59" src="https://github.com/hamsangjin/TIL/assets/103736614/2243c850-6222-4533-9d00-586e8479185a">
</p>

* java는 DB랑 연결하려면 JDBC 드라이버가 꼭 있어야 한다.

</br>

이제 DB와 연결하기 위해서는 접속 정보가 필요한데, 스프링 부트에서는 `application.properties`에 경로만 넣으면 드라이버 설치 세팅이 끝난다.
<img width="767" alt="스크린샷 2024-01-05 15 37 13" src="https://github.com/hamsangjin/TIL/assets/103736614/db4541e9-edde-45e6-aae8-c12d5259bffe">
* 스프링부트 2.4부터 `spring.datasource.username=sa`을 추가해야 오류가 발생 안 한다.

### Jdbc 리포지토리 구현

먼저 전에 생성했었던 구현체 `MemberRepository`를 이용해 메소드들을 불러오고 수정해준다.

- main/java/hello/hellospring/repository/JdbcMemberRepository.java
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.jdbc.datasource.DataSourceUtils;

import javax.sql.DataSource;
import java.sql.*;
import java.util.ArrayList;
import java.util.List;
import java.util.Optional;

public class JdbcMemberRepository implements MemberRepository {
    private final DataSource dataSource;

    public JdbcMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Override
    public Member save(Member member) {
        String sql = "insert into member(name) values(?)";

        Connection conn = null;
        PreparedStatement pstmt = null;
        ResultSet rs = null;
        ...
```
</br>

### 스프링 설정 변경

지금까지는 Spring Config에서 `MemoryMemberRepository`를 스프링 빈에 등록해 사용하고 있었다.

하지만 이제 h2 데이터베이스를 사용해 동작해야하니까 `MemoryMemberRepository`이 아닌 `JdbcMemberRepository`로 변경해준다.

- `DataSource` : 데이터베이스 커넥션을 획득할 때 사용하는 객체로, 스프링 부트는 데이터베이스 커넥션 정보를 바탕으로 DataSource를 생성하고 스프링 빈으로 만들어둔다. 그래야 DI(의존성 주입)을 받을 수 있다.

- main/java/hello/hellospring/service/SpringConfig.java
```java
package hello.hellospring.service;

import hello.hellospring.repository.JdbcMemberRepository;
import hello.hellospring.repository.MemberRepository;
import hello.hellospring.repository.MemoryMemberRepository;
import hello.hellospring.service.MemberService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class SpringConfig {

    private final DataSource dataSource;

    @Autowired
    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
//        return new MemoryMemberRepository();
        return new JdbcMemberRepository(dataSource);
    }
}
```

</br>

현재 변경한거라고는 `SpringConfig` 설정과 H2 드라이버 설치만 진행했다. </br>
이때, 메모리로 동작하던 코드가 h2 데이터베이스를 이용해 동작하는지 확인해보자. </br>

회원에 함상진을 추가하고, 회원 목록을 확인해보자.
<p align=center>
<img width="331" height="330 alt="스크린샷 2024-01-06 17 41 35" src="https://github.com/hamsangjin/TIL/assets/103736614/c898ef60-aab9-4577-afc8-078233766a4d">
<img width="331" height="330 alt="스크린샷 2024-01-06 17 39 37" src="https://github.com/hamsangjin/TIL/assets/103736614/7370dff5-bb24-4208-8c37-9270195022a6">
</p>

</br>

기존에 등록했던 **spring1**, **spring2**와 새로운 회원 **함상진**이 추가된 걸 볼 수 있다.

</br>

### 구현 클래스 추가 이미지
<p align=center>
<img width="601" alt="스크린샷 2024-01-06 17 48 22" src="https://github.com/hamsangjin/TIL/assets/103736614/801ef4a5-1c2b-4d1f-8295-b7fa246974f5">
</p>

- `다형성` : 인터페이스를 가지고 이것저것 구현할 수 있는 기능으로, 스프링 컨테이너가 지원을 해주며, DI를 통해 쉽게 할 수 있다. 

</br>

### 스프링 설정 이미지
<p align=center>
<img width="597" alt="스크린샷 2024-01-06 17 50 41" src="https://github.com/hamsangjin/TIL/assets/103736614/35a9c041-cbca-47bc-864a-78a0e6fcd20e">
</p>
</br>

- `개방 폐쇄 원칙(OCP, Open-Closed Principle)` : SOLID 중 하나로, 확장에는 열려있고, 수정(변경)에는 닫혀있다.
- 스프링의 DI를 사용하면 기존 코드를 **전혀 손대지 않고, 설정만으로 구현 클래스를 변경**할 수 있다.
- 이제 **메모리**가 아닌 DB에 저장하므로 서버를 다시 실행해도 데이터가 안전하게 저장되어 있다.

</br>

---

</br>

## 스프링 통합 테스트

스프링 컨테이너와 DB까지 연결했으니 이제 통합 테스트를 해보자

### 회원 서비스 스프링 통합테스트

> 기존에 했었던 테스트들은 순수 java 코드를 가지고 테스트를 했었지만, </br>
> 이제는 스프링부트가 데이터베이스 커넥션 정보를 들고있기 때문에 스프링을 이용해 통합테스트를 해야한다.

- test/java/hello/hellospring/service/MemberServiceIntegrationTest.java
```java
package hello.hellospring.service;

import hello.hellospring.domain.Member;
import hello.hellospring.repository.MemberRepository;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;

import static org.assertj.core.api.Assertions.assertThat;
import static org.junit.jupiter.api.Assertions.assertThrows;

@SpringBootTest
@Transactional
class MemberServiceIntegrationTest {

    @Autowired MemberService memberService;
    @Autowired MemberRepository memberRepository;

    @Test
    void 회원가입() {
        // given : 상황이 주어졌을 때
        Member member = new Member();
        member.setName("test");

        // when : 이거를 실행했을 때
        Long saveId = memberService.join(member);

        // then : 이거가 나와야 해
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외() {
        // given
        Member member1 = new Member();
        member1.setName("test");

        Member member2 = new Member();
        member2.setName("test");
        // when
        memberService.join(member1);

        IllegalStateException e = assertThrows(IllegalStateException.class, () -> memberService.join(member2));

        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
    }
}
```

</br>

- `@SpringBootTest` : 스프링 컨테이너와 테스트를 함께 실행하기 위한 어노테이션
- `@Transactional` : 테스트 시작 전에 **트랜잭션을 시작**하고, 테스트 완료 후에 항상 **롤백**을 해주는 어노테이션. 이렇게 하면 DB에 데이터가 남지 않으므로 다음 테스트에 영향을 주지 않는다.
  - 기존 테스트의 `@AfterEach`와 비슷한 역할을 한다.
  - `@Transactional`을 사용했을 때와 안 했을 때의 DB 조회 결과
  
<p align=center>
<img width="274" height="300" alt="스크린샷 2024-01-10 15 14 54" src="https://github.com/hamsangjin/TIL/assets/103736614/143152a1-b5d3-4e49-ae0c-4f0e34b229c2">
<img width="274" height="300" alt="스크린샷 2024-01-10 15 14 29" src="https://github.com/hamsangjin/TIL/assets/103736614/f6a1e64d-82df-4e94-8ee6-87bee3ee491f">
</p>

</br>

---

</br>

## 스프링 JdbcTemplate

- 순수 `Jdbc`와 동일하게 환경설정을 하면 된다.
- `JdbcTemplate`는 JDBC API에서 봤던 반복 코드를 대부분 제거해주지만, SQL은 직접 작성해야 한다.

### 스프링 JdbcTemplate 회원 리포지토리

- main/java/hello/hellospring/repository/JdbcTemplateMemberRepository.java

```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.jdbc.core.namedparam.MapSqlParameterSource;
import org.springframework.jdbc.core.simple.SimpleJdbcInsert;

import javax.sql.DataSource;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Optional;

public class JdbcTemplateMemberRepository implements MemberRepository{

    private final JdbcTemplate jdbcTemplate;

    // 생성자가 딱 하나만 있다면 자동으로 스프링빈으로 등록되므로
    // @Autowired를 생략할 수 있다.
    // @Autowired
    public JdbcTemplateMemberRepository(DataSource dataSource) {
        jdbcTemplate = new JdbcTemplate(dataSource);
    }

    @Override
    public Member save(Member member) {
        SimpleJdbcInsert jdbcInsert = new SimpleJdbcInsert(jdbcTemplate);
        jdbcInsert.withTableName("member").usingGeneratedKeyColumns("id");

        Map<String, Object> parameters = new HashMap<>();
        parameters.put("name", member.getName());

        Number key = jdbcInsert.executeAndReturnKey(new MapSqlParameterSource(parameters));
        member.setId(key.longValue());

        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        List<Member> result = jdbcTemplate.query("select * from member where id = ?", memberRowMapper(), id);
        return result.stream().findAny();
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = jdbcTemplate.query("select * from member where name = ?", memberRowMapper(), name);
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return jdbcTemplate.query("select * from member", memberRowMapper());
    }

    private RowMapper<Member> memberRowMapper(){
        return (rs, rowNum) -> {
            Member member = new Member();
            member.setId(rs.getLong("id"));
            member.setName(rs.getString("name"));
            return member;
        };
    }
}

```

- `jdbcTemplate.query()`와 `RowMapper`를 통해 순수 JDBC보다 코드가 간략해진 걸 볼 수 있다.
  - `jdbcTemplate.query()` : JdbcTemplate에서 `SELECT` 쿼리문을 다룰 때 사용되며, `INSERT`, `DELETE`, `UPDATE` 쿼리문들은 `jdbcTemplate.update()`를 사용한다.
  - `RowMapper` : row 단위로 ResultSet의 row를 매핑하기 위해 JdbcTemplate에서 사용하는 인터페이스

### JdbcTemplate을 사용하도록 스프링 설정 변경

이제 SpringConfig에서 스프링빈을 등록할 때 `JdbcMemberRepository`이 아닌 `JdbcTemplateMemberRepository`로 변경해주자
<p align=center>
<img width="497" alt="스크린샷 2024-01-10 18 12 40" src="https://github.com/hamsangjin/TIL/assets/103736614/abb55970-327f-44de-b35c-190f6d590bae">
</p>

</br>

그 후, **테스트**해보면 정상적으로 동작하는 것을 볼 수 있다.

<p align=center>
<img width="376" alt="스크린샷 2024-01-10 18 10 35" src="https://github.com/hamsangjin/TIL/assets/103736614/b9840a61-2eb7-46d2-95d8-3f7490db94d4">
</p>

</br>

---

</br>

## JPA

> JPA(Java Persisitence API) : 자바 진영의 ORM(Object-Relational Mapping) 기술 표준으로 사용되는 인터페이스의 모음이며, ORM의 표준 기술이다. </br>
> 즉, 실제적으로 구현된것이 아니라 구현된 클래스와 매핑을 해주기 위해 사용되는 프레임워크이며, JPA를 구현한 대표적인 오픈소스로는 Hibernate가 있다. </br>

- `ORM(Object-Relational Mapping)` : 객체와 RDB(Relational DataBase)의 테이블을 매핑한다는 뜻

### build.gradle 파일에 JPA, h2 데이터베이스 관련 라이브러리 추가


**jpa 라이브러리**는 **jdbc**를 포함하므로 **jdbc 라이브러리** 부분을 주석처리해줬다.
- **jpa**는 `EntityManager`로 모든 게 동작하는데, 이렇게 라이브러리에 **jpa**를 등록하면 스프링부트가 자동으로 `EntityManager`를 생성해주며 데이터베이스와 연결까지 해준다.

- build.gradle
```
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
	implementation 'org.springframework.boot:spring-boot-starter-web'
//	implementation 'org.springframework.boot:spring-boot-starter-jdbc'
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
	runtimeOnly 'com.h2database:h2'
	testImplementation 'org.springframework.boot:spring-boot-starter-test'
}
```

</br>

### 스프링 부트에 JPA 설정 추가
- application.properties
```
spring.jpa.show-sql=true
spring.jpa.hibernate.ddl-auto=none
```

- `spring.jpa.show-sql=true` : JPA가 생성하는 SQL을 출력
- `spring.jpa.hibernate.ddl-auto=none` : JPA는 테이블을 자동으로 생성하는 기능을 제공하는데 `none`를 사용하면 해당 기능을 끈다.
  - `create`를 사용하면 엔티티 정보를 바탕으로 테이블도 직접 생성해준다.

</br>

### JPA 엔티티 매핑

- main/java/hello/hello-spring/domain/Member.java
```java
package hello.hellospring.domain;

import jakarta.persistence.*;

@Entity
public class Member {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

// 만약 DB에 컬럼명이 username이라면 아래와 같이 어노테이션 붙여준다
//    @Column(name = "username")
    private String name;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

- `@Entity`를 붙여주면 jpa가 관리하는 entity가 되고, pk(기본 키)를 `@Id` 어노테이션을 이용해 매핑시켜준다.
- 현재 pk인 `id`는 DB에서 자동 생성해주고 있는데 이 전략을 `IDENTITY 전략`이라고 한다.
- 따라서, `@GeneratedValue(strategy = GenerationType.IDENTITY)`를 붙여준다.

</br>

### JPA 회원 리포지토리

- main/java/hello/hello-spring/repository/JpaMemberRepository.java
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import jakarta.persistence.EntityManager;

import java.util.List;
import java.util.Optional;

public class JpaMemberRepository implements MemberRepository{

    private final EntityManager em;

    public JpaMemberRepository(EntityManager em) {
        this.em = em;
    }

    @Override
    public Member save(Member member) {
        em.persist(member);
        return member;
    }

    @Override
    public Optional<Member> findById(Long id) {
        Member member = em.find(Member.class, id);
        return Optional.ofNullable(member);
    }

    @Override
    public Optional<Member> findByName(String name) {
        List<Member> result = em.createQuery("select m from Member m where m.name = :name", Member.class)
                .setParameter("name", name)
                .getResultList();
        return result.stream().findAny();
    }

    @Override
    public List<Member> findAll() {
        return em.createQuery("select m from Member m", Member.class)
                .getResultList();
    }
}

```

- **`save()`** : `em.persist()` 메소드를 이용하면 `jpa`가 삽입 쿼리문을 다 만들어서 db에 반영하고, 또, 엔티티에 매핑해놓은 정보로 `id`도 자동으로 만들어준다.
- **`findById()`** : 현재 **pk값은 id**이기 때문에 `em.find(조회할 타입, 식별자(pk값))` 메소드를 사용해 조회한다. `optional`로 반환을 하기 때문에 값이 없을 수도 있으므로, `ofNullable()`를 활용한다.
- **`findByNames()`, `findAll()`**
  - 단순히 특정 pk값의 객체를 조회할 때 `em.find()`를 사용했지만, 만약 조건이 더 복잡한 조회를 해야한다면 **JPQL**을 사용해야한다.
  - **`JPQL`** : **객체지향**적으로 쿼리문을 작성할 수 있게 도와주는 **JPA가 제공하는 객체 지향 쿼리 언어**
    - **JPQL**의 문법은 **SQL**과 비슷하지만, **엔티티 객체**를 대상으로 한다는 것이 다르며 JPQL은 특정 데이터베이스의 **SQL에 의존하지 않는다.**
  - `getResultList()` : 결과를 리스트로 반환하고 없다면 빈 리스트를 반환하므로 `null`이 반환되더라도 상관없다.
  - `setParameter()` : 파라미터로 쓰고싶은 변수 앞에 `:`를 붙여주고, 해당 변수에 값을 설정해준다.

</br>

### 서비스 계층에 트랜잭션 추가

- main/java/hello/hello-spring/service/MemberService.java
```java
import org.springframework.transaction.annotation.Transactional

@Transactional
public class MemberService {
    ...
}
```

- 스프링은 해당 클래스의 메소드를 실행할 때 트랜잭션을 시작하고, 메소드가 정상 종료되면 트랜잭션을 커밋한다. 만약 런타임 예외가 발생하면 롤백한다.
- 따라서, JPA를 통한 모든 데이터 변경은 트랜잭션 안에서 실행해야 한다.

</br>

### JPA를 사용하도록 스프링 설정 변경

늘 했던 것처럼, 스프링 설정을 변경해주자.

- SpringConfig.java
```
package hello.hellospring.service;

import hello.hellospring.repository.*;
import hello.hellospring.service.MemberService;
import jakarta.persistence.EntityManager;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class SpringConfig {

//    private final DataSource dataSource;
//
//    @Autowired
//    public SpringConfig(DataSource dataSource) {
//        this.dataSource = dataSource;
//    }

    private EntityManager em;

    public SpringConfig(EntityManager em) {
        this.em = em;
    }

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository(){
//        return new MemoryMemberRepository();
//        return new JdbcMemberRepository(dataSource);
//        return new JdbcTemplateMemberRepository(dataSource);
        return new JpaMemberRepository(em);
    }
}
```

- 이제는 **DataSourece**가 아닌 **EntityManager**를 필요로 해 **EntityManager**로 의존성 주입을 해준다.

</br>

---

</br>

## 스프링 데이터 JPA

> 스프링 부트와 JPA만 사용을 해도 개발 생산성이 많이 증가하고, 개발해야 할 코드도 많이 줄어든다. </br>
> 거기에 스프링 데이터 JPA를 사용하면 구현 클래스가 없어도 인터페이스만으로 개발을 할 수 있다. </br>
> 그리고, 반복 개발했었던 기본 CRUD 기능도 스프링 데이터 JPA가 모두 제공을 하므로, 개발자는 핵심 비즈니스 로직을 개발하는데 집중할 수 있게된다. </br>

### 스프링 데이터 JPA 회원 리포지토리

설정은 JPA 설정을 그대로 하면 된다.

- main/java/hello/hello-spring/repository/SpringDataJpaMemberRepository.java
```java
package hello.hellospring.repository;

import hello.hellospring.domain.Member;
import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface SpringDataJpaMemberRepository extends JpaRepository<Member,
Long>, MemberRepository {
	Optional<Member> findByName(String name);
}
```

- 인터페이스가 인터페이스를 받을 땐 `implements`가 아닌 `extends`로 하며, 인터페이스는 다중 상속이 된다.
- 스프링 데이터 JPA가 **JPA 리포지토리**를 받고있으면, 구현체를 자동으로 만들어 스프링 빈에 등록해준다.

</br>

### 스프링 데이터 JPA 회원 리포지토리를 사용하도록 스프링 설정 변경

- SpringConfig.java

```java
package hello.hellospring.service;

import hello.hellospring.repository.*;
import hello.hellospring.service.MemberService;
import jakarta.persistence.EntityManager;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import javax.sql.DataSource;

@Configuration
public class SpringConfig {

    // 아래와 같이 작성하면 스프링 데이터 JPA가 만들어놓은 구현체가 등록되고,
    // 아래 MemberService에 의존관계를 세팅해주면 됨
    private final MemberRepository memberRepository;

    // 생성자가 하나라 어노테이션 생략 가능함
    @Autowired
    public SpringConfig(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Bean
    public MemberService memberService() {
//        return new MemberService(memberRepository());
        return new MemberService(memberRepository);
    }
}
```

- 스프링 데이터 **JPA**가 `SpringDataJpaMemberRepository.java`를 **스프링 빈**으로 자동 등록해준다.(프록시 기술)

</br>

### 스프링 데이터 JPA 제공 클래스
<p align=center>
<img width="610" alt="스크린샷 2024-01-12 15 40 58" src="https://github.com/hamsangjin/TIL/assets/103736614/33481f58-7eb7-475f-b91d-54b0eda3c27a">
</p>

</br>

### 스프링 데이터 JPA 제공 기능

위 이미지를 보고 JPA 제공 기능을 알아보자.

- 인터페이스를 통한 기본적인 CRUD
- `findByName()`, `findByEmail()`처럼 메소드 이름만으로 조회 기능 제공
  - 공통적인 메소드들은 사용할 수 있지만, 내가 개발하려는 게 쇼핑몰이라고 가정했을 때, 주문 번호로 조회하려고 한다면 공통적인 메소드 사용이 어렵다.
  - 이때 **메소드 이름**과 **메소드 매개변수**만 변경해준다면 다르게 사용이 가능하다.
- 페이징 기능 자동 제공

> 복잡한 동적 쿼리는 **Querydsl**이라는 라이브러리를 사용하면 되고, 아니면 네이티브 쿼리를 사용하면 된다.

</br>

---

</br>
