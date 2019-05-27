---
title: "책뿌수기 - SQL 레벨업 4"
date: 2019-01-29T22:20:56+09:00
draft: false
tags: ["sql 튜닝"]
categories: ["sql 튜닝"]
author: "Jaejin Jang"
---


# 4. 집약과 자르기

## ch 12. 집약

-   COUNT, SUM, AVG, MAX, MIN(Aggregate function)

### 1) 여러 개의 리코드를 한 개의 레코드로 집합

-   필드 수가 다르면 UNION 적용이 불가능하다. 또한 UNION으로 여러개의 쿼리를 머지하는 것은 성능적으로 안티패턴이다.
-   GROUP BY 구로 집약을 했을 때 SELECT 구에 입력할 수 있는 것은
    -   상수
    -   GROUP BY 구에서 사용한 집약 키
    -   집약함수
-   집약함수가 적용되면 여러 요소가 있는 집합으로부터 연산결과가 나옴
-   집약, 해시, 정렬
    -   집약시에는 해쉬 알고리즘을 사용한다(때로는 정렬)
    -   GROUP BY 구에 지정된 필드를 해쉬 함수로 사용해 결과를 만들고, 같은 결과로 그룹을 만들어 집약한다. 고전적인 방법보다 효율적
    -   해쉬와 정렬 모두 메모미를 많이 사용하기 때문에, 충분한 워킹 메모리가 확보되지 않으면 스왑이 발생한다(극단적 성능저하 발생)

### 2) 합쳐서 하나

-   문제 : 연령대 별로 가격이 다른 제품중에서 0 ~ 100세가 이용가능한 제품 고르기
-   hint : 각 범위의 상수를 합해 101인 제품 선택하기

```
SELECT product_id
		FROM PriceByAge
	GROUP BY product_id
	HAVING SUM(high_age - low_age + 1) = 101;
```

## ch 13. 자르기

-   집약 이외에도 중요한 자르기 라는 기능이 있다

### 1) 자르기와 파티션

```
SELECT SUBSTRING(name, 1, 1) AS label,
		COUNT(*)
	FROM Persons
	GROUP BY SUBSTRING(name, 1, 1);
```

-   GROUP BY 구로 잘라 만든 하나 하나의 부분 집합을 ‘파티션’이라고 한다.
    
-   나이로 자르기
    
```
    SELECT CASE WHEN age < 20 THEN '어린이'
        			WHEN age BETWEEN 20 AND 69 THEN '성인'
     			WHEN age >= 70 THEN '노인'
     			ELSE NULL END AS age_class,
     			COUNT(*)
     	FROM PERSONS
     	GROUP BY CASE WHEN age <= 20 THEN '어린이'
     				WHEN age BETWEEN 20 AND 69 THEN '성인'
     				WHEN age >= 70 THEN '노인'
     				ELSE NULL END;
```   
-   자르기의 기준을 GROUP BY와 SELECT 구에 모두 입력하는 것이 포인트
    
-   집약 함수, GROUP BY의 실행 계획은 성능적인 측면에서 해시에 사용되는 워킹 메모리 용량에 주의하는 것이 전부
