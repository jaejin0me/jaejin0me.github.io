---
title: "프로그래머스 - 숫자의 표현"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

내 코드
```java
public class Expressions {

    public int expressions(int num) {
        int sum=0;
        int first = 0;
        int cnt=0;
        for(int i=1;i<=num/2;i++) {
            sum = 0;
            first = i;
            while(true) {
                sum+=first++;
                if(sum==num) {
                    cnt++;
                    break;
                }
                else if(sum>num) {
                    break;
                }
            }
        }

        return cnt+1;
    }

    public static void main(String args[]) {
        Expressions expressions = new Expressions();
        // 아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println(expressions.expressions(15));
    }
}
```

이 코드는 뭐냐.. 내 코드가 초라하다 시간날때봐야지
```java
public class Expressions {

    public int expressions(int num) {
        int answer = 0;
        for (int i = 1; i <= num; i += 2) {
            if (num % i == 0) {
                answer++;
            }
        }
        return answer;
    }

    public static void main(String args[]) {
        Expressions expressions = new Expressions();
        // 아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println(expressions.expressions(15));
    }
}
```
