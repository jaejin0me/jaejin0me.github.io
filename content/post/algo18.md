---
title: "int 배열 오름차순 정렬을 이용해 내림차순 쉽게 구하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

타입에러 때문에 제공하는 정렬함수를 쓸 수 없을때 그리고 형변환 하기 귀찮을 때<br>
comparator, comparable의 재정의 없이 간단하게 정렬할 수 있는 방법입니다.

```
//예; sort라는 오름차순정렬 함수가 정의된 경우
int[] B = ~~~~~~
//오름차순 정렬
sort(B);

//내림차순 정렬
for(int i=0;i<B.length;i++)
  B[i] = -B[i]

sort(B);

for(int i=0;i<B.length;i++)
  B[i] = -B[i]
```
