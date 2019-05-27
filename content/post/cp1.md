---
title: "COM 이란?"
date: 2017-12-26T02:08:34+09:00
draft: false
tags: ["cpp","Component Obect Model"]
categories: ["cpp"]
author: "Jaejin Jang"
---

앞서 ActiveX는 COM과 OLE를 합친 기술라고 설명했습니다.이번에는 그 중에 하나인 COM에 대해서 알아 보겠습니다. 퍼징을 위해 필요한 정도만 알아볼 것 입니다.

## COM, OLE, ActiveXCOM
은 마이크로소프트가 만든 컴포넌트 모델이다. 컴포넌트 모듈의 형태와 컴포넌트를 사용하기 위한 방법을 표준화 한 것이다. 
COM은 윈도 운영체제의 기본 모듈로 포함되며 OLE나 ActiveX의 근간이 된다. OLE는 COM을 바탕으로 마이크로소프트가 만든 컴포넌트 기술이다.
이를 인터넷에서 동작하도록 수정한 것이 ActiveX이다.
## COM 컴포넌트 
모듈 형태COM 컴포넌트는 메소드와 프로퍼티를 갖는 객체이다. 이것은 실행 파일이나 DLL의 형태가 된다. 
실행 파일의 형태를 가지면 아웃프로세스(Out-Process) 컴포넌트라고 하여 DLL 형태를 가지면 인프로세스(In-Process)라고 한다.
아웃프로세스는 느리지만 안정성이 좋고, 인프로세스는 빠르지만 안정성이 떨어진다. 
그 이유는 아웃프로세스는 별도의 프로세스로 실행되어 사용시 주소 변환이 수반되기 때문에 속도는 느리지만, 문제가 생겨도 영향을 받지 않기 때문이다. 
인프로세스의 경우 그 반대가 된다.하나의 모듈 안에 둘 이상의 컴포넌트가 존재해도 무방하다.

## COM 클라이언트 (COM Client)
COM 클라이언트란, 서비스를 제공하는 COM 객체를 만들어 사용하는 프로그램입니다.

## COM 서버 (COM Server)
COM 서버란, COM 클라이언트에게 서비스를 제공하는 COM 객체들입니다.

## COM 컴포넌트 
레지스트리수많은 컴포넌트를 구분하기 위해 128비트의 숫자이름을 부여한다. 이것은 클래스 ID(CLSID)라고 부른다. 
CLSID를 사람이 다루기 쉽도록 만든 것이 ProgID이다. HKEY_CLASSED_ROOT/CLSID에서 설치된 모든 컴포넌트를 확인할 수가 있다.
아웃프로세스의 경우 LocalServer32라는 서브키에 컴포넌트의 절대 경로가 위치한다. 인프로세스의 경우 InProcServer32 이다.
컴포넌트를 사용하기 위해서는 레즈스트리에 등록을 해야 한다.인프로세스의 경우

* DllRegisterServer : 컴포넌트를 레지스트리에 등록
* DllUnregisterServer : 컴포넌트를 레지스트리에서 제거아웃프로세스의 경우
* 등록 : 프로그램을 실행시키면서 인자로 /RegServer
* 제거 : 프로그램을 실행시키면서 인자로 /RegUnServer

![Fig](/static/cp1_1.png "아웃프로세스 레지스트리 등록/제거")

## COM 컴포넌트 
인터페이스COM 컴포넌트의 메소드와 프로퍼티는 인터페이스로 구성된다. 즉 멤버함수로만 구성된다. 프로퍼티는 변수 처럼사용하지만 실제로 함수를 이용해 구현된다.
모든 COM 컴포넌트는 IUnkonwon이라는 최상위 인터페이스로 부터 상속된다. 이 인터페이스는 3개의 멤버 함수(AddRef, Release, QueryInterface)가 존재 한다. 
따라서 모든 COM 컴포넌트는 이 3개의 함수를 기본적으로 갖고 있다. 인터페이스 또한 구분을 위한 128비트의 ID를 갖는다. 
IID라고 부른다.<br>

AddRef : 컴포넌트에 대한 레퍼런스가 생길 때마다 사용자 프로그램에서는 그 컴포넌트의 AddRef 메소드를 호출하여 레퍼런스 카운트를 하나 증가시킨다.<br>
Release : 더 이상 그 컴포넌트를 사용할 일이 없어지면 Release 메소드를 호출. 
Release 메소드에서는 내부적으로 유지하는 레퍼런스 카운트가 0이 되면 자신을 메모리에서 제거한다.<br>
QueryInterface : 컴포넌트에게 갖고 있는 인터페이스를 달라고 요청하는데 사용된다. 첫 번째 인자로 원하는 IID, 두 번째 인자로 그 인터페이스에 대한 포인터를 
받을 변수의 주소를 지정해야 한다(포인터의 포인터). 즉, 컴포넌트에서 자신이 사용하고자 하는 인터페이스를 받아오는데 사용된다.<br>
예를 들어 GetMemorySize라는 메소드를 갖는 ISystemInfo라는 인터페이스를 구현한다고 하면, 모든 인터페이스는 앞에 I를 붙여 표시하는 것이 일반적이다. 
당연히 IUnknown으로부터 계승이 되어야할 한다.<br>

C++로 구현한 예<br>
class ISystemInfo : public IUnknown <br>
{ AddRef... Release... QueryInterface... // .... GetMemorySize... };<br>

IUnknown 이외에도 모든 컴포넌트는 COM에서 자신을 쉽게 생성할 수 있도록 하는 용도로 IClassFactory라는 인터페이스를 지원해야 한다. 
이 것 역시 IUnknown 인터페이스로부터 계승되어야함은 물론이다. 이 인터페이스의 목적은 요청된 컴포넌트를 COM에서 실제로 생성하는데 있다. 
이 인터페이스에는 CreateInstance와 LockServer 두 개의 멤버 함수가 존재한다. CreateInstance가 실제로 컴포넌트를 생성하는데 사용된다. 
그러면 잠깐 컴포넌트와 컴포넌트 사용자간의 상호 동작을 한 번 살펴보도록 하자.

1.어떤 프로그램에서 특정 컴포넌트를 사용하려면 그 컴포넌트의 ProgID나 CLSID를 지정해야 한다. 
예를 들어 MyCom.MyObj라는 이름을 갖는 컴포넌트(인터페이스 ISystemInfo를 가짐)가 있고 이를 사용하는 클라이언트가 VB로 작성되었다고 하자.<br>

Set MyObj = CreateObject("MyCom.MyObj")<br>

위의 코드에서 명시적으로 보이지는 않지만 프로그램이 시작할 때 COM 라이브러리를 초기화하기 위해 CoInitialize라는 초기화 함수가 호출된 상태이다. 
종료시에는 CoUninitialize라는 함수를 호출해야 한다.<br>

2.위에서처럼 ProgID를 지정하면 COM에서 이를 CLSID로 변경하여 사용한다. 그 다음으로 COM은 레지스트리에서 해당 CLSID 정보를 뒤져 컴포넌트의 위치를 찾는다. 
찾아서 이를 메모리로 로드한다. 이 작업은 SCM(Service Control Manager)라는 COM내의 모듈이 담당한다.<br>

3.컴포넌트가 들어있는 파일이 메모리에 로드되었으면 거기서 IClassFactory 인터페이스에 대한 포인터를 얻는다. 
그럴 목적으로 모든 컴포넌트는 자신의 익스포트 함수로 DllGetClassObject라는 함수를 구현해야 한다.<br>

DllGetClassObject : 운영체제가 컴포넌트를 생성하기 위해 해당 모듈의 IClassFactory 인터페이스에 대한 포인터를 얻어내는데 사용된다.

COM에서는 이를 통해 그 컴포넌트의 IClassFactory 인터페이스를 얻어낸 다음에 그것의 멤버 함수인 CreateInstance를 호출한다. 
그래서 실제로 컴포넌트의 객체를 생성하고 포인터를 얻어낸 다음에 그 포인터를 리턴한다.

4.해당 컴포넌트의 원하는 메소드나 프로퍼티를 사용한다.<br>

Dim iMem As Long iMem = MyObj.GetMemorySize() <br>

위에서처럼 COM 컴포넌트를 비주얼 베이직과 같은 RAD 개발 도구에서 사용할 수 있으려면 사실 그 컴포넌트에서 IDispatch라는 인터페이스도 구현해야 한다. 
요즘처럼 컴포넌트들이 대부분 RAD 환경하에서 사용되는 세상에서는 반드시 구현해주어야 할 인터페이스인 셈이다. 
사실 COM 인터페이스 자체는 포인터라는 개념하에 동작하도록 되어 있기 때문에 포인터를 모르는 대부분의 RAD 환경하에서는 사용할 수가 없다. 
그래서 COM에 추가된 것이 바로 IDispatch라는 인터페이스이다. 위의 절차를 다시 보면 내가 만들어야 하는 직접적인 ISystemInfo라는 인터페이스 이외에도 
IUnknown, IClassFactory, IDispatch 인터페이스를 별도로 구현해주어야 한다. 사실 이 인터페이스들은 누가 구현해도 내용이 비슷할 수 밖에 없는 부분들이기
때문에 ATL이나 MFC, VB 등의 상위 레벨의 개발 도구를 사용하면 알아서(?) 구현해준다. 하지만 직접 COM 컴포넌트를 만들려 한다면 일일이 다 코딩해주어야 한다.

참고<br>
http://blog.naver.com/xinfra/80007894227 <br>
https://www.google.co.kr/search?q=/regserver&newwindow=1&source=lnms&tbm=isch&sa=X&ved=0ahUKEwi7zq7K_8XXAhUTO7wKHVNJA4EQ_AUICygC&biw=1918&bih=949&dpr=1#imgrc=nzHUqbHFVtWtiM: <br>
http://blog.naver.com/PostView.nhn?blogId=netrance&logNo=110052088818 <br>
