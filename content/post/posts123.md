---
title: "자바 클래스 관련 정리"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["java"]
categories: ["java"]
author: "Jaejin Jang"
---

문제를 풀다가 틀린 부분이나 새로 알게된 것들을 기술합니다. 시간날때 깔끔하게 정리하겠습니다.

매개변수가 없는 기본 생성자를 작성할 때, 매개변수가 있는 생성자를 사용하는 것이 재사용성이 좋다.

변수의 종류에는 인스턴스 변수, 클래스 변수, 지역변수가 있다.

생성자는 객체의 생성이 아니라 초기화의 목적이다.

초기화 블럭이 생성자보다 앞선다.

인스턴스메서드는 Heap에 할당된다.

초기화 과정에서 static 변수가 인스턴스 변수로 초기화 될 수 없다.

매개변수를 받는 생성자만 정의하고 기본생성자를 부를 경우 err를 일으킨다.

접근 지시자의 범위 순서는 public - protected(다른 패키지도 가능) - default(같은 패키지 내에만) - private이다

자바에서 final은 다시 할당 될 수 없음을 의미한다. const와 약간 다르다.

조상 타입의 인스턴스를 자손 타입으로 변환 할 수 없다. 반대는 가능

instanceof 는 인스턴스의 조상이나 구현한 인터페이스에 대해서만 true를 반환한다.

내부 클래스에서 변수는 가려지지만, 메소드는 실제 인스턴스의 메소드를 부른다.

매개변수 타입의 인터페이스로 올 수 있는 것은 null, 해당 인터페이스를 구현한 클래스 또는 그 자손 클래스이다.