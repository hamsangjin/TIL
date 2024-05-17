# Spring Data JDBC

## Batch 업데이트
여러 개의 SQL 문을 하나의 배치로 묶어서 실행하는 기법으로, 데이터베이스 성능을 향상시키고 네트워크 비용을 줄일 수 있다.

- `사용 시나리오`: 대량의 데이터를 데이터베이스에 삽입, 업데이트 또는 삭제해야 할 때(대용량 데이터 이전, 로그 처리 작업)

### 구현 예제
```java
import lombok.AllArgsConstructor;
import lombok.Getter;
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.jdbc.core.*;
import java.sql.*;
import java.util.*;

@SpringBootApplication
public class SpringJDBC01Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringJDBC01Application.class, args);
    }

    @Bean
    public CommandLineRunner batchUpdateDemo(JdbcTemplate jdbcTemplate) {
        return args -> {
            // 여러 사용자 정보 저장
            List<User> users = Arrays.asList(
                    new User(null, "Alice", "alice@example.com"),
                    new User(null, "Bob", "bob@example.com"),
                    new User(null, "Charlie", "charlie@example.com"),
                    new User(null, "David", "david@example.com")
            );

            // 사용자 정보를 데이터베이스에 삽입
            String sql = "INSERT INTO users (name, email) VALUES (?, ?)";

            // JdbcTemplate의 batchUpdate 메서드 : 각 배치 항목에 대한 PreparedStatement를 설정하는 데 사용
            int[] updateCounts = jdbcTemplate.batchUpdate(sql, new BatchPreparedStatementSetter() {
                public void setValues(PreparedStatement ps, int i) throws SQLException {
                    User user = users.get(i);
                    ps.setString(1, user.getName());
                    ps.setString(2, user.getEmail());
                }

                // 배치 크기: 사용자 목록의 크기
                public int getBatchSize() {
                    return users.size();
                }
            });
            System.out.println("Batch update completed. Number of rows affected: " + Arrays.toString(updateCounts));
        };
    }
}

@AllArgsConstructor
@Getter
class User {
    private Long id;
    private String name;
    private String email;
}
```

<br>

## 트랜잭션 관리

- `트랜잭션의 중요성`: 여러 작업을 그룹화하여 전체가 성공적으로 완료되거나 실패할 때 모두 롤백되는 원자성을 보장한다.
- `Spring의 선언적 트랜잭션 관리`: `@Transactional` 어노테이션을 사용하는 방법과 트랜잭션 전파 수준 설정 방법이 있다.
- `프로그래매틱 트랜잭션 관리`: 세밀한 제어가 필요한 상황에서 `TransactionTemplate`을 사용하여 트랜잭션을 관리할 수 있다.

### 선언적 트랜잭션 관리 예제
선언적 트랜잭션 관리를 위해 `@Transactional` 어노테이션을 사용해보자.

- `UserDao.java`
```java
import org.springframework.transaction.annotation.Transactional;

public interface UserDao {
    @Transactional
    void createAndUpdateUser(String name, String email, String newEmail);
}
```

- `UserDaoImpl.java`
```java
import lombok.RequiredArgsConstructor;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

@RequiredArgsConstructor
@Repository
public class UserDaoImpl implements UserDao {
    private final JdbcTemplate jdbcTemplate;

    @Override
    public void createAndUpdateUser(String name, String email, String newEmail) {
        jdbcTemplate.update("INSERT INTO users(name, email) VALUES (?, ?)", name, email);

        // Transcational 어노테이션 때문에 아래 오류가 실행되면
        // 위의 INSERT문이 실행됐다가 롤백되고 모든 작업이 취소됨
        if(newEmail.contains("error"))  throw new RuntimeException("Sumulated error");

        jdbcTemplate.update("UPDATE users SET email = ? WHERE name = ?", newEmail, name);
    }
}
```

- `SpringJDBC02Application.java`
```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class SpringJDBC02Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringJDBC02Application.class, args);
    }

    @Bean
    public CommandLineRunner transactionDemo(UserDao userDao) {
        return args -> {
            try {
                userDao.createAndUpdateUser("hamsangjin", "ham@sang.jin", "errorham@sang.jin");
            } catch (Exception e) {
                System.out.println(e.getMessage());
            }
        };
    }
}
```

<br>

### 프로그래매틱 트랜잭션 관리 예제

- `UserServer.java`
```java
package org.example.springjdbc03;

import lombok.RequiredArgsConstructor;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.transaction.support.TransactionTemplate;
import org.springframework.stereotype.Service;

@RequiredArgsConstructor
@Service
class UserService {
    private final JdbcTemplate jdbcTemplate;
    private final TransactionTemplate transactionTemplate;

    public void executeComplexBusinessProcess(String name, String email) {
        transactionTemplate.execute(status -> {
            jdbcTemplate.update("INSERT INTO users (name, email) VALUES (?, ?)", name, email);
            if (email.contains("error")) {
                // setRollbackOnly()는 속성만 변경을 하는 것이기 때문에 실제 rollback이 일어나는 시점은 commit이 되기 직전에 수행되게 된다.
                status.setRollbackOnly();
                throw new RuntimeException("Simulated error to trigger rollback");
            }
            jdbcTemplate.update("UPDATE users SET email = ? WHERE name = ?", "newham@sang.jin", name);
            return null;
        });
    }
}
```

- `SpringJDBC03Application.java`
```java
package org.example.springjdbc03;

import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.beans.factory.annotation.Autowired;

@SpringBootApplication
public class SpringJDBC03Application {

    @Autowired
    private UserService userService;

    public static void main(String[] args) {
        SpringApplication.run(SpringJDBC03Application.class, args);
    }

    @Bean
    public CommandLineRunner runApplication() {
        return args -> {
            try {
                userService.executeComplexBusinessProcess("ham", "ham@sang.jin");
                userService.executeComplexBusinessProcess("errorHam", "errorham@sang.jin"); // error 발생
            } catch (RuntimeException e) {
                System.out.println(e.getMessage());
            }
        };
    }
}
```

<br>

## RowMapper와 ResultSetExtractor

### RowMapper
`RowMapper`는 `결과 집합(ResultSet)`의 각 행을 개별 객체로 매핑하는 데 사용되며, 주로 단일 유형의 객체 목록을 반환할 때 사용되고, 결과 집합의 각 행마다 한 번씩 메소드가 호출된다.

보통 단일 타입의 객체 목록을 만들 때 사용된다.

- `SpringJDBC04Application.java`
```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.jdbc.core.*;
import java.sql.*;
import java.util.*;

@SpringBootApplication
public class SpringJDBC04Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringJDBC04Application.class, args);
    }

    @Bean
    public CommandLineRunner demo(JdbcTemplate jdbcTemplate) {
        return args -> {
            String sql = "SELECT id, name, email FROM users";
            List<User> users = jdbcTemplate.query(sql, new UserRowMapper());
            users.forEach(user -> System.out.println(user));
        };
    }

    private static class UserRowMapper implements RowMapper<User> {
        @Override
        public User mapRow(ResultSet rs, int rowNum) throws SQLException {
            Long id = rs.getLong("id");
            String name = rs.getString("name");
            String email = rs.getString("email");
            return new User(id, name, email);
        }
    }

    // User 클래스
    static class User {
        private Long id;
        private String name;
        private String email;
        public User(Long id, String name, String email) {
            this.id = id;
            this.name = name;
            this.email = email;
        }
        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", name='" + name + '\'' +
                    ", email='" + email + '\'' +
                    '}';
        }
    }
}
```

### ResultSetExtractor
전체 ResultSet을 단일 호출에서 사용하여 복잡한 객체를 구성 할 수 있는 방법을 제공해주며, 여러 행을 하나의 객체로 매핑할 때 유용하다.

- `SpringJDBC05Application.java`
```java
package org.example.springjdbc05;

import lombok.Setter;
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.jdbc.core.*;
import java.sql.*;

@SpringBootApplication
public class SpringJDBC05Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringJDBC05Application.class, args);
    }

    @Bean
    public CommandLineRunner demoExtractor(JdbcTemplate jdbcTemplate) {
        return args -> {
            String sql = "SELECT id, name, email FROM users";
            User user = jdbcTemplate.query(sql, new UserResultSetExtractor());
            System.out.println(user);
        };
    }

    private static class UserResultSetExtractor implements ResultSetExtractor<User> {
        @Override
        public User extractData(ResultSet rs) throws SQLException {
            User user = new User();
            if (rs.next()) {
                user.setId(rs.getLong("id"));
                user.setName(rs.getString("name"));
                user.setEmail(rs.getString("email"));
            }
            return user;
        }
    }

    @Setter
    static class User {
        private Long id;
        private String name;
        private String email;

        @Override
        public String toString() {
            return "User{" +
                    "id=" + id +
                    ", name='" + name + '\'' +
                    ", email='" + email + '\'' +
                    '}';
        }
    }
}
```

<br>

## SQL 관리

### 외부 SQL 파일 관리

#### SQL 파일 준비
`resources/sql` 디렉토리 아래에 SQL 쿼리를 파일로 저장해보자.

- `queries.sql`
```sql
-- SQL to insert a user
INSERT_USER=INSERT INTO users (name, email) VALUES (:name, :email);

-- SQL to fetch all users
GET_ALL_USERS=SELECT id, name, email FROM users;

-- SQL to update a user's email
UPDATE_USER_EMAIL=UPDATE users SET email = :email WHERE name = :name;

-- SQL to delete a user
DELETE_USER=DELETE FROM users WHERE name = :name;
```

#### SQL 파일 로딩
Spring에서 외부 SQL 파일을 로드하기 위해 `Resource` 객체를 사용할 수 있으며, 이 예제에서는 `NamedParameterJdbcTemplate`을 사용하여 이름 기반의 파라미터를 SQL 쿼리와 함께 사용한다.

- `SqlConfig.java`
```java
package org.example.springjdbc06;

import org.springframework.context.annotation.*;
import org.springframework.core.io.*;
import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
import org.springframework.jdbc.datasource.DriverManagerDataSource;
import javax.sql.DataSource;
import java.io.IOException;
import java.util.Properties;

@Configuration
public class SqlConfig {

    @Bean
    public NamedParameterJdbcTemplate namedParameterJdbcTemplate(DataSource dataSource) {
        return new NamedParameterJdbcTemplate(dataSource);
    }

    @Bean
    public Properties sqlQueries() throws IOException {
        Resource resource = new ClassPathResource("sql/queries.sql");
        Properties properties = new Properties();
        properties.load(resource.getInputStream());
        return properties;
    }
}
```

#### DAO 구현
- `UserDao.java`
```java
package org.example.springjdbc06;

import lombok.RequiredArgsConstructor;
import org.springframework.jdbc.core.namedparam.*;
import org.springframework.stereotype.Repository;
import java.util.Properties;

@Repository
@RequiredArgsConstructor
public class UserDao {

    private final NamedParameterJdbcTemplate jdbcTemplate;
    private final Properties sqlQueries;

    // INSERT_USER=INSERT INTO users (name, email) VALUES (:name, :email);
    public void insertUser(User user) {
        String sql = sqlQueries.getProperty("INSERT_USER");
        MapSqlParameterSource params = new MapSqlParameterSource();
        params.addValue("name", user.getName());
        params.addValue("email", user.getEmail());

        jdbcTemplate.update(sql, params);
    }
}
```

- `SpringJDBC06Application.java`
```java
package org.example.springjdbc06;

import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;

@SpringBootApplication
public class SpringJDBC06Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringJDBC06Application.class, args);
    }

    @Bean
    public CommandLineRunner run(UserDao userDao) {
        return args -> {
            User user = new User(null, "Esther", "esther@example.com");
            userDao.insertUser(user);
            System.out.println("user 입력 성공");
        };
    }
}
```

<br>

### Text Blocks를 이용한 SQL 쿼리 관리
Java 17에서 도입된 `Text Blocks` 기능은 자바 코드 내에서 직접 여러 줄의 문자열을 간편하게 작성할 수 있게 해주는 매우 유용한 기능이다.

이를 이용하면 SQL 쿼리와 같은 여러 줄에 걸친 문자열을 코드 내부에서 직접 관리하기 훨씬 쉬워지지만, `Text Blocks`를 이용해 외부 파일에서 문자열을 관리하는 직접적인 방법은 제공하지 않는다.

- `SpringJDBC07Application.java`
```java
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.jdbc.core.JdbcTemplate;

@SpringBootApplication
public class SpringJDBC07Application {
    public static void main(String[] args) {
        SpringApplication.run(SpringJDBC07Application.class, args);
    }

    @Bean
    public CommandLineRunner demo(JdbcTemplate jdbcTemplate) {
        return args -> {
            // Text Blocks를 사용한 멀티 라인 SQL 쿼리
            String sql = """
                         INSERT INTO users (name, email)
                         VALUES (?, ?);
                         """;
            jdbcTemplate.update(sql, "Esther", "esther@example.com");

            // 멀티 라인 쿼리로 여러 개의 데이터 조회
            String query = """
                           SELECT id, name, email
                           FROM users
                           WHERE email LIKE ?;
                           """;
            jdbcTemplate.query(query, rs -> {
                while (rs.next()) {
                    System.out.println("User: " + rs.getString("name") + " - " + rs.getString("email"));
                }
            }, "%example.com");
        };
    }
}
```

<br>

---

<br>

# Annotation

## Count100 어노테이션 직접 구현
```java
package annotation;

import java.lang.annotation.Retention;
import java.lang.annotation.Target;

import static java.lang.annotation.ElementType.METHOD;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

// 어디에 포함되어 있는거냐(RUNTIME, CLASS, SOURCE)
@Retention(RUNTIME)     // 실행될 때
// 어디서 사용할 것이냐
@Target(METHOD)         // 메소드에서
public @interface Count100 {
    int id = 100; // 상수 선언가능

    // 기본값 설정
    String value() default "100";
    int min() default 0;
}
```

<br>

## Count100 어노테이션을 사용한 메소드 구현
```java
package annotation;

public class Hello {
    @Count100()
    public void print1(){
        System.out.println("print1 메소드 실행");
    }

    @Count100("@")
    public void print2(){
        System.out.println("print2 메소드 실행");
    }

    @Count100(value = "@", min = 1)
    public void print3(){
        System.out.println("print3 메소드 실행");
    }
}
```

<br>

## 메소드 사용
```java
package annotation;

import java.lang.reflect.Method;

public class HelloRun {
    public static void main(String[] args) throws Exception {
        Hello hello = new Hello();

        Method[] methods = Hello.class.getDeclaredMethods();
        if(methods[0].isAnnotationPresent(Count100.class)){
            for (int i = 0; i < 5; i++) {
                hello.print1();
            }
        } else hello.print1();

        for(Method m : methods) {
            if(m.isAnnotationPresent(Count100.class)){
                for (int i = 0; i < 5; i++) {
                    System.out.print(m.getDeclaredAnnotation(Count100.class).value());
                }
            }else System.out.print(m.getDeclaredAnnotation(Count100.class).value());
            System.out.println();
        }
    }
}
```
