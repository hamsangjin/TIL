# Spring Data JDBC

## Spring JDBC 동작 과정
`Spring JDBC`는 데이터베이스 작업을 간소화하고 개발자가 직접 JDBC API를 사용하여 복잡한 작업을 처리하는 것을 피할 수 있다.<br>
각 Bean의 설정과 생성은 애플리케이션의 구성에 따라 자동으로 처리되며, Spring Boot의 자동 구성 기능 덕분에 별도의 구성 작업 없이도 쉽게 사용할 수 있다.

### 1. DataSource Bean의 생성
- `역할`: 데이터베이스 연결을 관리하고, `DataSource`는 데이터베이스 연결을 제공하는 객체로, 모든 데이터베이스 작업의 기초다.
- `생성 시기`: Spring Boot 애플리케이션이 시작될 때, DataSource가 생성되며, 이는 `application.properties` 혹은 `application.yml` 파일에서 정의된 데이터베이스 연결 정보를 바탕으로 설정된다.

### 2. JdbcTemplate Bean의 생성
- `역할`: SQL 쿼리 실행을 쉽게 만드는 Spring JDBC의 핵심 클래스이며, `DataSource`를 사용하여 데이터베이스 작업을 수행하며, 예외 처리, 결과 세트 처리, 연결 관리 등을 자동화한다. 
- `생성 시기`: `DataSource` 이후, 또는 설정을 통해 `DataSource JdbcTemplate Bean`이 생성되고, 이 Bean은 생성자 객체를 주입받아 초기화된다.

### 3. TransactionManager Bean의 설정(선택적)
- `역할`: 트랜잭션 관리를 담당합니다. 데이터베이스 작업의 일관성과 무결성을 보장하기 위해 사용된다.
- `생성 시기`: 필요한 경우, `TransactionManager Bean`도 초기화되고, 트랜잭션 관리가 필요한 경우, DataSource를 사용하여 트랜잭션 경계를 관리한다.

### 4. 쿼리 실행 과정
1. `애플리케이션 시작`: `SpringApplication.run()` 메소드 호출로 애플리케이션이 시작된다.
2. `Bean 생성`: `DataSource`, `JdbcTemplate`, 그리고 필요에 따라 `TransactionManager`가 생성된다.
3. `쿼리 실행`: `JdbcTemplate`을 사용하여 SQL 쿼리를 실행하고 예를 들어, `jdbcTemplate.query()` 또는 `jdbcTemplate.update()` 메소드를 사용할 수 있다.
4. `결과 처리`: `JdbcTemplate`은 `RowMapper`를 통해 SQL 쿼리의 결과를 자바 객체로 매핑한다.
5. `트랜잭션 관리`: 필요한 경우, `TransactionManager`를 사용하여 트랜잭션을 시작, 커밋 또는 롤백한다.

<br>

## DAO(Data Access Object)
`DAO`는 데이터베이스 또는 다른 저장소와의 상호작용을 캡슐화하는 디자인 패턴이다. 

DAO 패턴의 주요 목적은 애플리케이션의 나머지 부분과 데이터베이스 간의 상호작용을 분리하는 것으로, 이를 통해 데이터베이스 액세스 메커니즘과 비즈니스 로직 사이의 결합도를 낮추고 코드의 유지관리를 용이하게 한다.

### 구성 요소
1. `DAO 인터페이스`: 데이터 액세스를 위한 메서드를 정의하고, 인터페이스를 사용함으로써, 구현체가 변경되어도 인터페이스를 사용하는 코드를 수정할 필요가 없어 코드의 유연성이 증가한다.
2. `DAO 클래스`: 인터페이스를 구현하는 클래스로, 실제로 데이터베이스에 접근하여 데이터를 쿼리하거나 업데이트하는 작업을 수행하며, JDBC, JPA, Hibernate 등 다양한 데이터베이스 액세스 기술을 사용하여 구현될 수 있다.
3. `데이터 전송 객체(DTO)`: 때때로 데이터베이스의 데이터를 서비스 레이어로 전송하는 데 사용되는 객체를 DTO(데이터 전송 객체)라고 하고, DTO는 데이터베이스 테이블의 데이터를 단순히 전달하는 역할을 하며, 여러 DAO 메서드 간에 데이터를 캡슐화하여 전달하는 데 사용된다.

### 장점
1. `분리와 추상화`: 데이터 액세스 코드를 비즈니스 로직에서 분리함으로써, 데이터베이스 연산과 비즈니스 로직 사이의 결합도를 낮추고 각각 독립적으로 개발하고 유지 보수할 수 있다.
2. `재사용성 및 교체 용이성`: 데이터 액세스 로직을 중앙 집중화하여 코드의 중복을 줄이고, 필요에 따라 다른 데이터베이스 액세스 기술로 쉽게 교체할 수 있다.
3. `테스트 용이성`: 데이터베이스 액세스 로직을 캡슐화하므로, DAO를 모의 객체로 대체하여 비즈니스 로직의 단위 테스트를 수행하기가 더 쉬워진다.

### 예제
- `User.java`
```java
import lombok.*;

@AllArgsConstructor
@Getter
public class User {
    private Long id;
    private String name;
    private String email;
}
```

- `UserDao.java`
```java
package org.example.jdbc02.dao;

import java.util.List;

public interface UserDao {
    void insertUser(User user);
    List<User> findAllUsers();
    void updateUserEmail(String name, String email);
    void deleteUser(String name);
}
```

- `UserDaoImpl.java`
```java
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.BeanPropertyRowMapper;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;
import java.util.List;

@Repository
// 4. 어노테이션을 이용한 주입
@RequiredArgsConstructor  // -> final이 붙어있는 필드에게 의존성 주입을 해줌
public class UserDaoImpl implements UserDao {

    private final JdbcTemplate jdbcTemplate;

    // 1. 필드 주입
    // @Autowired
    // private JdbcTemplate jdbcTemplate;

    // 2. setter 주입
    // @Autowired
    // public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
    //      this.jdbcTemplate = jdbcTemplate;
    // }

    // 3. 생성자 주입
    // public UserDaoImpl(JdbcTemplate jdbcTemplate) {
    //     this.jdbcTemplate = jdbcTemplate;
    // }

    // Create
    @Override
    public void insertUser(User user) {
        jdbcTemplate.update("INSERT INTO users (name, email) VALUES (?, ?)", user.getName(), user.getEmail());
    }

    // Read
    // new BeanPropertyRowMapper<>(User.class)를 사용하면 기본 생성자를 호출하고, 매개변수들을 setter로 통해 설정하는 방식이라 기본 생성자가 있어야함
    @Override
    public List<User> findAllUsers() {
        List<User> users = jdbcTemplate.query("SELECT id, name, email FROM users", new BeanPropertyRowMapper<>(User.class));
        return users;
    }

    // RowMapper는 해당 매개변수를 바로 생성자에 넣기 때문에 기본 생성자를 사용하는 게 아니라 기본 생성자가 필수가 아님
    // @Override
    // public List<User> findAllUsers() {
    //     RowMapper<User> rowMapper = (rs, rowNum) -> new User(
    //         rs.getLong("id"),
    //         rs.getString("name"),
    //         rs.getString("email")
    //     );
    //     return jdbcTemplate.query("SELECT * FROM users", rowMapper);
    // }

    // Update
    @Override
    public void updateUserEmail(String name, String email) {
        jdbcTemplate.update("UPDATE users SET email = ? WHERE name = ?", email, name);
    }

    // Delete
    @Override
    public void deleteUser(String name) {
        jdbcTemplate.update("DELETE FROM users WHERE name = ?", name);
    }
}
```

- `SpringJDBCApplication02.java`
```java
import org.example.jdbc02.dao.User;
import org.example.jdbc02.dao.UserDao;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class SpringJDBCApplication02 {
    public static void main(String[] args) {
        SpringApplication.run(SpringJDBCApplication02.class, args);
    }

    @Bean
    public CommandLineRunner demo(UserDao userDao) {
        return args -> {
            // Create Test
            userDao.insertUser(new User(null, "ham", "ham@sangjin.com"));

            // Read Test
            userDao.findAllUsers();

            // Update Test
            userDao.updateUserEmail("ham", "ham@sangjin.com");

            // Delete Test
            userDao.deleteUser("ham");
        };
    }
}
```

<br>

## 데이터 접근 예외 처리

### DataAccessException 계층 구조
- `예외 처리의 중요성`: 데이터베이스 연산 중 예외가 발생할 경우, 적절한 예외 처리가 없다면 시스템의 안정성이 저하될 수 있으며, Spring은 이를 위해 `DataAccessException`을 포함한 일련의 체크되지 않은 예외를 제공한다.
- `Spring의 예외 추상화`: Spring JDBC는 Java의 JDBC에서 발생하는 다양한 예외를 추상화하여 `DataAccessException` 하위 클래스로 매핑하고, 이를 통해 데이터베이스 브랜드에 구애받지 않고 일관된 방식으로 예외를 처리할 수 있다.
- `주요 예외 클래스`: `DataAccessException`의 여러 하위 클래스

### 사용자 정의 예외 처리
- `사용자 정의 예외 클래스 구현`: 애플리케이션 특정 로직에 맞는 사용자 정의 예외 클래스를 만들 수 있다.
- `예외 전파`: 필요한 경우 예외를 적절히 전파하여, 호출 스택에서 상위 레벨에서 예외를 처리할 수 있도록 한다.
- `AOP를 활용한 예외 처리`: `Aspect-Oriented Programming(AOP)`를 사용하여 애플리케이션 전반에 걸쳐 일관된 예외 처리 로직을 구현할 수 있다.

### 예제
- `UserNotFoundException.java`
```java
package org.example.jdbc03;

public class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(String message) {
        super(message);
    }
}
```

- `UserDaoImpl.java`
```java
import lombok.RequiredArgsConstructor;
import org.example.jdbc03.UserNotFoundException;
import org.springframework.dao.DataAccessException;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;
import org.springframework.stereotype.Repository;

import java.util.List;

@Repository
@RequiredArgsConstructor
public class UserDaoImpl implements UserDao {

    private final JdbcTemplate jdbcTemplate;

    // Create
    @Override
    public void insertUser(User user) {
        try {
            jdbcTemplate.update("INSERT INTO users (name, email) VALUES (?, ?)", user.getName(), user.getEmail());
        } catch (DataAccessException e) {
            throw new DataAccessException("사용자 삽입 오류" + user.getName(), e){};
        }
    }

    // Read
     @Override
     public List<User> findAllUsers() {
         try {
             RowMapper<User> rowMapper = (rs, rowNum) -> new User(
                 rs.getLong("id"),
                 rs.getString("name"),
                 rs.getString("email")
             );
             return jdbcTemplate.query("SELECT * FROM users", rowMapper);
         } catch (DataAccessException e) {
             throw new DataAccessException("사용자 조회 오류", e){};
         }
     }

    // Update
    @Override
    public void updateUserEmail(String name, String email) {
        int update = jdbcTemplate.update("UPDATE users SET email = ? WHERE name = ?", email, name);
        if (update == 0) throw new UserNotFoundException(name);
    }

    // Delete
    @Override
    public void deleteUser(String name) {
        int delete = jdbcTemplate.update("DELETE FROM users WHERE name = ?", name);
        if (delete == 0) throw new UserNotFoundException(name);
    }
}
```

<br>

## Repository Interface

### 인터페이스 정의 및 확장
`CrudRepository`는 Spring Data의 핵심 인터페이스 중 하나로, `CRUD(Create, Read, Update, Delete)` 관련 기본적인 메소드를 제공한다.

이후 정의할 `UserRepository` 인터페이스를 CrudRepository 인터페이스를 상속받아 구현할 수 있다.

### 제네릭 타입
CrudRepository의 두 가지 제네릭 타입
- `T`: 엔티티 타입을 의미하며, 이 경우 User 클래스가 엔티티 타입
- `ID`: 엔티티의 ID 필드 타입을 의미하며, 이 경우 Integer 가 ID의 타입

### 기본 제공 메소드
`CrudRepository`를 확장하면 다음과 같은 메소드를 자동으로 상속받는다.
- `save(S entity)`: 엔티티를 저장(생성 또는 업데이트)
- `findById(ID id)`: 주어진 ID로 엔티티를 조회
- `existsById(ID id)`: 주어진 ID의 엔티티 존재 여부 확인
- `findAll()` : 모든 엔티티 조회
- `count()`: 저장소에 있는 엔티티 수 계산
- `deleteById(ID id)`: 주어진 ID의 엔티티 삭제
- `deleteAll()`: 모든 엔티티 삭제

또한, 필요에 따라 커스텀 쿼리 메소드를 인터페이스에 추가하여 특정 검색 조건 또는 연산을 수행하는 메소드를 선언할 수 있으며, `Spring Data JDBC`는 메소드 이름을 분석하여 SQL 쿼리를 자동으로 생성한다.

### 쿼리 메소드 이름 규칙
쿼리 메소드의 이름은 특정 규칙을 따르며, 메소드 이름만으로도 데이터베이스에 어떤 쿼리를 수행할지 예측할 수 있고, 이 기능은 JPA의 구현체인 `Hibernate`를 통해 실행되며, Spring Data JPA가 자동으로 적절한 SQL 쿼리로 변환해 실행한다.
1. `접두어`: 일반적으로 `find...By`, `read...By`, `query...By`, `count...By`, `get...By` 등이 사용되고, 이는 쿼리의 시작점을 나타낸다.
2. `조건 표현`: `By` 이후에 오는 부분은 쿼리의 조건을 표현한다. 예를 들어 `findByName`은 `name` 필드를 기준으로 데이터를 조회한다.
3. `조건 연결`: 여러 필드를 기준으로 검색하려면 `And`와 `Or`를 사용하여 필드를 연결한다. 예를 들어, `findByNameAndEmail`은 `name`과 `email` 두 필드를 모두 만족하는 데이터를 조회한다.
4. `조건 상세화`: 특정 조건을 더 세부적으로 표현하기 위해 `Containing`, `Between`, `LessThan`, `GreaterThan`, `Like`, `In` 등의 키워드를 사용할 수 있다. 예를 들어, `findByNameLike`은 `name` 필드에서 주어진 패턴을 만족하는 데이터를 조회한다.
5. `정렬`: `OrderBy` 다음에 필드 이름과 함께 `Asc` 또는 `Desc`를 사용하여 결과의 정렬 방식을 지정할 수 있다. 예를 들어, `findAllByOrderByLastNameAsc`와 같이 사용할 수 있다.
6. `리턴 타입`: 쿼리 메소드의 리턴 타입은 유연하게 지정할 수 있다. 예를 들어, `List`, `Set`, `Stream` 등의 컬렉션 타입 또는 단일 엔티티 타입(User), Optional<User> 등이 사용될 수 있다.

### 예제
- users 테이블에 unique 제약조건 추가
```sql
alter table users
add unique(name);
```

- `User.java`
```java
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import org.springframework.data.annotation.Id;
import org.springframework.data.relational.core.mapping.Table;

@Table("users")
@NoArgsConstructor
@Getter
@Setter
// DTO는 단순히 값만 담아서 전달되는 객체라면,
// Entity는 데이터베이스와의 관계를 알려주는 역할까지 해줌
public class User {
    @Id
    private Long id;
    private String name;
    private String email;

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }
}
```

- `UserRepository.java`
```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.repository.CrudRepository;
import org.springframework.stereotype.Repository;

import java.util.List;
import java.util.Optional;

@Repository
public interface UserRepository extends CrudRepository<User, Long> {
    // 쿼리 메소드 이름 규칙을 사용한 메소드 선언
    List<User> findByName(String name);                             // 이름으로 사용자를 조회
    Optional<User> findByNameAndEmail(String name, String email);   // 이름과 이메일로 정확히 매칭되는 사용자 조회
    List<User> findByNameLike(String namePattern);                  // 이름 패턴으로 사용자를 조회
    List<User> findByAgeLessThan(int age);                          // 나이가 주어진 값보다 작은 사용자들을 조회
    List<User> findByActiveTrue();                                  // 활성화된 사용자들만 조회
    Page<User> findAllBy(Pageable pageable);                        // 페이징 처리를 포함하여 모든 사용자 조회
}
```

- `MainApplication.java`
```java
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

import java.util.List;

@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }

    @Bean
    public CommandLineRunner demo(UserRepository userRepository) {
        return args -> {
            userRepository.save(new User("ham", "ham@sang.jin"));

            // 이름으로 사용자 검색
            List<User> user = userRepository.findByName("ham");
            for(User u : user)  System.out.println("User found: " + u.getName() + ", Email: " + u.getEmail());
        };
    }
}
```
