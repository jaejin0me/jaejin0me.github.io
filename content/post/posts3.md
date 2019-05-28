---
title: "프로그래머스 - 최고의 집합"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```java
import java.util.Arrays; //테스트로 출력해 보기 위한 코드입니다.

public class BestSet {

    public int[] bestSet(int n, int s){
        int[] answer = null;
        if(n>s) {
            answer = new int[1];
            answer[0]=-1;
            return answer;
        }

        answer = new int[n];
        int a = s/n;
        for(int i=0;i<n;i++) {
            answer[i]=a;
        }
        for(int i=0;i<s%n;i++) {
            answer[n-1-i]++;
        }

        return answer;
    }
    public static void main(String[] args) {
        BestSet c = new BestSet();
        //아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println(Arrays.toString(c.bestSet(3,13)));
    }

}
```
