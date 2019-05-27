---
title: "ActiveX 란?"
date: 2017-12-26T02:08:34+09:00
draft: false
tags: ["cpp","Component Obect Model","ActiveX"]
categories: ["cpp"]
author: "Jaejin Jang"
---

이번 글에서는 InternetExplore에서 ActiveX를 퍼징하기 위한 관점에서 ActiveX에 대해서 살펴보겠습니다.

ActiveX 개요마이크로소프트에서 만든 COM(Component Object Model)과 OLE(Object Linkg and Embedding)를 합쳐 새로운 이름을 붙여 준것이다. 
엄밀하게는 ActiveX는 전반적인 기술을 의미하고, ActiveX컨트롤은 ActiveX를 이용해 만든 프로그램을 의미한다.

ActiveX 컨트롤각각의 ActiveX는 독립된 사용자 인터페이스를 가지는 COM 서버로 동작하며 주로 인터넷으로 배포되어 웹 브라우저를 통해 실행된다. 
애니메이션을 보여주거나, 은행업무에 사용되는 공인인증, 파일 관련 작업 등이 있다.어떠한 면에서는 자바 애플릿과 비슷하다. 
ActiveX나 자바 애플릿을 사용하여 웹 브라우저가 해당 프로그램을 다운로드하여 실행하게 한다. 차이점을 다음과 같다.

호환성의 차이 : 자바 애플릿은 대부분의 플랫폼에서 동작할 수 있지만, ActiveX의 경우 IE와 윈도에서만 동작한다. <br>
가능한 작업 제약의 차이 : ActiveX의 경우 자바 애플릿에 비해 코드 실행에 대한 제약이 적어서 보안에 취약하다.

ActiveX와 IE와의 역사IE 3.0에서부터 HTML에서 Object 태그를 이용해 ActiveX 관리할 수 있는 기능을 추가하였다. 
당시에는 사용자의 간섭없이 ActiveX를 자동으로 받아 설치하였다. 보안의 허점이 되면서 IE 5에서 부터 자동으로 설치되는 것이 막혔다. 
하지만 한 번 설치된 경우 웹페이지를 방문하는 경우 바로 실행되는 문제를 안고 있었다.IE6에서 자동 실행을 방지 하고 설치 방지를 개선했다. 
윈도 XP 서비스팩 2에서는 ActiveX 설치를 권장하는 팝업을 차단하는 기능과, 불분명한 ActiveX는 설치가 차단 되도록 하였다. 
윈도우 비스타에서는 사용자의 관리자 권한을 제한하면서 이론상으로는 해결되었으나, 
사용자들이 무조건적으로 "예" 만을 누르고, 관리자 권한을 제한하는 기능을 꺼버리면서 계속해서 문제가 되었다.
윈도우 10에 와서는 IE를 버리고 새로운 웹브라우저 Edge를 만들면서 ActiveX문제를 해결하고 있다.현재 ActiveX는 마이크로소프트에서 버린 기술이라고 할 수 있다.

![Fig](/cp2_1.jpg "HTML에서, ActiveX를 사용한 예")

참고<br>
https://ko.wikipedia.org/wiki/%EC%95%A1%ED%8B%B0%EB%B8%8CX <br>
https://namu.wiki/w/ActiveXhttp://blog.naver.com/PostView.nhn?blogId=comgghh&logNo=140176061963
