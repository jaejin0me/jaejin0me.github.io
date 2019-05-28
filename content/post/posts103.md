---
title: "책뿌수기 - SQL 레벨업-2"
date: 2019-01-20T22:20:56+09:00
draft: false
tags: ["sql 튜닝"]
categories: ["sql 튜닝"]
author: "Jaejin Jang"
---


_인용하는 그림은 다양한 곳에서 가져왔음을 밝힙니다_

# 1. SQL 기초

## ch 6. SELECT 구문

### 1) SELECT 구와 FROM 구

-   SELECT 1 처럼 상수를 선택하는 경우 FROM이 필요없다.

### )2) WHERE 구

-   WHERE 구의 조건이 많을 경우 IN 으로 대체
-   SELECT 구문은 테이블을 반환하는 읽기 전용 함수 이다.

### 3) GROUP BY 구

-   일부 DBMS에서는 지원하지 않음

### 4) HAVING

-   GROUP BY에 조건을 걸때
-   WHERE가 레코드에 조건을 거는 것이라면, HAVING은 집합에 조건을 거는 것

### 5) ORDER BY

### 6) 뷰와 서브쿼리

-   자주 사용하는 SELECT 구문을 DB에 저장 = 뷰(view)
-   뷰는 내부에 데이터러 보유하지 않음(SELECT 구문을 저장할 뿐)
-   서브쿼리의 실행과 동일하다
-   WHERE 조건에 서브쿼리를 거면 조건이 바뀌어도 문제없음(조건을 하나하나 하드코딩하는 번거로움을 없앨수있다)

## ch 7. 조건 분기, 집합 연산, 윈도우 함수, 갱신

### 1) SQL과 조건 분기

-   SQL의 조건 분기는 CASE식을 통해 한다.
-   SQL의 조건 분기는 특정한 값을 리턴하는 것이 특징이다.
-   CASE는 식이기 때문에 활용성이 높은 것이 강점이다.

### 2) SQL의 집합 연산

-   UNION : 합집합(기본적으로 중복을 제거)
-   INTERSECT : 교집합
-   EXCEPT : 차집합

### 3) 윈도우 함수

-   집약 기능이 없는 GROUP BY 구
-   PARTITION BY
-   SELECT 구에만 사용됨
-   윈도우 전용함수로 RANK, ROW_NUMBER가 있다.

### 4) 트랜잭션과 갱신

-   INSERT, UPDATE, DELETE
