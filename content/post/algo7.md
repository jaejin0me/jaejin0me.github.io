---
title: "프로그래머스 - 다음 큰 숫자"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

내 풀이는 좀 허접...
```java
{
    public int nextBigNumber(int n)
    {
        int answer = 0;
        char temp;
        String a = Integer.toBinaryString(n);
        char[] str = a.toCharArray();
        int idx = a.lastIndexOf("01");
        str[idx] = '1';
        str[idx+1] = '0';
        for(int i=idx+2;i<str.length;i++) {
            if(str[i]=='1') {

                for(int j=str.length-1;j>i;j--) {
                    if(str[j]=='0') {
                        str[j]='1';
                        str[i]='0';
                        break;
                    }                   
                }               
            }
        }
        String b = new String(str);
        answer = Integer.parseInt(b, 2);
        return answer;
    }
    public static void main(String[] args)
    {
        TryHelloWorld test = new TryHelloWorld();
        int n = 78;
        System.out.println(test.nextBigNumber(n));
    }
}
```
이 코드는 직관적으로 이해가 잘가는 코드
```java
import java.lang.Integer;

class TryHelloWorld
{
    public int nextBigNumber(int n)
    {
      int a = Integer.bitCount(n);
      int compare = n+1;

      while(true) {
        if(Integer.bitCount(compare)==a)
          break;
        compare++;
      }

      return compare;
    }

    public static void main(String[] args)
    {
        TryHelloWorld test = new TryHelloWorld();
        int n = 78;
        System.out.println(test.nextBigNumber(n));
    }
}
```
이 코드는 약간 넘사벽
```java
class TryHelloWorld {
    public int nextBigNumber(int n) {
        int postPattern = n & -n, smallPattern = ((n ^ (n + postPattern)) / postPattern) >> 2;
        return n + postPattern | smallPattern;
    }
    public static void main(String[] args) {
        int n = 78;
        System.out.println(new TryHelloWorld().nextBigNumber(n));
    }
}
```
