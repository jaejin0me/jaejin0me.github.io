---
title: "windbg 사용법 정리"
date: 2017-12-25T12:47:56+08:00
draft: false
tags: ["security","windbg"]
categories: ["security"]
author: "Jaejin Jang"
---

제가 참고할 용도로 작성하고, 계속 추가해 나갈 예정입니다.

3. 로드되지 않은 심볼에 breakpoint 설정
특정 모듈이 로드되기 전에 모듈 내부에 bp를 걸고싶을때 사용  
bu xxx!print+0x1f3 // xxx모듈의 print 함수 +0x1f3 위치에 bp

2. 특정 모듈(dll,ocx) 로드시 breakpoint 설정
모듈이 로드되기 전에는 모듈내부에 bp를 걸수가 없어서, 모듈을 로드할때 bp를 걸고 이후에 모듈의 함수나 주소에 bp를 걸어서 디버깅하고 있습니다.  
sxe ld abc.dll    // abc 모듈을 로드하는 시점에 bp가 걸림

1. batch파일에서 심볼을 로드하는 명령어디버깅 환경설정의 편의를 위해 batch파일을 만들때 , 심볼을 로드하게 하는 스크립트입니다.
```
"C:\Program Files\Windows Kits\8.1\Debuggers\x86\windbg.exe" -y"SRV*c:\\symbols*http://msdl.microsoft.com/download/symbols" -c".load c:\\blwdbgue.dll;g" "C:\Program Files\Internet Explorer\iexplore.exe"
```

설명  
```
-y"SRV*c:\\symbols*http://msdl.microsoft.com/download/symbols" 
```
windbg 의 사용법을 찾아본 결과 심볼을 로드하기 위해서는 -c 옵션에서 커맨드는 넣는게 아니라 심볼관련 옵션인 -y가 존재합니다. 
심볼서버를 사용하겠다는 뜻이며 뒤의 주소가 심볼서버의 주소 입니다. 주소에서 다운받는 심볼정보들은 C:\symbols 폴더에 저장되게 됩니다.
