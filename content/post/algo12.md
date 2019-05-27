---
title: "프로그래머스 - 소수 찾기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

label이 break, continue와 사용가능해서 좋은 자바:)<br>
좀 더 좋은 소수찾는 방법으로 풀어봐야지 한번 더 시도해봐야지
```java
public class NumOfPrime {
    int numberOfPrime(int n) {
        int result = 0;
        loop1: 
        for(int i=2;i<=n;i++) {

            for(int j=2;j<i;j++) {
                if(i%j==0) {
                    continue loop1;
                }
            }
        result++;   
        }
        // 함수를 완성하세요.

        return result;
    }

    public static void main(String[] args) {
        NumOfPrime prime = new NumOfPrime();
        System.out.println(prime.numberOfPrime(10));
    }

}
```
