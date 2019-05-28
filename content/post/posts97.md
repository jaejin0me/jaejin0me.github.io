---
title: "책뿌수기 - 기초가 든든한 데이터 베이스 7"
date: 2018-11-29T00:30:56+09:00
draft: false
tags: ["database"]
categories: ["database"]
author: "Jaejin Jang"
---

*인용하는 그림은 다양한 곳에서 가져왔음을 밝힙니다*

# Ch 7. 데이터 관리

## Section 1. 데이터 입력

* INSERT INTO 테이블_이름 [열_이름 ~] VALUES (값 ~);

1.1 테이블에 데잍를 직접입력하는 방법

* INSERT INTO 이용

1.1 서브 쿼리를 이용해 입력

* INSERT INTO 출판사_경기 SELECT * FROM 출판사 WHERE 전화번호 LIKE '031%';

## Section 2. 데이터 수정

* UPDATE 테이블_이름 SET 열_이름 = 값 또는 식 [WHERE 조건];
* WHERE 조건이 없으면 모두 업데이트 되므로 주의!

## Section 3. 데이터 삭제

* DELETE [FROM] 테이블_이름 [WHERE 조건];
* WHERE 조건이 없으면 모두 삭제됨으로 주의!
