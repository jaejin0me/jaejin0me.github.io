---
title: "2017 암호경진대호 3번"
date: 2017-12-23T01:37:56+09:00
draft: false
tags: ["암호경진대회","부채널분석"]
categories: ["암호경진대회"]
author: "Jaejin Jang"
---

부채널 분석 문제인데요. 사전지식이 없는 쪽이라 마지막에 풀었는데 시간이 좀더 있었으면 했는데 아쉽네요. 푸신분 코드좀 주세용
matlab 코딩 실력이 좋았더라면..ㅜ

풀이:
참고문헌을 읽어 본 결과 CPA가 DPA에 보다 더 적은 샘플수로 분석이 가능 하다고 하여 전력 분석 중에서 CPA를 선택하였다. MSP430F2618보드의 경우 저전력의 특성의 IC로 CMOS구현되어 있다고 생각할 수 있었다. 그래서 Hamming Distance Model을 선택하였다. Hamming Distance Model에서 참고 되는 값은 보드에 구현된 ARIA의 소스코드를 분석하여 알 수 있었다.

소스 코드를 살펴보면, 변수 t가 초기에 0으로 설정된 후 라운드 키와 xor 연산 후 sbox를 거쳐 다시 저장되는 것을 확인할 수 있었다. 따라서 참고 되는 값을 0으로 잡았다. Hamming Distance Model을 선택하였지만 Hamming Weight Model과 동일한 값을 갖게 되었다.

```
void Crypt(const Byte *p, int R, const Byte *e, Byte *c)
{
int i, j;
Byte t[16]; // 변수 t가 할당되고 0으로 셋팅 될 것임.
for (j = 0; j < 16; j++) c[j] = p[j];
for (i = 0; i < R/2; i++)
{
for (j = 0; j < 16; j++) t[j] = S[ j % 4][e[j] ^ c[j]]; // 라운드키와 xor 연산후 sbox를 거쳐 변수 t에 저장됨.
??????????????
????????
?????
```

분석한 결과들을 바탕으로 Matlab을 이용하여 결과를 도출 하였으나, 유의미한 결과를 얻지 못하였다. 
