# Select

## 기본 문법
- Table에서 Data를 가져오기 위해서 SELECT 구문을 사용한다.
```sql
SELECT(DISTINCT) 칼럼명(ALIAS) FROM 테이블명
WHERE 조건식
GROUP BY 칼럼명
HAVING 조건식
ORDER BY 칼럼이나 표현식 (ASC 또는 DESC)
```

- `SELECT`: 검색하고자 하는 데이터(칼럼)를 나열 한다
- `DISTINCT`: 중복행을 제거
- `ALIAS`: 나타날 컬럼에 대한 다른 이름 부여
- `FROM`: 선택한 컬럼이 있는 테이블을 명시한다.

<br>

## 전체 컬럼이나 특정 컬럼 Select
```sql
select * from employees;                      # employees의 전체 컬럼 

select first_name, last_name from employees;  # employees의 first_name, last_name 컬럼
```

<br>

## 컬럼에 대한 별칭(ALIAS) 부여
컬럼에 대한 `별칭(ALIAS)`을 부여해서 나타내는 칼럼의 `HEADING`을 변경할 수 있다.
```sql
SELECT first_name AS 이름, hire_date AS 입사일 FROM employees;
```

<br>

## 컬럼의 합성
문자열 결합함수 `concat`을 사용하면 된다.
```sql
SELECT concat( first_name, ' ', last_name) AS 이름, hire_date AS 입사일
FROM employees;
```

<br>

## 중복행의 제거
중복되는 행이 출력되는 경우, `DISTINCT` 키워드로 중복행을 제거한다.
```sql
select manager_id from employees;           # 중복 미제거

select distinct manager_id from employees;  # 중복 제거
```

<br>

## 결과 정렬
- 정렬 기본 문법: `ORDER BY` 절을 사용하며, 정렬방법은 `오름차순 정렬 ASC(생략가능)` , `내림차순 정렬 DESC`을 선택한다.
```sql
SELECT(DISTINCT) 칼럼명(ALIAS)
FROM 테이블명
ORDER BY 칼럼이나 표현식 (ASC 또는 DESC);
```

- 예시 쿼리문
```sql
select first_name, last_name, hire_date from employees order by hire_date asc;
    
select first_name, last_name, hire_date from employees order by hire_date, first_name;     # hire_date로 정렬하고  hire_date가 같다면 first_name으로 정렬    

select first_name, last_name, hire_date from employees order by 3, 1;                      # 컬럼 순서 번호로도 정렬 가능
```

<br>

## WHERE와 함께 조회하기
- 문법
```sql
SELECT(DISTINCT) 칼럼명(ALIAS)
FROM 테이블명
WHERE 조건식
ORDER BY 칼럼이나 표현식 (ASC 또는 DESC)
```
- `조건식`: 컬럼이름이나 표현식의 상수, 연산자로 구성

<br>

## 특정 Row에 대한 Select
```sql
# last_name 이 'King'인 데이터를 조회한다.
select * from employees where last_name = 'King';

# 고용일이 1989-06-17 이후에 들어온 사원 정보 조회
select * from employees where hire_date > '1989-06-17';

# 논리 연산자 AND를 사용
select * from employees where last_name = 'KING' and first_name = 'Steven';

논리 연산자 OR를 사용
select * from employees where last_name = 'KING' or first_name = 'Steven';
```

<br>

## Null 다루기
NULL인지 아닌지 확인 하기 위해 `=`, `<` 또는 `<>` 와 같은 산술비교 연산자를 사용할 수 없고, 대신에 `IS NULL` 그리고 `IS NOT NULL` 연산자를 사용해야 한다.
```sql
select * from employees where commission_pct is null;

select * from employees where commission_pct is not null;
```

<br>

## 패턴 매칭
- MySQL 기본적으로 제공하는 것
  - 표준 SQL pattern matching
  - 정규표현식 pattern matching

- SQL Pattern matching
  - `LIKE` or `NOT LIKE` 비교 연산자를 사용해서 패턴매칭을 한다.
  - 기본적으로 영문자인 경우 **대소문자 구별을 안한다.**

- 특수문자
  - 와일드 카드를 사용하여 특정 문자를 포함한 값에 대한 조건을 처리
  - `%`는 0에서부터 여러 개의 문자열을 나타냄
  - `_`는 단 하나의 문자를 나타내는 와일드 카드

<br>

## 패턴 매칭 예제
```sql
# first_name이 b로 시작 하는 직원 정보 조회
select * from employees where first_name like 'b%';

# first_name이 t로 끝나는 직원 정보 조회
select * from employees where first_name like '%t';

# first_name이 w가 포함된 직원 정보 조회
select * from employees where first_name like '%w%';

# lastname이 5자인 직원 정보 조회 (5개)
select * from employees where first_name like '_____';

# last_name이 e로 시작하고 5자인 직원 정보 조회
select * from employees where first_name like 'e____';
```

<br>

## IN
```sql
# 부서 id가 90, 100인 사원의 데이터를 출력
select * from employees where department_id in ( 90, 100);
```

<br>

## 문자형 함수
```sql
# UCASE, UPPER
SELECT UPPER('SEoul'), UCASE('seOUL');

# LCASE, LOWER
SELECT LOWER('SEoul'), LCASE('seOUL');

# substring
SELECT SUBSTRING('Happy Day',3,2);

# LPAD, RPAD
SELECT LPAD('hi',5,'?'), LPAD('joe',7,'*');

# 사원 테이블에서 급여를 출력하되 급여는 10자리로 부족한 자리수는 *로 표시한다.
SELECT employee_id, LPAD( cast(salary as char), 10, '*')
FROM employees;

# TRIM, LTRIM, RTRIM
SELECT LTRIM(' hello '), RTRIM(' hello ');
SELECT TRIM(' hi '),TRIM(BOTH 'x' FROM 'xxxhixxx');
```

<br>

## 문자형 함수의 효율적인 사용
```sql
# 1989년에 입사한 직원 데이터를 출력하시오.
SELECT concat( first_name, ' ', last_name ) AS 이름, hire_date AS 입사일
FROM employees
WHERE substring( hire_date, 1, 4) = '1989'

# 위와같이 좌변을 변형시키는 것은 성능 하락
SELECT concat( first_name, ' ', last_name ) AS 이름, hire_date AS 입사일
FROM employees
WHERE hire_date like '1989%'
```

<br>

## 숫자형 함수
```sql
# ABS(x): x의 절대값
SELECT ABS(2), ABS(-2);

# MOD(n,m) or %: n을 m으로 나눈 나머지 값을 출력
SELECT MOD(234,10), 253 % 7, MOD(29,9);

# CEILING(x): x보다 작지 않은 가장 작은 정수를 반환
SELECT CEILING(1.23), CEILING(-1.23);

# ROUND(x): x에 가장 근접한 정수를 반환
SELECT ROUND(-1.23), ROUND(-1.58), ROUND(1.58);

# ROUND(x,d): x값 중에서 소수점 d자리에 가장 근접 한 수로 반환
SELECT ROUND(1.298,1),ROUND(1.298,0);

# POW(x,y) POWER(x,y): x의 y 제곱 승을 반환
SELECT POW(2,2),POWER(2,-2);

# SIGN(x): x가 음수면 -1을, x가 0이면 0을, x가 양수면 1을 출력
SELECT SIGN(-32), SIGN(0), SIGN(234);

# GREATEST(x,y,...): 가장 큰 값을 반환
SELECT GREATEST(2,0),GREATEST(4.0,3.0,5.0),GREATEST("B","A","C");

# LEAST(x,y,...): 가장 작은 값을 반환
SELECT LEAST(2,0),LEAST(34.0,3.0,5.0),LEAST("b","A","C");
```

<br>

## 날짜형 함수
```sql
# CURDATE(),CURRENT_DATE: 오늘 날짜를 YYYY- MM-DD나 YYYYMMDD 형식으로 반환
SELECT CURDATE(),CURRENT_DATE;

# CURTIME(), CURRENT_TIME: 현재 시각을 HH:MM:SS 나 HHMMSS 형식으로 반환
SELECT CURTIME(),CURRENT_TIME;

# NOW() SYSDATE(), CURRENT_TIMESTAMP: 오늘 현 시각을 YYYY-MM-DD HH:MM:SS나 YYYYMMDDHHMMSS 형식으로 반환
SELECT NOW(),SYSDATE(),CURRENT_TIMESTAMP;

# DATE_FORMAT(date,format) : 입력된 date를 format 형식으로 반환
SELECT DATE_FORMAT(CURDATE(),'%W %M %Y');
SELECT DATE_FORMAT(CURDATE(),'%Y.%m.%d');

# PERIOD_DIFF(p1,p2) : YYMM이나 YYYYMM으로 표기 되는 p1과 p2의 차이 개월을 반환
# 오늘까지 근무한 근무개월 수와 직원 이름을 출력하시오.
SELECT concat(first_name, ' ', last_name) AS name,
       PERIOD_DIFF(DATE_FORMAT(CURDATE(), '%Y%m'),
                   DATE_FORMAT(hire_date, '%Y%m'))
FROM employees

# DATE_ADD(date,INTERVAL expr type)
# DATE_SUB(date,INTERVAL expr type)
# ADDDATE(date,INTERVAL expr type)
# SUBDATE(date,INTERVAL expr type): 날짜 date에 type 형식으로 지정한 expr값을 더하거나 뺀다.
# DATE_ADD()와 ADDDATE()는 같은 동작이고, DATE_SUB()와 SUBDATE()는 같은 의미이다.
```

<br>

## 형변환 함수

CAST 함수는 type을 변경(지정)하는데 유용하다. 
- CAST 함수의 사용법
```sql
CAST(expression AS type)
# 또는
CONVERT(expression,type)
```

MySQL 타입 
- BINARY
- CHAR
- DATE
- DATETIME
- SIGNED {INTEGER}
- TIME
- UNSIGNED {INTEGER}

<br>

## 그룹 함수
그룹함수를 사용할 땐 그룹함수와 함께 그룹핑이 참여하지 않은 컬럼이 나올 수 없다.

- `COUNT(expr)`: non-NULL인 row의 숫자를 반환
- `COUNT(DISTINCT expr,[expr...])`: non-NULL인 중복되지 않은 row의 숫자를 반환
- `COUNT(*)`: row의 숫자를 반환
- `AVG(expr)`: expr의 평균값을 반환
- `MIN(expr)`: expr의 최소값을 반환
- `MAX(expr)`: expr의 최대값을 반환
- `SUM(expr)`: expr의 합계를 반환
- `GROUP_CONCAT(expr)`: 그룹에서 concatenated한 문자를 반환
- `VARIANCE(expr)`: 분산
- `STDDEV(expr)`: expr의 표준 편차를 반환

<br>

## GROUP BY절
`GROUP BY`절에 기술된 칼럼이 반드시 `SELECT`절 뒤에 올 필요는 없으나 `SELECT`문 결과의 의미를 명확히 하기 위해 기술하는 것이 좋다. 
```sql
# 부서별 연봉 평균을 출력하시오.
select department_id , avg(salary) from employees group by department_id;
```

`SELECT`절에 그룹함수가 오면 나머지 컬럼은 `GROUP BY`절에 기술 되어야 한다. 

즉, `SELECT`절에 `그룹함수`가 오거나, `GROUP BY`절 이하에 기술된 컬럼이 오면 나머지 칼럼은 `SELECT`절 뒤에 기술할 수 없다.
```sql
# Error
select department_id , avg(salary) from employees
select department_id , avg(salary) from employees where avg(salary) > 5000 group by department_id having;
select department_id , avg(salary) from employees group by department_id having avg(salary) > 5000;
```
