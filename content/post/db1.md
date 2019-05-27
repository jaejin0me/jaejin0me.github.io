---
title: "데이터베이스 정리"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["database"]
categories: ["database"]
author: "Jaejin Jang"
---

가장 배경지식이 부족한 데이터베이스.. 그래도 비중이 적고 문제가 쉽다고 하니 다행이다.
새로 알게 된 것이나 모르는 내용 위주로 정리를 한다.

인용부호 `'` `'`  에서만 대소문자를 구분한다.
문자열의 패턴을 검색할때는 like를 쓴다. string like pattern 과 같다. 패턴에서 % 는 임의의 문자열을 뜻하고(문자열이 없을 수 있음), _ 는 임의의 한 문자를 뜻한다.

null에 대한 산술 연산은 null이다. 비교 연산은 unknown이다. is null 키워드를 통해 null을 확인 가능하다.

order by를 통해 출력을 정렬할 수 있다.

서브쿼리는 where와 from 절에만 사용한다.

중복제거는 select distinct

C언어에서 쓰이는 논리연산자는 사용되지 않는다. and or not을 사용한다.

문자열 접합은 || 이다

특정 범위 조건을 걸때는 between a and b 이다

where a in (10,20) // a 가 10 또는 20인

col format a 10 // 컬럼 10글자 보이기

from dual // dual 이라는 기본 테이블이 존재한다

join 1. equijoin : 동일한 컬럼이 있을때
     2. non-equijoin : 동일한 컬럼이 없을때
     3. self join : 한 테이블 내에서 일어나는 equijoin
     4. Outer join : quijoin을 만족시키지 않는 값을 보기 위해 사용한다. 조인시킬 컬럼이 없는 쪽에 (+) 를 붙인다.

from table 1 inner join table2 on something // 두 테이블에서 on을 만족하는 레코드를 결합한다.
