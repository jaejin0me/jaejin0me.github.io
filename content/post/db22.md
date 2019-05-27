---
title: "책뿌수기 - SQL 레벨업 3"
date: 2019-01-20T22:20:56+09:00
draft: false
tags: ["sql 튜닝"]
categories: ["sql 튜닝"]
author: "Jaejin Jang"
---


# 3. SQL의 조건 분기

## ch 8. UNION을 사용한 쓸데없이 긴 표현

-   UNION을 사용한 조건 분기는 좋지 않다.
-   UNION은 내부적으로 여러개의 SELECT 구문을 실행하는 실행계획으로 해석된다 (= 테이블에 접근하는 횟수(I/O)가 늘어난다)

### 1) UNION을 사용한 조건 분기와 관련된 간단한 예제

-   UNION을 사용한 조건 분기

```
SELECT item_name, year, price_tax_ex AS price
		FROM Items
	WHERE year <= 2001
UNION ALL
SELECT item_name, year, price_tax_in AS price
		FROM Items
	WHERE year >= 2002
```

-   단점 : 1. 길다, 2. 테이블에 2회 접근한다.

### 2) WHERE 구에서 조건 분기를 하는 사람을 초보자

```
SELECT item_name, year
	CASE WHEN year <= 2001 TEHN price_tax_ex
		WHEN year >= 2002 THEN price_tax_in END AS price
FROM Items
```

### 3) SELECT 구를 사용한 조건 분기의 실행 계획

-   테이블 1회 접근으로 끝난다
-   구문 => 식, UNION => CASE

## ch 9. SELECT 구를 사용한 조건 분기의 실행 계획

### 1) 집계 대상으로 조건 분기

-   UNION을 사용한 방법

```
SELECT prefecture, SUM(pop_men) AS pop_men, SUM(pop_wom) AS pop_wom
	FROM(SELECT prefecture, pop AS pop_men null AS pop_wom)
		FROM Population
	WHERE sex = '1' # 남성
	UNION
	SELECT prefecture, null AS pop_men, pop AS pop_wom
		FROM Population
	WHERE sex = '2') TMP # 여성
GROUP BY prefecture
```
-   풀스캔이 2회 수행된다.
    
-   집계의 조건 분기도 CASE 식을 사용

```
    SELECT prefecture,
        	SUM(CASE WHEN sex = '1' THEN pop ELSE 0 END) AS pop_men,
     	SUM(CASE WHEN sex = '2' THEN pop ELSE 0 END) AS pop_wom
     FROM Population
     GROUP BY prefecture;
```

   -   풀스캔이 1회로 감소한다.

### 2) 집약 결과로 조건 분기

-   소속된 팀이 1개라면 해당 직원은 팀의 이름을 그대로 출력한다.
-   2개라면 ‘2개를 겸무’로 출력
-   3개라면 ‘3개를 겸무’로 출력
-   UNION을 사용한 조건 분기
```
SELECT emp_name,
		MAX(team) as team
	FROM Employees
	GROUP BY emp_name
	HAVING COUNT(*) = 1
UNION
SELECT emp_name,
		'2개를 겸무' as team
	FROM Employees
	GROUP BY emp_name
	HAVING COUNT(*) = 2
UNION
SELECT emp_name,
		'3개 이상을 겸무' as team
	FROM Employees
	GROUP BY emp_name
	HAVING COUNT(*) >= 3
```
-   조건 분기가 레코드가 아닌 집합에 적용되기 때문에 WHERE가 아니라 HAVING에 쓰인다
-   3번의 테이블 접근이 필요하다.
-   CASE 식을 사용한 조건 분기

```
SELECT emp_name,
		CASE WHEN COUNT(*) = 1 THEN MAX(team)
			 WHEN COUNT(*) = 2 THEN '2개를 겸무'
			 WHEN COUNT(*) >= 3 THEN '3개 이상을 겸무'
		EEND AS team
	FROM Employees
GROUP BY emp_name
```
-   1번의 테이블 접근과 HASH 연산이 이뤄진다.

## ch 10. 그래도 UNION이 필요한 경우

### 1) UNINON을 사용할 수 밖에 없는 경우

-   SELECT 하는 테이블이 다른 경우(FROM 구에서 테이블을 결합하고 CASE 식을 사용할 수 있으나 느려짐)

### 2) UNION을 사용하는 것이 성능적으로 더 좋은 경우

-   UNION을 사용할 때 인덱스 사용이 가능한 경우

```
SELECT key, name,
		date_1, flg_1,
		date_2, flg_2,
		date_3, flg_3
	FROM ThreeElements
WHERE date_1='2013-11-01'
	AND flg_1='T'
UNION
SELECT key, name,
		date_1, flg_1,
		date_2, flg_2,
		date_3, flg_3
	FROM ThreeElements
WHERE date_2='2013-11-01'
	AND flg_2='T'
UNION
SELECT key, name,
		date_1, flg_1,
		date_2, flg_2,
		date_3, flg_3
	FROM ThreeElements
WHERE date_3='2013-11-01'
	AND flg_3='T'
```
-   OR을 사용하는 방법

```
SELECT key, name
		date_1, flg_1,
		date_2, flg_2,
		date_3, flg_3
	FROM ThreeElements
	WHERE (date_1 = '2013-11-01' AND flg_1 = 'T')
	OR (date_2 = '2013-11-01' AND flg_2 = 'T')
	OR (date_3 = '2013-11-01' AND flg_3 = 'T')
```
-   date와 flg에 인덱스가 된 경우 UNION이 더 빠를 수 있다

## ch 11. 절차 지향형과 선언현

### 1) 구문 기반과 식 기반

-   사고의 전환이 필요, 구문 -> 식
