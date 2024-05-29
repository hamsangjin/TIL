# 관계 매핑

## @ManyToMany
`@ManyToMany` 관계는 두 엔티티가 양방향으로 여러 요소와 관계를 맺을 수 있을 때 사용된다.

### 예제 코드

- 테이블 생성 SQL
```sql
-- 쿼리로 생성 안 해도 Main에서 생성됨
CREATE TABLE employees (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
CREATE TABLE projects (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255) NOT NULL
);
CREATE TABLE employees_projects (
    employee_id BIGINT NOT NULL,
    project_id BIGINT NOT NULL,
    PRIMARY KEY (employee_id, project_id),
    FOREIGN KEY (employee_id) REFERENCES employees(id),
    FOREIGN KEY (project_id) REFERENCES projects(id)
);
```

- 테이블 INSERT문
```sql
-- Employees 추가
INSERT INTO employees (name) VALUES ('carami Smith');
INSERT INTO employees (name) VALUES ('Diana Crain');
-- Projects 추가
INSERT INTO projects (title) VALUES ('New Website Development');
INSERT INTO projects (title) VALUES ('Marketing Research');
-- Employees_Projects 관계 설정
INSERT INTO employees_projects (employee_id, project_id) VALUES (1, 1);
INSERT INTO employees_projects (employee_id, project_id) VALUES (1, 2);
INSERT INTO employees_projects (employee_id, project_id) VALUES (2, 1);
```

- Entity 클래스 - `Employee.java`
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;
import java.util.*;

@Entity
@Table(name="employees")
@Getter@Setter
@NoArgsConstructor
public class Employee {
    @Id@GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @ManyToMany
    @JoinTable(name = "employees_projects",
            joinColumns = @JoinColumn(name = "employee_id"),
            inverseJoinColumns = @JoinColumn(name = "project_id")
    )
    private Set<Project> projects = new HashSet<>();

    public Employee(String name) {
        this.name = name;
    }
}
```

- Entity 클래스 - `Project.java`
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;
import java.util.*;

@Entity
@Table(name = "projects")
@Getter@Setter
@NoArgsConstructor
public class Project {
    @Id@GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @ManyToMany(mappedBy = "projects")
    private Set<Employee> employees = new HashSet<>();

    public Project(String name) {
        this.name = name;
    }
}
```

- Main 클래스
```java
package com.example.jpa;

import jakarta.persistence.EntityManager;
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class EmployeeExamMain {

    public static void main(String[] args) {
//        find();
//        create();
//        update();
        delete();
    }


    private static void find(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();

        em.getTransaction().begin();
        try{
            Project project = em.find(Project.class, 1L);
            log.info("프로젝트 이름: " + project.getName());

            for (Employee employee : project.getEmployees()) {
                log.info("사원 이름: " + employee.getName());
            }

            em.getTransaction().commit();
        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }

    private static void create(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();

        em.getTransaction().begin();
        try{
            Employee employee = new Employee("함상진");
            Project project = new Project("어마어마한 프로젝트");

            project.getEmployees().add(employee);
            employee.getProjects().add(project);

            em.persist(employee);
            em.persist(project);

            em.getTransaction().commit();

        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }

    private static void update(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();

        em.getTransaction().begin();
        try{
//            Employee employee = em.find(Employee.class, 2L);
//            employee.setName("진상함");

            // 1번 사원이 2번 프로젝트에서 빠지게 하고 싶다면?
            em.find(Employee.class, 1L).getProjects().remove(em.find(Project.class, 2L));


            em.getTransaction().commit();
        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }

    private static void delete(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();

        em.getTransaction().begin();
        try{
            Employee employee = em.find(Employee.class, 3L);
            em.remove(employee);

            em.getTransaction().commit();
        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }
}
```

<br>

## @OneToOne
- 테이블 생성 SQL
```sql
CREATE TABLE persons (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL
);
CREATE TABLE passports (
    id BIGINT AUTO_INCREMENT PRIMARY KEY,
    passport_number VARCHAR(255) NOT NULL,
    person_id BIGINT UNIQUE,
    FOREIGN KEY (person_id) REFERENCES persons(id)
);
```

- 테이블 INSERT문
```sql
-- Persons 추가
INSERT INTO persons (name) VALUES ('carami');
INSERT INTO persons (name) VALUES ('urstory');
-- Passports 추가
INSERT INTO passports (passport_number, person_id) VALUES ('A1234567', 1);
INSERT INTO passports (passport_number, person_id) VALUES ('B7654321', 2);
```

- Entity 클래스 - `Person.java`
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "persons")
@Getter@Setter
@NoArgsConstructor
public class Person {
    @Id@GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @OneToOne(mappedBy = "person", cascade = CascadeType.ALL, orphanRemoval = true)
    private Passport passport;

    public Person(String name) {
        this.name = name;
    }
}
```

- Entity 클래스 - `Passport.java`
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "passports")
@Getter@Setter
@NoArgsConstructor
public class Passport {
    @Id@GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // 컬럼명이 _가 포함되어 있지만 자바에선 카멜표기법으로 사용하므로 컬럼명을 명시해주고 변수명은 카멜 표기법으로 정의해준다.
    @Column(name = "passport_number", nullable = false)
    private String passportNumber;

    @OneToOne
    @JoinColumn(name = "person_id", unique = true)
    private Person person;

    public Passport(String passportNumber) {
        this.passportNumber = passportNumber;
    }
}
```

- Main 클래스
```java
package com.example.jpa;

import jakarta.persistence.EntityManager;
import lombok.extern.slf4j.Slf4j;

@Slf4j
public class PersonExamMain {
    public static void main(String[] args) {
//        find();
//        create();
//        update();
        delete();
    }

    private static void find(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            Passport passport = em.find(Passport.class, 1L);
            log.info("passport number: {}", passport.getPassportNumber());
            log.info("person name: {}", passport.getPerson().getName());

            Person person = em.find(Person.class, 2L);
            log.info("passport number: {}", person.getPassport().getPassportNumber());
            log.info("person name: {}", person.getName());

            em.getTransaction().commit();
        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }

    private static void create(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            Person person = new Person("함상진");
            Passport passport = new Passport("123456");


            passport.setPerson(person);
            person.setPassport(passport);

            // person 객체와 passport 객체가 서로 포함하고 있으므로 영속화를 하나만 해도 된다.
            em.persist(person);

            em.getTransaction().commit();
        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }

    private static void update(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            Person person = em.find(Person.class, 1L);
            person.setName("진상함");

            em.getTransaction().commit();
        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }

    private static void delete(){
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();
        try{
            // Person person = em.find(Person.class, 4L);
            // em.remove(person);  // 여권 정보도 같이 삭제

            // passport만 삭제하는 건 안 됨 -> passport가 person 객체를 가지고있기 때문에, person 객체를 삭제할 수 없으므로 삭제가 안 된다.
            Passport passport = em.find(Passport.class, 2L);
            // 따라서 person 객체를 null로 만들어주면 삭제가 가능하다.
            if(passport != null){
                passport.getPerson().setPassport(null);
            }
            em.remove(passport);


            em.getTransaction().commit();
        } catch (Exception e){
            if(em.getTransaction().isActive())  em.getTransaction().rollback();
        } finally {
            em.close();
        }
    }
}
```

<br>

## 상속 매핑 전략
JPA에서 `상속 매핑 전략`은 객체 지향 모델에서의 상속 구조를 어떻게 관계형 데이터베이스 스키마에 매핑할지를 정의한다.

### 단일 테이블 전략
`단일 테이블 전략`은 상속 계층의 모든 클래스를 하나의 테이블에 매핑하여, 모든 필드가 하나의 테이블에 포함되므로 다형적 쿼리의 성능이 빠르다. 

하지만, 테이블에는 상속 계층의 모든 속성에 대한 컬럼이 존재하므로, 특정 하위 클래스의 속성이 비어 있는 경우가 많아져 데이터의 중복이나 낭비가 발생할 수 있다.

### 단일 테이블 전략 예제 코드
- `Vehicle.java`
```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "type", discriminatorType = DiscriminatorType.STRING)
public abstract class Vehicle {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String manufacturer;
}

@Entity
@DiscriminatorValue("CAR")
public class Car extends Vehicle {
    private int seatCount;
}
@Entity
@DiscriminatorValue("TRUCK")
public class Truck extends Vehicle {
    private double payloadCapacity;
}
```

- `CarExamMain1.java`
```java
package com.example.jpa;

import jakarta.persistence.EntityManager;
import lombok.extern.slf4j.Slf4j;

import java.util.List;

@Slf4j
public class CarExamMain1 {
    public static void main(String[] args) {
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();

        // Car 2개, Truck 2개 INSERT
        Car car1 = new Car();
        car1.setManufacturer("Kia");
        car1.setSeatCount(5);
        em.persist(car1);

        Car car2 = new Car();
        car2.setManufacturer("HYUNDAI");
        car2.setSeatCount(4);
        em.persist(car2);

        Truck truck1 = new Truck();
        truck1.setManufacturer("SAMSUNG");
        truck1.setPayloadCapacity(20.0);
        em.persist(truck1);

        Truck truck2 = new Truck();
        truck2.setManufacturer("BMW");
        truck2.setPayloadCapacity(30.0);
        em.persist(truck2);

        // Vehicle 엔티티의 엔티티 객체들을 조회해서 리스트로 저장 -> JPQL 쿼리
        List<Vehicle> vehicles = em.createQuery("SELECT v FROM Vehicle v", Vehicle.class).getResultList();

        // 각각의 인스턴스에 따라 형변환 후 정보 출력
        for (Vehicle vehicle : vehicles) {
            if(vehicle instanceof Car) {
                Car car = (Car) vehicle;
                log.info("Car Info: {}, {}",  car.getManufacturer(), car.getSeatCount());
            } else if(vehicle instanceof Truck) {
                Truck truck = (Truck) vehicle;
                log.info("Truck Info: {}, {}", truck.getManufacturer(), truck.getPayloadCapacity());
            }
        }

        em.getTransaction().commit();
    }
}
```
<img width="946" alt="스크린샷 2024-05-29 14 57 13" src="https://github.com/hamsangjin/TIL/assets/103736614/4b596b5d-1f72-4140-9eef-026dec18477d">

### 조인 테이블 전략 
`조인 테이블 전략`은 각 클래스를 별도의 테이블로 매핑하고, 상속 관계에 있는 클래스 간에는 조인을 사용한다.

이 전략은 데이터의 정규화가 잘 되어 있어 데이터 중복이 없지만, 객체를 로드하거나 저장할 때 여러 조인이 필요해 성능이 저하될 수 있다.

### 조인 테이블 예제 코드
- `Vehicle2.java`
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Inheritance(strategy = InheritanceType.JOINED)       // 조인 테이블 전략: 각 클래스를 별도의 테이블로 매핑하고, 상속 관계에 있는 클래스 간에는 조인
@Getter@Setter
public abstract class Vehicle2 {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String manufacturer;
}

@Entity
@Getter@Setter
class Car2 extends Vehicle2 {
    private int seatCount;
}

@Entity
@Getter@Setter
class Truck2 extends Vehicle2 {
    private double payloadCapacity;
}
```

- `CarExamMain2.java`
단일 테이블 예제 코드와 클래스명을 제외하고는 동일하다.

![](https://github.com/hamsangjin/TIL/assets/103736614/422a5cfd-a9f6-4717-aaf5-6c69537861a5) | ![](https://github.com/hamsangjin/TIL/assets/103736614/a09e2cde-3016-4a58-9a93-f7490493d38c) | ![](https://github.com/hamsangjin/TIL/assets/103736614/d5728337-e4e7-482a-b05a-a16851af1461)
-- | -- | -- |


### 테이블 당 구체 클래스 전략
테이블 당 구체 클래스 전략은 각 구체 클래스를 자신의 테이블로 매핑한다. 

이 전략을 사용하면 데이터 중복이 발생할 수 있으며, 다형적 쿼리를 수행할 때 모든 관련 테이블을 조회해야 하므로 성능이 떨어질 수 있다.

### 테이블 당 구체 클래스 전략 예제 코드
- `Vehicle3.java`
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)    // 테이블 당 구체 클래스 전략: 테이블 당 구체 클래스 전략은 각 구체 클래스를 자신의 테이블로 매핑
@Getter@Setter
public abstract class Vehicle3 {
    @Id
    // GenerationType.TABLE: 각 구체 클래스마다 독립적인 테이블을 가지기 때문에, 테이블 기반 키 생성 전략은 각 테이블에 대해 독립적인 키 생성이 가능하도록 합니다.
    @GeneratedValue(strategy = GenerationType.TABLE)
    private Long id;
    private String manufacturer;
}

@Entity
@Getter@Setter
class Car3 extends Vehicle3 {
    private int seatCount;
}

@Entity
@Getter@Setter
class Truck3 extends Vehicle3 {
    private double payloadCapacity;
}
```

- `CarExamMain3.java`
단일 테이블 예제 코드와 클래스명을 제외하고는 동일하다.

![](https://github.com/hamsangjin/TIL/assets/103736614/145f82e1-4640-46ae-a24e-788c01d2de54) | ![](https://github.com/hamsangjin/TIL/assets/103736614/e54842ed-78fa-4f9b-9a3c-771a46706ab2)
-- | -- |

<br>

## 임베디드 타입 사용법
`임베디드 타입(Embedded Types)` 또는 `복합 값 타입`은 JPA에서 엔티티의 일부로 정의된, 재사용 가능한 도메인 모델의 일부를 표현하는데 사용된다.

이 방식을 통해 엔티티 내에서 여러 속성을 논리적으로 그룹화하고, 중복 없이 여러 엔티티 간에 공통 구조를 공유 할 수 있습니다.

### @Embeddable 클래스 정의 - Address.java
```java
package com.example.jpa;

import jakarta.persistence.Embeddable;
import lombok.*;

@Embeddable
@Getter@Setter
@NoArgsConstructor
@AllArgsConstructor
public class Address {
    private String street;
    private String city;
    private String state;
    private String zipCode;
    private String country;
}
```

### @Embedded 사용(엔티티) - Company.java
```java
package com.example.jpa;

import jakarta.persistence.*;
import lombok.*;

@Entity
@Table(name = "companies")
@Getter@Setter
@NoArgsConstructor
public class Company {
    @Id@GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Embedded
    private Address address;

    public Company(String name, Address address) {
        this.name = name;
        this.address = address;
    }
}
```

### Main 클래스
```java
package com.example.jpa;

import jakarta.persistence.EntityManager;
import java.util.List;

public class CompanyExamMain {
    public static void main(String[] args) {
        EntityManager em = JPAUtil.getEntityManagerFactory().createEntityManager();
        em.getTransaction().begin();

        try{
            Address address1 = new Address("123 Main St", "Springfield", "IL", "62701", "USA");
            Company company1 = new Company("Company A", address1);
            em.persist(company1);

            Address address2 = new Address("456 Elm St", "Metropolis", "NY", "10001", "USA");
            Company company2 = new Company("Company B", address2);
            em.persist(company2);
            em.getTransaction().commit();

            List<Company> companies = em.createQuery("SELECT c FROM Company c", Company.class).getResultList();
            for (Company company : companies) {
                System.out.println("Company: " + company.getName() +
                        ", Address: " + company.getAddress().getStreet() +
                        ", " + company.getAddress().getCity() +
                        ", " + company.getAddress().getState() +
                        ", " + company.getAddress().getZipCode() +
                        ", " + company.getAddress().getCountry());
            }
        } finally {
            em.close();
        }
    }
}
```
<img width="1074" alt="스크린샷 2024-05-29 15 30 22" src="https://github.com/hamsangjin/TIL/assets/103736614/738c1f19-cfd8-4e01-aa22-efd27068a58d">
