---
title: "플러터 위젯 사용법"
date: 2021-04-19T00:12:52+09:00
tags: [
	"dart",
	"flutter",
	"플러터",
	"Doit플러터"
]
categories: ["flutter"]
author: "Jaejin Jang"
---
- 머티리얼 디자인을 통해 일관된 디자인을 제공할 수 있음
- 스캐폴드(scaffold) 클래스를 통해 머티리얼 디자인 레이아웃으로 개발 가능함

### Scaffold 클래스 더 활용해보기
- 이전 포스트에서 만들 었던 버튼누르면 글자바뀌는 위젯을 계속해서 사용하겠습니다.
- appBar 추가하기
- floatingActionButton 추가하기
- Row, Column 클래스 이용해 가로 또는 세로로 여러 위젯 추가하기

```dart
import 'package:flutter/material.dart';

class ChangeTextByButton extends StatefulWidget {
  @override
  State<StatefulWidget> createState() {
    print('createState()'); // 여기 출력!
    return _ChangeTextByButton();
  }
}

class _ChangeTextByButton extends State<ChangeTextByButton> {
  var swVal = false;
  var str = 'Jaejin Jang off';
  @override
  void initState() {
    super.initState(); // 여기 출력!
    print('initState()');
  }

  @override
  Widget build(BuildContext context) {
    print('build'); // 여기 출력!
    return MaterialApp(
      title: 'ChangeTextByButton',
      theme: ThemeData(
          primaryColor: Colors.blue,
          visualDensity: VisualDensity.adaptivePlatformDensity),
      darkTheme: ThemeData.light(),
      home: Scaffold(
        appBar: AppBar(
          // appbar 추가
          title: Text('App Bar 추가해보기'),
        ),
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
                    print('setState'); // 여기 출력!
                    swVal = value;
                    str = value ? 'Jaejin Jang On' : 'Jaejin Jang off';
                  });
                },
              ),
              Row(
                // 가로위젯 여러개 넣어보기
                mainAxisAlignment: MainAxisAlignment.center,
                children: [
                  Text(
                    '가로로 위젯 추가해보기 1',
                    style: TextStyle(color: Colors.orange, fontSize: 20),
                  ),
                  Icon(Icons.access_alarm),
                  Icon(Icons.access_time),
                  Icon(Icons.account_tree),
                  Icon(Icons.add_to_drive),
                  Icon(Icons.laptop),
                ],
              ),
            ])),
        floatingActionButton: FloatingActionButton(
          // 플로팅버튼 추가
          child: Icon(Icons.add),
          onPressed: () {},
        ),
      ),
    );
  }
}
```
