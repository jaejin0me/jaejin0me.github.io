---
title: "프로그래머스 - 피보나치 수"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
public class Fibonacci {
    public long fibonacci(int num) {
        if(num==0) {
            return 0;
        }
    else if(num==1){
      return 1;
    }
        else{
            return fibonacci(num-1)+fibonacci(num-2);           
        }
    }

  // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        Fibonacci c = new Fibonacci();
        int testCase = 3;
        System.out.println(c.fibonacci(testCase));
    }
}
```
다음에는 dp로 풀어봐야겠다.
