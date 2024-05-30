# Spring Data JPA((Java Persistence API))
`Spring Data JPA`는 Spring 프레임워크의 일부로, 자바 개발자들이 관계형 데이터베이스의 데이터 접근을 더욱 용이하게 할 수 있도록 설계했다.

## 주요 기능 및 장점
1. `리포지토리 추상화`: 리포지토리라는 개념을 사용하여 CRUD 연산을 위한 공통 인터페이스를 제공
2. `쿼리 메서드 생성`: 메서드 이름만으로 쿼리를 생성할 수 있는 기능을 제공
3. `@Query 어노테이션`: 복잡한 SQL 쿼리 또는 JPQL 쿼리를 메서드에 어노테이션으로 직접 정의
4. `페이징과 정렬`: 페이징과 정렬을 위한 기능을 추상화하여, 복잡한 쿼리를 쉽게 페이징하고 정렬할 수 있도록 지원
5. `명시적 트랜잭션 관리`: @Transactional 어노테이션을 사용하여 선언적 트랜잭션 관리를 지원

<br>

## Spring Data JDBC와 Spring Data JPA의 차이점
- `Spring Data JDBC`는 JDBC에 대한 더 간단하고 직접적인 접근을 제공하며, 도메인 모델을 데이터베이스에 1:1로 매핑한다.
- 이는 JPA에 비해 더 가벼운 접근 방식을 제공하고, 애플리케이션의 성능을 최적화하는 데 도움을 줄 수 있다.

<br>

- `Spring Data JPA`는 Java Persistence API를 사용하여, 도메인 모델과 데이터베이스 간의 복잡한 매핑과 데이터 관리를 처리한다. 
- 이는 객체 관계 매핑(ORM)의 모든 이점을 제공하지만, 설정과 구현이 더 복잡할 수 있다.


<br>

## Spring Data JPA 예제 프로젝트

### application.yml
```yml
spring:
  application:
    name: springdatajpa

  datasource:
    url: jdbc:mysql://localhost:3306/examplesdb
    username: 유저명
    password: 비밀번호
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
```

### User 엔티티
```java
package hello.springdatajpa.domain;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "jpa_user")
@Getter @Setter
@NoArgsConstructor
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
}
```

### UserRepository 인터페이스
```java
package hello.springdatajpa.repository;

import hello.springdatajpa.domain.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository extends JpaRepository<User, Long>{
}
```

### UserRepository 테스트
```java
package hello.springdatajpa.repository;

import hello.springdatajpa.domain.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

@SpringBootTest
@Transactional      // 테스트를 할 때 db에 추가, 수정, 삭제를 하더라도 각각의 테스트를 할 때마다 롤백을 해준다.
class UserRepositoryTest {
    @Autowired
    private UserRepository userRepository;

    @Test
    void save() {
        User user = new User("kim", "kim@exam.com");
        userRepository.save(user);

        assertThat(userRepository.findById(user.getId()).orElseThrow()).isEqualTo(user);
    }

    @Test
    void deleteById(){
        userRepository.deleteById(6L);
        assertThat(userRepository.findById(6L)).isEmpty();
    }
}
```

<br>

---

<br>

# 쿼리 메소드

## 쿼리 메소드 작성 방법
Spring Data JPA의 쿼리 메서드는 조회 작업에 국한되지 않고, 조회 외에도 입력, 수정, 삭제 작업을 수행할 수 있는 메서드를 정의할 수 있다.

이는 `@Modifying` 어노테이션과 함께 `@Query` 어노테이션을 사용하여 쿼리를 명시적으로 정의함으로써 가능하다.

### 쿼리 메소드 예제
- UserRepository 인터페이스
```java
import hello.springdatajpa.domain.User;
import org.springframework.data.jpa.repository.*;
import org.springframework.data.repository.query.Param;

import java.util.List;

public interface UserRepository extends JpaRepository<User, Long>{
    List<User> findByName(String name);
    User findByEmail(String email);
    List<User> findByNameAndEmail(String name, String email);
    List<User> findByNameOrEmail(String name, String email);
    
    @Modifying  // @Query 어노테이션을 통해 작성된 변경이 일어나는 쿼리(INSERT, DELETE, UPDATE )를 실행할 때 사용
    @Query("UPDATE User u SET u.email = :email WHERE u.id = :id")       // 해당 쿼리문을 실행하는 쿼리 메소드 생성
    int updateUserEmail(@Param("id") Long id, @Param("email") String email);

    @Modifying
    @Query("DELETE FROM User u WHERE u.email = :email")
    void deleteByEmail(String email);
}

```

- UserRepository 테스트
```java
package hello.springdatajpa.repository;

import hello.springdatajpa.domain.User;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;

import java.util.*;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

@SpringBootTest
@Transactional      // 테스트를 할 때 db에 추가, 수정, 삭제를 하더라도 각각의 테스트를 할 때마다 롤백을 해준다.
class UserRepositoryTest {
    @Autowired
    private UserRepository userRepository;

    @Test
    void save() {
        User user = new User("kim", "kim@exam.com");
        userRepository.save(user);

        assertThat(userRepository.findById(user.getId()).orElseThrow()).isEqualTo(user);
    }

    @Test
    void deleteById(){
        userRepository.deleteById(6L);
        assertThat(userRepository.findById(6L)).isEmpty();
    }

    @Test
    void findByname(){
        List<User> users1 = userRepository.findByName("sangjin");
        assertThat(users1.size()).isEqualTo(1);

        List<User> users2 = userRepository.findByName("hamsangjin");
        assertThat(users2.size()).isEqualTo(1);
    }

    @Test
    void findByEmail(){
        String email1 = "ham@sang.jin";
        User user1 = userRepository.findByEmail(email1);
        assertThat(user1.getEmail()).isEqualTo(email1);

        String email2 = "new@sang.jin";
        User user2 = userRepository.findByEmail(email2);
        assertThat(user2.getEmail()).isEqualTo(email2);
    }

    @Test
    void findByNameAndEmail(){
        List<User> users1 = userRepository.findByNameAndEmail("hamsangjin", "ham@sang.jin");
        assertThat(users1.size()).isEqualTo(1);

        List<User> users2 = userRepository.findByNameAndEmail("newsangjin", "new@sang.jin");
        assertThat(users2.size()).isEqualTo(1);
    }

    @Test
    void findByNameOrEmail(){
        List<User> users1 = userRepository.findByNameOrEmail("error", "ham@sang.jin");
        assertThat(users1.size()).isEqualTo(1);

        List<User> users2 = userRepository.findByNameOrEmail("hamsangjin", "error");
        assertThat(users2.size()).isEqualTo(1);
    }

    @Test
    void updateUserEmail(){
        userRepository.updateUserEmail(4L, "update@sang.jin");

        Optional<User> user = userRepository.findById(4L);

        assertThat(user.get().getEmail()).isEqualTo("update@sang.jin");
    }

    @Test
    void deleteByEmail(){
        userRepository.deleteByEmail("update@sang.jin");

        assertThat(userRepository.findByEmail("update@sang.jin")).isNull();
    }
}
```

<br>

## @Query 어노테이션 사용법
`@Query` 어노테이션은 `Spring Data JPA`에서 제공하는 강력한 기능으로, 개발자가 리포지토리 인터페이스 메서드에 `JPQL` 또는 `SQL` 쿼리를 직접 정의할 수 있게 해준다.

- `JPQL`: 엔티티 객체를 대상으로 하는 쿼리 언어로, 데이터베이스 테이블이 아닌 엔티티 클래스와 그 필드를 기반으로 쿼리를 작성한다.
- `SQL`: 데이터베이스에 직접 작성하는 네이티브 쿼리 언어로, 플랫폼에 특화된 SQL을 사용할 수 있어 특정 데이터베이스 기능을 최대로 활용할 수 있다.

### JPQL의 Select 문법

#### 기본 형식
```sql
SELECT <projection>
FROM <entity_name> [AS] <alias>
[WHERE <condition>]
[GROUP BY <grouping_expression>]
[HAVING <having_condition>]
[ORDER BY <order_expression>]
```
- `<projection>`: 반환할 필드나 객체를 지정한다. 특정 필드만 선택하거나, 엔티티 객체 전체를 반환할 수 있다.
- `<entity_name> [AS] <alias>`: 쿼리 대상 엔티티와, 선택적으로 사용할 수 있는 별칭을 지정한다.
- `[WHERE <condition>]`: 결과를 필터링하는 조건을 지정한다.
- `[GROUP BY <grouping_expression>]`: 결과를 그룹화하는 기준을 지정한다. 
- `[HAVING <having_condition>]`: 그룹화된 결과에 대해 필터를 적용한다. 
- `[ORDER BY <order_expression>]`: 결과를 정렬하는 기준을 지정한다.

#### 예제
```sql
SELECT u FROM User u                                               # 모든 인스턴스를 조회
SELECT u FROM User u WHERE u.age > 18                              # 18세 이상의 사용자만 조회
SELECT u.name, u.email FROM User u                                 # 사용자의 이름과 이메일만 조회
SELECT u FROM User u JOIN u.orders o WHERE o.amount > 100          # 100달러 이상 주문한 사용자를 조회
SELECT AVG(u.age) FROM User u GROUP BY u.gender                    # 성별에 따른 사용자의 평균 나이를 계산
SELECT u, p FROM User u, Profile p WHERE u.id = p.user.id          # 사용자와 그들의 프로필을 함께 조회
```

#### 기본 사용법
```java
import org.springframework.data.jpa.repository.*;
import org.springframework.data.repository.query.Param;
import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {
    @Query("SELECT u FROM User u WHERE u.email LIKE %:email%")                 // 이름 기반 바인딩(:email)
    List<User> findByEmailContaining(@Param("email") String email);

    @Query(value = "SELECT * FROM users WHERE name = ?1", nativeQuery = true)  // 위치 기반 바인딩(?1)
    List<User> findByName(String name);

    // 수정 쿼리: @Query 어노테이션과 함께 @Modifying 어노테이션을 사용하여 데이터를 변경하는 쿼리를 실행할 수 있다
    @Modifying
    @Transactional
    @Query("UPDATE User u SET u.email = :email WHERE u.id = :id")
    void updateUserEmail(@Param("id") Long id, @Param("email") String email);
}
```

<br>

## @Query 어노테이션 예제

- 테이블 생성
```sql
CREATE TABLE jpa_customer (
                              id BIGINT NOT NULL AUTO_INCREMENT,
                              name VARCHAR(255),
                              email VARCHAR(255),
                              age INT,
                              PRIMARY KEY (id)
);

CREATE TABLE jpa_order (
                           id BIGINT NOT NULL AUTO_INCREMENT,
                           product VARCHAR(255),
                           date DATE,
                           customer_id BIGINT,
                           PRIMARY KEY (id),
                           FOREIGN KEY (customer_id) REFERENCES jpa_customer(id)
);
```

- 테이블 삽입
```sql
INSERT INTO jpa_customer (name, email, age) VALUES ('홍길동', 'hong@example.com', 30);
INSERT INTO jpa_customer (name, email, age) VALUES ('김철수', 'kim@example.com', 25);
INSERT INTO jpa_customer (name, email, age) VALUES ('이영희', 'lee@example.com', 27);
INSERT INTO jpa_customer (name, email, age) VALUES ('박민수', 'park@example.com', 35);
INSERT INTO jpa_customer (name, email, age) VALUES ('최지현', 'choi@example.com', 40);
INSERT INTO jpa_customer (name, email, age) VALUES ('정수현', 'jung@example.com', 22);
INSERT INTO jpa_customer (name, email, age) VALUES ('강지훈', 'kang@example.com', 28);
INSERT INTO jpa_customer (name, email, age) VALUES ('윤서준', 'yoon@example.com', 33);
INSERT INTO jpa_customer (name, email, age) VALUES ('오지영', 'oh@example.com', 29);
INSERT INTO jpa_customer (name, email, age) VALUES ('임도연', 'lim@example.com', 31);

INSERT INTO jpa_order (product, date, customer_id) VALUES ('노트북', '2024-05-01', 1);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('스마트폰', '2024-05-02', 1);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('태블릿', '2024-05-03', 2);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('키보드', '2024-05-04', 2);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('마우스', '2024-05-05', 3);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('모니터', '2024-05-06', 4);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('프린터', '2024-05-07', 5);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('헤드셋', '2024-05-08', 6);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('웹캠', '2024-05-09', 7);
INSERT INTO jpa_order (product, date, customer_id) VALUES ('스피커', '2024-05-10', 8);
```

- Customer 엔티티
```java
package hello.springdatajpa.domain;

import jakarta.persistence.*;
import lombok.*;
import java.util.List;

@Entity
@Table(name = "jpa_customer")
@Getter @Setter
@NoArgsConstructor
public class Customer {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private String email;

    private int age;

    @OneToMany(mappedBy = "customer", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Order> orders;

    public Customer(String name, String email) {
        this.name = name;
        this.email = email;
    }
}
```

- Order 엔티티
```java
package hello.springdatajpa.domain;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDate;

@Entity
@Table(name = "jpa_order")
@Getter @Setter
@NoArgsConstructor
public class Order {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String product;

    private LocalDate date;

    @ManyToOne
    @JoinColumn(name = "customer_id")
    private Customer customer;

    public Order(String product, LocalDate date, Customer customer) {
        this.product = product;
        this.date = date;
        this.customer = customer;
    }
}
```

- CustomerRepository 인터페이스
```java
package hello.springdatajpa.repository;

import hello.springdatajpa.domain.Customer;
import org.springframework.data.jpa.repository.*;

import java.util.List;

public interface CustomerRepository extends JpaRepository<Customer, Long> {
    List<Customer> findByName(String name);
    List<Customer> findByEmail(String email);
    List<Customer> findByEmailContaining(String email);
    // 각 고객과 고객의 주문 수
    @Query("SELECT c, count(o) FROM Customer c LEFT JOIN c.orders o GROUP BY c")
    List<Object[]> findCustomerOrders();
}

```

- CustomerRepository 테스트
```java
package hello.springdatajpa.repository;

import hello.springdatajpa.domain.Customer;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;

import java.util.*;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

@SpringBootTest
@Transactional
class CustomerRepositoryTest {
    @Autowired
    CustomerRepository customerRepository;

    @Test
    void save() {
        Customer customer = new Customer("hamsangjin", "ham@sang.jin");
        customerRepository.save(customer);

        assertThat(customerRepository.findById(11L).orElseThrow().getName()).isEqualTo(customer.getName());
    }

    @Test
    void delete(){
        customerRepository.deleteById(10L);
        assertThat(customerRepository.findById(10L)).isNotPresent();
    }

    @Test
    void findByName(){
        List<Customer> customers = customerRepository.findByName("hamsangjin");

        assertThat(customers).hasSize(4);
    }

    @Test
    void findByEmail(){
        List<Customer> customers = customerRepository.findByEmail("ham@sang.jin");

        assertThat(customers).hasSize(4);
    }

    @Test
    void findByEmailContaining(){
        List<Customer> customers = customerRepository.findByEmailContaining("ham");
        assertThat(customers).hasSize(4);
    }

    @Test
    void findCustomerOrders(){
        List<Object[]> customerOrders = customerRepository.findCustomerOrders();
        customerOrders.forEach(result -> {
            Customer customer = (Customer) result[0];
            Long count = (Long) result[1];
            System.out.println(customer.getName() + " " + count);
        });
    }
}
```
