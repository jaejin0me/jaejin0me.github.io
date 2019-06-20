---
title: "책뿌숴짐 - SQL 레벨업 9"
date: 2019-05-29T01:39:17+09:00
tags: ["sql 튜닝"]
categories: ["sql 튜닝"]
author: "Jaejin Jang"
mathjax: true
---

# 9장. 갱신과 데이터 모델 - 망치와 못

## 26강. 갱신을 효율적으로

- 갱신을 효율적으로 수행하는 SQL을 케이스 스터디

### 1. NULL 채우기

```sql
UPDATE OmitTbl
	SET val = (SELECT val
		FROM OmitTbl	OT1
		WHERE OT1.keycol = (SELECT MAX(seq)
		FROM OmitTbl OT2
		WHERE OT2.keycol = OmitTbl.keycol
		AND OT2.seq < OmitTbl.seq
		AND OT2.val IS NOT NULL))
WHERE val IS NULL;
```

### 2. 반대로 NULL을 설정

```sql
UPDATE OmitTbl
	SET val = CASE WHEN val
		= (SELECT val
		FROM OmitTbl 01
		WHERE 01.keycol = OmitTbl.keycol
		AND 01.seq
		= (SELECT MAX(seq)
		FROM OmitTbl 02
		WHERE 02.keycol = OmitTbl.keycol
		AND 02.seq < OmitTbl.seq))
 	THEN NULL
 	ELSE val END;
```

## 27강. 레코드에서 필드로의 갱신

### 1. 필드를 하나씩 갱신

```sql
UPDATE ScoreCols
	SET score_en = (SELECT score
		FROM ScoreRow SR
		WHERE SR.studend_id = ScoreCols.studend_id
		AND subject = '영어'),
		score_nl = (SELECT score
		FROM ScoreRow SR
		WHERE SR.studend_id = ScoreCols.studend_id
		AND subject = '국어'),
		score_mt = (SELECT score
		FROM ScoreRow SR
		WHERE SR.studend_id = ScoreCols.studend_id
		AND subject = '수학');
```

 - 명확하지만 항목별로 서브쿼리를 필요로하기 때문에 비효율적이다.

### 2. 다중 필드 할당

 - 여러 개의 필드를 리스트화하고 한번에 갱신

```sql
UPDATE ScoreCols
	SET (score_en, score_nl, score_mt)
		= (SELECT MAX(CASE WHEN subject = '영어'
				THEN score
				ELSE NULL END) AS score_en,
				MAX(CASE WHEN subject = '국어'
				THEN score
				ELSE NULL END) AS score_nl,
				MAX(CASE WHEN subject = '수학'
				THEN score
				ELSE NULL END) AS score_mt
		FROM ScoreRows SR
WHERE SR.student_id = ScoreCols.student_id);
```

 - 상관 서브쿼리가 하나로 정리된 대신, ScoreRows 테이블에 대한 접근이 INDEX UNIQUE SCAN -> INDEX RANGE SCAN, MAX 함수의 정렬리 추가되므로 트레이드오프가 있다.

#### 1) 다중 필드 할당

#### 2) 스칼라 서브쿼리

 - MAX함수를 적용해 집약시킴으로써 가능해진다.

### 3. NOT NULL 제약이 걸려있는 경

#### 1) UPDATE 구문 사용

```sql
UPDATE ScoreColsNN
	SET score_en = COALESCE((SELECT score
		FROM ScoreRow SR
		WHERE SR.studend_id = ScoreCols.studend_id
		AND subject = '영어'), 0),
		score_nl = COALESCE((SELECT score
		FROM ScoreRow SR
		WHERE SR.studend_id = ScoreCols.studend_id
		AND subject = '국어'), 0),
		score_mt = COALESCE((SELECT score
		FROM ScoreRow SR
		WHERE SR.studend_id = ScoreCols.studend_id
		AND subject = '수학'), 0);
WHERE EXISTS (SELECT *
			FROM ScoreRows
			WHERE student_id = ScoreColsNN.studend_id);
```

 - 테이블 사이에 일치하지 않는 레코드는 제거
 - 학생은 존재하지만 과목이 없는 경우 처리

#### 2) MERGE 구문 사용

```sql
MERGE INTO ScoreColsNN
	USING (SELECT student_id,
		COALESCE(MAX(CASE WHEN subject = '영어'
				THEN score
				ELSE NULL END), 0) AS Score_en,
		COALESCE(MAX(CASE WHEN subject = '국어'
				THEN score
				ELSE NULL END), 0) AS Score_nl,
		COALESCE(MAX(CASE WHEN subject = '수학'
				THEN score
				ELSE NULL END), 0) AS Score_mt
			FROM ScoreRows
			GROUP By studend_id) SR
			ON	(ScoreColsNN.student_id = SR.student_id)
	WHEN MATCHED THEN
	UPDATE SET ScoreColsNN.score_en = SR.score_en,
			coreColsNN.score_nl = SR.score_nl,
			ScoreColsNN.score_mt = SR.score_mt;
```

 - 결합조건을 ON 구 하나에 묶을 수 있다.
 - ScoreRows 테이블 풀 스캔 1회 + 정렬 1회로 고정된다.
 - 나은 선택지로 고려해볼만 하다.

## 28강. 필드에서 레코드로 변경

```sql
UPDATE ScoreRows
	SET Score = (SELECT CASE ScoreRows.subject
		WHEN '영어' THEN score_en
		WHEN '국어' THEN score_nl
		WHEN '수학' THEN score_nt
		ELSE NULL
		END
	FROM ScoreCols
	WHERE student_id = ScoreRow.student_id);
```

## 29강. 같은 테이블의 다른 레코드로 갱신

### 1. 상관 서브쿼리 사용

```sql
INSERT INTO Stocks2
SELECT brand, sale_date, price,
	CASE SIGN(price -
			(SELECT price
			FROM Stocks S1
			WHERE brand = Stocks.brand
			AND sale_date =
				(SELECT MAX(sale_date)
				FROM Stocks S2
				WHERE brand = Stocks.brand
				AND sale_date < Stock.sale_date)))
	WHEN -1 THEN '아래화살표'
	WHEN 0 THEN '오른쪽화살표'
	WHEN 1 THEN '위쪽화살표'
	ELSE NULL
	END
FROM Stocks S2;
```

 - 상관서브쿼리 때문에 테이블에 여러번 접근해야 한다.

### 2. 윈도우 함수 적용

```sql
INSERT INTO Stocks2
SELECT brand, sale_date, price,
	CASE SIGN(price -
		MAX(price) OVER (PARTITION BY brand
			ORDER BY sale_date
			ROWS BETWEEN 1 PRECEDING
			AND  1 PRECEDING))
	WHEN -1 THEN '아래화살표'
	WHEN 0 THEN '오른쪽화살표'
	WHEN 1 THEN '위쪽화살표'
	ELSE NULL
	END
FROM Stocks S2;
```

 - 매우 간단하고 효율적이 된다.

### 3. INSERT와 UPDATE 어떤 것이 좋을까?

 - INSERT SELECT 장점
  - 처리가 더 빠름
  - 자기참조를 허가하지 않는 DB에서도 사용가능
 - 단점
  - 같은 데이털르 두개 만들어야하므로 저장소를 2배 이상 소비
 - 뷰로 만들어도 되나 트레이드 오프가 있다

## 30강. 갱신이 초래하는 트레이드오프

 - 주문일과 배송 예정일 차이가 3일 이상 차이나는 것

### 1. SQL을 사용하는 방법

```sql
SELECT O.order_id,
	O.order_name,
	ORC.delivery_date - O.order_date AS diff_days
FROM Orders O
		INNER JOIN OrderReceipts ORC
		ON O.order_id = ORC.order_id
WHERE ORC.delivery_date - O.order_date >= 3;
```

 - 주문별로 최대 지연일을 알고 싶은 경우

```sql
SELECT O.order_id,
	MAX(O.order_name), -- SELECT 하기 위해 MAX를 사용
	MAX(ORC.delivery_date - O.order_date) AS max_diff_days
FROM Orders O
		INNER JOIN OrderReceipts ORC
		ON O.order_id = ORC.order_id
WHERE ORC.delivery_date - O.order_date >= 3;
GROUP BY O.order_id;
```

### 2. 모델 갱신을 사용하는 방법

 - 쿼리를 통해 찾는게 아니라 필드를 하나 추가해 해결할수도 있을 것
 - 코딩 외의 방법도 해결수단이 된다.

## 31강. 모델 갱신의 주의점

 - 3가지의 트레이드 오프가 있다

### 1. 높아지는 갱신 비용

 - 배송 지연 플래그 필드를 갱신하는 비용이 든다

### 2. 갱신까지의 시간 랙 발생

 - 갱신 되기 전까지의 시간차이가 발생

### 3. 모델 갱신 비용 발생

 - 모델의 수정은 대단히 큰 수정이 요구됨, 특히나 이미 운영을 시작했다면 더욱 더

## 32강. 시야 협착 : 관련 문제

 - 시야 협착에 빠지기 쉬운 경우를 살펴본다
 - 31강의 문제를 그대로 사용한다
 - 주문번호마다 몇개의 상품이 주문되었는지 확인(주문번호, 주문자 이름, 주문일, 상품수)

### 1. 다시 SQL을 사용한다면

```sql
SELECT O.order_id,
	MAX(O.order_name) AS order_name,
	MAX(O.order_date) AS order_date,
	COUNT(*) AS item_count
FROM Orders O
	INNER JOIN OrderReceipts ORC
		ON O.order_id = ORC.order_id
GROUP BY O.order_id;
```

```sql
SELECT O.order_id,
	   O.order_name,
	   O.order_date,
	   COUNT(*) OVER (PARTITION BY O.order_id) AS item_count
FROM Orders O
	INNER JOIN OrderReceipts ORC
		ON O.order_id = ORC.order_id;
```

 - 집약을 쓰든 윈도우 함수를 쓰든 모두 결합과 집약을 수행하기 때문에 실행 비용은 비슷하다. 가독성과 확장성 측면에서 윈도우 함수를 쓰는 것이 더 좋다.

### 2. 다시 모델 갱신을 사용한다면

 - 주문이 등록될 때 수를 알 수 있으므로 필드를 추가해 개수를 넣기 쉽다. 다만, 주문 변경이 일어나는 경우의 처리를 고려해야 한다.

### 3. 초보자보다 중급자가 경계해야

## 33강. 데이터 모델을 지배하는 자가 시스템을 지배한다.

 - 현명한 데이터 구조와 멍청한 코드의 조합이 멍청한 데이터 구조와 현명한 코드의 조합보다 좋다.
 - 엔지니어의 사명은 전략적 실패를 만회하는 전술을 찾는 것이 아닌 올바른 전략을 고려하는 것

## 34강. 인덱스와 B-tree

 - RDB의 인덱스 구조, 1. B-tree, 2. 비트맵, 3. 해시

### 1. 만능형 : B-tree

 - 대부분 DB의 B+tree 라는 수정 버전을 사용한다.
 - B+tree는 리프노드에만 키값을 저장한다.
 - B+tree는 루트와 리프의 거리를 가능한 일정하게 유지하려고 하기 때문에 검색 속도가 안정적이다.

### 2. 기타 인덱스

 - 비트맵 인덱스 : 데이터를 비트 플래그로 변환해서 저장하는 형태의 인덱스로 카디널리티가 낮은 필드에 대해 효과를 발휘한다. 하지만 갱신할 때 오버헤드가 키 BI/DWH 용도로 사용된다.
 - 해시 인덱스 : 검색 외에 효과가 없어 거의 사용되지 않는다.

## 35강. 인덱스를 잘 활용하려면

 - B+tree는 양이 증가해도 검색이 안정적이고, 범위 검색도 쉬움

### 1. 카디널리티(값의 균형)와 선택률(전체중에서 몇개가 선택되는지)

 - 클러스터링 팩터(저장소에 같은 값이 어느정도 물리적으로 뭉쳐 존재하는지)가 낮을수록 좋다. 하지만 이것은 구현에 의족한다.

### 2. 인덱스를 사용하는 것이 좋은지 판단하려면

 - 카디널리티가 높은 것
 - 선택률이 낮은 것 5 ~ 10 % 이하, 저장소의 성능향상과 반비례한다.

## 36강. 인덱스로 성능 향상이 어려운 경우

 - 인덱스 설계는 테이블 정의와 SQL만 봐서는 할 수 없다.

### 1. 압축 조건이 존재하지 않음

 - WHERE 구가 없이 SELECT 하는 경우

### 2. 레코드를 제대로 압축하지 못하는 경우

 - WHERE 구 조건의 선택률이 너무 높아 인덱스를 만들기 비효율적이다.

#### 1) 입력 매개변수에 따라 선택률이 변동하는 경우 - 1
#### 2) 입력 매개변수에 따라 선택률이 변동하는 경우 - 2

### 3. 인덱스를 사용하지 않는 검색 조건

#### 1) 중간 일치, 후방 일치 LIKE 연산자

 - LIKE를 사용하는 경우 인덱스는 전방일치에만 적용 가능하다.

#### 2) 색인 필드로 연산하는 경우(함수를 적요하는 경우에도 안됨)

 - 다음과 같이 바꾸면 됨
 - WHERE col_1 * 1.1 > 100 -> col_1 > 100/1.1

#### 3) IS NULL을 사용하는 경우

 - 색인 필드 데이터에는 NULL이 존재하지 않기 때문에 인덱스를 사용불가

#### 4) 부정형을 사용하는 경우

 - <>, !=, NOT IN

## 37강. 인덱스를 사용할 수 없는 경우 대처법

### 1. 외부 설정으로 처리 - 깊고 어두운 강 건너기

#### 1) UI 설계로 처리

 - 사용자가 선택할 수 있는 조건을 제한한다.

### 2. 외부 설정을 사용한 대처 방법의 주의점

 - 개발전에 합의해야 한다. 서로의 이해관계를 파악하는 것이 중요!

### 3. 데이터 맡로 대처

 - 특정한 쿼리에서 필요한 데이터만을 저장하는, 상대적으로 작은 크기의 테이블이다. 서브셋
 - 원래 대규모의 데이터를 다뤄야 하는(성능 조건이 중요함) BI/DWH 분야에 사용되는 기술이다. 접근 대상 테이블의 크기를 작게해서 I/O양을 줄이는 것이 목적이다.

### 4. 데이터 마트를 채택할 시 주의점

#### 1) 데이터 신선도

 - 동기 시점에 따라 신선도와 성능간의 트레이드 오프 고려 할 것

#### 2) 데이터 마트 크기

 - I/0 양을 줄이는 것이 목적이므로 테이블의 크기가 딱히 줄어들지 않는다면 필요없다.
 - GROUP BY절을 미리 사용해 집계를 마치고 데이터 마트로 만들면 필드 수와 레코드수를 크게 줄일 수 있다.

#### 3) 데이터 마트수

 - 목적에 맞게 잘 만들어 관리해야, 용량과 성능문제가 안생긴다.
 - 빠르다고 무작점 만들어서 쓰면 안됨

#### 4) 배치 윈도우

 - 데이터 마트도 만드는데 시간이 걸리고 변경에 따른 갱신도 발생하므로 배치 윈도우를 압박한다. 배치윈도우와 Job Net도 고려할 것

### 5. 인덱스 온리 스캔으로 대처

 - I/O 감소를 목적으로하는 데이터 마트와 접근이 같고, 데이터 동기 문제를 해결할 수 있다.
 - 풀 스캔을 할 때 검사대상을 테이블이 아닌 인덱스로 바꿀 수 있다.
 - 압축 요건이 존재하지 않는 경우
 - CREATE INDEX CoveringIndex ON Orders (order_id, receive_date);
 - SQL 구문에서 필요한 필드를 인덱스만으로 커버할 수 있는 경우, 테이블 접근을 생략하는 기술 이다.
 - 레코드를 제대로 압축하지 못하는 검색 조건
 - CREATE INDEX CoveringIndex_1 ON Orders (process_flg, order_id, receive_date);
 - 압축은 되지만 인덱스를 사용하지 않는 검색 조건
 - CREATE INDEX CoveringIndex_2 ON Orders (shop_name, order_id, receive_date);
 - 로우 지향 DB를 컬럼 지향 DB로 만드는 방법이라고 생각하면 된다.

### 6. 인덱스 온리 스캔의 주의사항

#### 1) DBMS에 따라 사용할 수 없는 경우도 있다.

#### 2) 한 개의 인덱스에 포함할 수 있는 필드 수에 제한이 있다.

#### 3) 갱신 오버 헤드가 커진다.

 - 커버링 인덱스는 성질살 필연적으로 필드 수가 많아 크기가 큰 인덱스가 되기 쉽다.
 - 따라서 테이블을 갱신할 때의 오버헤드도 일반적인 인덱스에 비해 커진다.

#### 4) 정기적인 인덱스 리빌드가 필요

 - 인덱스에만 접근한다는 것은 성능이 인덱스의 크기에 의존한다는 것이다. 따라서 정기적인 모니터링과 리빌드를 필요로 한다.

#### 5) SQL에 새로운 필드가 추가되면 사용할 수 없다.

 - 일반적인 인덱스에 비해 어플리케이션 유지 보수에 약한 타입의 튜닝이라고 할 수 있다.
