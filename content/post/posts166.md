---
title: "CLSID를 이용한 COM 메서드 확인"
date: 2017-12-25T12:47:56+08:00
draft: false
tags: ["powershell","Component Obect Model"]
categories: ["powershell"]
author: "Jaejin Jang"
---

HTML에서는 ActiveX(COM)를 부를 때 CLSID를 사용합니다.웹 페이지상에서 소스 보기를 이용해서 CLSID를 볼 수 있기 때문에, 
구해진 CLSID로 Powershell을 통해 메소드를 확인할 수 있습니다.
ActiveX뿐만 아니라 COM 객체는 모두 확인할 수 있습니다.

```
$type = [Type]::GetTypeFromCLSID("8E974B5A-A286-37DA-94F8-5872C500EB0E")$obj = [Activator]::CreateInstance($type)$obj | get-member
```

![Fig](/posts166_1.jpg "posts166_1.jpg")

참고[MSDN](https://msdn.microsoft.com/en-us/library/ms952644.aspxhttp://hyunmini.tistory.com/105https://www.vistax64.com/powershell/60455-can-i-use-uuid-new-object-comobject.html)
