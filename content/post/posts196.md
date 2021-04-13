---
title: "Dart 연습"
date: 2021-04-13T19:05:52+09:00
tags: [
	"dart",
	"flutter",
	"플러터",
	"Doit플러터"
]
categories: ["flutter"]
author: "Jaejin Jang"
---

- 구구단

```dart
void main() {
  for (int i = 2; i < 10; i++) {
    for (int j = 1; j < 10; j++) {
      print('${i} * ${j} = ${i * j}');
    }
  }
}
```

- 자동차 클래스

```dart
class Car {
  int maxSpeed;
  num price;
  String name;

  Car(int maxSpeed, num price, String name) {
    this.maxSpeed = maxSpeed;
    this.price = price;
    this.name = name;
  }

  num saleCar() {
    this.price *= 0.9;
    return this.price;
  }
}

void main() {
  Car bmw = new Car(320, 100000, 'BMW');
  Car benz = new Car(250, 70000, 'BENZ');
  Car ford = new Car(200, 80000, 'FORD');

  bmw.saleCar();
  bmw.saleCar();
  bmw.saleCar();

  print(bmw.price);
}
```

- 로또 번호 생성

```dart
import 'dart:math';

void main() {
  Random r = Random();
  Set<int> s = {};

  while (s.length != 6) {
    s.add(r.nextInt(45) + 1);
  }

  print(s);
}
```