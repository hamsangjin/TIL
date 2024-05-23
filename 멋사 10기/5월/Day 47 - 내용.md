# 데이터 접근과 비즈니스 로직

## 레이어드 아키텍처
`레이어드 아키텍처`는 소프트웨어 설계에서 널리 사용되는 패턴 중 하나로, 애플리케이션을 명확하게 분리된 여러 계층으로 구조화하는 방식이다.

1. `프레젠테이션 계층(Presentation Layer)`: 사용자 인터페이스와 직접적으로 상호 작용하는 계층
    -  `Controller`: 사용자의 요청을 받아 해당 비즈니스 로직을 호출하고, 결과를 뷰에 전달하는 역할을 한다.
2. `비즈니스 로직 계층(Business Logic Layer)` 또는 `서비스 계층(Service Layer)`: 애플리케이션의 핵심 기능과 비즈니스 규칙을 처리하는 계층
    - `Service`: 실제 비즈니스 로직을 수행하며, 여러 데이터 모델을 조합하고 복잡한 계산이나 조건에 따라 다양한 작업을 실행한다.
3. `데이터 접근 계층(Data Access Layer)`: 데이터베이스나 파일 시스템과 같은 데이터 소스를 직접적으로 관리하고 접근 하는 기능을 제공하는 계층
    - `Repository`: 데이터베이스와의 모든 상호작용을 캡슐화하고 데이터베이스 쿼리를 직접 관리한다.

<br>

## Service 계층의 설계
`Service 계층`은 Spring MVC 아키텍처에서 비즈니스 로직을 처리하는 핵심 부분이다.

### Service 계층 설계의 주요 원칙
1. `비즈니스 로직 중심`: 비즈니스 요구사항을 반영한 메서드를 제공하여, 컨트롤러가 직접 데이터를 처리하거나 비즈니스 규칙을 구현하지 않도록 해야 한다.
2. `재사용성`: 각 서비스 메서드는 재사용 가능하도록 일반적인 비즈니스 기능을 수행해야 한다.
3. `분리된 관심사`: 데이터베이스, 웹 프레임워크, UI와 같은 기타 계층의 구현 세부사항으로부터 독립되어야 하므로, 인터페이스를 사용하여 구현 세부사항을 추상화하는 것이 일반적이다.
4. `트랜잭션 관리`: 일반적으로 트랜잭션 관리의 경계를 정의하고, Spring의 선언적 트랜잭션 관리를 활용하여 비즈니스 로직 내에서 데이터 일관성을 보장한다.
5. `예외 처리`: 예외 처리 메커니즘을 구현하여, 발생 가능한 예외를 적절히 처리하거나 변환하여 상위 계층으로 전파한다.

<br>

## Spring Data JDBC의 통합 사용
`Spring Data JDBC`는 Spring Data 프로젝트의 일부로, 데이터 접근 계층을 단순화하는 목적으로 설계되었고, JDBC를 이용해 데이터베이스와 상호작용할 때 발생하는 반복적인 코드를 대폭 줄일 수 있다.

### 의존성 추가
- `build.gradle`
```gradle
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

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jdbc'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation group: 'mysql', name: 'mysql-connector-java', version: '8.0.33'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-devtools'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    testRuntimeOnly 'org.junit.platform:junit-platform-launcher'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

<br>

### sql 연결 및 포트 설정
- `application.yml`
```yml
spring:
  application:
    name: friendexample

  datasource:
    url: jdbc:mysql://localhost:3306/examplesdb
    username: sangjin
    password: sangjin
    driver-class-name: com.mysql.cj.jdbc.Driver

server:
  port: 90
```

<br>

### 테이블 생성 및 필드 추가
```sql
CREATE TABLE friend (
    id SERIAL PRIMARY KEY,
    name VARCHAR(255),
    email VARCHAR(255)
);

INSERT INTO friend (name, email) VALUES
          ('김민수', 'minsu.kim@example.com'),
          ('이하은', 'haeun.lee@example.com'),
          ('박서준', 'seojun.park@example.com'),
          ('최지우', 'jiwoo.choi@example.com'),
          ('정다현', 'dahyun.jung@example.com'),
          ('손예진', 'yejin.son@example.com'),
          ('노준호', 'junho.noh@example.com'),
          ('윤서아', 'seoah.yoon@example.com'),
          ('임태현', 'taehyun.lim@example.com'),
          ('한지안', 'jian.han@example.com'),
          ('조유리', 'yuri.jo@example.com');
```

<br>

### Friend 도메인 객체
- `Friend.java`
```java
import lombok.*;
import org.springframework.data.annotation.Id;

@Getter
@Setter
@AllArgsConstructor
@NoArgsConstructor
@ToString
@EqualsAndHashCode
public class Friend {
    @Id
    private Long id;
    private String name;
    private String email;
}

```

<br>

### 데이터 접근 계층
- `FriendRepository.java`
```java
import org.example.friendexample.domain.Friend;
import org.springframework.data.repository.*;

// PagingAndSortingRepository<Friend, Long>: 페이징 처리
public interface FriendRepository extends CrudRepository<Friend, Long>, PagingAndSortingRepository<Friend, Long> {
}
```

<br>

### 비즈니스 로직 계층
- `FriendService.java`
```java
import lombok.RequiredArgsConstructor;
import org.example.friendexample.domain.Friend;
import org.example.friendexample.repository.FriendRepository;
import org.springframework.data.domain.*;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

@Service
@RequiredArgsConstructor        // 초기화 되지않은 final 필드나, @NonNull이 붙은 필드에 대해 생성자를 생성해준다.
public class FriendService {

    private final FriendRepository friendRepository;

    @Transactional(readOnly = true)
    public Iterable<Friend> findAllFriends() {
        return friendRepository.findAll();
    }

    @Transactional
    public Friend save(Friend friend) {
        // id값이 이미 해당하면 수정하고 없다명 생성한다.
        return friendRepository.save(friend);
    }

    @Transactional(readOnly = true)
    public Friend findFriendById(Long id) {
        return friendRepository.findById(id).orElse(null);
    }

    @Transactional
    public void delete(Long id) {
        friendRepository.deleteById(id);
    }

    // 페이징 처리된 친구 목록 조회
    public Page<Friend> findAllFriends(Pageable pageable) {
        Pageable sortedByDescId = PageRequest.of(
                pageable.getPageNumber(),
                pageable.getPageSize(),
                Sort.by(Sort.Direction.DESC, "id")
        );

        return friendRepository.findAll(sortedByDescId);
    }
}
```

<br>

### 프레젠테이션 계층 - 컨트롤러
- `FriendController.java`
```java
import lombok.RequiredArgsConstructor;
import org.example.friendexample.domain.Friend;
import org.example.friendexample.service.FriendService;
import org.springframework.data.domain.*;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.servlet.mvc.support.RedirectAttributes;

@Controller
@RequiredArgsConstructor
@RequestMapping("/friends")     // 매핑하는 경로 기본값 설정
public class FriendController {

    private final FriendService friendService;

    // /friends
//    @GetMapping
//    public String friends(Model model) {
//        Iterable<Friend> friends = friendService.findAllFriends();
//        model.addAttribute("friends", friends);
//        return "friends/list";
//    }

    @GetMapping
    public String friends(Model model,
                          @RequestParam(defaultValue = "1") int page,
                          @RequestParam(defaultValue = "5") int size) {
        Pageable pageable = PageRequest.of(page-1, size);
        Page<Friend> friends = friendService.findAllFriends(pageable);
        model.addAttribute("friends", friends);
        model.addAttribute("currentPage", page);
        return "friends/list";
    }

    @GetMapping("/add")
    public String addFriend(Model model) {
        model.addAttribute("friend", new Friend());
        return "friends/add";
    }

    @PostMapping("/add")
    public String addFriend(@ModelAttribute Friend friend,
                            RedirectAttributes redirectAttributes) {
        // 성공적으로 이루어졌음을 사용자에게 알리기 위해 다음 요청에 성공 메시지를 전달하는 역할을 합니다.
        redirectAttributes.addFlashAttribute("msg", "친구등록 성공 !!");
        friendService.save(friend);

        return "redirect:/friends";
    }

    @GetMapping("/{id}")
    public String detailFriend(@PathVariable Long id, Model model) {
        model.addAttribute("friend", friendService.findFriendById(id));
        return "friends/detail";
    }

    @GetMapping("/delete/{id}")
    public String deleteFriend(@PathVariable Long id,
                               RedirectAttributes redirectAttributes) {
        redirectAttributes.addFlashAttribute("msg", "친구 삭제 성공 !!");
        friendService.delete(id);
        return "redirect:/friends";
    }

    @GetMapping("/edit/{id}")
    public String editFriend(@PathVariable Long id, Model model) {
        model.addAttribute("friend", friendService.findFriendById(id));
        return "friends/edit";
    }

    @PostMapping("/edit")
    public String editFriend(@ModelAttribute Friend friend){
        friendService.save(friend);
        return "redirect:/friends";
    }
}
```

<br>

### 프레젠테이션 계층 - 뷰
- `templates/friends/list.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org/" lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>친구 목록</title>
        <link rel="stylesheet" th:href="@{/css/friend.css}">
    </head>
    <body>
        <h1> 친구 목록 </h1>
        <table th:if="${!friends.empty}">
            <thead>
                <tr>
                    <th> ID </th>
                    <th> Name </th>
                    <th> Email </th>
                </tr>
            </thead>
            <tbody>
                <tr th:each="friend : ${friends}">
                    <td th:text="${friend.id}"></td>
                    <td>
                        <a th:href="@{/friends/{id} (id=${friend.id})}" th:text="${friend.name}"></a>
                    </td>
                    <td th:text="${friend.email}"></td>
                </tr>
            </tbody>
        </table>

        <div th:if="${friends.totalPages > 1}">
            <ul>
                <li th:each="i : ${#numbers.sequence(1, friends.totalPages)}">
                    <a th:href="@{/friends(page=${i})}" th:text="${i}"></a>
                </li>
            </ul>
        </div>

        <a th:href="@{/friends/add}"> 친구 등록 </a>
    </body>
</html>
```

- `templates/friends/add.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org/" lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>친구 등록</title>
        <link rel="stylesheet" th:href="@{/css/friend.css}">
    </head>
    <body>
        <h1> 친구 등록 </h1>
        <form th:action="@{/friends/add}" method="post" th:object="${friend}">
            <label for="name">Name : </label>
            <!--     required: 필수 입력 속성       -->
            <input type="text" id="name" th:field="*{name}" required>

            <br>

            <label for="email">Email : </label>
            <input type="text" id="email" th:field="*{email}" required>

            <input type="submit" value="등록">
        </form>

        <a th:href="@{/friends}">친구목록</a>
    </body>
</html>
```

- `templates/friends/detail.html`
```html
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org/" lang="ko">
    <head>
        <meta charset="UTF-8">
        <title>친구 상세 정보</title>
        <link rel="stylesheet" th:href="@{/css/friend.css}">
    </head>
    <body>
        <h1> 친구 상세 정보 </h1>
        <h2 th:text="'이름: ' + ${friend.name}">name</h2>
        <h2 th:text="'이메일: ' + ${friend.email}">email</h2>

        <a th:href="@{/friends}">친구목록</a>
        <a th:href="@{/friends/edit/{id}(id=${friend.id})}">수정</a>
        <a th:href="@{/friends/delete/{id}(id=${friend.id})}">삭제</a>
    </body>
</html>
```

- `templates/friends/edit.html`
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>친구 정보 수정</title>
    </head>
    <body>
        <h1> 친구 정보 수정 </h1>
        <form th:action="@{/friends/edit}" th:object="${friend}" method="post">
            <!--      서버로는 전송되지만 사용자에겐 보이진 않음      -->
            <input type="hidden" th:field="*{id}">
            <label for="name">Name : </label>
            <!--     required: 필수 입력 속성       -->
            <input type="text" id="name" th:field="*{name}" required>

            <br>

            <label for="email">Email : </label>
            <input type="text" id="email" th:field="*{email}" required>

            <input type="submit" value="수정">
        </form>
    </body>
</html>
```

- `static/css/friend.css`
```css
body {
    font-family: "Arial", sans-serif;
    margin: 40px;
}
table {
    width: 100%;
    border-collapse: collapse;
    margin-bottom: 20px;
}
th,
td {
    border: 1px solid #dddddd;
    text-align: left;
    padding: 8px;
}
th {
    background-color: #f2f2f2;
}
tr:nth-child(even) {
    background-color: #f9f9f9;
}
a {
    color: #337ab7;
    text-decoration: none;
}
a:hover {
    text-decoration: underline;
}
ul {
    list-style: none;
    padding: 0;
}
li {
    display: inline;
    margin-right: 5px;
}
li a.active {
    font-weight: bold;
    text-decoration: underline;
}
.write-link {
    display: block;
    text-align: center;
    margin-top: 20px;
}
```

### 실행 화면
![](https://github.com/hamsangjin/TIL/assets/103736614/02b10f4d-c208-447d-91f8-9e5121d085a4) | ![](https://github.com/hamsangjin/TIL/assets/103736614/76def236-afd9-4fe0-a409-7137a133dcaa)
 -- | -- | 

![](https://github.com/hamsangjin/TIL/assets/103736614/d274c5af-96c1-4f76-aad2-98b7f44b86ad) | ![](https://github.com/hamsangjin/TIL/assets/103736614/af4b1939-3393-4c45-a9dd-df1f04da8173)
-- | -- | 

