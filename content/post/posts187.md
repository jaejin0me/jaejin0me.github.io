---
title: "ftss patch for apache 2.4"
date: 2019-06-06T15:39:15+09:00
tags: ["apache 2.4", "ftss"]
categories: ["apache 2.4", "ftss"]
author: "Jaejin Jang"
mathjax: true
---

~~ftss 라는 아파치 웹 서버를 모니터링하는 명령어를 사용하고 있습니다.~~

~~mod_status 같은 모듈과는 다르게 웹서버 접속없이 쉘에서 바로 확인할 수 있어서 웹서버 부하시에 활용되는데요.~~

~~최근에 아파치를 2.4로 업그레이드하면서 ftss 명령어를 수행하면 아래의 에러메시지가 떴는데~~

~~ftss가 더이상 업데이트가 안되고 있어서 수정을 했습니다.

```
ftss: Wrong scoreboard size.
```

~~2.4 버전으로 오면서 구조체 크기를 계산하는 매크로가 생겼는데 ftss를 그걸활용하지 않아 사이즈가 맞지 않았고~~

~~포인터가 증가하지 않는 버그가 있어서 수정했습니다.~~

~~[ftss-0.9.3-apache_2.4.patch](https://github.com/jaejin0me/ftss-0.9.3-apache_2.4.patch)~~


#### ~~한국어~~

1. ~~구조체 크기 계산시에 매크로 활용~~
2. ~~포인터 증가하지 않는 버그 수정~~

#### ~~English~~

1. ~~fix structure size calculation~~
2. ~~fix pointer doesn't increase~~

---
(2019.06.13 업데이트)

패치한걸 contribute 하고 싶어서 메일을 보냈었습니다.

답장이 왔는데.. 내용은 이미 패치가 있다는 이야기네요 ㅎㅎ

공식 저장소(아마도 아파치)에 접근 권한이 이제 없어서, 다른 저장소에서 유지하고 있다고 하네요.

구글에서 대강 검색해봤을때 없는줄알았더니 이렇게 숨어있을줄이야..

패치된 버전을 보니 그래도 제 코드보다는 좋네요. 역시 오리지널개발자네요.

아무튼, 오픈소스에 기여는 못했지만 ftss의 author에게 리뷰도 받아보고 좋은 경험이였습니다.

![Fig](/posts187_1.jpg "posts187_1.jpg")
