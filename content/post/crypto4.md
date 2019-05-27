---
title: "2017 암호경진대회 4번"
date: 2017-12-23T01:37:56+09:00
draft: false
tags: ["암호경진대회","리버싱"]
categories: ["암호경진대회"]
author: "Jaejin Jang"
---

가장 쉽게 풀었죠. 30분도 안걸렸던거 같네요.
프로그램을 통해서 암호문의 유효성이 검증되는데요, 역공학을 통해서 암호문을 쉽게 찾을 수 있습니다.

답안 :
Snow White and the Seven Dwarfs!.

풀이 :
Oracle함수가 CRC를 계산하기 위해서는 중간에 평문이 복구될 것으로 보였다. 제공하는 dll을 이용하여 Oracle함수의 입력으로 암호문을 넣은 프로그램을 작성하였다. IDA와 Immunity Debugger를 이용해 프로그램을 역공학하여 얻을 수 있었다.
