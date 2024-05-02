# SELECT 문 사용 (기본 검색)

1. `employees` 테이블에서 직원의 이름(first_name), 성(last_name)을 조회하세요.
```sql
select first_name, last_name from employees;
```

2. `departments` 테이블에서 모든 부서의 이름(department_name)과 위치 ID(location_id)를 조회하세요.
```sql
select department_name, location_id from departments;
```

3. `jobs` 테이블에서 직업 ID(job_id)와 직업 타이틀(job_title)을 조회하세요.
```sql
select job_id, job_title from jobs;
```

4. `locations` 테이블에서 각 위치의 주소(street_address)와 우편번호(postal_code)를 조회하세요.
```sql
select street_address, postal_code from locations;
```

5. `countries` 테이블에서 국가 ID(country_id)와 국가 이름(country_name)을 조회하세요.
```sql
select country_id, country_name from countries;
```

6. `regions` 테이블에서 지역 ID(region_id)와 지역 이름(region_name)을 조회하세요.
```sql
select region_id, region_name from regions;
```

7. `employees` 테이블에서 모든 직원의 직업 ID(job_id)를 조회하세요.
```sql
select job_id from employees;
```

8. `departments` 테이블에서 부서 ID(department_id)별로 부서 이름(department_name)을 조회하세요.
```sql
select department_id, department_name 
from departments 
group by department_id;
```

9. `job_history` 테이블에서 직원 ID(employee_id)와 부서 ID(department_id)를 조회하세요.
```sql
select employee_id, department_id from job_history;
```

10. `employees` 테이블에서 직원 ID(employee_id), 이름(first_name)과 성(last_name)을 조회하세요.
```sql
select employee_id, first_name, last_name from employees;
```

<br>

---

<br>


# WHERE 구문 사용 (조건 검색)
1. `employees` 테이블에서 급여(salary)가 10000 이상인 직원의 이름과 급여를 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, salary
from employees
where salary >= 10000;
```
2. `departments` 테이블에서 위치 ID(location_id)가 1700인 부서의 이름을 조회하세요.
```sql
select department_name, location_id
from departments
where location_id = 1700;
```

3. `employees` 테이블에서 직업 ID(job_id)가 'IT_PROG'인 직원들의 전체 정보를 조회하세요.
```sql
select *
from employees
where job_id = 'IT_PROG';
```

4. `employees` 테이블에서 부서 ID(department_id)가 90인 직원들의 이름과 급여를 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, salary, department_id
from employees
where department_id = 90;
```
5. `jobs` 테이블에서 최소 급여(min_salary)가 5000 이상인 직업의 타이틀을 조회하세요.
```sql
select job_title, min_salary
from jobs
where min_salary >= 5000;
```

6. `employees` 테이블에서 성(last_name)이 'King'인 직원의 전체 정보를 조회하세요.
```sql
select *
from employees
where last_name = 'King';
```

7. `locations` 테이블에서 국가 ID(country_id)가 'US'인 위치의 상세 정보를 조회하세요.
```sql
select *
from locations
where country_id = 'US';
```

8. `job_history` 테이블에서 시작 날짜(start_date)가 '2001-01-01' 이전인 기록을 조회하세요.
```sql
select *
from job_history
where start_date <= DATE_FORMAT('2001-01-01', '%Y-%m-%d');
```

9. `employees` 테이블에서 성(last_name)에 'a'가 포함되는 직원들의 이름과 이메일을 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, email
from employees
where last_name like '%a%';
```

10. `departments` 테이블에서 부서 이름(department_name)이 'Sales'인 부서의 모든 직원 정보를 조회하세요.
```sql
select e.*
from employees e, departments d
where e.department_id = d.department_id and department_name = 'Sales';
```

<br>

---

<br>

# 날짜형 함수 사용 (날짜 데이터 처리)
1. `employees` 테이블에서 각 직원의 입사 연도(year)를 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, Year(hire_date) as hire_year
from employees;
```

2. `job_history` 테이블에서 각 기록의 근무 기간을 월(months) 단위로 계산하세요.
```sql
select TIMESTAMPDIFF(MONTH, start_date, end_date) AS '근무기간(월)'
from job_history;
```

3. `employees` 테이블에서 오늘로부터 입사한 지 25년이 된 직원들의 이름과 입사 날짜를 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, hire_date
FROM employees
WHERE DATEDIFF(DATE_FORMAT(CURDATE(), '%Y-%m-%d'), hire_date) = 365*25
ORDER BY hire_date;
```

4. `employees` 테이블에서 이번 달에 생일(hire_date)인 직원들의 이름과 생일을 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, DATE_FORMAT(hire_date, "%m-%d") as 생일
from employees
where MONTH(CURDATE()) = MONTH(hire_date);
```

5. `employees` 테이블에서 입사 날짜가 최근 25년 이내인 직원들의 이름과 입사 날짜를 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, hire_date
from employees
where DATEDIFF(DATE_FORMAT(CURDATE(), '%Y-%m-%d'), hire_date) <= 365*25
order by hire_date;
```

6. `job_history` 테이블에서 직원이 특정 부서에 근무한 기간을 일(days) 단위로 조회하세요.
```sql
select employee_id, TIMESTAMPDIFF(DAY, start_date, end_date) as '근무기간(일)'
from job_history;
```

7. `employees` 테이블에서 가장 오래전에 입사한 직원의 이름과 입사 날짜를 조회하세요.
```sql
select concat(first_name, ' ', last_name) as employee_name, hire_date
from employees
where hire_date = (select MIN(hire_date) from employees);
```

8. `employees` 테이블에서 각 직원의 마지막 생일(hire_date)로부터 경과한 일수를 조회하세요.
```sql
select first_name,
  last_name, 
  hire_date,
  case
    when CONCAT(YEAR(NOW()), '-', MONTH(hire_date), '-', DAY(hire_date)) <= NOW()
      then DATEDIFF(NOW(), CONCAT(YEAR(NOW()), '-', MONTH(hire_date), '-', DAY(hire_date)))
    else DATEDIFF(NOW(), CONCAT(YEAR(NOW()) - 1, '-', MONTH(hire_date), '-', DAY(hire_date)))
  end as days_since_last_birthday
from employees;
```

9. `job_history` 테이블에서 1993년대에 시작된 모든 근무 기록을 조회하세요.
```sql
select *
from job_history
where YEAR(start_date) = 1993;
```

10. `employees` 테이블에서 각 직원의 입사 날짜를 요일로 표시하세요.
```sql
select DATE_FORMAT(hire_date, '%W') as 요일
from employees;
```

```sql
select dayname(hire_date) as 요일
from employees;
```

<br>

---

<br>

# 숫자형 함수 사용 (수치 데이터 처리)

1. `employees` 테이블에서 각 직원의 급여에 10% 인상된 금액을 계산하여 조회하세요.
```sql
select employee_id, round(salary*1.1) as salary
from employees;
```

2. `jobs` 테이블에서 각 직업의 최소 급여와 최대 급여의 차이를 계산하세요.
```sql
select (max_salary - min_salary) as '최소 최대 차이'
from jobs;
```

3. `employees` 테이블에서 각 직원의 급여를 원화(KRW)로 환산하여 조회하세요 (환율 가정: 1 USD = 1200 KRW).
```sql
select concat(first_name, ' ', last_name) as 이름, salary*1200 as '급여(원화)'
from employees;
```

4. `employees` 테이블에서 전체 직원의 평균 급여보다 높은 급여를 받는 직원들의 이름과 급여를 조회하세요.
```sql
select concat(first_name, ' ', last_name) as 이름, salary
from employees
where salary > (select avg(salary) from employees);
```
5. `employees` 테이블에서 각 직원의 급여에서 평균 급여를 뺀 금액을 계산하여 조회하세요.
```sql
select concat(first_name, ' ', last_name) as 이름, salary - (select avg(salary) from employees) as '급여-평균급여'
from employees;
```

6. `employees` 테이블에서 급여의 표준 편차를 계산하여 조회하세요.
```sql
select stddev(salary) as '급여 표준 편차'
from employees;
```

7. `employees` 테이블에서 각 직원의 연봉을 계산하고, 연봉이 가장 높은 순으로 정렬하여 조회하세요.
```sql
select concat(first_name, ' ', last_name) as 이름, salary*12 as 연봉
from employees
order by salary desc;
```

8. `job_history` 테이블에서 각 기록에 대해 직업 변경 횟수를 계산하세요.
```sql
select employee_id, count(job_id)
from job_history
group by employee_id;
```
    
9. `employees` 테이블에서 직원들의 급여를 오름차순으로 정렬하여 조회하세요.
```sql
select *
from employees
order by salary;
```

<br>

---

<br>

# 문자 함수를 이용한 SQL 연습 문제
1. 직원의 성(last_name)을 대문자로 변환하여 조회하기:
```sql
select UPPER(last_name)
from employees;
```

2. 직원의 이름(first_name)의 첫 글자를 추출하기:
```sql
select substr(first_name, 1, 1)
from employees;
```

3. 직원의 성(last_name)에서 'a'가 몇 번 나오는지 세기:
```sql
select length(last_name) - length(REPLACE(last_name, 'a', ''))
from employees;
```

4. 직원의 이메일에서 도메인 부분만 추출하기 (@ 이후 문자열):
```sql
select RIGHT(email, locate('I', email)-1)
from employees;
```

5. 직원의 전체 이름을 성과 이름으로 구분하여 조회하기:
```sql
select *
from employees;
```

6. 직원의 이름(first_name)에서 세 번째 문자부터 세 글자 가져오기
```sql
select substr(first_name, 4, 3)
from employees;
```

7. 모든 직원의 성(last_name)을 쉼표와 공백 후 이름(first_name)으로 표시하기:
```sql
select concat(last_name, ', ', first_name) as 이름
from employees;
```

8. 직원의 이름(first_name)의 길이를 구하여 조회하기:
```sql
select length(first_name)
from employees;
```

9. 직원의 성(last_name)이 'King'인 직원 찾기 (대소문자 구분 없이):
```sql
select *
from employees
where last_name = 'King';
```

10. 직원의 성(last_name) 중 'M'으로 시작하는 사람들의 수 구하기:
```sql
select *
from employees
where last_name like 'M%';
```
