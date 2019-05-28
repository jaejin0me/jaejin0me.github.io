---
title: "Join"
date: 2018-02-05T01:37:56+09:00
draft: false
tags: ["database"]
categories: ["database"]
author: "Jaejin Jang"
---

# 1. Join

* Join을 통해서 2개 이상의 테이블을 연결할 수 있다.
* Join은 기본적으로 기본키나 외부키의 값의 연관에 의해 성립되지만, 논리적인 값들의 연관만으로도 성립할 수 있다.
* Join의 조건은 where절에 기술한다.
* Join의 종류는 다음과 같다.

| Join 방법          | 의미                                                         |
| :-----------------:| :-----------------------------------------------------------------|
| equi join          | 두 개의 테이블 간의 칼럼값들이 정확하게 일치하는 경우에 사용 |
| non- equi join     | 두 개의 테이블 간의 칼럼값들이 정확하게 일치하지 않는 경우에 사용 |
| outer join         | Join의 조건을 만족하지 않는 데이터들도 보고자 하는 경우 |
| self join          | 두 개의 테이블이 아니라 같은 테이블 행들을 Join하는 경우 |

# 2. Cross Join(Cartesian Product)

* 테이블들 간의 Join 조건을 생략하거나 조건을 잘못 설정했을 경우, 첫 번째 테이블의 모든 행들에 대해서 두번째 테이블의 모든 행들이 Join되어 조회되는 데이터의 형태를 말함.
* 카티션 곱이 발생하면 조회되는 데이터의 수가 기하급수적으로 증가하기 때문에 DB나 네트워크에 부담이 된다.

# 2.1 예

```
SELECT D.DEPARTMENT_ID, D.DEPARTMENT_NAME, E.FIRST_NAME, E.LAST_NAME FROM HR_DEPARTMENTS D, HR_EMPLOYEES E 
```

# 3. Equi join(Inner Join,등가 조인) : '=' 연사자 이용

* 두 테이블의 일치하는 칼럽을 연결해서 Join하는 것을 등가조인이라고 함.
* 두 테이블간의 칼럼 값들이 일치하는 경우에 사용하며, 테이블별로 alias를 사용해 구분해주는 것이 좋다.
* Join을 하는 테이블이 N개인 경우, 최소 N-1개의 Join조건이 where절에 와야 한다.

## 3.1 예제

```
//사원테이블로 부서 테이블에서 부서아이디가 일치하는 경우, 부서아이디, 부서이름, 직원 이름 조회하기
SELECT D.DEPARTMENT_ID, D.DEPARTMENT_NAME, E.FIRST_NAME, E.LAST_NAME FROM HR_DEPARTMENTS D, HR_EMPLOYEES E WHERE D.DEPARTMENT_ID = E.DEPARTMENT_ID
```

# 4. Non-Equi Join(비등가조인)

* 두 개의 테이블간의 칼럼값들이 정확하게 일치하는 않는 경우에 사용(범위를 이용해 등급을 나눌때 용이).
* = 이 아닌 다른 연산자 들을 사용한다.

# 5. Outer Join(Left/Right Join)

* 조건을 만족하지 않는 데이터들도 보고싶을 때 사용한다
* equi join을 사용하면 칼럼값이 일치하는 데이터만 볼 수 있지만, outer join을 사용하면 다른 데이터 들도 볼 수 있음
* (+) 연산자를 쓰거나, 보고자하는 테이블이 왼쪽이면 Left, 오른쪽이며 Right를 명시에 사용한다.

## 5.1 예제

```
//직원 테이블과 부서테이블에서 부서아이디가 일치하는 경우 부서아이디 별로 묶어서 개수를 보여주고 부서아이디로 오름차순 정렬, 일치하는 부서가 없는 경우도 보여주기
SELECT D.DEPARTMENT_ID, COUNT(*) FROM HR_EMPLOYEES E RIGHT JOIN HR_DEPARTMENTS D ON E.DEPARTMENT_ID = D.DEPARTMENT_ID GROUP BY E.DEPARTMENT_ID ORDER BY DEPARTMENT_ID ASC
```

# 6. Self Join

* 하나의 테이블에서 두 개의 테이블을 Join하는 것처럼 사용하는 Join
* alias명을 지정해서 구별해 준다

## 6.1 예제

```
//각 사원의 매니저가 실제로 직업이 매니저인 경우 사원아이디, 이름, 매니저아이디, 매니저의 사원아이디, 매니저 이름, 매니저 직업아이디를 출력하라
SELECT E1.EMPLOYEE_ID, E1.FIRST_NAME, E1.MANAGER_ID, E2.EMPLOYEE_ID, E2.FIRST_NAME, E2.JOB_ID  FROM HR_EMPLOYEES E1, HR_EMPLOYEES E2 WHERE E1.MANAGER_ID = E2.EMPLOYEE_ID AND E2.JOB_ID LIKE '%MAN'
```
