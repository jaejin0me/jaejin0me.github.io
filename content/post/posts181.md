---
title: "책뿌수기 - SQL 레벨업 8"
date: 2019-05-28T23:49:45+09:00
tags: ["sql 튜닝"]
categories: ["sql 튜닝"]
author: "Jaejin Jang"
mathjax: true
---

# 8장. SQL의 순서 - 깨어나는 절차 지향

 - sql은 관계 모델의 이론에 기초하고 있기 때문에 순번을 다루기 위한 기능이 없다.
 - 하지만 레코드에 순번을 붙여 처리하는 경우가 많아 관련기능(시퀀스 객체, ID 필드, 윈도우 함수)을 추가하고 있다.

## 23강. 레코드에 순번 붙이기

### 1. 기본키가 한 개의 필드일 경우

#### 1) 윈도우 함수로 사용

 - ROW_NUMBER 함수를 사용

```sql
SELECT student_id, 
		ROW_NUMBER() OVER (ORDER BY student_id) AS Seq 
		FROM Weights;
```

#### 2) 상관 서브쿼리 사용

```sql
SELECT student_id, 
	(SELECT COUNT(*) 
		FROM Weight W2 
		WHERE W2.student_id <= W1.student_id) AS Seq 
	FROM Weights W1;
```

 - 기능은 동일하지만 1) 방법의 성능이 좋다. 스캔 횟수 1):1회, 2):2회

### 2. 기본 키가 여러 개의 필드로 구성되는 경우 

#### 1) 윈도우 함수를 사용

```sql
SELECT class, student_id,
	ROW_NUMBER() OVER (ORBER BY class, stduent_id) AS Seq
	FROM Weight2;
```

#### 2) 상관 서브쿼리를 사용

 - 다중 필드 비교하기(문자, 숫자, 3개 비교도 가능)

```sql
SELECT class, student_id,
	(SELECT	COUNT(*)
			FROM Weight W2
		WHERE (W2.class, W2.student_id)
			<= (W1.class, W1.stduent_id) AS Seq
		FROM Weight W1;
```

### 3. 그룹마다 순번을 붙이는 경우

#### 1) 윈도우 함수를 사용

 - class 필드에 PARTITION BY 적용

```sql
SELECT class, student_id,
	ROW_NUMBER() OVER(PARTITION BY class ORDER BY student_id) AS Seq
	FROM Weight2;
```

#### 2) 상관서브쿼리를 사용

```sql
SELECT class, student_id,
	(SELECT COUNT(*)
		FROM COUNT(*)
	WHERE W2.class = W1.class
		AND W2.student_id <= W1.student_id) AS Seq
	FROM Weight2 W1;
```

### 4. 순번과 갱신

#### 1) 윈도우 함수를 사용

 - 셀렉트 쿼리를 SET에 넣으면 됨

```sql
UPDATE Weights3
	SET Seq = (SELECT Seq
			FROM (SELECT class, student_id,
					ROW_NUMBER() OVER (PARTITION BY class
							ORDER BY student_id) AS Seq
			FROM Weights3) SeqTbl
	WHERE Weights3.class = SeqTbls.class
		AND Weights3.student_id = SeqTbl.student_id)
```

#### 2) 상관 서브쿼리를 사용

```sql
UPDATE Weight3
		SET seq = (SELECT COUNT(*)
				FROM Weight3 W2
			WHERE W2.class = Weights3.class
				AND W2.student_id <= Weight3.student_id)
```

## 24강. 레코드에 순번 붙이기 응용

### 1. 중앙값 구하기

#### 1) 집합 지향적 방법

```sql
SELECT AVG(weight)
	FROM (SELECT W1.weight
			FROM Weights W1, Weights W2
		GROUP BY W1.weight
		HAVING	SUM(CASE WHEN W2.weight >= W1.weight THEN
			1 ELSE 0 END) >= COUNT(*)/2
		AND SUM(CASE WHEN W2.weight <= W1.weight THEN
			1 ELSE 0 END) >= COUNT(*)/2) TMP;
```

 - 코드가 복잡하다
 - 성능이 나쁘다. w1과 w2간에 결합이 발생

#### 2) 절차 지향적 방법 1 - 세계의 중심을 향해

 - sql에서 자연수의 특징을 활용하면 '양쪽 끝부터 숫자 세기'를 할 수 있다

```sql
SELECT AVG(weight) AS median
	FROM (SELECT weight,
		ROW_NUMBER() OVER (ORDER BY weight ASC, student_id ASC) AS hi,
		ROW_NUMBER() OVER (ORDER BY weight DESC, student_id DESC) AS lo
		FROM Weights) TMP
	WEHRE hi IN(lo, lo+1, lo+2);
```

 - RANK 또는 DENSE_RANK를 사용해서는 안된다. 순위가 겹치거나 빌 수 있다.
 - 테이블 접근 1회로 감소, 대신 정렬이 2회로 늘었다. ROW_NUMBER에서 사용하는 정렬이 오름/내림차순 2개라서 그렇다.

#### 3) 절차 지향적 방법 2 - 2빼기 1은 1

 - 성능적으로 개선하기

```sql
SELECT AVG(weight)
	FROM (SELECT weight,
			2 * ROW_NUMBER() OVER(ORDER BY weight)
			- COUNT(*) OVER() AS diff
		FROM Weights) TMP
	WHERE diff BETWEEN 0 AND 2;
```

 - 정렬리 1회로 줄어든다. 이 방법이 SQL 표준으로 중앙값을 구하는 가장 빠른 방법이다.

### 2. 순번을 사용한 테이블 분할

 - 비어있는 자리 출력하기

#### 1) 집합 지향적 방법 - 집합의 경계선

```sql
SELECT (N1.num + 1) AS gap_start,
	   '~',
	   (MIN(N2.min - 1) AS gap_end
	FROM Number N1 INNER JOIN Numbers N2
	ON N2.num > N1.num
		GROUP BY N1.num
	HAVING (N1.num + 1) < MIN(N2.num);
```

 - 코드도 간단하며 집합 지향적인 방식이라 좋다. 다만, 자기 결합을 사용해야 한다(Nested Loop).

#### 2) 절차 지향적 방법 - 다음 레코드와 비교

 - 컨셉 : 현재 레코드와 다음 레코드를 비교해 차이가 1이 아니면

```sql
SELECT NUM+1 AS gap_start,
	   '~',
	   (num + diff - 1) As gap_end
	FROM (SELECT num,
			MAX(num)
				OVER(ORDER BY num
					ROWS BETWEEN 1 FOLLOWING
							AND 1 FOLLOWING) - num
		FROM numbers) TMP (num, diff)
	WHERE diff <> 1
```

 - 테이블 접근 1회, 정렬 1회로 안정적 성능

### 3. 테이블에 존재하는 시퀀스 찾기

 - 친구 또는 가족 인원수에 맞게 자리를 예약하는 경우 활용됨

#### 1) 집합 지향적 방법 - 다시, 집합의 경계선

```sql
SELECT MIN(NUM) AS low,
	   '~',
	   MAX(num) AS high
	FROM (SELECT N1.num,
			COUNT(N2.num) - N1.num
		FROM Numbers N1 INNER JOIN Numbers N2
			ON N2.num <= N1.num
			GROUP BY N1.num) N(num, gp)
		GROUP BY gp;
```

#### 2) 집합 지향적 방법 - 다시, 다음 레코드 하나와 비교

```sql
SELECT low, high
	FROM(SELECT low,
			CASE WHEN high IS NULL
				THEN MIN(high)
					OVER (ORDER BY seq
						ROW BETWEEN CURRENT ROW
							AND UNBOUNDED FOLLOWING)
				ELSE high END AS high
		FROM (SELECT CASE WHEN COALESCE(prev_diff, 0) <> 1
						THEN num ELSE NULL END AS low,
					CASE WHEN COALESCE(next_diff, 0) <> 1
						THEN num ELSE NULL END As high,
					seq
			FROM (SELECT num,
						MAX(num)
						OVER(ORDER BY num
								ROWS BETWEEN 1 FOLLOWING
									AND 1 FOLLOWING) - num AS next_diff,
						num - MAX(num)
							OVER(ORDER BY num
									ROWS BETWEEN 1 PRECEDING
										AND 1 PRECEDING) AS prev_diff,
						ROW_NUMBER() OVER (ORDER BY num) AS seq
					FROM Numbers) TMP1 ) TMP2 ) TMP3
WHERE low IS NOT NULL;
```

## 25강. 시퀀스 객체, IDENTIFY 필드, 채번 테이블

 - 표준 SQL에는 순번을 다루는 기능으로 시퀀셜 객체나 IDENTIFY 필드가 존재한다. 하지만 사용하는 것을 권장하지는 않고, 사용한다면 시퀀스 객체를 권한다.

### 1. 시퀀스 객체

 - 테이블 또는 뷰처럼 스키마 내부에 존재하는 객체 중 하나

```sql
CREATE SEQUENCE testseq
START WITH 1
INCREMENT BY 1
MAXVALUE 10000
MINVALUE 1
CYCLE;
```

- INSERT 구문에서 흔히 사용된다.

#### 1) 시퀀스 객체의 문제점

 - 표준화가 늦어서, 구현에 따라 구문이 달라 이식성이 없고, 사용할 수 없는 구현도 있다
 - 시스템에서 자동으로 생성되는 값이므로 실제 엔티티 속성이 아니다.
 - 성능적인 문제를 일으킨다

#### 2) 시퀀스 객체로 발생하는 성능 문제

 - 순서성(순번의 대소 관계가 유지됨), 유일성, 연속성
 - 사용자 A가 시퀀스 객체에서 NEXT VALUE를 검색할 때의 처리
  1. 시퀀스 기개체에 배타 락을 적용
  1. NEXT VALUEfmf rjator
  1. CURRENT VALUE를 1만큼 증가
  1. 시퀀스 객체에 배타 락을 해제

#### 3) 시퀀스 객체로 발생하는 성능 문제의 대처

##### (1) CACHE

  1. 읽어들일 변수를 메모리에 설정하는 것, 다만 시스템 장애시 정상동작을 담보할 수 없다.

##### (1) NOORDER

  1. 순서성을 담보하지 않음으로써 오버 헤드를 줄인다.

#### 4) 순번을 키로 사용할 때의 성능 문제

 - Hot spot 과 관련된 문제임
 - DBMS는 비슷한 데이털르 연속적으로 INSERT하면 물리적으로 같은 영역에 저장한다. 이 때 특정 물리적 블록에만 I/O 부하가 커지므로 성능 악화가 발생 = Hot spot, Hot block
 - 시퀀스 객체를 사용해 INSERT를 반복하는 경우 발생하고, 대처가 불가능

#### 5) 순번을 키로 사용할 때의 성능 문제에 대처

##### (1) Oracle의 열 키 인덱스

 - 연속된 값을 도입하는 경우라도 DBMS 내부에서 변화를 주어 제대로 분산할 수 있는 구조를 사용하는 것, 다만 SELECT 성능이 나빠질 수 있다.

##### (2) 인덱스에 복잡한 필드를 추가해서 데이터의 분산도를 높인다.

 - 논리적으로 좋은 설계가 아님

### 2. IDENTIFY 필드

 - '자동 순번 필드'라고도 한다. 테이블의 필드로 정의하고, INSERT 발생할때마다 자동을 순번을 붙여주는 기능이다.
 - 시퀀스 객체에 비해 단점이 많다.
  - 시퀀스 객체는 여러 테이블에서 사용가능하지만, IDENTIFY 필드는 특정 테이블에 국한된다.
  - CACHE, NOORDER를 지정할 수도 없다.
 - 이점이 거의 없다.

### 3. 채번 테이블

 - 순번을 부여하기 위해 어플리케이션에서 채번 테이블이라는 것을 만들어 사용했었다.
 - 구시대 유물이며 문제가 안생기기를 바라는 것이 최선(튜닝할 방법도 없다)
