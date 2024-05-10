# Bean

## 프로그래머가 직접 생성하는 인스턴스
- Java에서 인스턴스 생성
  - 프로그래머가 직접 인스턴스를 만든다.
    ```java
    Book book = new Book();
    ```
- Bean은 컨테이너가 관리하는 객체
  - 객체의 생명주기를 컨테이너를 관리한다.
  - 객체를 싱글톤으로 만들 것인지, 프로토타입으로 만들 것인지 선택

<br>

## Spring IoC컨테이너가 관리하는 Bean
Spring Boot에서 `Bean`은 Spring `IoC 컨테이너`가 관리하는 객체를 의미하며, Spring 프레임워크에서는 객체의 생성, 생명주기 관리, 그리고 객체 간의 의존성을 컨테이너가 처리하며, 이렇게 관리되는 객체를 `Bean`이라고 부른다.

<br>

## Bean의 주요 특징 
1. `관리의 자동화`: Bean은 Spring 컨테이너에 의해 인스턴스화, 구성, 관리되며, 개발자는 복잡한 객체 생성 및 관리 과정에 대해 걱정할 필요가 없다.
2. `싱글턴 패턴`: 기본적으로, Spring에서 Bean은 싱글턴 패턴을 따라 하나의 인스턴스만 생성되며 즉, 어플리케이션 내에서 해당 Bean에 대한 요청이 있을 때마다 `동일한 객체 인스턴스`가 반환된다. 필요에 따라 `프로토타입`과 같은 다른 스코프를 설정할 수도 있다.
3. `의존성 주입`: Bean은 필요한 의존성을 자동으로 주입받고, 설정 파일, 어노테이션 등을 통해 구성할 수 있으며, `생성자 주입`이나 `세터 주입` 등의 방법을 사용할 수 있다.

<br>

## Bean 정의 방법
1. `XML 기반 구성`: XML 파일 내에 `<bean>` 태그를 사용하여 Bean을 선언하고 구성할 수 있으며, 초기 Spring에서 널리 사용되었다.
2. `자바 기반 구성`: `@Configuration` 클래스 내부에서 `@Bean` 어노테이션을 사용하여 메소드로부터 Bean을 생성하고 구성할 수 있으며, 코드 내에서 직접적이고 명확한 구성을 제공한다.
3. `컴포넌트 스캐닝`: `@Component`, `@Service`, `@Repository`, `@Controller` 등의 어노테이션을 클래스에 적용하여, Spring Boot의 자동 스캔 기능을 사용해 자동으로 Bean을 등록할 수 있다.
  - `@SpringBootApplication` 어노테이션은 `@ComponentScan`을 포함 하고 있어서, 어플리케이션의 메인 클래스가 위치한 패키지 및 그 하위 패키지를 자동으로 스캔한다.

<br>

## 인스턴스 생성 예제
일단 공통적으로 사용하는 `Book` 클래스를 작성해줬다.

### `Book.java`
```java
public class Book {       
    private String title;
    private int price;

    public Book(String title, int price) {
        this.title = title;
        this.price = price;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public int getPrice() {
        return price;
    }

    public void setPrice(int price) {
        this.price = price;
    }
}
```

<br>

### 1. 프로그래머가 직접 인스턴스를 생성
- `BookExam01.java`
```java
public class BookExam01 {
    public static void main(String[] args) {
        Book book = new Book("Java", 10000);
        System.out.println(book.getTitle());
        System.out.println(book.getPrice());
    }
}
```
- `new Book("Java", 10000)`: 생성자가 호출되면 Heap 메모리에 인스턴스 생성
- `Book book`: 생성된 인스턴스를 참조한다.

<br>

### 2. Spring Boot에서 JavaConfig를 이용해 인스턴스 생성
- `BookConfig.java`
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class BookConfig {
    @Bean
    public Book book() {
        return new Book("Java", 10000);
    }
}
```

- `MainApplication.java`
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(MainApplication.class, args);
        Book book = context.getBean(Book.class);
        System.out.println("Book Title: " + book.getTitle());
        System.out.println("Book Price: " + book.getPrice());
    }
}
```

<br>

### 3. 컴포넌트 스캔으로 인스턴스 생성

- `Book.java`
```java
import org.springframework.stereotype.Component;

@Component
public class Book {
    private String title;
    private int price;

    public Book() {
        System.out.println("Book Constructor 생성!");
    }
    ...
}
```

- `BookConfig.java`
```java
import org.springframework.context.annotation.*;

@Configuration
@ComponentScan(basePackages = "sample.bean")
public class BookConfig {}
```

- `BookExam.java`
```java
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import sample.bean.Book;
import sample.config.BookConfig;

public class BookExam {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(BookConfig.class);

        Book book = context.getBean(Book.class);
        book.setTitle("모두의 자바");
        System.out.println(book);
    }
}
```
- `basePackages`: applicationContext를 구성할때 이렇게 명시적으로 내가 읽어들여야하는 component들이 있는 package를 넣어줘야 한다.

<br>

## Bean Scope
DI컨테이너는 빈의 생존 기간도 관리하고, 빈의 생존 기간을 빈 스코프(Bean Scope) 라고 한다.
- `singleton`: DI컨테이너를 기동할 때 빈 인스턴스 하나가 만들어지고, 이후부터는 그 인스턴스를 공유하는 방식이며, 기본 스코프다.
- `prototype`: DI컨테이너에 빈을 요청할 때마다 새로운 빈 인스턴스가 만들어지고, 멀티스레드 환경에서 오동작이 발생하지 않아야 하는 빈일 경우 사용한다.
- `request`: HTTP 요청이 들어올 때마다 새로운 빈 인스턴스가 만들어진다. 웹 애플리케이션을 만들 때만 사용할 수 있다.
- `session`: HTTP 세션이 만들어질 때마다 새로운 빈 인스턴스가 만들어진다. 웹 애플리케이션을 만들 때만 사용할 수 있다.

<br>

## BeanScope 예제
- `BookConfig.java`
```java
import org.springframework.context.annotation.*;

@Configuration
public class BookConfig {
    @Bean
    @Scope("singleton")
    public Book singletonBook() {
        return new Book("Singleton Book", 10000);
    }

    @Bean
    @Scope("prototype")
    public Book prototypeBook() {
        return new Book("Prototype Book", 20000);
    }
}
```

- `MainApplication.java`
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ApplicationContext;

@SpringBootApplication
public class MainApplication {
    public static void main(String[] args) {
        ApplicationContext context = SpringApplication.run(MainApplication.class, args);

        // Singleton bean
        Book singletonBook1 = context.getBean("singletonBook", Book.class);
        Book singletonBook2 = context.getBean("singletonBook", Book.class);

        // Prototype bean
        Book prototypeBook1 = context.getBean("prototypeBook", Book.class);
        Book prototypeBook2 = context.getBean("prototypeBook", Book.class);
        System.out.println(singletonBook1 == singletonBook2);    // true
        System.out.println(prototypeBook1 == prototypeBook2);    // false
    }
}
```

<br>

## 의존성 주입 방법

- `생성자 주입`: 생성자 기반 의존성 주입 방식은 각 클래스의 필수 의존성을 생성자를 통해 주입받는 방식
- `setter 주입`: 객체의 필수 의존성이 아닌 선택적 의존성을 주입할 때 주로 사용
- `필드 주입`: Spring Framework에서 `@Autowired` 애노테이션을 사용하여 클래스 필드에 직접 의존성을 주입하는 방법

<br>

### 의존성 주입 예제
- `Post.java`
```java
public class Post {
    private Long id;
    private String title;
    private String content;

    public Post(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }

    // Getters and Setters
    public Long getId() {
        return id;
    }
    public void setId(Long id) {
        this.id = id;
    }
    public String getTitle() {
        return title;
    }
    public void setTitle(String title) {
        this.title = title;
    }
    public String getContent() {
        return content;
    }
    public void setContent(String content) {
        this.content = content;
    }
}
```

- `PostRepository.java`
```java
import java.util.*;

public class PostRepository {
    private List<Post> posts = new ArrayList<>();
    public void save(Post post) {
        posts.add(post);
    }
    public Optional<Post> findById(Long id) {
        return posts.stream().filter(post -> post.getId().equals(id)).findFirst();
    }
    public List<Post> findAll() {
        return posts;
    }
}
```

- `PostService`
```java
import java.util.List;
import org.springframework.beans.factory.annotation.Autowired;

public class PostService {
    
    // 1. 생성자 주입
    private final PostRepository postRepository;
    public PostService(PostRepository postRepository) {
        this.postRepository = postRepository;
    }
    // 여기서 사용한 2, 3번의 방법은 확실하지 않음
    // 2. setter 주입
    // private final PostRepository postRepository;
    // public setPostRepository(PostRepository postRepository) {
    //     this.postRepository = postRepository;
    // }

    // 3. 필드 주입
    // @Autowired
    // private PostRepository postRepository;

  
    public void publishPost(Post post) {
       postRepository.save(post);
    }
    public List<Post> getAllPosts() {
       return postRepository.findAll();
    }
}
```
