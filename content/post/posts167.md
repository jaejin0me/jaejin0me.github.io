---
title: "파이썬의 subprocess 모듈을 이용해 COM 메소드 확인하기"
date: 2017-12-25T12:47:56+08:00
draft: false
tags: ["python","subprocess","powershell","Component Obect Model"]
categories: ["python"]
author: "Jaejin Jang"
---

[참고](http://hyunmini.tistory.com/105)

파워쉘을 이용해 COM의 메소드를 확인할 수 있습니다. 파이썬에서 subprocess 모듈을 이용해서 파워쉘을 실행시키고 결과를 얻어오는 코드입니다.

```
import subprocess

if __name__ == '__main__':
 clsid = "2E146AEF-5879-4310-BCC9-8094E3916613"  // CLSID 예
 powershell = subprocess.Popen(["powershell.exe","-command",
 "&{ $clsid = New-Object Guid '%s';$type = [Type]::GetTypeFromCLSID($clsid);$object= [Activator]::CreateInstance($ty    pe);get-member -inputObject $object; }" % clsid]
  ,stdout=subprocess.PIPE,stderr=subprocess.PIPE)  #출력을 처리하기 위해 subprocess로 보냄
result, err = powershell.communicate() # subprocess에 있는 출력을 가져옴
```
가져온 출력을 처리하여 사용하게 되는데, IDE에서 실행하여 디버깅하면서 확이인는 경우 출력이 콘솔에 뜨지 않을 수 있습니다.<br>
그때는 cmd나 파워쉘창을 열어서 실행하면 출력을 확인할 수 있습니다.
