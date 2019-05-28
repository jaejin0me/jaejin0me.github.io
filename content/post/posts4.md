---
title: "프로그래머스 - 줄서는 방법"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

문제가 점점 어려워지는군욤
```java
import java.util.ArrayList;
import java.util.Arrays;

public class LineCombination {
    public int[] setAlign(int n, long k) {
        int[] answer = new int[n];
        ArrayList<Integer> list = new ArrayList<Integer>();

        int[] fac = new int[n+1];
        fac[0] = 1;
        for(int i=1;i<n;i++) {
            fac[i] = fac[i-1]*i; 
        }

        int idx = 0;
        for(int i=0;i<n;i++) {
            list.add(i+1);
        }

        for(int i=0;i<n;i++) {
            idx=(int) ((k-1)/fac[n-1-i]);
            answer[i]=list.get(idx);
            list.remove(idx);
            k=(k-1)%fac[n-1-i]+1;       
        }       

        return answer;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        LineCombination lc = new LineCombination();
        System.out.println(Arrays.toString(lc.setAlign(4, 1)));
    }
}
```
