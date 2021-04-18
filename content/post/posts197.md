---
title: "플러터 기본적인 위젯 사용해보기"
date: 2021-04-13T20:05:52+09:00
tags: [
	"dart",
	"flutter",
	"플러터",
	"Doit플러터"
]
categories: ["flutter"]
author: "Jaejin Jang"
---

## 사용하는 위젯 설명
> MaterialApp - Material 디자인 제공  
> Scaffold - 기본적인 Material 디자인 레이아웃 제공  
> Center - 상하 가운데 정렬  
> Swich - 토글스위치 제공  
> Text - 텍스트  
> Column - chlidren 속성이 있어 여러개의 하위 위젯을 배치가능


### 글자 표시하는 위젯

```dart
import 'package:flutter/material.dart';

class PrintText extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'PrintText',
      theme: ThemeData(
          primaryColor: Colors.blue,
          visualDensity: VisualDensity.adaptivePlatformDensity),
      darkTheme: ThemeData.light(),
      home: Scaffold(
        body: Center(
          child: Text(
            'Here is for Text',
            textAlign: TextAlign.center,
            style: TextStyle(color: Colors.blue, fontSize: 20),
          ),
        ),
      ),
    );
  }
}
```

- StatelessWidget
  - MaterialApp
    - Scaffold 
      - Center 
        - Text 

### 토글스위치 위젯

```dart
import 'package:flutter/material.dart';

class ToggleSwitch extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return _ToggleSwitch();
  }
}

class _ToggleSwitch extends State<ToggleSwitch> {
  var swVal = false;
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'ToggleSwitch',
      theme: ThemeData(
          primaryColor: Colors.blue,
          visualDensity: VisualDensity.adaptivePlatformDensity),
      darkTheme: ThemeData.light(),
      home: Scaffold(
        body: Center(
          child: Switch(
            value: swVal,
            onChanged: (value) {
              setState(() {
                swVal = value;
              });
            },
          ),
        ),
      ),
    );
  }
}
```

- StatefulWidget
  - createState()
- State
  - MaterialApp
    - Scaffold 
      - Center 
        - Switch
          - onChanged()
            - setState()


### 버튼누르면 글자바뀌는 위젯

```dart
import 'package:flutter/material.dart';

class ChangeTextByButton extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    return _ChangeTextByButton();
  }
}

class _ChangeTextByButton extends State<ChangeTextByButton> {
  var swVal = false;
  var str = 'Jaejin Jang off';
  @override
  Widget build(BuildContext context) {
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

- StatefulWidget
  - createState()
- State
  - MaterialApp
    - Scaffold 
      - Center 
        - Column 
          - Text
          - Switch
		    - onChanged()
		    - setState()