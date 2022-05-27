---
title: "HeidiSQL 정리"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["database"]
categories: ["database"]
author: "Jaejin Jang"
---

[참고사이트](https://mariadb.com/kb/ko/basic-sql-statements/)

### 1.SELECT - 선택해서 보기 위함
```sql
//중복제거
SELECT DISTINCT
//조건 추가
WHERE 컬럼조건
//특정 패턴의 컬럼
LIKE
//이산적 값 만족
IN
//별칭(출력되는 컬럼명으로는 알아 보기 힘들 때 사용)
SELECT 컬럼 AS 별칭
//출력행 개수 조절, 리눅스 head와 비슷
LIMIT 숫자
//두개 이상의 SELECT문 연결, 기본적으로 DISTINCT 출력임
SELECT 컬럼 FROM 테이블 UNION SELECT 컬럼 FROM 테이블
-- 조건을 추가할 경우 처음오는 SELECT의 컬럼명을 사용해야 한다
SELECT 컬럼1 FROM 테이블 UNION SELECT 컬럼2 FROM 테이블 WHERE 컬럼1
//중복있는 UNION
셀렉문 UNION ALL 셀렉문
//컬럼 정렬
셀렉문 WHERE 컬럼 ORDER BY ASC|DESC
```

### 2.INSERT - 행을 추가하기 위함
```sql
INSERT INTO 테이블 VALUES(값, 값, 값)
//특정 컬럼에만 값 넣기
INSERT INTO (컬럼2) 테이블 VALUES(값)
```
### 3.DELECT - 행삭제, 조건절 중요
```sql
DELECT FROM 테이블 WHERE 식
DELECT FROM 테이블 // 모든 행이 다 지워져버림
```
### 4.UPDATE - 테이블 내 레코드 값 변경, 조건절 중요
```sql
UPDATE 테이블 SET 컬럼1=값, 컬럼2=값, 컬럼3=값 WHERE 식
UPDATE 테이블 SET 컬럼1=값, 컬럼2=값, 컬럼3=값 // 모든 레코그값이 다 변경 됨
```
### 5.CREATE - DB나 테이블 생성
```sql
CREATE DATABASE DB명
CREATE TABLE 테이블명
(
컬럼1 자료형.
컬럼2 자료형,
컬럼3 자료형
)
```
### 6.DROP - DB나 테이블 삭제, drop the bit, 비트주세요
```sql
DROP DATABASE DB명
DROP TABLE 테이블명
```
### 7.TRUNCATE - 테이블 유지하면서 레코드만 삭제
```sql
TRUNCATE TABLE 테이블명
```
### 8.ALTER - 컬럼 추가, 삭제, 수정

### 9.JOIN - 두개 이상의 테이블로부터 질의하기 위해
```sql
INNER JOIN - 두 테이블에서 값이 일치하는 행들 반환
SELECT 컬럼 FROM 테이블1 INNER JOIN 테이블2 ON 테이블1.컬럼 = 테이블2.컬럼
LEFT JOIN - 일치여부에 상관없이 왼쪽 테이블의 값들은 반환됨
SELECT 컬럼 FROM 테이블1 LEFT JOIN 테이블2 ON 테이블1.컬럼 = 테이블2.컬럼
RIGHT JOIN - 일치여부에 상관없이 오른쪽 테이블의 값들은 반환됨
SELECT 컬럼 FROM 테이블1 RIGHT JOIN 테이블2 ON 테이블1.컬럼 = 테이블2.컬럼
```

### 10.CONSTRAINTS - 테이블에 삽입될 자료형 제한
```sql
- NOT NULL
CREATE TABLE 테이블명
(
컬럼1 자료형 NOT NULL,
컬럼2 자료형,
컬럼3 자료형
)

- UNIQUE
CREATE TABLE 테이블명
(
컬럼1 자료형 UNIQUE,
컬럼2 자료형,
컬럼3 자료형
#UNIQUE(컬럼1)
)

- PRIMARY KEY
CREATE TABLE 테이블명
(
컬럼1 자료형 PRIMARY KEY,
컬럼2 자료형,
컬럼3 자료형
#PRIMARY KEY(컬럼1)
)

- FOREIGN KEY
CREATE TABLE 테이블명
(
컬럼1 자료형 FOREIGN KEY,
컬럼2 자료형,
컬럼3 자료형
#FOREIGN KEY(컬럼1)
)

- CHECK
CREATE TABLE 테이블명
(
컬럼1 자료형 CEHCK(식),
컬럼2 자료형,
컬럼3 자료형
#CEHCK(식)
)

- DEFAULT
CREATE TABLE 테이블명
(
컬럼1 자료형 DEFAULT 값,
컬럼2 자료형,
컬럼3 자료형
#UNIQUE(컬럼1)
)
```

### 11.주석
```sql
# 한줄 주석
-- 한줄 주석
/* 범위 주석 */
```

### 12.AUTO_INCREMENT - 테이블 생성시 ID 자동증가용으로 쓰임
```sql
create table student2(
id int not null  auto_increment,
name varchar(255),
grade int,
house varchar(255),
primary key(id)
);
```
### 13.NULL
- 0과 NULL은 다름
```sql
#조회
WHERE 컬럼 IS NULL
WHERE 컬럼 IS NOT NULL
```
### 14.VIEW - 가상 테이블 만들기

- 원하는 값들을 골라 가상 테이블을 만들 수 있다. 일반 테이블처럼 사용가능
```sql
#생성
CREATE VIEW 뷰명 AS SELECT 컬럼 FROM 테이블 WHERE 조건
#삭제 DROP VIEW 뷰명
```

### 15.INDEX - 데이터를 빠르게 찾기 위함

### 16.GROUP BY - 그룹별로 나뉘어 통계를 보기 위해
```sql
SELECT JOB_ID,AVG(SALARY),MIN(SALARY),MAX(SALARY),COUNT(SALARY) FROM HR_EMPLOYEES GROUP BY JOB_ID
```
### 17.HAVING - 그룹에 조건을 걸기위해(SELECT의 WHERE와 같다)
```sql
SELECT JOB_ID,AVG(SALARY),MIN(SALARY),MAX(SALARY),COUNT(SALARY) FROM HR_EMPLOYEES GROUP BY JOB_ID HAVING JOB_ID='AC_ACCOUNT'
```