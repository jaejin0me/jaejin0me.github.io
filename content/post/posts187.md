---
title: "ftss patch for apache 2.4"
date: 2019-06-06T15:39:15+09:00
tags: ["apache 2.4", "ftss"]
categories: ["apache 2.4", "ftss"]
author: "Jaejin Jang"
mathjax: true
---

ftss 라는 아파치 웹 서버를 모니터링하는 명령어를 사용하고 있습니다.

mod_status 같은 모듈과는 다르게 웹서버 접속없이 쉘에서 바로 확인할 수 있어서 웹서버 부하시에 활용되는데요.

최근에 아파치를 2.4로 업그레이드하면서 ftss 명령어를 수행하면 아래의 에러메시지가 떴는데

ftss가 더이상 업데이트가 안되고 있어서 수정을 했습니다.

```
ftss: Wrong scoreboard size.
```

2.4 버전으로 오면서 구조체 크기를 계산하는 매크로가 생겼는데 ftss를 그걸활용하지 않아 사이즈가 맞지 않았고

포인터가 증가하지 않는 버그가 있어서 수정했습니다.

[ftss-0.9.3-apache_2.4.patch](https://github.com/jaejin0me/ftss-0.9.3-apache_2.4.patch)


#### 한국어

1. 구조체 크기 계산시에 매크로 활용
2. 포인터 증가하지 않는 버그 수정

#### English

1. fix structure size calculation
2. fix pointer doesn't increase
