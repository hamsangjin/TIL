# RDS(Relational Database Services)
`Amazon RDS`는 클라우드 환경에서 관계형 데이터베이스를 쉽게 설정, 운영 및 확장할 수 있게 해주는 완전 관리형 서비스다.

## 실습 : MySQL용 DB 인스턴스 생성, 클라이언트를 통한 DB 연결

### MySQL용 DB 인스턴스 생성
![](https://github.com/hamsangjin/TIL/assets/103736614/6769aed1-2338-4ed6-9e91-a73ba85cfd80) | ![](https://github.com/hamsangjin/TIL/assets/103736614/b8474377-f5e5-41f2-8029-af822d468b10) | ![](https://github.com/hamsangjin/TIL/assets/103736614/f22d3f96-9d0e-4528-853e-1167097db470)
-- | -- | -- |

![](https://github.com/hamsangjin/TIL/assets/103736614/2cf60a2c-83d6-4de5-886e-d9aa451ec7e3) | ![](https://github.com/hamsangjin/TIL/assets/103736614/48f5ebc3-4f84-43e8-ae4c-d3ea309d55fc) | ![](https://github.com/hamsangjin/TIL/assets/103736614/d1e93fcb-9c0a-4276-87ff-3370f92df86a)
-- | -- | -- |

### 보안 그룹에서 인바운드 규칙 추가
![](https://github.com/hamsangjin/TIL/assets/103736614/86c6bf2f-26b8-43f9-8ad8-129b8de1879a)

### 클라이언트를 통한 DB 연결
![](https://github.com/hamsangjin/TIL/assets/103736614/9db97d7e-cff6-450b-b10d-92596e16be84) | ![](https://github.com/hamsangjin/TIL/assets/103736614/be3e1cf0-b9b3-4360-b31e-4f31e155d5ac)
-- | -- |

### 데이터베이스 생성
```sql
CREATE DATABASE mydatabase
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
CREATE USER 'jinsang'@'%' IDENTIFIED BY 'jinsang';
GRANT ALL PRIVILEGES ON mydatabase.* TO 'jinsang'@'%';
FLUSH PRIVILEGES;
```

### time_zone 설정 변경
![](https://github.com/hamsangjin/TIL/assets/103736614/9f732a5e-a5db-4451-9667-3910fc5da4fb) | ![](https://github.com/hamsangjin/TIL/assets/103736614/bb8f9686-6506-4d5f-92c5-da808ee7c58c) | ![](https://github.com/hamsangjin/TIL/assets/103736614/bc915544-923d-4e38-8c04-9f2dcda383d0)
-- | -- | -- |

![](https://github.com/hamsangjin/TIL/assets/103736614/357a4c13-3cfb-4852-8193-30c544f9efa4) | ![](https://github.com/hamsangjin/TIL/assets/103736614/27ccafd9-0fa9-4ba2-b430-6ae74f366569)
-- | -- |


<br>

## 실습 : Spring Boot Application에서 RDS(MySQL) 연결하기

### build.gradle
```
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    runtimeOnly 'com.mysql:mysql-connector-j'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}
```

### application.yml
```yaml
spring:
  application:
    name: todoapp
  datasource:
    url: jdbc:mysql://lion-db.cxigeay4wb3w.ap-northeast-2.rds.amazonaws.com:3306/mydatabase
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQLDialect
```

### 환경변수(user/password) 설정
![](https://github.com/hamsangjin/TIL/assets/103736614/b66068ed-dd78-463a-b58c-291e2ff08561)

### Entity
```java
import jakarta.persistence.*;
import lombok.*;

@Entity
@Getter @Setter
@Table(name = "users")
public class User {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String name;
    private String email;
}
```

### Repository
```java
import hello.todoapp.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
}
```

### Service
```java
import hello.todoapp.entity.User;
import hello.todoapp.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User getUserById(Long id) {
        return userRepository.findById(id).orElse(null);
    }

    public User createUser(User user) {
        return userRepository.save(user);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

### Controller
```java
import hello.todoapp.entity.User;
import hello.todoapp.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public List<User> getAllUsers() {
        return userService.getAllUsers();
    }

    @GetMapping("/{id}")
    public User getUserById(@PathVariable("id") Long id) {
        return userService.getUserById(id);
    }

    @PostMapping
    public User createUser(@RequestBody User user) {
        return userService.createUser(user);
    }

    @DeleteMapping("/{id}")
    public void deleteUser(@PathVariable("id") Long id) {
        userService.deleteUser(id);
    }
}
```

### HTTP Test
```
### Create a new user
POST http://localhost:8080/users
Content-Type: application/json

{
  "name": "hamsangjin",
  "email": "hamsangjin@example.com"
}

### Get all users
GET http://localhost:8080/users
Content-Type: application/json

### Delete user by ID
DELETE http://localhost:8080/users/1
Content-Type: application/json

### Get user by ID
GET http://localhost:8080/users/1
Content-Type: application/json
```

![](https://github.com/hamsangjin/TIL/assets/103736614/f2a442f2-fbd0-4de0-b127-154955bbf5cc) | ![](https://github.com/hamsangjin/TIL/assets/103736614/f729f3ca-f6ee-4466-9ee8-b15cff99fb6d)
-- | -- |

![](https://github.com/hamsangjin/TIL/assets/103736614/79284064-8cf9-4725-83e2-9e68a5098ce2) | ![](https://github.com/hamsangjin/TIL/assets/103736614/ccf4d5ff-7644-4ea4-b5c8-a46729dd41ab)
-- | -- |

<br>

---

<br>

# Amazon S3
`Amazon S3`란 안전하고 확장 가능한 객체 스토리지 서비스로, 데이터를 무한대로 저장할 수 있다. 

또한, 버킷 정책, 버전 관리, 수명 주기 정책 등을 사용하여 데이터를 효율적으로 관리할 수 있다.

##  실습: 무한대로 저장 가능한 Amazon S3 생성하기
### 버킷 관련 권한 설정
![](https://github.com/hamsangjin/TIL/assets/103736614/198dff85-2663-4c0d-92db-4a2df7515b71)

### S3 버킷 생성 
![](https://github.com/hamsangjin/TIL/assets/103736614/8b65cc72-e974-43cf-af57-935e8ed5d985) | ![](https://github.com/hamsangjin/TIL/assets/103736614/c76e4da5-ae7d-48d6-806a-ee1ff49b632b) | ![](https://github.com/hamsangjin/TIL/assets/103736614/91fe7cee-67ca-4461-b68d-e0674f983bc7)
-- | -- | -- |

### 파일 업로드 및 조회, 삭제
![](https://github.com/hamsangjin/TIL/assets/103736614/92101ff5-68ac-4947-baf0-04f98cf0ef34) | ![](https://github.com/hamsangjin/TIL/assets/103736614/b3a42d37-f9cf-404b-8d2c-5622ef0a2beb) | ![](https://github.com/hamsangjin/TIL/assets/103736614/8bb276ce-6f1d-4945-b14b-0c0d55e475d0)
-- | -- | -- |

### S3 버킷 삭제
![](https://github.com/hamsangjin/TIL/assets/103736614/a3508254-e6d6-4e33-a171-1ffdd5533e57)

