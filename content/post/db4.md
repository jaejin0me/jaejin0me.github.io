---
title: "SELECT"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["database"]
categories: ["database"]
author: "Jaejin Jang"
---

1.부서번호가 30인 직원의 모든 정보 출력
SELECT * FROM HR_EMPLOYEES WHERE DEPARTMENT_ID=30

2.성이 KING인 직원의 모든 정보 출력
SELECT * FROM HR_EMPLOYEES WHERE LAST_NAME='KING'

3.입사일이 00년 2월 7일 이후 입사한 직원들 정보를 출력하라.
SELECT * FROM HR_EMPLOYEES WHERE HIRE_DATE>='2000-02-07'

4.이름이 J로 시작하는 직원들 정보 출력
SELECT * FROM HR_EMPLOYEES WHERE LAST_NAME LIKE 'J%'

5.이름이 J보다 큰(알파벳상 뒤에 위치) 직원들 정보 출력
SELECT * FROM HR_EMPLOYEES WHERE LAST_NAME>'J'

6.이름이 B와 Q사이로 시작하는 직원들의 정보를 출력하라.
-- SELECT * FROM HR_EMPLOYEES WHERE LAST_NAME>'B' AND LAST_NAME<'Q'
SELECT * FROM HR_EMPLOYEES WHERE LAST_NAME BETWEEN  'B' AND 'Q'

7.이름의 2번째 문자가 B인 직원 출력
SELECT * FROM HR_EMPLOYEES WHERE LAST_NAME LIKE '_B%'

8.커미션이 널이 아닌 직원의 이름,성,커티션 출력
SELECT LAST_NAME, FIRST_NAME, COMMISSION_PCT FROM HR_EMPLOYEES WHERE COMMISSION_PCT IS NOT NULL

9.부서번호가 90번이고 월급이 5000이상인 직원들의 이름, 부서번호, 월급 출력
SELECT LAST_NAME, FIRST_NAME, DEPARTMENT_ID,SALARY FROM HR_EMPLOYEES WHERE SALARY>=5000 AND DEPARTMENT_ID=90

10.부서번호가 80번이고 월급이 6000이상인 직원들의 이름, 부서번호, 월급을 이름 내림차순으로
SELECT LAST_NAME, FIRST_NAME, DEPARTMENT_ID,SALARY FROM HR_EMPLOYEES WHERE SALARY>=6000 AND DEPARTMENT_ID=80 ORDER BY LAST_NAME DESC

11. 입사 년도가 99년 1월이 아닌 직원 출력
 SELECT * FROM HR_EMPLOYEES WHERE HIRE_DATE NOT LIKE '1999-01%'

12.이름이 J로 시작하고 N로 끝나는 직원 출력
 SELECT * FROM HR_EMPLOYEES WHERE FIRST_NAME LIKE 'J%N'

13.이름이 5글자 이면서 J로 시작하고 N로 끝나는 직원 출력
 SELECT * FROM HR_EMPLOYEES WHERE FIRST_NAME LIKE 'J___N'

14.emp에서 이름, 급여, 커미션 금액, 총액(급여+커미션금액)을 구하여 총액이 많은 순서대로 출력하라.
select first_name, salary, salary*commission_pct, (salary + salary*commission_pct) total from hr_employees where commission_pct is not null

15.80번 부서의 모든사람들에게 급여의 13%를 보너스로 지불하기로 했다. 이름, 급여, 보너스 금액, 부서번호를 출력하라
select first_name, salary, salary*0.13, department_id from hr_employees where department_id=80

16.80번 부서의 연봉을 계산하여 이름, 부서번호, 급여, 연봉을 출력하라. 단 연말에 급여의 150%를 보너스를 지급한다.
select first_name, department_id, salary, salary*12+salary*1.5 total from hr_employees where department_id=80

17.부서번호가 80인 부서의 시간당 임금을 계산하여 이름, 급여, 시간당 임금(소수점이하 1자리에서 반올림)을 출력하라. 1달의 근무일수는 12일이고, 1일당 근무시간은 5시간이다.
select first_name, salary, round(salary/(12*5),1) sph from hr_employees where department_id=80

18.급여가 1500부터 3000 사이의 사람은 급여의 15%를 회비로 지불하기로 했다. 이름, 급여, 회비(2째자리반올림) 출력하라.
select first_name, salary, round(salary*0.15) h from hr_employees where salary between 1500 and 3000

19.급여가 2000 이상인 모든 사람은 급여가 15%를 경조비로 내기로 했다. 이름, 급여 ,경조비를 출력하라
select first_name, salary, round(salary*0.15) g from hr_employees where salary>=2000

20.입사일부터 지금까지의 날짜수를 구하라. 부서번호, 이름, 입사일, 현재일, 근무일수, 근무년수, 근무월수(30일 기준), 근무주수를 구하라.
