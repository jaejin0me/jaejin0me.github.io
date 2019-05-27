---
title: "[책뿌수기 1탄 : 만들면서 배우는 OS커널의 구조와 원리] - ch1. 부트스트랩"
date: 2018-03-26T00:42:42+09:00
draft: true
tags: ["책뿌수기"]
categories: ["책뿌수기"]
author: "Jaejin Jang"
---

이것 저것 잡다하게 하다보니 하나를 정해서 해야겠다는 생각이 들어 책을 하나 파려고 합니다.

커널 관력 책을 복습할까 하다가, 새로운 책을 보는게 나을꺼 같아 고른책이 "만들면서 배우는 OS 커널의 구조와 원리" 입니다.

너무 늘어지지 않게 타이트하게 달려서 끝내보겠습니다.

# 시작하기 전에 준비해야 할 것들

NASM Win32 버전(환경변수 PATH에 디렉토리 경로 추가 필요), Rawwrite 0.7버전

# 1. OS없이 프로그램을 실행시킨다.

컴퓨터에게 전원이 들어온 후 하는 과정을 부트스트랩(bootstrap)이라고 합니다. 컴퓨터에 전원이 들어오면 CPU는 바이오스 ROM에 새겨진 프로그램을 실행시켜
현재 사용하는 마더보드의 상태를 체크하고 어떤 주변 장치가 접속되어 있는지 확인합니다.

바이오스 프로그램이 마지막에 하는 일은 현재 유저가 부팅에 사용할 것이라고 정해놓은 디스크의 첫 512바이트(MBR이라고 부릅니다)에 있는 프로그램을 읽어들여

실행하는 것 입니다. 이 MBR을 아래와 같이 작성합니다.

```
[org 0] // 프로그램의 시작 메모리번지수
[bits 16] // 16비트 기반이라고 알려줌
	jmp 0x07c0:start

	start:
		mov ax, cs
		mov ds, ax
		mov ax, 0xB800
		mov es, ax
		mov di, 0
		mov ax, word [msgBack]
		mov cx, 0x7FF

	paint:
		mov word [es:di], ax
		add di, 2
		dec cx
		jnz paint

		mov edi, 0
		mov byte [es:edi], 'A'
		inc edi
		mov byte [es:edi], 0x06
		inc edi
		mov byte [es:edi], 'B'
		inc edi
		mov byte [es:edi], 0x06
		inc edi
		mov byte [es:edi], 'C'
		inc edi
		mov byte [es:edi], 0x06
		inc edi
		mov byte [es:edi], '1'
		inc edi
		mov byte [es:edi], 0x06
		inc edi
		mov byte [es:edi], '2'
		inc edi
		mov byte [es:edi], 0x06
		inc edi
		mov byte [es:edi], '3'
		inc edi
		mov byte [es:edi], 0x06
		jmp $

		msgBack db '.', 0xE7

		times 510-($-$$) db 0
		dw 0xAA55
```

그리고 nasm을 이용해 바이너리 파일로 컴파일 합니다

```
//바이너리로 컴파일 하는 옵션과 아웃풋 파일의 이름을 지정해 주는 옵션을 지정
>nasm -b bin -o boot.bin boot.txt
```

이렇게 하면 MBR이 완성됩니다. 책에서는 플로피디스크에 rawwite를 이용하여 바이너리를 기록하라고 하지만 요즘에 플로피디스크 달려있는 컴퓨터도 없고, 달아서 기록하고 실제 컴퓨터를
재부팅해가면서 하기에는 번거롭기만 합니다.

저는 vmware에서 가상이미지를 만들어 플로피디스크 드라이버를 추가한후에, 이미지 파일을 선택해서 해당 파일로 부팅하게끔 설정하여 진행하고 있습니다. 여러 방법들을 찾아봤지만 이 방법이 가장 편합니다.

정상적으로 동작한다면 아래와 같은 그림이 나옵니다.

![Fig](/static/img11.png "bb1_1.png")

# 1. boot.txt 프로그램 소스의 해설

아래의 코드는 boot.bin 을 디스어셈블했을때의 코드 입니다.

```
//디스어셈블 하는 방법, 16비트 명령어로 디스어셈블 옵션
>disasm -b16 boot.bin > disasm.txt
```

```
00000000  EA0500C007        jmp word 0x7c0:0x5
00000005  8CC8              mov ax,cs
00000007  8ED8              mov ds,ax
00000009  B800B8            mov ax,0xb800
0000000C  8EC0              mov es,ax
0000000E  BF0000            mov di,0x0
00000011  A17C00            mov ax,[0x7c]
00000014  B9FF07            mov cx,0x7ff
00000017  268905            mov [es:di],ax
0000001A  81C70200          add di,0x2
0000001E  49                dec cx
0000001F  75F6              jnz 0x17
00000021  66BF00000000      mov edi,0x0
00000027  2667C60741        mov byte [es:edi],0x41
0000002C  6647              inc edi
0000002E  2667C60706        mov byte [es:edi],0x6
00000033  6647              inc edi
00000035  2667C60742        mov byte [es:edi],0x42
0000003A  6647              inc edi
0000003C  2667C60706        mov byte [es:edi],0x6
00000041  6647              inc edi
00000043  2667C60743        mov byte [es:edi],0x43
00000048  6647              inc edi
0000004A  2667C60706        mov byte [es:edi],0x6
0000004F  6647              inc edi
00000051  2667C60731        mov byte [es:edi],0x31
00000056  6647              inc edi
00000058  2667C60706        mov byte [es:edi],0x6
0000005D  6647              inc edi
0000005F  2667C60732        mov byte [es:edi],0x32
00000064  6647              inc edi
00000066  2667C60706        mov byte [es:edi],0x6
0000006B  6647              inc edi
0000006D  2667C60733        mov byte [es:edi],0x33
00000072  6647              inc edi
00000074  2667C60706        mov byte [es:edi],0x6
00000079  E9FDFF            jmp word 0x79
0000007C  2EE700            cs out 0x0,ax
0000007F  0000              add [bx+si],al
00000081  0000              add [bx+si],al
00000083  0000              add [bx+si],al
00000085  0000              add [bx+si],al
00000087  0000              add [bx+si],al
00000089  0000              add [bx+si],al
0000008B  0000              add [bx+si],al
0000008D  0000              add [bx+si],al
0000008F  0000              add [bx+si],al
00000091  0000              add [bx+si],al
00000093  0000              add [bx+si],al
00000095  0000              add [bx+si],al
00000097  0000              add [bx+si],al
00000099  0000              add [bx+si],al
0000009B  0000              add [bx+si],al
0000009D  0000              add [bx+si],al
0000009F  0000              add [bx+si],al
000000A1  0000              add [bx+si],al
000000A3  0000              add [bx+si],al
000000A5  0000              add [bx+si],al
000000A7  0000              add [bx+si],al
000000A9  0000              add [bx+si],al
000000AB  0000              add [bx+si],al
000000AD  0000              add [bx+si],al
000000AF  0000              add [bx+si],al
000000B1  0000              add [bx+si],al
000000B3  0000              add [bx+si],al
000000B5  0000              add [bx+si],al
000000B7  0000              add [bx+si],al
000000B9  0000              add [bx+si],al
000000BB  0000              add [bx+si],al
000000BD  0000              add [bx+si],al
000000BF  0000              add [bx+si],al
000000C1  0000              add [bx+si],al
000000C3  0000              add [bx+si],al
000000C5  0000              add [bx+si],al
000000C7  0000              add [bx+si],al
000000C9  0000              add [bx+si],al
000000CB  0000              add [bx+si],al
000000CD  0000              add [bx+si],al
000000CF  0000              add [bx+si],al
000000D1  0000              add [bx+si],al
000000D3  0000              add [bx+si],al
000000D5  0000              add [bx+si],al
000000D7  0000              add [bx+si],al
000000D9  0000              add [bx+si],al
000000DB  0000              add [bx+si],al
000000DD  0000              add [bx+si],al
000000DF  0000              add [bx+si],al
000000E1  0000              add [bx+si],al
000000E3  0000              add [bx+si],al
000000E5  0000              add [bx+si],al
000000E7  0000              add [bx+si],al
000000E9  0000              add [bx+si],al
000000EB  0000              add [bx+si],al
000000ED  0000              add [bx+si],al
000000EF  0000              add [bx+si],al
000000F1  0000              add [bx+si],al
000000F3  0000              add [bx+si],al
000000F5  0000              add [bx+si],al
000000F7  0000              add [bx+si],al
000000F9  0000              add [bx+si],al
000000FB  0000              add [bx+si],al
000000FD  0000              add [bx+si],al
000000FF  0000              add [bx+si],al
00000101  0000              add [bx+si],al
00000103  0000              add [bx+si],al
00000105  0000              add [bx+si],al
00000107  0000              add [bx+si],al
00000109  0000              add [bx+si],al
0000010B  0000              add [bx+si],al
0000010D  0000              add [bx+si],al
0000010F  0000              add [bx+si],al
00000111  0000              add [bx+si],al
00000113  0000              add [bx+si],al
00000115  0000              add [bx+si],al
00000117  0000              add [bx+si],al
00000119  0000              add [bx+si],al
0000011B  0000              add [bx+si],al
0000011D  0000              add [bx+si],al
0000011F  0000              add [bx+si],al
00000121  0000              add [bx+si],al
00000123  0000              add [bx+si],al
00000125  0000              add [bx+si],al
00000127  0000              add [bx+si],al
00000129  0000              add [bx+si],al
0000012B  0000              add [bx+si],al
0000012D  0000              add [bx+si],al
0000012F  0000              add [bx+si],al
00000131  0000              add [bx+si],al
00000133  0000              add [bx+si],al
00000135  0000              add [bx+si],al
00000137  0000              add [bx+si],al
00000139  0000              add [bx+si],al
0000013B  0000              add [bx+si],al
0000013D  0000              add [bx+si],al
0000013F  0000              add [bx+si],al
00000141  0000              add [bx+si],al
00000143  0000              add [bx+si],al
00000145  0000              add [bx+si],al
00000147  0000              add [bx+si],al
00000149  0000              add [bx+si],al
0000014B  0000              add [bx+si],al
0000014D  0000              add [bx+si],al
0000014F  0000              add [bx+si],al
00000151  0000              add [bx+si],al
00000153  0000              add [bx+si],al
00000155  0000              add [bx+si],al
00000157  0000              add [bx+si],al
00000159  0000              add [bx+si],al
0000015B  0000              add [bx+si],al
0000015D  0000              add [bx+si],al
0000015F  0000              add [bx+si],al
00000161  0000              add [bx+si],al
00000163  0000              add [bx+si],al
00000165  0000              add [bx+si],al
00000167  0000              add [bx+si],al
00000169  0000              add [bx+si],al
0000016B  0000              add [bx+si],al
0000016D  0000              add [bx+si],al
0000016F  0000              add [bx+si],al
00000171  0000              add [bx+si],al
00000173  0000              add [bx+si],al
00000175  0000              add [bx+si],al
00000177  0000              add [bx+si],al
00000179  0000              add [bx+si],al
0000017B  0000              add [bx+si],al
0000017D  0000              add [bx+si],al
0000017F  0000              add [bx+si],al
00000181  0000              add [bx+si],al
00000183  0000              add [bx+si],al
00000185  0000              add [bx+si],al
00000187  0000              add [bx+si],al
00000189  0000              add [bx+si],al
0000018B  0000              add [bx+si],al
0000018D  0000              add [bx+si],al
0000018F  0000              add [bx+si],al
00000191  0000              add [bx+si],al
00000193  0000              add [bx+si],al
00000195  0000              add [bx+si],al
00000197  0000              add [bx+si],al
00000199  0000              add [bx+si],al
0000019B  0000              add [bx+si],al
0000019D  0000              add [bx+si],al
0000019F  0000              add [bx+si],al
000001A1  0000              add [bx+si],al
000001A3  0000              add [bx+si],al
000001A5  0000              add [bx+si],al
000001A7  0000              add [bx+si],al
000001A9  0000              add [bx+si],al
000001AB  0000              add [bx+si],al
000001AD  0000              add [bx+si],al
000001AF  0000              add [bx+si],al
000001B1  0000              add [bx+si],al
000001B3  0000              add [bx+si],al
000001B5  0000              add [bx+si],al
000001B7  0000              add [bx+si],al
000001B9  0000              add [bx+si],al
000001BB  0000              add [bx+si],al
000001BD  0000              add [bx+si],al
000001BF  0000              add [bx+si],al
000001C1  0000              add [bx+si],al
000001C3  0000              add [bx+si],al
000001C5  0000              add [bx+si],al
000001C7  0000              add [bx+si],al
000001C9  0000              add [bx+si],al
000001CB  0000              add [bx+si],al
000001CD  0000              add [bx+si],al
000001CF  0000              add [bx+si],al
000001D1  0000              add [bx+si],al
000001D3  0000              add [bx+si],al
000001D5  0000              add [bx+si],al
000001D7  0000              add [bx+si],al
000001D9  0000              add [bx+si],al
000001DB  0000              add [bx+si],al
000001DD  0000              add [bx+si],al
000001DF  0000              add [bx+si],al
000001E1  0000              add [bx+si],al
000001E3  0000              add [bx+si],al
000001E5  0000              add [bx+si],al
000001E7  0000              add [bx+si],al
000001E9  0000              add [bx+si],al
000001EB  0000              add [bx+si],al
000001ED  0000              add [bx+si],al
000001EF  0000              add [bx+si],al
000001F1  0000              add [bx+si],al
000001F3  0000              add [bx+si],al
000001F5  0000              add [bx+si],al
000001F7  0000              add [bx+si],al
000001F9  0000              add [bx+si],al
000001FB  0000              add [bx+si],al
000001FD  0055AA            add [di-0x56],dl
```

첫번째 열은 기계어 코드가 담긴 실제 메모리번지이고, 두번째 열은 명령어입니다. 세번째는 명령어가 어셈블리로 해석된 것 입니다.

배경 지식으로 Real Mode와 Protected Mode를 알아야 합니다. 그리고 세그먼트와 오프셋을 알아야 합니다.

## 리얼 모드

리얼 모드란 컴퓨터에 전원이 들어온 후 CPU가 처음 움직이기 시작하면서 활동하는 모드입니다. MS-DOS같은 경우는 부팅부터 종료까지 리얼모드로 동작합니다.
리얼 모드에서는 프로그램이 한 번에 한 개씩밖에 동작하지 못합니다. 하나의 프로그램이 동작을 마친 후에야 다른 프로그램의 실행을 시작할 수 있습니다. 그리고 한 프로그램은 현재 컴퓨터가 가지고 있는 램의
모든 영역을 자기 마음대로 사용할 수 있습니다. 옛날에 사용하던 모드로 반복문에 잘못 걸리면 사람이 인터럽트를 걸어 멈추지 않는 이상 다른 프로그램은 실행될 수도 없었으며, 램 영역의 모든 부분을
접근가능 하기 때문에 시스템이 망가지는 경우가 허다 했습니다. 현재에는 사용되지 않지만, 8086 CPU용읜 DOS프로그램을 계속 사용할 수 있도록 하기 위한 하위호환성 문제 때문에 남겨져 있었으나 win10부터는 완전히 사라졌습니다.
아무튼 근래까지만 해도 컴퓨터에 전원이 들어오면 리얼 모드에서 여러 가지 하드웨어 설정을 확인하고 프로젝티드 모드로 CPU를 전환했습니다.

## 프로텍티드 모드

프로텍티드 모드란 윈도나 리눅스에서 동작하는 모드입니다. 이 모드에서는 모든 프로그램이 한꺼벅에 동작합니다. 유저 모드와 커널 모드로 나눠지는데, 프로그램들이 실행되는 도중에 정해진 루틴에 의해 커널 모드에 진입하면서
프로그램이 멈추어지게 되고 다른 프로그램이 커널에 의해 다시 실행되는 방식으로 여러 프로그램이 조금씩 실행됩니다. 이 동작은 매우 빠르게 실행되기 때문에 사람에게는 한꺼번에 실행되는 것 처럼 보입니다.
그리고 각 프로그램이 사용할 수 있는 램의 영역도 커널에 의해 정해지고 되고, 제어나 사용을 위해서는 커널 모드로 진입해 요청하고 할당을 받아야 사용할 수 있습니다. 유저 모드 프로그램에서는 정해진 루틴 외에는 다른곳에
접근 할 수도 없습니다. CPU의 사용효율과 안정성을 더 높인 방식이 프로텍티드 모드입니다.

## 세그먼트와 오프셋

앞서 컴퓨터에 전원이 들어오면 바이오스 롬에 새겨진 프로그램을 실행하여 여러 가지 일을 하고 마지막에 디스크의 첫 512바이트(1섹터)를 램으로 읽어들인다고 했습니다. 이 과정에서 바이오스 프로그램은 MBR을 램의
0x07c00번지에 복사합니다.

일반적으로 0x07c00:0000, 0x0000:07c00, 0x0700:0c00 등 많은 방법으로 표현할 수 있습니다. 여기서 0x07c00를 물리 주소라고 부르고, 0x07c00:0000 등을 논리 주소라고 부릅니다. 물리 주소는 CPU의 RAM의 연결선인 어드레스 버스에
실제로 나타나는 전기 신호입니다.

논리 주소는 프로그래머가 소스를 작성할 떄와 컴파일러가 이 소스를 기계어로 컴파일했을 때의 결과물인 기계어에서 사용됩니다. 이 기계어가 CPU에서 실행될때 CPU의 하드웨어적인 펌웨어가 논리 주소를 물리 주소로 변환시켜서 사용합니다.

변환 시킬 때에는 세그먼트에 있는 값에 16을 곱하여 거기에 오프셋을 더합니다.

따라서 0x07c0:0000은 다음과 같이 계산됩니다.

0x7c00 + 0x0000 = 0x7c00 = 0x07c00

앞에 붙어있는 0은 아무런 의미가 없습니다. 보기좋게 자리수를 맞춘 것일뿐입니다. 나머지 논리 주소들도 계산하면 동일한 물리주소를 갖게 됩니다.

이러한 주소 지정 방식이 리얼 모드에서 사용하는 주소 지정방식입니다. 프로텍티드 모드에서는 다른 방식을 사용합니다.
