---
title: "플러터 위젯의 생명주기"
date: 2021-04-16T00:05:52+09:00
tags: [
	"dart",
	"flutter",
	"플러터",
	"Doit플러터"
]
categories: ["flutter"]
author: "Jaejin Jang"
---

- StatelessWidget - 생명주기 없음
- StatefulWidget - 10단계의 생명주기를 가짐

![Fig](/posts198_1.png "posts198_1.png")
- 출처: https://www.raywenderlich.com/books/flutter-apprentice/v1.0.ea3/chapters/4-understanding-widgets

### 1. createState() - 상태생성
- StatefulWidget이 State를 생성하기 위해 무조건 호출해야 하는 함수

### 2. mounted == true - setState() 호출가능
- State가 생성되면 mounted property가 true로 설정됨
- 위젯을 제어할 수 있는 buildContext 클래스에 접근 가능해지며 setState()를 호출할 수 있게 된다.
- setState() 호출전에 mounted property를 체크하면 더 안전한 코드를 작성할 수 있다.

```dart
if (mounted) {
	setState();
}
```

### 3. initState() - 위젯초기화
- 위젯 초기화시 1번 호출한다.
- 위젯에 필요한 데이터를 준비하기 좋은 구간

### 4. didChangeDependencies() - 의존성 변경시 호출
- initState() 이후에 호출된다.
- 데이터에 의존하는 위젯이라면 화면에 표시하기 전에 꼭 호출해야함
- 상속받은 위젯을 사용할때 Super(Parent)가 변경되면 호출함

### 5. build() - 위젯 반환
- 위젯을 반환한다. 반환된 위젯이 렌더링되어 화면에 표시된다.

### 6. didUpdateWidget() - 위젯 갱신시
- Parent 위젯이나 데이터가 변경되어 위젯을 갱신해야 할 때 호출한다.
- 위젯이 생성되고 나서 갱신을 해야 할때는 initState()가 아니라 didUpdateWidget()을 호출해야 함

### 7. setState() - 상태 갱신시
- 데이터가 변경되었다는 것을 알리고 UI를 갱신함

### 8. deactivate() - 상태 관리 멈춰!
- State가 플러터의 구성 트리로부터 제거됨
- 따라서, 관리는 되지 않으나 메모리 해제까지 한것은 아니라 사용가능함

### 9. dispose() - 상태 관리 끝
- State를 소멸함
- 위젯을 없앨때 해줘야하는 작업이 있다면 여기에 작성
- deactivate() 된 위젯을 다른 트리에서 재사용하는 경우 dispose()하 호출되지 않을 수 있음

### 10. mounted == false - setState() 호출가능

### 생명주기 출력해보기

```dart
import 'package:flutter/material.dart';

class ChangeTextByButton extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    print('createState()');
    return _ChangeTextByButton();
  }
}

class _ChangeTextByButton extends State<ChangeTextByButton> {
  var swVal = false;
  var str = 'Jaejin Jang off';
  @override
  void initState() {
    super.initState();
    print('initState()');
  }

  void didChangeDependencies() {
    super.didChangeDependencies();
    print('didChangeDependencies()');
  }

  @override
  Widget build(BuildContext context) {
    print('build');
    return MaterialApp(
      title: 'ChangeTextByButton',
      theme: ThemeData(
          primaryColor: Colors.blue,
          visualDensity: VisualDensity.adaptivePlatformDensity),
      darkTheme: ThemeData.light(),
      home: Scaffold(
          body: Center(
              child: Column(
                  mainAxisAlignment: MainAxisAlignment.center,
                  children: <Widget>[
            Text(
              str,
              style: TextStyle(color: Colors.blue, fontSize: 20),
            ),
            Switch(
              value: swVal,
              onChanged: (value) {
                setState(() {
                  swVal = value;
                  str = value ? 'Jaejin Jang On' : 'Jaejin Jang off';
                });
              },
            ),
          ]))),
    );
  }
}

```
