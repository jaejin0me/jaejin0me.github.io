---
title: "프로그래머스 - 최대공약수와 최소공배수"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

유클리트 호제법으로 gcd를 구하고, gcd를 이용해 lcm을 구했다. 포인트는 gcd를 이용해 lcm을 구하는 것이다.
```
import java.util.Arrays;

class TryHelloWorld {


    public int gcd(int a,int b) {
        if(a==0){
            return b;           
        }
        else {
            return gcd(b%a,a);
        }

    }

    public int[] gcdlcm(int a, int b) {

        int gcd = gcd(a,b);
        int lcm = a*b/gcd;
        int[] answer = new int[2];
        answer[0] = gcd;
        answer[1] = lcm;

        return answer;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        TryHelloWorld c = new TryHelloWorld();
        System.out.println(Arrays.toString(c.gcdlcm(3, 12)));
    }
}
```
