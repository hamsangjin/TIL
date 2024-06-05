# REST Controller 예제
```java
import org.springframework.web.bind.annotation.*;
import java.util.*;

@RestController
public class MyRestController {
    @GetMapping("/api/greeting")
    public Map<String, String> greet(@RequestParam(name = "name", required = false, defaultValue = "World") String name) {
        Map<String, String> response = new HashMap<>();
        response.put("message", "Hello " + name);
        return response;
    }
}
```

![image](https://github.com/hamsangjin/TIL/assets/103736614/54a1b0d9-8ca4-460c-a0f7-e0cc10d3ca05)

- `RequestParam`
    - `name`: 파라미터의 이름
    - `required`: 파라미터의 필수 여부
    - `defaultValue`: 파라미터에 값이 없는 경우 기본 값을 적용
- `@RestController`: 클래스 레벨에서 @RestController 애노테이션을 사용하면, 클래스 내의 모든 메서드는 기본적으로 HTTP 응답 본문에 반환 값을 매핑한다.
- `@GetMapping("/api/greeting")`: 쿼리 파라미터 name을 받아 Map 타입의 `"Hello, [name]!"` 메시지를 JSON 객체로 변환해서 반환한다.
    - JSON 변환의 경우 `MappingJackson2HttpMessageConverter`가 일반적으로 사용되며, 이 컨버터는 자바 객체를 JSON으로, JSON을 자바 객체로 변환하는 역할을 수행한다.

<br>

---

<br>

# [값을 저장하는 4가지 영역](https://velog.io/@chamominedev/4%EA%B0%80%EC%A7%80-SCOPE-%EC%98%81%EC%97%AD)

![image](https://github.com/hamsangjin/TIL/assets/103736614/ce6627f1-d6cf-4735-9ffa-1e81fe4e40d2)

- `Page`: 해당 페이지가 실행되는 동안만 유지되는 영역
- `Request`: http 요청을 WAS가 받아서 웹 브라우저에게 응답할 때까지 변수값을 유지하고자 할 경우 사용
- `Session`: 웹 브라우저별로 변수를 관리하고자 할 경우에 사용
- `Application`: 서버가 가동되는 순간부터 서버가 종료되는 순간까지의 모든 범위

<br>

---

<br>

# Restful 서비스 개념
`RESTful 서비스`는 웹 서비스의 한 형태로서, REST(Representational State Transfer) 아키텍처 스타일을 따른다.

## Restful 서비스의 주요 원칙
1. `자원(Resource) 기반`: RESTful 서비스에서 모든 콘텐츠는 `자원`으로 표현되고, `자원`은 URI(Uniform Resource Identifier)로 식별된다.
2. `무상태(Stateless)`: 각 요청은 독립적이며, 서버는 클라이언트의 상태 정보를 저장하지 않는다.
3. `연결성(Connectivity)`: 자원은 하이퍼링크를 통해 서로 연결될 수 있다.
4. `표준 메소드 사용`: HTTP 표준 메소드를 사용하여 리소스에 대한 CRUD 작업을 수행한다.
5. `다양한 표현`: RESTful 서비스는 JSON, XML 등 여러 형태의 데이터 포맷을 지원하여 클라이언트의 요구에 맞춰 데이터를 제공할 수 있습니다.

<br>

## Spring MVC와 RESTful 서비스
- `@RestController`: 해당 클래스가 RESTful 웹 서비스의 컨트롤러임을 나타내며, `@Controller`와 `@ResponseBody`를 합친 것과 동일한 효과를 가진다.
- `@RequestMapping 및 HTTP 메소드 어노테이션들`: URI, HTTP 메소드, 요청 헤더, 매개변수 등에 따라 핸들러 메소드를 매핑한다.

<br>

---

<br>

# JSON/XML 데이터 처리

## JSON 데이터 처리
1. `Jackson 라이브러리`: Spring MVC는 Jackson 라이브러리를 사용하여 JSON 데이터를 자바 객체로 구조화해주며, `@RestController`나 `@ResponseBody` 어노테이션이 붙은 메소드에서 자동으로 처리된다.
2. `@RequestBody와 @ResponseBody`
    - `@RequestBody` 어노테이션을 사용하여 HTTP 요청 본문을 자바 객체로 매핑할수 있다.
    - `@ResponseBody` 어노테이션을 메소드에 사용하여 반환값을 HTTP 응답 본문에 쓰고, 이 데이터는 JSON 형식으로 클라이언트에 전송된다.

<br>

## XML 데이터 처리
1. `JAXB 라이브러리`: XML 데이터 처리를 위해 Spring은 JAXB 라이브러리를 사용하며, JAXB를 통해 자바 객체와 XML 사이의 매핑이 가능하다.
2. `Content Negotiation`: Spring MVC에서는 Accept 헤더를 기반으로 클라이언트가 요청한 데이터 형식(JSON 또는 XML)에 따라 응답 형식을 결정할 수 있다.

<br>

## JSON/XML 데이터 처리 예제 코드
- `build.gradle 추가 내용` → xml 형식 사용
```
implementation 'com.fasterxml.jackson.dataformat:jackson-dataformat-xml'
```

- `MyRestController`
```java
package hello.restexam.controller;

import hello.restexam.domain.Product;
import org.springframework.web.bind.annotation.*;
import java.util.*;

@RestController
public class MyRestController {
    // produces 속성: 응답 타입을 지정
    @GetMapping(value = "/productjson/{id}", produces = "application/json")
    public Product getProductAsJson(@PathVariable("id") Long id){
        return new Product(id, "pen", 9.99);
    }

    @GetMapping(value = "/productxml/{id}", produces = "application/xml")
    public Product getProductAsXml(@PathVariable("id") Long id){
        return new Product(id, "pen", 9.99);
    }
}

```

![image](https://github.com/hamsangjin/TIL/assets/103736614/3619e080-9009-46a8-adaf-7171fe8c409a) | ![image](https://github.com/hamsangjin/TIL/assets/103736614/6b2caf5c-6ecb-465d-b4fd-93792d5a5bfa)
-- | -- |

<br>

---

<br>

# ResponseEntity와 HTTP 상태 코드 관리
`ResponseEntity 클래스`는 Spring MVC에서 HTTP 요청에 대한 응답을 구성할 때 사용하며, HTTP 상태 코드, 헤더, 응답 본문을 포함하여 HTTP 응답을 전체적으로 제어할 수 있게 해준다.

- `기본 사용법`: ResponseEntity 객체는 `status` 메소드로 상태 코드를 설정하고, `body` 메소드로 응답 본문을 설정할 수 있습니다. 또한, `headers` 메소드를 사용하여 HTTP 헤더를 추가할 수 있다.
- `HTTP 상태 코드 관리`: 다양한 HTTP 상태 코드를 효과적으로 관리할 수 있다.
- `에러 핸들링`: 예외를 처리하고, 적절한 HTTP 상태 코드와 에러 메시지를 반환할 수 있다.
- `클라이언트와의 상호 작용 강화`: `ResponseEntity`를 사용하면 클라이언트가 요청의 결과를 더 잘 이해하고 적절히 대응할 수 있도록 돕는다.

<br>

---

<br>

# ResponseEntity 예제
- `MemoRestController.java`
```java
package hello.restexam.controller;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.*;
import java.util.concurrent.atomic.AtomicLong;

@RestController
@RequestMapping("/api/memos")
public class MemoRestController {
    private final Map<Long, String> memos = new HashMap<>();
    // AtomicLong은 Long 자료형을 갖고 있는 Wrapping 클래스
    // Thread-safe로 구현되어 멀티쓰레드에서 synchronized없이 사용할 수 있다.
    // 또한 synchronized보다 적은 비용으로 동시성을 보장할 수 있다.
    private final AtomicLong counter = new AtomicLong();

    @PostMapping
    public ResponseEntity<Long> createMemo(@RequestBody String content) {
        Long id = counter.incrementAndGet();        // 현재 값 리턴하고 +1 증가
        memos.put(id, content);
        return ResponseEntity.ok(id);
    }

    @GetMapping("/{id}")
    public ResponseEntity<String> getMemo(@PathVariable("id") Long id) {
        String memo = memos.get(id);
        if (memo == null) return ResponseEntity.notFound().build();  // 404 오류
        return ResponseEntity.ok(memo);
    }

    @PutMapping("/{id}")
    public ResponseEntity<String> updateMemo(@PathVariable Long id, @RequestBody String content) {
        if (!memos.containsKey(id))     return ResponseEntity.notFound().build();
        memos.put(id, content);
        return ResponseEntity.ok("Memo 수정 성공");
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteMemo(@PathVariable Long id) {
        String removedMemo = memos.remove(id);
        if (removedMemo == null)        return ResponseEntity.notFound().build();
        return ResponseEntity.ok("Memo 삭제 성공");
    }
}
```

- `memo.http`

```
### POST
POST http://localhost/api/memos
Content-Type: application/json

{
  "memo": "sangjin memo"
}

### GET /memos/{id}
GET http://localhost/api/memos/65
Accept: application/json

### PUT
PUT http://localhost/api/memos/1
Content-Type: application/json

{
  "content": "sangjin update memo"
}
```

- 실행결과

![image](https://github.com/hamsangjin/TIL/assets/103736614/61a9f1b2-e9b1-45f1-a9a8-3d160a23400a) | ![image](https://github.com/hamsangjin/TIL/assets/103736614/3dab4aa1-aa83-4dd9-bf39-599c5bcad5d2)
-- | -- |

![image](https://github.com/hamsangjin/TIL/assets/103736614/60393d7a-2341-41a2-acdf-a8f4860056d7) | ![image](https://github.com/hamsangjin/TIL/assets/103736614/dcf0e4ca-10b2-43e1-b6ca-dc0ad094757c)
-- | -- |

<br>

---

<br>

# 예외 처리와 에러 핸들링

## Controller Advice를 이용한 예외 처리
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.http.*;

@RestControllerAdvice   // @ControllerAdvice 는 전역적으로 예외를 처리할 수 있는 방법을 제공
public class RestControllerExceptionHandler {
    @ExceptionHandler(Exception.class)  // Exception.class의 예외가 발생하면 처리
    public ResponseEntity<String> handleException(Exception e) {
        return ResponseEntity
                .status(HttpStatus.INTERNAL_SERVER_ERROR)
                .body("공습 경보 !!");
    }
}
```

<br>

## error 발생시키기
```java
import org.springframework.web.bind.annotation.*;

@RestController
public class ExamController {
    @GetMapping("/api/error")
    public String apiError() {
        throw new RuntimeException("API Error");
    }
}
```

![image](https://github.com/hamsangjin/TIL/assets/103736614/2128a94e-a80c-4939-9d13-d57875044416)

<br>

## Response Status Exception
Spring 5 이후, `ResponseStatusException 클래스`를 사용하여 예외를 직접 처리할 수 있다.

```java
import org.springframework.web.server.ResponseStatusException;
import org.springframework.web.bind.annotation.*;

@RestController
public class UserController {
    @GetMapping("/user/{id}")
    public User getUserById(@PathVariable Long id) {
        return userService.findById(id)
                .orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND, "User not found"));
		} 
}
```

<br>

---

<br>

# 상품등록, 수정, 삭제, 1건 조회, 여러건 조회 예제

## Product 클래스
```java
import lombok.*;

@Getter @Setter
@AllArgsConstructor
public class Product {
    private Long id;
    private String name;
    private double price;
}
```

<br>

## ProductController
```java
import hello.restexam.domain.Product;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.*;
import java.util.concurrent.atomic.AtomicLong;

@RestController
@RequestMapping("/api/products")
public class ProductController {
    /*
    * POST /api/products : 새 상품을 등록합니다. 각 상품은 고유한 ID를 자동으로 할당 받습니다.
    * GET /api/products/{id} : 특정 ID의 상품을 조회합니다.
    * GET /api/products : 저장된 모든 상품의 목록을 조회합니다.
    * PUT /api/products/{id} : 특정 ID의 상품을 수정합니다.
    * DELETE /api/products/{id} : 특정 ID의 상품을 삭제합니다.
    */
    private final Map<Long, Product> products = new HashMap<>();
    private final AtomicLong counter = new AtomicLong();

		// 새 상품 등록
    @PostMapping
    public ResponseEntity<Long> addProduct(@RequestBody Product product) {
        long id = counter.incrementAndGet();
        product.setId(id);
        products.put(id, product);
        return ResponseEntity.ok(id);
    }
		
		// 특정 ID의 상품 조회
    @GetMapping("/{id}")
    public ResponseEntity<Product> getProduct(@PathVariable Long id) {
        Product product = products.get(id);
        if (product == null) return ResponseEntity.notFound().build();
        return ResponseEntity.ok(product);
    }

		// 모든 상품의 목록 조회
    @GetMapping
    public ResponseEntity<List<Product>> getProducts() {
        if (products.isEmpty()) return ResponseEntity.notFound().build();
        List<Product> productList = products.values().stream().toList();
        return ResponseEntity.ok(productList);
    }

		// 특정 ID의 상품 수정
    @PutMapping("/{id}")
    public ResponseEntity<String> updateProduct(@PathVariable Long id, @RequestBody Product product) {
        if (!products.containsKey(id))     return ResponseEntity.notFound().build();
        product.setId(id);
        products.put(id, product);
        return ResponseEntity.ok("Product 업데이트 성공");
    }

		// 특정 ID의 상품 삭제
    @DeleteMapping("/{id}")
    public ResponseEntity<String> deleteProduct(@PathVariable Long id) {
        if (!products.containsKey(id))     return ResponseEntity.notFound().build();
        products.remove(id);
        return ResponseEntity.ok("Product 삭제 성공");
    }
}
```

<br>

## http 테스트
```
### POST
POST http://localhost/api/products
Content-Type: application/json

{
  "name": "pen",
  "price": 9.99
}

### GET /api/products
GET http://localhost/api/products
Accept: application/json

### GET /api/products/{id}
GET http://localhost/api/products/1
Accept: application/json

### PUT
PUT http://localhost/api/products/1
Content-Type: application/json

{
  "name": "new pen",
  "price": 8.88
}

### DELETE
DELETE http://localhost/api/products/1
Content-Type: application/json

{
  "id": 1
}
```

<br>

---

<br>

# Event 예제
일정(ID, 제목, 설명)을 추가, 조회, 수정 및 삭제하는 기능을 제공하는 RESTful API 예제

> 위 예제와 동일하므로 코드 작성 생략

<br>

---

<br>

# 파일 업로드 및 json을 요청 바디로 보내기

## Spring Boot REST Controller 생성
파일을 업로드 받을 수 있는 REST API를 구현해야 하기 위해 `@RestController`와 `MultipartFile`을 활용할 수 있고, `MultipartFile 인터페이스`는 Spring에서 파일 업로드를 처리할 때 사용된다.

- `UploadInfo.java`
```java
package hello.restexam.domain;

import lombok.*;

@Getter @Setter
public class UploadInfo {
    private String description;
    private String tag;
}
```

- `FileUploadController.java`
```java
package hello.restexam.controller;

import hello.restexam.domain.UploadInfo;
import org.springframework.http.ResponseEntity;
import org.springframework.util.StreamUtils;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;
import java.io.*;

@RestController
public class FileController {

    @PostMapping("/upload")
    public ResponseEntity<String> handleFileUpload(
            @RequestParam("file")MultipartFile file,
            // @RequestPart: HTTP request body에 multipart/form-data 가 포함되어 있는 경우에 사용하는 어노테이션
            // 포함되어 있지 않다면 @RequestBody와 같이 동작하게 된다.
            // @RequestBody: JSON 형태의 데이터를 Java 객체에 매핑할 때 사용하는 어노테이션
            @RequestPart("info")UploadInfo uploadInfo
    ){
        String message="";
        System.out.println(file.getOriginalFilename()+"=======================");
        System.out.println(uploadInfo.getDescription()+"=======================");
        System.out.println(uploadInfo.getTag()+"=======================");

        try{
            InputStream inputStream = file.getInputStream();
            StreamUtils.copy(inputStream,
                    new FileOutputStream("/Users/sangjin/Desktop/"+file.getOriginalFilename()));

            message = "업로드 성공" + file.getOriginalFilename() + "!";
            return ResponseEntity.ok().body(message);
        }catch (IOException e){
            message = "업로드 실패 " + file.getOriginalFilename() + "!";
            return ResponseEntity.badRequest().body(message);
        }
    }
}
```

<br>

## Spring Boot 파일 업로드 설정 및 테스트

- `application.yml`
```yaml
# 최대 파일 크기 
spring.servlet.multipart.max-file-size=2MB
# 최대 요청 크기 
spring.servlet.multipart.max-request-size=4MB
```

- `file.http`
```
# 오류 발생

### File Upload Test
POST http://localhost/upload
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="dldldl.jpg"
Content-Type: image/jpeg

< /Users/sangjin/Desktop/cat.jpg
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="info"
Content-Type: application/json

{"description": "Sample file upload", "tag": "test"}
------WebKitFormBoundary7MA4YWxkTrZu0gW--
```

```
# 오류 해결(이유 못 찾음)
 
### File Upload Test
POST http://localhost/upload
Content-Type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW

------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="file"; filename="cat.jpg"
Content-Type: image/jpeg

< /Users/sangjin/Desktop/cat.png
------WebKitFormBoundary7MA4YWxkTrZu0gW
Content-Disposition: form-data; name="info" filename="info.json"
Content-Type: application/json

{"description": "Sample file upload", "tag": "test"}
------WebKitFormBoundary7MA4YWxkTrZu0gW-
```

![image](https://github.com/hamsangjin/TIL/assets/103736614/3afbd270-f4a8-44f3-834c-0399dff03f2a) | ![image](https://github.com/hamsangjin/TIL/assets/103736614/3b490f0f-981f-4068-b5ba-94f01ce0ab5e)
-- | -- |

<br>

---

<br>

# 파일 다운로드
- `@RestController`는 주로 JSON이나 XML 같은 데이터를 반환하는 RESTful 서비스에 적합하며, HTTP 응답 본문에 직접 데이터를 작성한다.
- `@Controller`는 뷰 테크놀로지와 함께 사용하여 HTML 페이지를 렌더링하는 데 더 적합하고, 파일 다운로드 기능을 구현할 수 있습니다.

- `FileController.java`
```java
package hello.restexam.controller;

import jakarta.servlet.http.HttpServletResponse;
import org.springframework.util.StreamUtils;
import org.springframework.web.bind.annotation.*;
import java.io.*;
import java.nio.file.*;

@RestController
public class FileController {

    @GetMapping("/download")
    public void downloadFile(HttpServletResponse response){
        Path path = Paths.get("/Users/sangjin/Desktop/cat.jpg");
        response.setContentType("image/jpeg");
        try {
            InputStream inputStream = Files.newInputStream(path);
            StreamUtils.copy(inputStream, response.getOutputStream());
            response.flushBuffer();
        }catch (IOException e){
            e.printStackTrace();
        }
    }
}
```

- `file.http`
```
### File Download Test
GET http://localhost/download
Accept: image/jpeg
```

![image](https://github.com/hamsangjin/TIL/assets/103736614/c6641d81-a235-4979-be71-7ce9fb7befef)
