---
title: "플러터 이미지와 폰트 추가하기"
date: 2021-04-20T23:12:52+09:00
tags: [
	"dart",
	"flutter",
	"플러터",
	"Doit플러터"
]
categories: ["flutter"]
author: "Jaejin Jang"
---

### 이미지 추가하기
- 적당한 폴더에 이미지를 저장한다.
- pubspec.yaml 의 aseets 에 이미지를 추가한다.
- 이미지를 호출하는 방법은 크게 3가지
  - file : 외부의 파일을 사용
  - asset : 앱에 미리 넣어놓은 파일 사용(지금 사용하는 방식)
  - memory : 메모리에 올라와있는 이미지 데이터 사용

```yaml
flutter:

  # The following line ensures that the Material Icons font is
  # included with your application, so that you can use the icons in
  # the material Icons class.
  uses-material-design: true

  # To add assets to your application, add an assets section, like this:
  assets:
    - image/dog.jpg
```


### 폰트 추가하기
- 적당한 폴더에 폰트를 저장한다.
- pubspec.yaml 의 fonts 에 이미지를 추가한다.
- 폰트파일이름은 영어만 가능하다.

```yaml
  fonts:
    - family: Pacifico
      fonts:
        - asset: font/Pacifico-Regular.ttf
```

### 위젯에서 이미지와 폰트 사용해보기
- 이전 포스트에서 사용한 위젯에서 추가한 이미지와 폰트를 사용해봅니다.

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
                style: TextStyle(
                    fontFamily: 'Pacifico', color: Colors.blue, fontSize: 20), //내가 추가한 폰트 지정!
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
              Image.asset('image/dog.jpg', // 그림 나타내기!
                  height: 300,
                  width: 400,
                  fit: BoxFit.contain),
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