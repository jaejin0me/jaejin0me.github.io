---
title: "책뿌수기 - 기초가 든든한 데이터 베이스 6"
date: 2018-11-29T00:27:56+09:00
draft: false
tags: ["database"]
categories: ["database"]
author: "Jaejin Jang"
---

*인용하는 그림은 다양한 곳에서 가져왔음을 밝힙니다*

# Ch 6. 테이블 관리

## Section 1. 테이블 생성

1.1 데이터 형식을 사용한 테이블 정의

* CREATE TABLE 테이블이름  
(  
열_이름 데이터 형식 [NOT NULL, UNIQUE, DEFAULT, CHECK ~],  
[PRIMARY KEY(열_이름)],  
[FOREIGN KEY(열_이름) REFERENCES 테이블_이름(열_이름)]  
);

* 문자
 * CHAR(n) : 고정형 n개의 문자 저장
 * varchar(n) : 최대 n개의 문자를 저장하는 가변형식
* 숫자
 * INT : 정수
 * DECIMAL[(c,[s])] : p는 총 자릿수, s는 소숫점 이하 최대 자릿수
* 날짜
 * DATE : 날짜
 * DATETIME : 날짜와 시간

* 데이터 형식 선택시의 고려 사항
 * 정수 - 숫자가 최대 100까지만 간다면 TINYINT를 사용해 공간을 최소화 한다
 * 문자 - 문자의 길이가 비슷하게 저장된다면 고정형 사용이 낫고, 길이가 크게 가변적이라면 가변형 사용이 나음

* 특정 DB 선택
 * USE BOOKSTORE;

1.2 NULL 값 사용

* NULL = 알 수 없음. 따라서 NULL 끼리 동등비교를 해도 참이 아니다.

1.3 기본키 제약조건 설정

* 기본 키로 지정된 열에는 값이 반드시 NOT NULL이 되야 한다.
* 기본 키로 설정된 열에는 인덱스가 자동으로 생성된다.

* CREATE TABLE 고객  
(  
고객번호 INT PRIMAR KEY / CONSTRAINT PK_고객번호 PRIMARY KEY)  
, 고객명 VARCHAR(30) NULL  
);

* CREATE TABLE 고객  
(  
고객번호 INT NOT NULL  
, 고객명 VARCHAR(30) NULL  
, PRIMARY KEY(고객번호)  
);

* ALTER TABLE 고객 DROP CONSTRAINT PK_고객번호;

* ALTER TABLE 고객 ADD CONSTRAINT PK_고객번호 PRIMARY KEY(고객번호);

1.4 외래키 제약조건 설정

* 외래키 제약조건 : 두 테이블 간의 관계를 정의하는 제약조건이다.
* 참조 무결성 관계가 설정되면 테이블들 간 관련 있는 데이터들에 대해 실수로 변경하거나 삭제하는 것을 막을 수 있다.
* 참조하는 테이블(child table)의 열을 외래 키(foreign key)
* 참조되어지는 테이블(parent table)의 열을 외래 키(reference key)

* 외래 키 설정 제약 조건
 * 참조키는 기본키 or UNIQUE or 고유인덱스를 가져야 한다.
 * 외래 키와 참조 키의 형식이 동일해야 한다.
 * 부모/자식 테이블이 같은 DB에 저장되어야 한다.

1.5 기타 제약조건 추가

* UNIQUE 제약조건
 * 기본키 제약조건 + 널값 허용 (단, 1개만)
 * CONSTRAINT U_주민번호 UNIQUE
* IDENTITY 속성
 * 일반적으로 기본 키 제약조건과 함께 사용되어 테이블에 대한 고유한 행 식별자 역할을 한다.
 * 고객번호 INT IENTITY (1,1) NOT NULL
* DEFAULT
 * 열에 입력 값이 생략된 경우 넣을 값
 * 고객명 VARCHAR(50) NULL CONSTRAINT DF_고객명 DEFAULT '무명씨'
* CHECK
 * 입력가능 범위나 조건 설정
 * 상태 CHAR(4) NULL CONSTRAINT CH_고객상태 CHECK (상태 IN ('신규', '기존', '이탈'))
* RULE
 * CHECK와 동일하나, 룰을 만들어 적용

## Section 2. 테이블 구조 변경

2.1 열 추가

* ALTER TABLE 테이블_이름 ADD 열_이름 데이터 형식 NULL_속성 DEFAULT 기본값;
* NULL_속성의 기본값은 NULL이다. NOT NULL 열로 만들면 DEFAULT 제약 조건이 있어야 디는데, WITH VALUES 옵션이 사용된다.
* 열을 추가하는 경우 마지막에 추가 된다.
* ALTER TABLE 고객 ADD 입력일자 DATETIME NULL CONSTRAINT 입력일자 기본값 DEFAULT getDate() with VALUES;

2.2 열 삭제

* 2개 이상의 열이 존재하는 테이블에서만 실행할 수 있다
* ALTER TABLE 테이블_이름 DROP COLUMN 열_이름
* ALTER TABLE 고객 DROP CONSTRAINT 입력일자_기본값 <= 열삭제가 안되는 경우 제약조건 해제 후 시도

2.3 열 변경

* ALTER TABLE 테이블_이름
* ALTER COLUMN (열_이름 데이터_형식 [DEFAULT 식] [NULL 속성])

## Section 3. 테이블 제거

* DROP TABLE 테이블_이름;
