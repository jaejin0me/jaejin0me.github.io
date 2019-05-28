---
title: "프로그래머스 - 멀리 뛰기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

dp
dp[i] = dp[i-2] + dp[i-1]
```java
public class JumpCase {

    static int[] dp = new int[10000];

    public int jumpCase(int num) {
        int answer = 0;
        dp[0] = 1;
        dp[1] = 2;
        for(int i=2;i<num;i++) {
            dp[i] = dp[i-2]+dp[i-1];
        }

        return dp[num-1];
    }

    public static void main(String[] args) {
        JumpCase c = new JumpCase();
        int testCase = 4;
        //아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println(c.jumpCase(testCase));
    }
}
```
자연스레 dp로 풀었는데 재귀적으로 푼 코드가 있어서 풀이첨부
```java
class JumpCase {

    public int jumpCase(int num) {
        int answer = 0;

        if (num <= 2) return num;
                answer = jumpCase(num-1) + jumpCase(num-2);      

        return answer;
    }

    public static void main(String[] args) {
        JumpCase c = new JumpCase();
        int testCase = 2;
        //아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println(c.jumpCase(testCase));
    }
```
