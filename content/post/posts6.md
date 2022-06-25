---
title: "프로그래머스 - 공항 건설하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

원래 dp로 풀었는데 알고보니, 단순 비교문제였음..  
그냥 사람수가 가장 많은 도시는 고르면 되는 문제
```java
import java.util.Arrays;

public class TryHelloWorld
{
    public int chooseCity(int n, int [][]city)
    {
        int idx=city[0][0];
        int max = city[0][1];
        for(int i=1;i<n;i++) {
            if(city[i][1]>max) {
                max = city[i][1];
                idx = city[i][0];
            }
        }

        return idx;
    }

    public static void main(String[] args)
    {
        TryHelloWorld test = new TryHelloWorld();
        int tn = 3;
        int [][]tcity = {{1,5},{2,2},{3,3}};
        System.out.println(test.chooseCity(tn,tcity));
    }

}
```
