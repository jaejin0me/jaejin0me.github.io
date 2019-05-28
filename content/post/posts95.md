---
title: "책뿌수기 - 기초가 든든한 데이터 베이스 5"
date: 2018-11-29T00:17:56+09:00
draft: false
tags: ["database"]
categories: ["database"]
author: "Jaejin Jang"
---

*인용하는 그림은 다양한 곳에서 가져왔음을 밝힙니다*

# Ch 5. SQL Server 설치 및 예제 데이터 베이스 구축

## Section 2. 예제 데이터베이스 구축

* 데이터베이스는 시스템 DB와 사용자 DB로 나뉜다.

| 시스템 데이터베이스 | 역할 |
| :-------- | :-------- |
| master 데이터베이스 | 시스템 정보, 초기값, 설정 기록 |
| tempdb 데이터베이스 | 임시 테이블, 임시 저장 프로시저, 임시 저장공간 |
| model 데이터베이스 | 생성되는 DB에 대한 템플릿 |
| msdb 데이터베이스 | SQL Server 에이전트에서 알림과 작업 예약에 사용 |

![Fig](/posts95_1.jpg "posts95_1.jpg")
