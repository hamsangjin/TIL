# DDL(Data Definition Language)
`DDL`은 데이터베이스를 정의하는 언어이며, 데이터를 생성, 수정, 삭제하는 등의 데이터의 전체의 골격을 결정하는 역할을 하는 언어이고, 아래와 같은 동작을 한다.

- `CREATE TABLE`: 테이블 생성
- `ALTER TABLE`: 테이블 관련 변경
- `DROP TABLE`: 테이블 삭제
- `RENAME`: 이름 변경
- `TRUNCATE`: 테이블의 모든 데이터 삭제
- `COMMENT`: 테이블에 설명 추가

<br>

---

<br>

# 제약 조건

`제약 조건`이란 데이터의 무결성을 지키기 위해 데이터를 입력받을 때 실행되는 검사 규칙을 말하며, 이러한 제약 조건은 `CREATE 문`으로 테이블을 생성할 때나, `ALTER 문`으로 필드를 추가할 때도 설정할 수 있다.

- `NOT NULL`: NULL을 허용하지 않는다.
- `UNIQUE`: 해당 필드는 서로 다른 값을 가져야만 한다.
- `PRIMARY KEY`: 해당 필드가 NOT NULL과 UNIQUE 제약 조건의 특징을 모두 가진다. 
- `FOREIGN KEY`: 특정 테이블의 칼럼이 특정 테이블의 칼럼을 참조하게 한다. 
- `DEFAULT`: 해당 필드의 기본값을 설정합니다.
- `AUTO_INCREMENT`: 해당 필드의 값을 1부터 시작하여 새로운 레코드가 추가될 때마다 1씩 증가된 값을 자동으로 저장하고, `AUTO_INCREMENT` 키워드 다음에 `대입 연산자(=)`를 사용하여 시작값을 변경할 수 있다.
  - `Oracle`은 `sequence`객체를 이용해 자동으로 필드의 값을 증가시킬 수 있다.

- 예시 쿼리문
```sql
create table students(
    id int primary key auto_increment,
    name varchar(50) not null,
    email varchar(100) unique,
    reg_date date default(current_date),
    major_id int,
    FOREIGN KEY (major_id) REFERENCES majors(id)
    # 부모키가 삭제될 때 student의 major_id를 null로 바꿔줌.
    on delete set null
    # 부모키가 변경될 때 student의 major_id도 같이 변경해줌.
    on update set null,

    # 부모키가 삭제될 때 같이 삭제됨.
    # on delete cascade
    # 부모키가 변경될 때 같이 변경됨
    # on update cascade

    # email이 '%@%.%'와 같은 정규표현식에 해당하는지 확인해서 동작을 실행함
    check ( email like '%@%.%')
);
```

<br>

---

<br>

# 데이터베이스 설계
1. `요구조건 분석 단계`: 데이터 및 처리 요구 조건
2. `개념적 설계 단계 (E-R 다이어그램)`: DBMS 독립적 개념 스키마 설계, 트랜잭션 모델링
3. `논리적 설계 단계 (정규화)`: 목표 DBMS에 맞는 스키마 설계, 트랜잭션 인터페이스 설계
4. `물리적 설계 단계 (반정규화)`: 목표 DBMS에 맞는 물리적 구조 설계, 트랜잭션 세부 설계
5. `구현 단계`: 목표 DBMS DDL로 스키마 작성, 트랜잭션(응용프로그램) 작성

<br>

---

<br>

# Transaction
`Transaction`이란 DB에서 하나의 작업으로 처리되는 논리적 작업 단위를 말한다.

## 구성
- `DML(INSERT, UPDATE, DELETE)`의 집합
- `DDL`이나 `DCL`은 한 문장이 트랜잭션으로 처리됨

<br>

## 트랜잭션 시작과 종료
- `시작`: 명시적인 트랜잭션 시작 명령이 없으며, 첫 DML이 시작되면 트랜잭션이 시작한다.
- `명시적 종료`: `COMMIT`, `ROLLBACK`
- `묵시적종료`: DDL, DCL 등이 수행될때는 `automatic commit`되며, 시스템 오류가 발생하면 `automatic rollback`된다.
