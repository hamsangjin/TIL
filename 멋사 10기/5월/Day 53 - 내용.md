# 쿼리 메소드

## 네이티브 쿼리 활용
특정 데이터베이스 기능이나 구문을 직접 사용해야 할 경우, Spring Data JPA에서는 네이티브 SQL 쿼리를 작성하고 실행할 수 있는 기능을 지원한다.

### 네이티브 쿼리 사용방법
`@Query` 어노테이션 내에서 `nativeQuery=true` 로 설정하고, SQL 쿼리를 직접 작성하면 된다.
```java
import org.springframework.data.jpa.repository.*;
import java.util.List;

public interface UserRepository extends JpaRepository<User, Long> {
    // 네이티브 SQL을 사용한 사용자 조회
    @Query(value = "SELECT * FROM jpa_user WHERE email LIKE %?1%", nativeQuery = true)
    List<User> findByEmailNative(String email);
}
```

### 네이티브 쿼리 장단점

#### 장점
- 데이터베이스의 특정 기능을 최대한 활용할 수 있다.
- 복잡한 쿼리나 기존에 작성된 쿼리를 그대로 사용할 수 있어 편리하다.

#### 단점
- 데이터베이스 간 이식성이 낮아져, 한 데이터베이스 시스템에서 작성된 쿼리가 다른 시스템에서는 작동하지 않을 수 있다.
- JPA의 다양한 추상화 기능과 최적화를 활용하기 어려울 수 있다.


### 네이티브 쿼리 예제
- User 엔티티
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

- UserDto
```java
package hello.springdatajpa.dto;

import lombok.*;

@Getter @Setter
@NoArgsConstructor
@AllArgsConstructor
public class UserDto {
    private String name;
    private String email;
}
```

- UserRepository 인터페이스
```java
package hello.springdatajpa.repository;

import hello.springdatajpa.domain.User;
import org.springframework.data.jpa.repository.*;
import org.springframework.data.repository.query.Param;

import java.util.List;

public interface UserRepository extends JpaRepository<User, Long>{
    // 네이티브 SQL을 사용한 사용자 조회
    @Query(value = "SELECT * FROM jpa_user WHERE email LIKE %?1%", nativeQuery = true)
    List<User> findByEmailNative(String email);

    @Query(value = "SELECT COUNT(*) FROM jpa_user WHERE age > 30 AND status = 'ACTIVE'", nativeQuery = true)
    int countActiveUsersOlderThan30();

    // 네이티브 쿼리를 사용하여 특정 칼럼을 조회하는 예제 Object[]을 여러개 List로 반환
    @Query(value = "SELECT name, email FROM jpa_user WHERE name LIKE %:name%", nativeQuery = true)
    List<Object[]> findUsersByNameNative(@Param("name") String name);
}
```

- UserRepository 테스트
```java
package hello.springdatajpa.repository;

import hello.springdatajpa.domain.User;
import hello.springdatajpa.dto.UserDto;
import lombok.extern.slf4j.Slf4j;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.transaction.annotation.Transactional;

import java.util.*;
import java.util.stream.Collectors;

import static org.assertj.core.api.Assertions.*;
import static org.junit.jupiter.api.Assertions.*;

@Slf4j
@SpringBootTest
@Transactional 
class UserRepositoryTest {
    @Autowired
    private UserRepository userRepository;

    @Test
    void findByEmailNative(){
        List<Object[]> byEmailNative = userRepository.findUsersByNameNative("ham");
        // 조회 결과를 dto로 전달
        List<UserDto> userDtos = byEmailNative.stream()
                        .map(result -> new UserDto((String) result[0], (String) result[1]))
                                .collect(Collectors.toList());

        userDtos.forEach(userDto -> log.info("이름으로 찾은 사용자: " + userDto.getName() + ", 이메일: " + userDto.getEmail()));

        assertThat(userDtos).isNotEmpty();  // 값이 들어있냐
        assertThat(userDtos).hasSize(1);    // 결과가 1개냐

        // 첫 번째 사용자 검증
        UserDto firstUser = userDtos.get(0);
        assertThat(firstUser.getName()).isEqualTo("hamsangjin");     // 첫 번째 사용자의 name이 hamsangjin이냐
        assertThat(firstUser.getEmail()).isEqualTo("ham@sang.jin");  // 첫 번째 사용자의 email이 ham@sang.jin이냐
    }
}
```

<br>

---

<br>

# Criteria AP를 이용한 프로그래밍
`Criteria API`는 JPA의 일부로서, 프로그래밍 방식으로 타입-세이프한 쿼리를 구성할 수 있는 API이다.

SQL이나 JPQL 문자열을 직접 작성하지 않고도 동적으로 쿼리를 생성하고 실행할 수 있게 해준다.

## 구성 요소
- `CriteriaBuilder`: 쿼리를 생성하고 수정하는 데 사용되는 팩토리 클래스로, 쿼리의 각 조건과 표현을 생성한다.
- `CriteriaQuery`: 생성할 쿼리를 정의하고, 반환할 엔티티 타입과 WHERE, GROUP BY, ORDER BY 같은 쿼리 조건을 지정할 수 있다.
- `Root`: 쿼리의 FROM 절에 해당하는 주 엔티티를 지정하며, Root 객체를 사용하여 쿼리의 기준이 되는 엔티티를 정의한다.

<br>

## 예제
```java
@Override
public List<User> findUsersByName(String name) {
    CriteriaBuilder cb = entityManager.getCriteriaBuilder();      // CriteriaBuilder
    CriteriaQuery<User> query = cb.createQuery(User.class);       // CriteriaQuery
    Root<User> user = query.from(User.class);                     // Root
    query.select(user).where(cb.equal(user.get("name"), name));   // 쿼리 생성
    return entityManager.createQuery(query).getResultList();      // 쿼리 실행결과 리턴
}
```

<br>

## Criteria API를 이용한 동적 쿼리 생성
정적 쿼리만으로는 모든 사용 사례를 처리할 수 없으며, 사용자가 제공하는 여러 필터 옵션에 따라 결과를 반환해야 하는 경우, 동적 쿼리가 필요해진다. 

### Criteria API의 동적 쿼리 생성 절차
1. `CriteriaBuilder 인스턴스 생성`: `EntityManager`로부터 `CriteriaBuilder` 객체를 얻으며, 쿼리의 조건 및 구조를 정의하는 데 사용된다.
2. `CriteriaQuery 객체 생성`: `CriteriaBuilder`를 사용하여 `CriteriaQuery` 객체를 생성하며, 반환될 결과의 형식을 정의한다.
3. `Root 정의`: 쿼리의 FROM 절에 해당하는 Root 객체를 생성히며, 쿼리의 메인 테이블을 나타낸다.
4. `조건 추가`: 필요한 검색 조건을 `CriteriaBuilder`를 통해 생성하고 `CriteriaQuery`에 추가한다.
5. `쿼리 실행`: 완성된 `CriteriaQuery`를 `EntityManager`를 통해 실행한다.

### 예제
```java
 public List<User> findUsersDynamically(String name, String email) {
      CriteriaBuilder cb = entityManager.getCriteriaBuilder();    // 1
      CriteriaQuery<User> query = cb.createQuery(User.class);     // 2
      Root<User> user = query.from(User.class);                   // 3

      // 4
      List<Predicate> predicates = new ArrayList<>();
      if (name != null) {
          predicates.add(cb.equal(user.get("name"), name));
      }

      if (email != null) {
          predicates.add(cb.equal(user.get("email"), email));
      }

      query.select(user).where(cb.and(predicates.toArray(new Predicate[0])));

      // 5
      return entityManager.createQuery(query).getResultList();
}
```

<br>

---

<br>

# 실전 예제

## ERD - hr
<img width="427" alt="스크린샷 2024-05-31 16 14 07" src="https://github.com/hamsangjin/TIL/assets/103736614/3a2079a7-48d6-4ce2-920b-5b88c2aba801">

<br>

## Entity 생성

ERD를 보고 엔티티들의 `기본/외래키`, `속성`, `null 여부`, `조인 관계` 등을 정의할 수 있다.

이때, 조인 관계를 정의하면서 단방향 관계인 경우에 무조건 단방향으로 매핑한다고 생각할 수 있지만, 단방향이더라도 해당 객체가 사용할 가능성이 있다면 양방향으로도 매핑할 수 있다.

- `Employee.java`
```java
package hello.springdatajpa2.domain;

import jakarta.persistence.*;
import lombok.*;
import java.time.LocalDate;
import java.util.List;

@Entity
@Table(name = "employees")
@Getter @Setter
@NoArgsConstructor
public class Employee {
    @Id
    @Column(name = "employee_id")
    private int employeeId;

    @Column(name = "first_name")
    private String firstName;

    @Column(name = "last_name", nullable = false)
    private String lastName;

    @Column(nullable = false)
    private String email;

    @Column(name = "phone_number")
    private String phoneNumber;

    @Column(name = "hire_date", nullable = false)
    private LocalDate hireDate;

    @ManyToOne
    @JoinColumn(name = "job_id", nullable = false)
    private Job job;

    @Column(name = "salary", nullable = false)
    private Double salary;

    @Column(name = "commission_pct")
    private Double commission_pct;

    @ManyToOne
    @JoinColumn(name = "manager_id")
    private Employee manager;

    @ManyToOne
    @JoinColumn(name = "department_id")
    private Department department;

    // 단방향 - JobHistory 객체 사용 예정
    @OneToMany(mappedBy = "employee")
    private List<JobHistory> jobHistories;

    @OneToMany(mappedBy = "manager")
    private List<Employee> subordinates;

    // 단방향 - 사용 X
    // @OneToMany(mappedBy = "manager")
    // private List<Department> departments;
}
```

<br>

- `Department.java`
```java
package hello.springdatajpa2.domain;

import jakarta.persistence.*;
import lombok.*;
import java.util.List;

@Entity
@Table(name = "departments")
@Getter @Setter
@NoArgsConstructor
public class Department {
    @Id
    @Column(name = "department_id")
    private int departmentId;

    @Column(name = "department_name", nullable = false)
    private String departmentName;

    @ManyToOne
    @JoinColumn(name = "manager_id")
    private Employee manager;

    @ManyToOne
    @JoinColumn(name = "location_id")
    private Location location;

    // 단방향 - Employee 객체 사용 예정
    @OneToMany(mappedBy = "department")
    private List<Employee> employees;

    // 단방향 - 사용 X
    // @OneToMany(mappedBy = "department")
    // private List<JobHistory> jobHistories;
}
```

<br>

- `JobHistory`
```java
package hello.springdatajpa2.domain;

import jakarta.persistence.*;
import lombok.*;
import java.time.*;

@Entity
@Table(name = "job_history")
@Getter @Setter
@NoArgsConstructor
public class JobHistory {
    @Id
    @JoinColumn(name = "employee_id")
    @ManyToOne
    private Employee employee;

    @Id
    @Column(name = "start_date")
    private LocalDate startDate;

    @Column(name = "end", nullable = false)
    private LocalDate endDate;

    @ManyToOne
    @JoinColumn(name = "job_id", nullable = false)
    private Job job;

    @ManyToOne
    @JoinColumn(name = "department_id", nullable = false)
    private Department department;
}
```

<br>

- `Job.java`
```java
package hello.springdatajpa2.domain;

import jakarta.persistence.*;
import lombok.*;
import java.util.List;

@Entity
@Table(name = "jobs")
@Getter @Setter
@NoArgsConstructor
public class Job {
    @Id
    @Column(name = "job_id")
    private String jobId;

    @Column(name = "job_title", nullable = false)
    private String jobTitle;

    @Column(name = "min_salary")
    private Double minSalary;

    @Column(name = "max_salary")
    private Double maxSalary;

    // 단방향 - Employee 객체 사용 예정
    @OneToMany(mappedBy = "job")
    private List<Employee> employees;

    // 단방향 - JobHistory 객체 사용 예정
    @OneToMany(mappedBy = "job")
    private List<JobHistory> jobHistories;
}

```

<br>

- `Location.java`
```java
package hello.springdatajpa2.domain;

import jakarta.persistence.*;
import lombok.*;
import java.util.List;

@Entity
@Table(name = "locations")
@Getter @Setter
@NoArgsConstructor
public class Location {

    @Id @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "location_id")
    private int locationId;

    @Column(name = "street_address")
    private String streetAddress;

    @Column(name = "postal_code")
    private String postalCode;

    @Column(name = "city", nullable = false)
    private String city;

    @Column(name = "state_province")
    private String stateProvince;

    @ManyToOne
    @JoinColumn(name = "country_id", nullable = false)
    private Country country;

    // 단방향 - 사용 X
    @OneToMany(mappedBy = "location")
    private List<Department> departments;
}
```

<br>

- `Country.java`
```java
package hello.springdatajpa2.domain;

import jakarta.persistence.*;
import lombok.*;
import java.util.List;

@Entity
@Table(name = "countries")
@Getter @Setter
@NoArgsConstructor
public class Country {
    @Id
    @Column(name = "country_id")
    private String countryId;

    @Column(name = "country_name")
    private String countryName;

    @ManyToOne
    @JoinColumn(name = "region_id", nullable = false)
    private Region region;

    // 단방향 - 사용 X
    @OneToMany(mappedBy = "country")
    private List<Location> locations;
}
```

<br>

- `Region.java`
```java
package hello.springdatajpa2.domain;

import jakarta.persistence.*;
import lombok.*;
import java.util.List;

@Entity
@Table(name = "regions")
@Getter @Setter
@NoArgsConstructor
public class Region {
    @Id
    @Column(name = "region_id")
    private int regionId;

    @Column(name = "region_name")
    private String regionName;

    // 단방향 - 사용 X
    @OneToMany(mappedBy = "region")
    private List<Country> countries;
}
```

<br>

## Repository
- `EmployeeRepository.java`
```java
package hello.springdatajpa2.repository;

import hello.springdatajpa2.domain.Employee;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

import java.time.LocalDate;
import java.util.*;

@Repository
public interface EmployeeRepository extends JpaRepository<Employee, Integer> {
    List<Employee> findByLastName(String lastName);
    List<Employee> findBySalaryGreaterThanEqual(Double salary);
    List<Employee> findBySalaryLessThanOrSalaryGreaterThan(Double salary1, Double salary2);
    List<Employee> findByHireDateBetween(LocalDate startDate, LocalDate endDate);
    List<Employee> findByDepartmentIdIn(List<Integer> departmentIds);
    List<Employee> findByDepartmentIdInAndSalaryBetween(List<Integer> departmentIds, Double minSalary, Double maxSalary);
    List<Employee> findByManagerIdIsNull();
    List<Employee> findByManagerIdIsNotNull();
    List<Employee> findByCommissionPctNotNullOrderBySalaryDescCommissionPctDesc();
    List<Employee> findByLastNameStartingWith(String prefix);
    Optional<Employee> findById(Integer employeeId);
}
```

## MainApplication
- Application.java
```java
package hello.springdatajpa2;

import hello.springdatajpa2.repository.EmployeeRepository;
import org.slf4j.*;
import org.springframework.boot.*;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import java.time.LocalDate;
import java.util.Arrays;

@SpringBootApplication
public class Application {
    private static final Logger log = LoggerFactory.getLogger(Application.class);
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

    @Bean
    public CommandLineRunner demo(EmployeeRepository employeeRepository) {
        return (args) -> {
        log.info("Employees with last name 'Doe':");
        employeeRepository.findByLastName("Doe").forEach(employee -> log.info(employee.toString()));

        log.info("Employees with salary greater than or equal to 55000:");
        employeeRepository.findBySalaryGreaterThanEqual(55000.0).forEach(employee -> log.info(employee.toString()));

        log.info("Employee with employee ID 1:");
        employeeRepository.findById(1).ifPresent(employee -> log.info(employee.toString())); // 수정된 부분

        log.info("Employees with salary less than 5000.0 or greater than 10000.0:");
        employeeRepository.findBySalaryLessThanOrSalaryGreaterThan(5000.0, 10000.0).forEach(employee -> log.info(employee.toString()));

        log.info("Employees hired between 2018-01-01 and 2021-01-01:");
        employeeRepository.findByHireDateBetween(LocalDate.of(2018, 1, 1), LocalDate.of(2021, 1, 1)).forEach(employee -> log.info(employee.toString()));

        log.info("Employees in departments 1 and 2:");
        employeeRepository.findByDepartmentIdIn(Arrays.asList(1, 2)).forEach(employee -> log.info(employee.toString()));

        log.info("Employees in department 1 with salary between 2900 and 3100:"); employeeRepository.findByDepartmentIdInAndSalaryBetween(Arrays.asList(30), 2900.0, 3100.0).forEach(employee -> log.info(employee.toString())); log.info("Employees with no manager:");
        employeeRepository.findByManagerIdIsNull().forEach(employee -> log.info(employee.toString()));

        log.info("Employees with a manager:");
        employeeRepository.findByManagerIdIsNotNull().forEach(employee -> log.info(employee.toString()));

        log.info("Employees with commission percentage not null, ordered by salary desc and commission percentage desc:");
        employeeRepository.findByCommissionPctNotNullOrderBySalaryDescCommissionPctDesc().forEach(employee -> log.info(employee.toString()));

        log.info("Employees with last name starting with 'Do':");
        employeeRepository.findByLastNameStartingWith("Do").forEach(employee -> log.info(employee.toString()));
        };
    }
}
```

<br>

---

<br>

# JOIN
`JOIN`은 데이터베이스 관리 시스템에서 두 개 이상의 테이블을 연결하여 데이터를 조회하는 데 사용되는 방법이다.

## JOIN 유형

### 내부 조인(INNER JOIN)
- 가장 일반적인 형태의 JOIN으로, 두 테이블의 교집합만을 결과로 반환한다.
- 두 테이블 간에 일치하는 데이터가 있는 경우에만 해당 데이터를 표시한다.

### 외부 조인(OUTER JOIN)
내부 JOIN과 달리, 한 테이블의 값이 다른 테이블에 일치하는 값이 없어도 해당 데이터를 포함하여 결과를 반환한다.
  - `LEFT OUTER JOIN`: 왼쪽 테이블의 모든 데이터와 오른쪽 테이블의 일치하는 데이터를 반환한다.
  - `RIGHT OUTER JOIN`: 오른쪽 테이블의 모든 데이터와 왼쪽 테이블의 일치하는 데이터를 반환한다.
  - `FULL OUTER JOIN`: 양쪽 테이블의 모든 데이터를 반환하며, 일치하는 데이터가 없는 경우 `NULL`로 채운다.

### 크로스 조인(CROSS JOIN)
- 두 테이블 간의 카테시안 곱을 반환하며, 첫 번째 테이블의 모든 행이 두 번째 테이블의 모든 행과 결합됨을 의미한다.
- 일반적으로 WHERE절이나 다른 형태의 필터링 없이 사용되며, 매우 큰 결과 집합을 생성할 수 있다.

### 자연 조인(NATURAL JOIN)
- 두 테이블 간에 이름이 같은 모든 컬럼에 대해 암묵적인 INNER JOIN을 수행한다.
- 명시적으로 조인 조건을 제시하지 않고, 두 테이블에서 이름이 같은 컬럼을 기준으로 JOIN을 수행한다.

<br>

## 쿼리 메소드 분석
- 쿼리 메소드
```java
List<Employee> findByDepartmentIdInAndSalaryBetween(
    List<Integer> departmentIds,
    Double minSalary,
    Double maxSalary
);
```

- sql문
```sql
select *
from employees e1_0 left join departments d1_0
on d1_0.department_id = e1_0.department_id
where d1_0.department_id in (?) and e1_0.salary between ? and ?
```

<br>

## EAGER vs LAZY

### LAZY Fetching(지연 로딩)
관계된 엔티티를 실제로 필요할 때까지 로드하지 않는다. 즉, 관련 엔티티에 접근할 때 데이터베이스에서 해당 데이터를 가져온다.
- `장점`: 초기 로딩 시 불필요한 데이터를 로드하지 않기 때문에 성능 최적화에 유리하고, 메모리 사용량을 줄일 수 있다. 
- `단점`: 관련 엔티티에 접근할 때마다 추가 쿼리가 발생할 수 있고, View 계층에서 데이터 접근 시 `LazyInitializationException`이 발생할 수 있습니다.

```sql
@ManyToOne(fetch = FetchType.LAZY)
@JoinColumn(name = "department_id")
private Department department;
```

### EAGER Fetching(즉시 로딩)
엔티티가 로드될 때 관계된 엔티티도 함께 로드한다. 즉, 부모 엔티티를 조회할 때 관련된 모든 자식 엔티티를 즉시 데이터베이스에서 가져온다.
- `장점`: 한 번의 쿼리로 모든 관련 데이터를 가져올 수 있기 때문에 지연 로딩에서 발생 할 수 있는 다수의 쿼리 실행을 피할 수 있으며, 지연 로딩 관련 예외(LazyInitializationException)가 발생하지 않는다.
- `단점`: 초기 로딩 시 불필요한 데이터를 모두 로드하기 때문에 메모리 사용량이 많아 질 수 있고, 불필요한 데이터를 가져오는 데 시간이 소요될 수 있다.

```sql
@ManyToOne(fetch = FetchType.EAGER)
@JoinColumn(name = "department_id")
private Department department;
```

### 기본 페치 타입
- `@ManyToOne`: 기본적으로 EAGER 로딩
- `@OneToOne`: 기본적으로 EAGER 로딩
- `@OneToMany`: 기본적으로 LAZY 로딩 
- `@ManyToMany`: 기본적으로 LAZY 로딩

<br>

---

<br>

# Service 레이어와 트랜잭션 처리

## Service 레이어
`Service 레이어`는 애플리케이션의 비즈니스 로직을 처리하는 계층이며, 컨트롤러와 데이터 접근 계층(DAO/Repository) 사이에서 중재 역할을 한다. 

1. `비즈니스 로직의 집중화`: 모든 비즈니스 로직은 `Service 레이어`에 위치하며, 이를 통해 코드의 재사용성을 높이고 유지보수를 용이하게 한다.
2. `트랜잭션 관리`: Service 레이어는 트랜잭션 경계를 정의하는 곳이다. 즉, 하나의 서비스 메소드가 하나의 트랜잭션으로 처리된다.
3. `컨트롤러와 분리`: 컨트롤러는 요청을 받고 응답을 반환하는 역할만 수행하며, 비즈니스 로직은 Service 레이어에서 처리되고, 이를 통해 역할을 명확히 분리할 수 있다.

<br>

## 트랜잭션 처리
`트랜잭션`은 데이터베이스의 일관성과 무결성을 유지하기 위해 여러 작업을 하나의 단위로 묶어주는 기능이다. 

Spring에서는 `@Transactional` 어노테이션을 사용하여 트랜잭션을 관리한다.
1. `트랜잭션 시작`: `@Transactional 어노테이션`이 붙은 메소드가 호출되면 Spring은 트랜잭션을 시작한다.
2. `트랜잭션 경계 설정`: 메소드 실행이 완료되면 트랜잭션이 종료되고, 이때 메소드가 성공적으로 완료되면 트랜잭션을 커밋하고, 예외가 발생하면 롤백한다.
3. `전파(propagation)`: 트랜잭션 전파는 메소드 간 트랜잭션 동작 방식을 정의하고, 기본값은 `REQUIRED`로, 현재 트랜잭션이 존재하면 이를 사용하고, 없으면 새로운 트랜잭션을 시작한다.
4. `격리 수준(isolation level)`: 트랜잭션 격리 수준은 동시에 실행되는 트랜잭션 간의 데이터 처리 방식을 정의하고, Spring에서는 다양한 격리 수준을 제공하며, 기본값은 `DEFAULT`다.

<br>

---

<br>

# OSiV 패턴
`OSiV(Open Session in View)` 패턴은 웹 애플리케이션에서 `Hibernate`같은 ORM 프레임워크를 사용할 때, 데이터베이스 세션을 뷰 렌더링까지 유지하는 패턴이다. 

## 작동 방식
1. `요청 수신`: 클라이언트의 요청을 서버가 수신한다.
2. `세션 시작`: 요청을 처리하는 동안 데이터베이스 세션이 열린다.
3. `서비스 및 리포지토리 호출`: 서비스 계층 및 리포지토리 계층에서 데이터베이스 작업을 수행한다.
4. `지연 로딩 허용`: 데이터베이스 세션이 열려있기 때문에 지연 로딩이 가능하다.
5. `뷰 렌더링`: 데이터베이스 세션이 열린 상태로 뷰를 렌더링한다.
6. `세션 종료`: 요청 처리가 끝난 후 데이터베이스 세션이 닫힌다.

<br>

## 주의점
1. `성능 문제`: 데이터베이스 세션이 뷰 렌더링까지 열려있으면, 요청 처리 시간이 길어 질 수 있고, 이는 데이터베이스 리소스 사용을 증가시켜 성능 문제를 야기할 수 있다.
2. `트랜잭션 경계 모호화`: 트랜잭션 경계가 명확하지 않을 수 있고, 비즈니스 로직과 뷰 렌더링이 동일한 트랜잭션 경계를 갖게 되어, 예기치 않은 데이터 변경이 발생할 수 있다.
3. `지연 로딩 남용`: 지연 로딩을 남용하게 될 경우, 데이터베이스 쿼리가 뷰 레이어에서 발생하여 성능 저하를 초래할 수 있다.
