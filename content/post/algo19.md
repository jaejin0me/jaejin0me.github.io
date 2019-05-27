---
title: "프로그래머스 - 최솟값 만들기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
import java.util.Arrays;

public class TryHelloWorld
{

    public int getMinSum(int []A, int []B)
    {
        int answer = 0;
        Arrays.sort(A);
        Arrays.sort(B);

        for(int i=0;i<B.length;i++) {
            answer += A[i]*B[B.length-1-i];
        }

        return answer;
    }
    public static void main(String[] args)
    {
        TryHelloWorld test = new TryHelloWorld();
        int []A = {1,2};
        int []B = {3,4};
        System.out.println(test.getMinSum(A,B));
    }
}
```
