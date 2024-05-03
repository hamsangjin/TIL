# JOIN
`JOIN`은 하나 이상의 테이블로부터 연관된 데이터를 검색해 오는 방법을 말한다.

## 테이블 관계
<img width="471" alt="스크린샷 2024-05-03 09 31 17" src="https://github.com/hamsangjin/TIL/assets/103736614/a623e4b2-2894-4b41-bbf8-99f13ba76e87">

위 그림에서 파란색으로 적힌 컬럼은 `기본키(Primary Key)`라고 하며, NULL값과 데이터 중복을 허용하지 않는다.

<br>

## 카테시안 조인(Cartesian Join)
Join 에 대한 조건이 생략되거나 잘못 기술되어 한 테이블에 있는 모든 행들이 다른 테이블에 있는 모든 행들과 Join이 되어서 얻어진 경우를 `카테시안 곱`한다

### 문법
```sql
select * from employees, departments;
```
카테시안 곱을 얻지 않기 위해서 반드시 `WHERE 절`을 써야하며, `(JOIN하는 테이블의 수 - 1) 개`의 `JOIN 조건`이 필요하다.

<br>

## Simple Join

### 문법
```sql
SELECT t1.col1, t1.col2, t2.col1 ... FROM Table1 t1, Table2 t2
WHERE t1.col3 = t2.col3
```

FROM 절에 필요로 하는 테이블을 모두 적는다.
- 컬럼 이름의 모호성을 피하기 위해 `Table` 이름에 `Alias` 사용한다. (테이블 이름으로 직접 지칭 가능)
- 적절한 `Join 조건`을 `Where 절`에 부여하며, 일반적으로 `테이블 개수 -1 개`의 `조인 조건`이 필요하다.
- 일반적으로 `PK`와 `FK`간의 `= 조건`이 붙는 경우가 많음

<br>

## JOIN 종류
- `Cross Join(Cartesian Product)`: 모든 가능한 쌍이 나타남
- `Inner Join`: Join 조건을 만족하는 튜플만 나타남
- `Outer Join`: Join 조건을 만족하지 않는 튜플 (짝이 없는 튜플)도 null과 함께 나타남
- `Theta Join`: 조건(theta)에 의한 조인
- `Equi-Join`: Theta Join & 조건이 Equal (=)
- `Natural Join`: Equi-join & 동일한 Column명 합쳐짐.
- `Self Join`: 자기 자신과 조인

<br>

## EQUIJOIN
컬럼에 있는 값이 정확하게 일치하는 경우에 `=` 연산자를 사용하여 `JOIN`하는 방법

```sql
SELECT 테이블명.컬럼명, 테이블명.컬럼명. ... FROM 테이블1, 테이블2
WHERE 테이블1.컬럼1 = 테이블2.컬럼2
```
- `테이블명.컬럼명`:  검색해올 데이터가 어디에서 오는지 테이블과 컬럼을 밝혀둔다.
- `테이블1.칼럼1 = 테이블2.컬럼2`: 두 테이블간에 논리적으로 값을 연결시키는 칼럼 간의 조건을 기술한다.

### EQUIJOIN 문법
- 컬럼에 있는 값들이 정확히 일치하는 경우에 `=` 연산자를 사용해서 조인
- 일반적으로 `PK-FK` 관계에 의하여 JOIN이 성립
- `WHERE절` 혹은 `ON절`을 이용
- 액세스 효율을 향상시키고 좀 더 명확히 하기 위해 칼럼 이름앞에 `테이블 이름`을 밝힌다.
- `같은 이름의 칼럼`이 조인대상 테이블에 존재하면 반드시 `컬럼 이름앞에 테이블이름`을 밝혀주어야 한다.
- `JOIN을 위한 테이블`이 `N개`라고 하면 `JOIN을 위한 최소한의 = 조건`은 `N-1개`이다.

<br>

## 조건(theta) Join
```sql
SELECT e.ename, e.sal, s.grade
FROM emp e, salgrade s
WHERE  e.sal BETWEEN s.losal AND s.hisal
```
- 임의의 조건을 Join 조건으로 사용가능
- `Non-Equi Join`이라고도 함

<br>

## Natural Join
```sql
# departments와 locations 테이블에 deptno이라는 공통적인 칼럼이 존재
select * from departments natural join locations;
```
- 두 테이블에 `공통 칼럼`이 있는 경우 별다른 조인 조건없이 공통 칼럼처럼 `묵시적으로 조인`이 되는 유형
- ANSI / ISO SQL1999를 따르는 `ANSI JOIN` 문법

<br>

## INNER JOIN - JOIN~ USING
- `Natural Join`로 조인하고자 하는 두 테이블에 같은 이름이 칼럼이 많아 사용하지 못할 때, 특정한 칼럼으로만 조인하고 싶다면 `USING절`을 사용해서 기술한다.
- ANSI / ISO SQL1999를 따르는 ANSI JOIN 문법
```sql
# 아래처럼 Natural Join을 사용하려고 하는데
# 두 테이블에 같은 이름이 칼럼이 많아 사용하지 못하는 경우
select * from employees natural join departments;

# 아래처럼 using절 사용
select * from employees join departments using(department_id);
```

<br>

## INNER JOIN - JOIN ~ ON
- 공통된 이름의 칼럼이 없는 경우 가장 보편적으로 사용할 수 있는 유형
- WHERE 절에 일반조건만 쓸 수 있게하고 조인 조건은 ON에 두어 보다 의미를 명확히 하고 알아보기도 쉽다.
- ANSI / ISO SQL1999를 따르는 ANSI JOIN 문법. ON 부분을 where절에서 작성가능하다.
```sql
select * from employees e join departments d on(e.department_id = d.department_id);
```

<br>

## Outer Join
정의
- Join 조건을 만족하지 않는 (짝이 없는 ) 튜플의 경우 Null을 포함하여 결과를 생성
- 모든 행이 결과 테이블에 참여

종류
- `Left Outer Join`: 왼쪽의 모든 튜플은 결과 테이블에 나타남
```sql
select e.ename, d.dname
from emp e left outer join dept d using(deptno);
```
- `Right Outer Join`: 오른쪽의 모든 튜플은 결과 테이블에 나타남
```sql
select e.ename, d.dname
from emp e right outer join dept d using(deptno);
```
- `Full Outer Join`: 양쪽 모두 결과 테이블에 참여
```sql
select e.ename, d.dname
from emp e left outer join dept d using(deptno)
UNION
select e.ename, d.dname
from emp e right outer join dept d using(deptno);
```

표현 방법
- NULL이 올 수 있는 쪽 조건에 (+)를 붙인다.(Oracle일 경우)

### 문법
```sql
SELECT table1.column, table2.column FROM table1
[CROSS JOIN table2] |
[NATURAL JOIN table2] |
[JOIN table2 USING (column_name)] |
[JOIN table2
ON(table1.column_name = table2.column_name)] | [LEFT|RIGHT|FULL OUTER JOIN table2
ON (table1.column_name = table2.column_name)];
```

`left outer`는 left, `right outer`는 right로 사용가능
```sql
select *
from employees e LEFT OUTER JOIN departments d
ON (e.department_id = d.department_id);
```

<br>

## SELF JOIN
같은 테이블이 두 개 있다고 생각하고 접근하면 편하다

```sql
select e.employee_id as '사원id', e.first_name '사원이름', m.employee_id as '상사id', m.first_name as '상사 이름'
from employees e join employees m
on (e.manager_id = m.employee_id);
```
상사가 없는 사람도 출력하려면?

### SELF LEFT OUTER JOIN
```sql
select e.employee_id as '사원id' , e.first_name '사원이름', m.employee_id as '상사id', m.first_name as '상사 이름'
from employees e left join employees m
on (e.manager_id = m.employee_id);
```

<br>

---

<br>

# SubQuery
`SubQuery`란 하나의 SQL 질의문 속에 다른 SQL 질의문이 포함되어 있는 형태를 말한다.

- `예시 문제`: SCOTT의 급여보다 높은 급여를 받는 사람의 이름을 출력하시오.
```sql
SELECT ename
FROM emp
WHERE sal >( SELECT sal
             FROM emp
             WHERE ename = 'SCOTT' );
```

<br>

## Single-Row Subquery
`Single-Row Subquery`란 Subquery의 결과가 한 ROW인 경우를 의미하며, `Single-Row`에 대한 연산`(= , > , >=, < , <=, <>)`을 사용해야 한다. 

- 예시 문제
```sql
# 1. emp테이블에서 이름으로 정렬했을 때 첫번째 나오는 이름의 이름, 급여, 부서번호를 출력하시오.
SELECT ename, sal, deptno
FROM emp
WHERE ename = (SELECT MIN(ename) FROM emp);

# 2. 사원의 평균 급여보다 작은 급여를 받는 사원의 이름과 급여를 출력하시오.
SELECT ename, sal
FROM emp
WHERE sal < (SELECT AVG(sal)FROM emp);

# 3. 부서이름이 SALES인 부서의 사원 이름과 부서 번호를 출력하시오.
SELECT  ename, deptno
FROM emp
WHERE deptno =  (SELECT  deptno
                    FROM dept
                    WHERE dname = 'SALES');
```

<br>

## Multi-Row Query
`Multi-Row Query`란 Subquery의 결과가 둘 이상의 Row인 경우를 의미하며, `Multi-Row에 대한 연산(ANY, ALL, IN, EXIST...)`을 사용해야 한다. 

결과가 둘 이상인 row에 `Single-Row`에 대한 연산(=)을 적용하면 오류가 발생한다.
```sql
# 오류 발생 - `Single-Row`에 대한 연산(=)
SELECT ename, sal, deptno FROM emp
WHERE ename = (SELECT MIN(ename)
                FROM  emp GROUP BY deptno);

# 정상 출력 - `Multi-Row`에 대한 연산(IN)
SELECT ename, sal, deptno FROM emp
WHERE ename IN (SELECT MIN(ename)
                FROM  emp GROUP BY deptno);
```

### Multi-Row에 대한 연산
- **`Any`**
  - 다수의 비교값 중 한개라도 만족하면 `true`이다.
  - `IN`과 다른점은 비교 연산자를 사용한다는 점이다.
  ```sql
  SELECT ename, sal, deptno
  FROM emp
  WHERE ename = ANY (SELECT MIN(ename)
                     FROM  emp GROUP BY deptno);
  ```

  - 아래 쿼리의 결과는 950보다 큰 값은 모두 출력하게 된다.
  - 아래의 쿼리는 `sal > 950`과 같은 결과고, `Any`는 `Subquery`와 함께 사용할때 의미가 있다.
  ```sql
  SELECT * FROM emp WHERE sal >ANY(950, 3000, 1250)
  ```

- **`ALL`**
  - 전체 값을 비교하여 모두 만족해야만 `true`이다.
  - 아래의 쿼리는 모두를 만족할 수는 없기 때문에 결과가 없다. 
  - `Oracle`은 오류가 발생하지 않지만, `MySQL`은 `Subquery`에서만 사용가능하다.
  ```sql
  SELECT * FROM emp WHERE sal = ALL(950, 3000, 1250);
  ```
  ```sql
  SELECT * FROM emp
  WHERE sal < ALL(select e.sal
                  from emp e
                  where e.deptno in (30, 10));
  ```

<br>
  
---

<br>

# Set Operator
두 질의의 결과를 가지고 있다면 집합 연산을 할 수 있다.

## 1. 연습용 테이블 만들기
```sql
create table A (
        name int
);
create table B (
name int
);

insert into A values ('1');
insert into A values ('2');
insert into A values ('3');

insert into B values ('2');
insert into B values ('3');
insert into B values ('4');
```
원래 PK가 없는 테이블을 만들면 안 되지만 연습용으로 생성해보자.

<br>

## 2. UNION(합집합)
```sql
select * from A union select * from B;
```
<img width="706" alt="스크린샷 2024-05-03 20 17 52" src="https://github.com/hamsangjin/TIL/assets/103736614/750ccc79-7be2-4950-9d4d-73887afb48d9">

<br>

## 3. UNION ALL(중복을 포함한 합집합)
```sql
select * from A union all select * from B;
```
<img width="719" alt="스크린샷 2024-05-03 20 17 58" src="https://github.com/hamsangjin/TIL/assets/103736614/1e122c6e-03df-4b20-ac6c-cc8b796224c4">

<br>

## 4. INTERSECT(교집합)
```sql
# Oracle(MySQL은 지원되지 않음)
select * from A intersect select * from B;

# Mysql
select A.name from A, B where A.name = B.name;
```
<img width="719" alt="스크린샷 2024-05-03 20 18 23" src="https://github.com/hamsangjin/TIL/assets/103736614/89bef2b6-5e01-4a25-9c39-914172c95074">

<br>

## 5. MINUS(차집합)
```sql
# Oracle(MySQL은 지원되지 않음)
select * from A minus select * from B;

# Mysql
select A.name from A where A.name not in (select B.name from B);
```
<img width="694" alt="스크린샷 2024-05-03 20 19 37" src="https://github.com/hamsangjin/TIL/assets/103736614/4ff511f9-aace-45ba-92d7-8b4ce13a075c">

## 6. 테이블 삭제
```sql
drop table A;
drop table B;
```
이제 다 사용했으니 테이블을 삭제해주자

