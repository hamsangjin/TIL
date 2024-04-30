# 데이터베이스
`데이터베이스`: 여러 응용 시스템들의 통합된 정보들을 저장해 운영할 수 있는 공용 데이터의 집합을 말한다.

<br>

## 데이터베이스의 특성
1. `실시간 접근성`: 사용자의 요구를 즉시 처리할 수 있다.
2. `계속적인 변화`: 정확한 값을 유지하려고 삽입, 삭제, 수정 작업 등을 이용해 데이터를 지속적으로 갱신할 수 있다.
3. `동시 공유성`: 사용자마다 서로 다른 목적으로 사용하므로, 동시에 여러 사람이 동일한 데이터에 접근하고 이용할 수 있다.
4. `내용 참조`: 저장한 데이터 레코드의 위치나 주소가 아닌 사용자가 요구하는 데이터의 내용 즉, 데이터 값에 따라 참조할 수 있어야 한다.

<br>

---

<br>

# MySQL 설치
강의 시간엔 `Docker-Desktop`을 이용해 `MySQL`을 설치했지만, 나는 로컬에 이미 설치되어 있는 `MySQL`을 그냥 사용했다.

<br>

## IntelliJ에서 Database 이용

### 1. 데이터베이스 추가
<img width="1728" alt="스크린샷 2024-04-30 17 25 11" src="https://github.com/hamsangjin/TIL/assets/103736614/f9bff5fe-2c92-4989-97ca-b2b52dcbd9cc">

- `Driver`가 설치가 안 되어있다면 설치를 해준다.

<br>

### 2. 스키마 추가
- DB 우클릭 > 새로 작성 > 스키마 선택
<img width="1728" alt="스크린샷 2024-04-30 17 31 27" src="https://github.com/hamsangjin/TIL/assets/103736614/63b222ed-5847-463a-9cbb-22d4359c33fd">

- 미리보기에 스키마 내용 작성
<img width="786" alt="스크린샷 2024-04-30 17 33 26" src="https://github.com/hamsangjin/TIL/assets/103736614/3f94352f-0121-4df4-a752-367e3b07bcad">

이와 같은 방법으로 `hr` 스키마도 적용해준다.

- 적용 확인
<img width="1178" alt="스크린샷 2024-04-30 17 35 21" src="https://github.com/hamsangjin/TIL/assets/103736614/ee0f1c86-585a-4afc-8a7d-dcdd977cb518">

<br>

---

<br>

# 사용자 관리 쿼리문

## 쿼리문 특징
- MySQL은 문장의 끝을 라인으로 구분하는 것이 아니라 `;`으로 구분하기 때문에 여러 줄에 거쳐 문장을 쓰는 것도 가능하다.
- MySQL은 찾은 전체 row를 출력하고, 마지막에 전체 row 수와 쿼리실행에 걸린 시간을 표시한다.
- 키워드는 대소문자 구별이 없고, 아래 쿼리들은 모두 같다.
```sql
SELECT VERSION(), CURRENT_DATE;
select version(), current_date;
SeLeCt vErSiOn(), current_DATE;
```

<br>

## 1. 사용자 생성
```sql
CREATE USER 'id'@'%' IDENTIFIED BY 'password';         -- 외부 접속용
CREATE USER 'id'@'localhost' IDENTIFIED BY 'password'; -- 로컬 접속용
```

<br>

## 2. 권한 부여
사용자가 생성된 후, 해당 사용자에게 특정 DB에 대한 권한을 부여할 수 있다.
```sql
GRANT ALL PRIVILEGES ON db.* TO 'id'@'%';
GRANT ALL PRIVILEGES ON db.* TO 'id'@'localhost';
```

<br>

## 3. 권한 적용
모든 권한 변경 사항을 적용하기 위해서는 아래 명령을 실행해야 한다.
```sql
FLUSH PRIVILEGES;
```

<br>

## 4. 권한 확인
해당 user의 권한을 확인할 수 있다.
```sql
SHOW GRANTS FOR id@%;
SHOW GRANTS FOR id@localhost;
```

<br>

## 5. 사용자 존재 확인
해당 사용자가 시스템에 존재하는지 확인할 수 있다.
```sql
SELECT user, host FROM mysql.user WHERE user = 'id';    -- 특정 id 존재 확인
SELECT host, user FROM user;                            -- id 전체 확인
```

<br>

## 6. 사용자 삭제
만약 id 사용자가 존재한다면, 아래 명령을 실행해야 한다.
```sql
DROP USER 'id'@'%';
DROP USER 'id'@'localhost'; -- 만약 localhost에도 사용자가 있다면 삭제
```

<br>

## 7. 권한 삭제

### 특정 권한만 제거
```sql
REVOKE ALL PRIVILEGES ON db.* FROM 'id'@'%';  -- 유저가 가지고 있는 db에 대한 모든 권한을 삭제

REVOKE INSERT, UPDATE ON db.* FROM 'id'@'%';  -- db에 대한 INSERT, UPDATE의 권한만 제거하는 경우
```

### 모든 권한을 제거하고 사용자 삭제
```sql
REVOKE ALL PRIVILEGES ON *.* FROM 'id'@'%';  -- 모든 데이터베이스에 대한 권한을 제거
REVOKE GRANT OPTION ON *.* FROM 'id'@'%';    -- `GRANT OPTION` 권한도 제거

DROP USER 'id'@'%';          -- 사용자 삭제
FLUSH PRIVILEGES;            -- 권한 테이블 새로 고침
```

<br>

---

<br>

# 테이블, 열, 행, 필드

## 테이블(Tabel)
`테이블`이란 빠른 참조를 위해 적당한 형태로 자료를 모아 놓은 것으로, **관계 데이터베이스 모델**에서 자료의 구조를 2차원의 표로 나타낸 것을 말한다. 

테이블을 사용하면, 행과 열의 형태로 관리되며 키를 지정함으로써 원하는 자료를 빠르고 쉽게 찾아낼 수도 있다.

데이터베이스 테이블은 `행(ROw)`와 `열(Column)` 또는 `필드`로 구성되어 있다고 간주할 수 있다.

<br>

## 열(Column)
**관계형 데이터베이스 테이블**에서 특정한 단순 자료형의 일련의 데이터값과 테이블에서의 각 컬럼을 말하며, 어떻게 구성되어야 할지에 대한 구조를 제공한다.

관계형 데이터베이스 용어에서 `속성(attribute)`이 컬럼과 같은 의미로 사용되고, `필드(field)`가 종종 컬럼의 대용으로 동일한 의미로 사용되지만, 필드와 필드값은 한 컬럼이나 한 컬럼 사이의 사이의 교차로 존재하는 단일 항목을 특정할 때 언급하는 것이다

<br>

## 행(Row)
관계형 데이터베이스에서 `레코드(record)` 또는 `튜플(tuple)`로 불리기도 하며, 어떤 테이블에서 단일 구조 데이터 항목을 가리킨다.

각 테이블의 행은 일련의 관련 자료를 나타내며, 테이블에서 모든 행은 동일한 구조를 가지고 있다.

<br>

## 필드(Field)
`필드`, `항목`, 어떠한 의미를 지니는 정보의 한 조각으로, 데이터베이스 시스템에서 처리의 최소 단위가 되는 것.
