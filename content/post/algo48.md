---
title: "프로그래머스 - 2 x n 타일링"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

주의해야 할것은 오버플로우! 나머지 연산이 포인트
```java
public class TryHelloWorld {

    static long[] dp = new long[10000];

    public int tiling(int n) {
        dp[0] = 1;
        dp[1] = 2;
        for(int i=2;i<n;i++) {
            dp[i] = (dp[i-2]*2)+(dp[i-1]-dp[i-2]);
            if(dp[i]>99999)
                dp[i]%=100000;
    }
        return (int) dp[n-1];
    }

    public static void main(String args[]) {
        TryHelloWorld tryHelloWorld = new TryHelloWorld();
        // 아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.print(tryHelloWorld.tiling(262));
    }}
```
