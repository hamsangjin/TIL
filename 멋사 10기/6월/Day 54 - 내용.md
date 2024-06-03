# @Controller와 @RestController의 차이

## @Controller
`@Controller 애노테이션`은 Spring MVC의 기본 컨트롤러 애노테이션으로, 주로 웹 애플리케이션에서 HTTP 요청을 처리하는 역할을 한다. 

이 애노테이션은 클래스 레벨에서 사용되며, 해당 클래스를 웹 요청을 처리하는 컨트롤러로 등록한다.

### 특징 및 사용법
- `@Controller`는 주로 뷰 템플릿을 반환하고, MVC 패턴의 `V` 즉, 뷰(View)와 관련이 깊다.
- 이 애노테이션이 붙은 클래스의 메서드에서는 `ModelAndView`, `String`, `Model`, `Map` 등을 반환할 수 있으며, 이를 통해 데이터를 뷰로 전달하고 뷰를 렌더링할 수 있다.
- 반환된 뷰 이름은 `ViewResolver`에 의해 해석되어 최종적으로 사용자에게 보여질 페이지를 생성한다.

<br>

## @RestController
`@RestController 애노테이션`은 `@Controller`와 `@ResponseBody`를 결합한 것으로, RESTful 웹 서비스를 쉽게 만들 수 있도록 돕는다. 

이 애노테이션은 클래스가 HTTP 요청을 처리하고 데이터를 `JSON`이나 `XML` 형태로 클라이언트에게 직접 반환한다는 것을 나타낸다.

### 특징 및 사용법
- `@RestController`는 각 메서드가 기본적으로 HTTP 응답 본문에 데이터를 매핑하도록 한다.
- 이 애노테이션이 적용된 컨트롤러의 메서드는 `HttpMessageConverter`를 사용하여 반환된 객체를 HTTP 응답 본문으로 직접 쓰게 되고, 객체는 자동으로 `JSON`이나 `XML`로 변환된다.
- REST API를 개발할 때 주로 사용되며, 클라이언트에게 데이터 모델 자체를 반환하는데 적합하다.

<br>

## 주요 차이점
- `응답 처리`: `@Controller`는 뷰를 반환하는 것에 중점을 두며, 일반적으로 웹 페이지를 반환하는 데 사용되지만, `@RestController` 는 RESTful API를 구현하는 데 사용되며, JSON이나 XML 등의 형태로 데이터를 직접 반환한다.
- `뷰 렌더링`: `@Controller`와 함께 사용되는 메서드는 뷰 이름을 반환하고, `ViewResolver`를 통해 뷰를 렌더링하지만, `@RestController`의 메서드는 뷰 렌더링없이 `데이터 자체`를 반환한다.

<br>

---

<br>

# TodoList 예제 - RestController 사용

## application.yml
```yml
spring:
  application:
    name: restexam

  datasource:
    url: jdbc:mysql://localhost:3306/examplesdb
    username: username
    password: passowrd
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update
      show-sql: true

server:
  port: 80
```

<br>

## Entity
```java
package hello.restexam.domain;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "todos")
@Getter @Setter @ToString
@NoArgsConstructor
public class Todo {
    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String todo;

    private boolean done;

    public Todo(String todo) {
        this.todo = todo;
    }
}
```

<br>

## Repository
```java
package hello.restexam.repository;

import hello.restexam.domain.Todo;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

@Repository
public interface TodoRepository extends JpaRepository<Todo, Long> {
}
```

<br>

## Service
```java
package hello.restexam.service;

import hello.restexam.domain.Todo;
import hello.restexam.repository.TodoRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.data.domain.Sort;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
@RequiredArgsConstructor
public class TodoService {
    private final TodoRepository todoRepository;

    @Transactional(readOnly = true)
    public List<Todo> getTodos() {
        return todoRepository.findAll(Sort.by("id").descending());
    }

    @Transactional(readOnly = true)
    public Todo getTodo(Long id) {
        return todoRepository.findById(id).orElse(null);
    }

    @Transactional
    public Todo addTodo(String todo) {
        return todoRepository.save(new Todo(todo));
    }

    @Transactional
    public Todo updateTodo(Long id) {
        Todo todo = null;
        try{
            todo = todoRepository.findById(id).orElseThrow();
            todo.setDone(!todo.isDone());
        }catch (Exception e){
            e.printStackTrace();
        }
        return todo;
    }

    @Transactional
    public Todo updateTodo(Todo todo) {
        Todo updatedTodo = null;
        try{
            updatedTodo = todoRepository.save(todo);
        }catch (Exception e){
            e.printStackTrace();
        }
        return updatedTodo;
    }

    @Transactional
    public void deleteTodo(Long id) {
        todoRepository.deleteById(id);
    }
}
```

<br>

## Controller
```java
package hello.restexam.controller;

import hello.restexam.domain.Todo;
import hello.restexam.service.TodoService;
import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.*;
import java.util.List;

@RestController
@RequestMapping("/api/todos")
@RequiredArgsConstructor
public class TodoController {
    private final TodoService todoService;

    @GetMapping
    public List<Todo> getTodos(){
        return todoService.getTodos();
    }
    @GetMapping("/{id}")
    public Todo getTodo(@PathVariable("id") Long id){
        return todoService.getTodo(id);
    }
    @PostMapping
    public Todo addTodo(@RequestBody Todo todo){
        return todoService.addTodo(todo.getTodo());
    }

    // put: 객체 자체가 바뀔 때
    // patch: 객체의 일부분만 수정될 때
    @PatchMapping("/{id}")
    public Todo updateTodoById(@PathVariable("id") Long id){
        return todoService.updateTodo(id);
    }

    @PatchMapping
    public Todo updateTodo(@RequestBody Todo todo){
        return todoService.updateTodo(todo.getId());
    }

    @DeleteMapping
    public String deleteTodo(@RequestBody Todo todo){
        todoService.deleteTodo(todo.getId());
        return "ok";
    }
}
```

<br>

---

<br>

# curl을 사용한 요청 및 응답
curl은 기본적으로 웹 페이지의 내용만을 출력한다.

## 기본 요청 및 응답 보기
```bash
# HTTP 헤더를 포함한 전체 응답
curl -i http://localhost/api/todos
```

<br>

## HTTP 헤더만 보기
```bash
# HTTP GET 요청 헤더만 출력
curl -I http://localhost/api/todos
```

<br>

## 자세한 요청 및 응답 정보 보기
```bash
# 요청 헤더, 응답 헤더 및 바디를 포함한 전체 통신 과정을 출력
curl -v http://example.com
```

<br>

## 데이터와 함께 요청 보내기
```bash
# -H 옵션: 헤더 설정
# -d 옵션: 폼/json 데이터 전송
curl -X POST http://localhost:8080/api/todos -H "Content-Type: application/json" -d '{"todo": "createTodo"}'
```

<br>

---

<br>

# IntelliJ에서 http 사용방법
IntelliJ IDEA에서는 HTTP 클라이언트 기능을 내장하고 있어서 `.http` 파일을 사용하여 직접 HTTP 요청을 보내고 결과를 받을 수 있다.

## .http 파일 작성 방법
1. `파일 생성`: IntelliJ에서 새 파일을 생성하고 파일 확장자를 `http`로 설정한다.
2. `요청 작성`
    - 각 요청은 요청 메소드, URL, 필요한 경우 헤더와 바디를 포함해서 HTTP 요청을 작성한다.
    - 여러 요청을 한 파일 내에 작성할 수 있고, 요청 사이에는 `###`을 사용하여 구분한다.

## http 예제
- `todo.http`
```http
### GET /todos
GET http://localhost/api/todos
Accept: application/json

### GET /todos/{id}
GET http://localhost/api/todos/3
Accept: application/json

### POST /todos
POST http://localhost/api/todos
Content-Type: application/json

{
  "todo": "New Todo Item"
}

### PATCH /todos/{id}
PATCH http://localhost/api/todos/2
Accept: application/json

### PATCH /todos
PATCH http://localhost/api/todos
Content-Type: application/json

{
  "id": 2,
  "todo" : "Update Todo"
}

### DELETE /todos
DELETE http://localhost/api/todos
Content-Type: application/json

{
  "id": 2
}
```
