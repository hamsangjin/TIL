# 수정, 삭제 사용

## UserDao.java
```java
package com.example.jpa;

import jakarta.persistence.*;

public class UserDao {
    // update
    public void updateUser(User user) {
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        try{
            em.getTransaction().begin();

            em.merge(user);     // 해당 id를 가진 유저가 영속성 컨텍스트에 없다면 db에서 가져와서 스냅샷을 생성

            // 업데이트가 중간에 여러번 일어나더라도, 커밋할 때 스냅샷에 저장된 값(db)과 똑같으면 update 자체가 일어나지 않는다.
            em.getTransaction().commit();
        } finally {
            em.close();
        }
    }

    // delete
    public void deleteUser(User user) {
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        try {
            em.getTransaction().begin();

            // 해당 유저가 있어야 삭제가 될테니까 있는지 확인 -> em.contains()
            // 있다면 입력받은 user를 삭제
            // 없다면 merge를 통해 user를 생성한 후, 삭제
            em.remove(em.contains(user) ? user : em.merge(user));

            em.getTransaction().commit();
        } finally {
            em.close();
        }
    }
}
```

<br>

## UserMain.java
```java
package com.example.jpa;

import org.slf4j.*;

public class UserMain {
    private static final Logger logger = LoggerFactory.getLogger(UserMain.class);
    public static void main(String[] args) {
        UserDao userDao = new UserDao();

        // update
        User user1 = new User();
        user1.setId(1L);
        user1.setName("sangjin");
        user1.setEmail("han@sang.jin");
        userDao.updateUser(user1);

        // delete
        User user2 = new User();
        user2.setId(2L);
        userDao.deleteUser(user2);
    }
}
```

<br>

---

<br>

# 엔티티
`엔티티`는 데이터베이스의 테이블을 객체지향적으로 추상화한 것으로, 각 엔티티 인스턴스는 테이블의 개별 레코드(행)에 해당하고, 엔티티 클래스의 속성은 테이블의 컬럼과 매핑된다.

## 엔티티 클래스의 조건
1. `@Entity 어노테이션`: 클래스는 `@Entity` 어노테이션으로 표시하여, 해당 클래스가 JPA 엔티티임을 나타낸다.
2. `식별자`: 각 엔티티는 유일하게 식별될 수 있는 식별자를 `@Id` 어노테이션을 사용하여 지정한다.
3. `기본 생성자`: 엔티티 클래스는 JPA 구현체가 엔티티 인스턴스를 프로그래밍적으로 생성할 수 있도록 `public` 또는 `protected`의 기본 생성자를 가져야한다.
4. `클래스 수준에서의 제한`: 최종 클래스(final class)가 아니어야 하며, 변경 가능해야 하고, 일반 Java 객체처럼 메서드나 속성을 추가할 수 있다.

<br>

## 엔티티 예제 코드
```java
package com.example.jpa;

import jakarta.persistence.*;

@Entity
@Table(name = "user")
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)  // AUTO_INCREMENT 속성과 같음
    private Long id;
    @Column(name = "username")
    private String username;
    @Column(name = "email")
    private String email;

    // 기본 생성자
    public User() { }

    // 추가적인 생성자와 setter, getter가 올 수 있다.
}
```

<br>

## 엔티티에서의 hashCode와 Equals

### 필요성
1. `컬렉션 사용`: `HashSet`이나 `HashMap` 같은 컬렉션에서 엔티티를 키로 사용할 경우, `hashCode`와 `equals`를 올바르게 구현해야 컬렉션이 정상적으로 작동한다.
2. `영속성 컨텍스트`: JPA에서는 영속성 컨텍스트가 엔티티의 동일성을 관리하며, 엔티티가 컬렉션에 저장되거나 다른 엔티티와 비교될 때 `equals`와 `hashCode` 메소드가 중요한 역할을 한다.


### 구현 권장 사항
- `식별자 필드(id) 기반 비교`: 일반적으로 엔티티의 `equals`와 `hashCode` 메소드는 식별자 필드를 기준으로 구현된다.
- `일관성 유지`: `hashCode` 메소드는 `equals` 메소드의 결과와 일관성을 유지해야 한다.
- `비영속 엔티티 처리`: 엔티티가 영속화되지 않은 상태에서는 `id`가 `null`일 수 있으므로, `hashCode`와 `equals` 구현에서 `null` 처리를 올바르게 해야 한다.

### 예제 코드
```java
package com.example.jpa;

import jakarta.persistence.*;

@Entity
@Table(name = "user")
public class User {

    ......

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        User user = (User) o;
        return id != null && id.equals(user.id);
    }

    @Override
    public int hashCode() {
        return id != null ? id.hashCode() : 0;
    }
}
```

<br>

## 엔티티 매니저와 영속성 컨텍스트

### 엔티티 매니저(EntityManager)
`엔티티 매니저`는 JPA에서 데이터베이스 작업을 관리하는 주요 인터페이스로, 엔티티 매니저는 엔티티의 생명주기를 관리하며, CRUD 연산을 수행하는 API를 제공한다.

엔티티 매니저는 `EntityManagerFactory`에 의해 생성되며, 각 `EntityManager` 인스턴스는 데이터베이스 세션과 직접 연결되어 독립적인 작업을 수행할 수 있다.

### 영속성 컨텍스트(Persistence Context)
`영속성 컨텍스트`는 엔티티를 영구 저장하는 환경을 말하며, 엔티티의 생명주기와 상태를 관리한다. <br>
`엔티티 매니저`를 통해 엔티티가 영속성 컨텍스트에 저장될 때, JPA는 엔티티의 상태를 관리하고, 데이터베이스와의 동기화를 담당한다.

1. `1차 캐시`: 영속성 컨텍스트는 엔티티의 1차 캐시 역할을 수행하여, 한 트랜잭션 내에서 반복된 데이터베이스 호출을 최소화한다. -> `수정, 삭제 예제 커밋 부분`
2. `엔티티의 생명주기 관리`: 엔티티의 상태(비영속, 영속, 삭제, 분리)를 관리한다.
3. `변경 감지`: 트랜잭션이 커밋될 때, 영속성 컨텍스트에 있는 엔티티들을 검사하여 변경된 엔티티를 자동으로 데이터베이스에 반영한다. -> `스냅샷과 비교해서 반영`
4. `지연 로딩`: 엔티티와 그 연관된 객체들을 필요한 시점까지 로딩을 지연시켜 성능을 최적화한다.

### 엔티티 매니저와 영속성 컨텍스트의 상호작용
```java
import jakarta.persistence.EntityManager;
import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;

public class Example {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("ExamplePU");
        EntityManager em = emf.createEntityManager();

        em.getTransaction().begin();

        // 새 엔티티 생성 및 영속화
        User newUser = new User("carami", "carami@example.com");
        em.persist(newUser);

        // 엔티티 조회 -> 위 newUser가 영속화되었기 때문에 영속성 컨텍스트에 있는 newUser를 리턴
        User user = em.find(User.class, newUser.getId());
        em.getTransaction().commit();
        em.close();
    }
}
```

<img width="556" alt="스크린샷 2024-05-28 11 37 32" src="https://github.com/hamsangjin/TIL/assets/103736614/e2d06ec0-f498-42dd-bd30-33bcb868ca89">

<br>
<br>

## 지연 실행

### 지연 실행
- `배칭`: JPA는 여러 SQL 문을 함께 배치 처리할 수 있어 데이터베이스로의 왕복 횟수 를 줄일 수 있다.
- `순서 보장`: SQL 작업이 올바른 순서로 실행되도록 보장하여 참조 무결성을 유지한다.

### 변경 축적
- `최적화`: JPA는 실행해야 할 SQL 문의 수를 최적화할 수 있습니다. 예를 들어, 한 트랜 잭션 내에서 엔티티가 여러 번 수정된 경우, JPA는 최종 상태에 대해서만 하나의 UPDATE 문을 발행할 필요가 있습니다.
- `불필요한 작업 감소`: 데이터베이스에 불필요한 쓰기 작업을 방지합니다.

### 트랜잭션 무결성 보장
- `원자성`: 트랜잭션 경계 내에서 발생하는 모든 변경 사항은 완전히 커밋되거나 오류가 발생할 경우 전체적으로 롤백된다.
- `격리성`: 트랜잭션이 커밋될 때까지 변경사항이 다른 트랜잭션에 보이지 않는다.

### 자원 사용 최적화
- `잠금 감소`: 쓰기 작업을 지연함으로써 데이터베이스 행에 대한 잠금 시간을 줄일 수 있으며, 이는 잠금 경합을 줄이고 애플리케이션의 동시성을 개선해준다.
- `연결 사용 효율성`: 데이터베이스 연결이 실제 필요한 SQL 실행에만 사용되므로, 연결 사용이 더 효율적이다.

### 예시 코드 - 트랜잭션 관리
```java
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

// 엔티티가 저장되지만 데이터베이스에는 즉시 기록되지 않습니다.
User newUser = new User("carami", "carami@example.com"); em.persist(newUser);

// 추가 수정 사항이 있을 수 있음
newUser.setEmail("carami@example.com");

// 커밋: 모든 변경사항이 데이터베이스에 효율적으로 반영됩니다.
em.getTransaction().commit();

em.close();
```

### 예시 코드 - 조회시 변경사항에 따라 자동 업데이트
```java
import jakarta.persistence.EntityManager;
import jakarta.persistence.EntityManagerFactory;
import jakarta.persistence.Persistence;
public class UpdateExample {
    public static void main(String[] args) {
        EntityManagerFactory emf = Persistence.createEntityManagerFactory("ExamplePU");
        EntityManager em = emf.createEntityManager();
        try {
            em.getTransaction().begin();

            // ID로 특정 엔티티를 조회
            User user = em.find(User.class, 1L);

            // 엔티티의 필드 값을 변경
            if (user != null) {
                user.setEmail("updated.email@example.com");   // 이메일 필드 수정
            }

            // 트랜잭션을 커밋할 때 변경된 내용이 자동으로 데이터베이스에 반영
            em.getTransaction().commit();
        } finally {
            em.close();
            emf.close();
        }
    }  
}
```

<br>

## 엔티티의 생명주기
![image](https://github.com/hamsangjin/TIL/assets/103736614/8367699e-1ae8-4715-be41-43c7606c0f2f)

### 주요 생명주기 상태
1. `비영속(New) 상태`: 엔티티가 새로 생성되었지만 아직 영속성 컨텍스트에는 관리되지 않는 상태
2. `영속(Managed) 상태`: 엔티티가 영속성 컨텍스트에 저장된 상태
3. `분리된(Detached) 상태`: 엔티티가 영속성 컨텍스트에서 분리된 상태
4. `삭제된(Removed) 상태`: 엔티티가 삭제되어 데이터베이스에서도 삭제될 상태

### 생명주기 전환 메서드
- `persist(entity)`: 엔티티를 영속 상태로 만든다.
- `merge(entity)`: 분리된 상태의 엔티티를 다시 영속 상태로 복구한다.
- `remove(entity)`: 엔티티를 삭제 상태로 전환한다.
- `detach(entity)`: 엔티티를 분리 상태로 만들며, 이를 통해 성능 및 메모리 사용 최적화, 트랜잭션 관리, 테스트와 디버깅 등을 수행할 수 있다.

<br>

## 영속성 유닛(Persistence Unit)
`영속성 유닛`은 엔티티 클래스와 데이터베이스 연결 설정을 그룹화하는 JPA의 구성 단위이며, 애플리케이션에서 데이터를 관리하는 방법을 정의하고, 주로 `persistence.xml` 파일 내에 설정된다.

### 영속성 유닛의 주요 구성 요소
1. `엔티티 클래스 목록`: 영속성 유닛은 하나 이상의 엔티티 클래스를 포함할 수 있으며, 이들은 데이터베이스 테이블과 매핑된다.
2. `데이터 소스 설정`: 데이터베이스 연결을 위한 설정 정보를 포함하며, 이는 JPA 구현체가 데이터베이스와의 연결을 관리하는 데 사용된다.
3. `트랜잭션 타입`: JPA는 `리소스 로컬(Resource-local)`과 `JTA(Java Transaction API)` 트랜잭션 두 가지 타입을 지원하며, 사용 환경에 따라 적합한 트랜잭션 관리 방식을 선택할 수 있다.

<br>

---

<br>

# 엔티티 매핑

## 기본 엔티티 매핑

### @Entity
`@Entity 어노테이션`은 클래스가 JPA 엔티티임을 나타내며, 이 클래스의 인스턴스는 데이터베이스에 저장될 데이터를 나타낸다.

#### 주요 속성
- `name`: 엔티티 이름 지정
- `catalog`: 엔티티가 속할 데이터베이스 카탈로그 지정
- `schema`: 엔티티가 속할 데이터베이스 스키마 지정
- `table`: 엔티티에 매핑될 데이터베이스 테이블 이름 지정
- `readOnly`: 엔티티의 읽기 전용 여부 설정
- `inheritance`: 엔티티의 상속 전략 지정

### @Id
`@Id 어노테이션`은 클래스의 필드를 테이블의 기본 키(primary key)로 지정하며, 각 엔티티 인스턴스를 유일하게 식별하는 데 사용된다.

### GeneratedValue
`@GeneratedValue 어노테이션`은 기본 키의 값을 자동으로 생성할 방법을 명시한다. 

#### 주요 기본 키 생성 전략
- `GenerationType.AUTO`: JPA 구현체가 자동으로 가장 적합한 전략을 선택한다. 
- `GenerationType.IDENTITY`: 데이터베이스의 ID 자동 생성 기능을 사용하여 기본 키를 생성한다.
- `GenerationType.SEQUENCE`: 데이터베이스의 시퀀스를 사용하여 기본 키를 생성한다.
- `GenerationType.TABLE`: 키 생성을 위한 별도의 데이터베이스 테이블을 사용한다.

### @Table
`@Table 어노테이션`은 엔티티 클래스가 데이터베이스의 어떤 테이블에 매핑될 것인지를 명시한다.

#### 주요 속성
- `name`: 매핑될 테이블의 이름을 지정하며, 지정하지 않으면 클래스 이름을 사용한다.
- `catalog`: 엔티티가 속할 데이터베이스 카탈로그를 지정한다.
- `schema`: 엔티티가 속할 데이터베이스 스키마를 지정한다.
- `uniqueConstraints`: 테이블 수준에서 유니크 제약 조건을 지정하며, 여러 열을 포함할 수 있는 제약 조건을 정의할 때 사용된다.

### @Column
`@Column 어노테이션`은 엔티티의 필드가 데이터베이스 테이블의 어떤 열에 매핑될 것인지를 정의한다. 

#### 주요 속성
- `name`: 필드가 매핑될 테이블의 열 이름을 지정하며, 지정하지 않으면 필드 이름이 사용된다.
- `nullable`: 열이 널(Null) 값을 허용할지 여부를 지정하며, 기본값은 `true`이다.
- `length`: 문자열 필드의 경우 열의 최대 길이를 지정하며, 기본값은 `255`입니다.
- `unique`: 이 열이 테이블 내에서 유니크한 값을 가져야 할지 지정한다.
- `insertable` 및 `updatable`: 이 필드가 데이터베이스에 삽입하거나 업데이트할 수 있는지 여부를 지정하며, 둘 다 기본값은 `true`이다.

<br>

## 관계 매핑

### @OneToMany, @ManyToOne
`@OneToMany`와 `@ManyToOne` 관계는 엔티티 간의 일대다 관계를 매핑할 때 사용하는 어노테이션이다. 

- `@OneToMany` 관계: 한 엔티티가 다른 엔티티 여러 개와 관계를 가질 수 있다.
- `@ManyToOne` 관계: 엔티티 여러 개가 다른 엔티티 하나와 관계를 가질 수 있다.

### 예제
- 테이블 생성
```sql
CREATE TABLE schools (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);

CREATE TABLE students (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    school_id BIGINT,
    FOREIGN KEY (school_id) REFERENCES schools(id)
);
```

- 테이블 INSERT문
```sql
-- Schools 추가
INSERT INTO schools (name) VALUES ('Greenwood High School');
INSERT INTO schools (name) VALUES ('Riverside Academy');

-- Students 추가
INSERT INTO students (name, school_id) VALUES ('Alice', 1);
INSERT INTO students (name, school_id) VALUES ('Bob', 1);
INSERT INTO students (name, school_id) VALUES ('Charlie', 1);
INSERT INTO students (name, school_id) VALUES ('David', 2);
INSERT INTO students (name, school_id) VALUES ('Eva', 2);
```

- Entity 클래스 - School
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name="schools")
@Getter@Setter
@NoArgsConstructor
public class School {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    // cascade: 학교가 수정되거나 삭제되면 연관되어있던 학생도 같이 수정되거나 삭제
    // orphanRemoval: 부모(학교) 엔티티와 연관관계가 끊어진 자식(학생) 엔티티를 삭제
    @OneToMany(mappedBy = "school", cascade = CascadeType.ALL, orphanRemoval = true)    // 학교 1 : 학생 n
    private List<Student> students = new ArrayList<>();                                 // 일대다 관계

    public School(String name) {
        this.name = name;
    }
}
```

- Entity 클래스 - Student
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;

@Entity
@Table(name="students")
@Getter@Setter
@NoArgsConstructor
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @ManyToOne                          // 본인 기준 관계(학생 n : 학교 1)
    @JoinColumn(name = "school_id")     // 조인 컬럼명
    private School school;  // school_id를 사용하기 위한 방법

    public Student(String name, School school) {
        this.name = name;
        this.school = school;
    }
}
```

- 조회, 삽입, 수정, 삭제 예제
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class SchoolExamMain {

    private static void find(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            // 1번 학교 인스턴스 생성 후 이름 출력
            School school = em.find(School.class, 1L);
            log.info("schhol name : {}", school.getName());

            // 1번 학교의 학생들 이름 출력
            for (Student student : school.getStudents()) {
                log.info("student name : {}", student.getName());
            }

            // 5번 학생의 이름과 학교이름 출력
            Student student = em.find(Student.class, 5L);
            log.info("student name: {}", student.getName());
            log.info("school name: {}", student.getSchool().getName());
            em.getTransaction().commit();

        } finally {
            em.close();
        }
    }

    private static void create(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            School school = new School("LionSchool");

            Student student1 = new Student("홍길동", school);
            Student student2 = new Student("아무개", school);
            Student student3 = new Student("삽살개", school);

            school.getStudents().add(student1);
            school.getStudents().add(student2);
            school.getStudents().add(student3);

            // 영속화
            em.persist(school);

            // 실제 db와 비교해 반영
            em.getTransaction().commit();
        }finally {
            em.close();
        }
    }

    private static void update(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            School school = em.find(School.class, 2L);      // find: 이미 영속성 컨텍스트에 존재

            school.setName("update School Name");

            em.getTransaction().commit();
        }finally {
            em.close();
        }
    }

    private static void delete(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            School school = em.find(School.class, 5L);

            // cascade 속성으로 학교가 삭제될 때 해당 학교에 소속되어 있는 학생들도 삭제된다.
            em.remove(school);

            em.getTransaction().commit();
        }finally {
            em.close();
        }
    }

    public static void main(String[] args) {
//        find();
//        create();
//        update();
        delete();
    }
}
```
