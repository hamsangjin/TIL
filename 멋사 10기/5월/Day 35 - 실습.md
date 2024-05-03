# 연습 문제 - part 1

## 1. **각 직원의 이름과 그들이 속한 부서 이름을 조회하세요.**
```sql
select e.last_name, e.first_name, d.department_name
from employees e, departments d
where e.department_id = d.department_id;

select e.last_name, e.first_name, d.department_name
from employees e join departments d using(department_id);
```

## 2. **모든 직원의 이름과 그들의 직무 타이틀을 조회하세요.**
```sql
select e.last_name, e.first_name, j.job_title
from employees e, jobs j
where e.job_id = j.job_id;

select e.last_name, e.first_name, j.job_title
from employees e join jobs j using(job_id);
```

## 3. **모든 직원의 이름, 부서 이름 및 그들이 근무하는 국가 이름을 조회하세요.**
```sql
select e.last_name, e.first_name, d.department_name, c.country_name
from employees e, departments d, locations l, countries c
where e.department_id = d.department_id and d.location_id = l.location_id and l.country_id = c.country_id;

select e.last_name, e.first_name, d.department_name, c.country_name
from employees e
join departments d using(department_id)
join locations l using(location_id)
join countries c using(country_id);
```

## 4. **1999년 이후에 입사한 직원들과 그들의 직무 타이틀을 조회하세요.**
```sql
select e.last_name, e.first_name, j.job_title, e.hire_date
from employees e, jobs j
where YEAR(e.hire_date) >= 1999 and e.job_id = j.job_id;

select e.last_name, e.first_name, j.job_title, e.hire_date
from employees e join jobs j using (job_id)
where YEAR(e.hire_date) >= 1999;
```

## 5. **모든 직원의 이름과 그들이 근무한 지역 이름을 조회하세요.**
```sql
select e.last_name, e.first_name, d.department_name, r.region_name
from employees e, departments d, locations l, countries c, regions r
where e.department_id = d.department_id and d.location_id = l.location_id and l.country_id = c.country_id and c.region_id = r.region_id;

select e.last_name, e.first_name, d.department_name, r.region_name
from employees e
join departments d using (department_id)
join locations l using(location_id)
join countries c using (country_id)
join regions r using (region_id);
```

## 6. **각 부서에서 근무하는 직원 수를 부서 이름과 함께 조회하세요.**
```sql
select count(e.employee_id) as 직원수, d.department_name
from employees e, departments d
where e.department_id = d.department_id
group by d.department_name;

select count(e.employee_id) as 직원수, d.department_name
from employees e join departments d using (department_id)
group by d.department_name;
```

## 7. **모든 지역(region)과 해당 지역에 위치한 국가들을 조회하세요.**
```sql
select r.region_name, c.country_name
from countries c, regions r
where c.region_id = r.region_id
order by r.region_name asc;

select r.region_name '지역', c.country_name '해당 지역에 위치한 국가'
from regions r join countries c using(region_id)
order by r.region_name asc;
```

## 8. **각 부서의 위치 정보와 해당 위치의 도시 이름을 조회하세요.**
```sql
select d.department_name, l.*
from departments d, locations l
where d.location_id = d.location_id;

select l.street_address '부서의 위치 정보', l.city '해당 위치 도시 이름', d.department_name '부서명'
from locations l join departments d using(location_id)
order by l.city asc;
```

## 9. **각 부서에서 근무하는 모든 직원의 이름과 부서 이름, 그리고 직무를 조회하세요.**
```sql
select e.last_name, e.first_name, d.department_name, j.job_title
from employees e
join departments d on e.department_id = d.department_id
join jobs j on e.job_id = j.job_id;

select e.last_name, e.first_name, d.department_name, j.job_title
from employees e
join departments d using (department_id)
join jobs j using (job_id);
```

## 10. **각 직원의 이름과 그들의 입사 날짜, 그리고 그들이 근무한 모든 부서의 이름을 조회하세요.**
```sql
select e.first_name, e.last_name, e.hire_date, d.department_name
from employees e, departments d, job_history j
where e.employee_id = j.employee_id and j.department_id = d.department_id;

select e.first_name, e.last_name, e.hire_date, d.department_name
from job_history j
join departments d using (department_id)
join employees e using (employee_id);

select concat(e.first_name, ' ', e.last_name) as name,
       e.hire_date as hire_date,
       d.department_name as department_name
from job_history j
         join employees e on e.employee_id = j.employee_id
         join departments d on e.department_id = d.department_id;
```

<br>

---

<br>

# 연습 문제 - part 2

## 문제 1: 각 부서의 평균 급여를 조회하세요.
```sql
select d.department_name, round(avg(e.salary)) as '평균 급여'
from employees e, departments d
where e.department_id = d.department_id
group by d.department_name;

select d.department_name, round(avg(e.salary)) as '평균 급여'
from employees e
         join departments d using (department_id)
group by d.department_name;
```

## 문제 2: 최고 급여를 받는 직원의 이름과 급여를 조회하세요.
```sql
select e.last_name, e.first_name, e.salary
from employees e
where salary = (select max(salary) from employees);

select e.last_name, e.first_name, e.salary
from employees e
order by e.salary desc limit 1;
```

## 문제 3: 각 부서에서 근무하는 직원 수를 조회하세요.
```sql
select d.department_name, count(e.employee_id) as '직원 수'
from employees e, departments d
where e.department_id = d.department_id
group by d.department_name;

select d.department_name, count(e.employee_id) as '직원 수'
from employees e join departments d using(department_id)
group by d.department_name;
```

## 문제 4: 각 직원의 이름, 직무 타이틀, 부서 이름을 조회하세요.
```sql
select e.first_name, e.last_name, j.job_title, d.department_name
from employees e, jobs j, departments d
where e.department_id = d.department_id and e.job_id = j.job_id;

select e.first_name, e.last_name, j.job_title, d.department_name
from employees e
join jobs j using (job_id)
join departments d using (department_id);
```

## 문제 5: 각 직원의 입사일로부터 경과한 일수를 조회하세요.
```sql
select e.last_name, e.first_name, e.hire_date, DATEDIFF(CURRENT_DATE, e.hire_date)
from employees e;

select last_name, first_name, hire_date, DATEDIFF(CURRENT_DATE, hire_date)
from employees;
```

## 문제 6: 모든 직원의 이름과 그들의 매니저 이름을 조회하세요.
```sql
select e1.first_name, e1.last_name, e2.first_name, e2.last_name
from employees e1 left join employees e2
on e1.manager_id = e2.employee_id;
```

## 문제 7: 국가 코드가 'US'인 모든 위치의 도시 이름을 조회하세요.
```sql
select l.country_id, l.city
from locations l, countries c
where l.country_id = c.country_id and c.country_id = 'US';

select l.country_id, l.city
from locations l join countries c using (country_id)
where c.country_id = 'US';
```

## 문제 8: 2005년 이후에 입사한 모든 직원의 이름과 입사 날짜를 조회하세요.
```sql
select e.last_name, e.first_name, e.hire_date
from employees e
where YEAR(e.hire_date) >= 1999;
```

## 문제 9: 각 직원의 전체 이름과 이메일을 조회하세요.
```sql
select e.first_name, e.last_name, e.email
from employees e;
```

## 문제 10: 직업이 'IT_PROG'인 직원들의 이름과 급여를 조회하세요.
```sql
select e.first_name, e.last_name, e.salary
from employees e
where e.job_id = 'IT_PROG';
```
