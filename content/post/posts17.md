---
title: "프로그래머스 - N개의 최소공배수"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

2개의 값에 대하여 gcd를 이용해 lcm을 구하는 것을 N개에 대해 반복함
```java
public class NLCM {
    public long gcd(long a,long b) {
        if(a==0)
            return b;
        else
            return gcd(b%a,a);      
    }
    public long lcm(long a,long b) {
        return a*b/gcd(a,b);        
    }   

    public long nlcm(int[] num) {
        long answer=num[0];
        for(int i=0;i<num.length-1;i++) {
            answer = lcm(answer,num[i+1]);
        }
        return answer;
    }

    public static void main(String[] args) {
        NLCM c = new NLCM();
        int[] ex = { 2, 6, 8, 14 };
        // 아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println(c.nlcm(ex));
    }
}
```
