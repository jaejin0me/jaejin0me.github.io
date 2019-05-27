---
title: "프로그래머스 - 약수의 합"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
public class SumDivisor {
    public int sumDivisor(int num) {
        int answer = 0;
        for(int i=1;i<=num/2;i++) {
            if(num%i==0) {
                answer+=i;
            }
        }
        answer+=num;

        return answer;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        SumDivisor c = new SumDivisor();
        System.out.println(c.sumDivisor(12));
    }
}
```
원래는 반복문의 범위가 [0,num] 까지 였는데 아래의 풀이를 보고 바꿨다. 좋은거 배웠다.
```
class SumDivisor {
    public int sumDivisor(int num) {
        int answer = 0;

        for(int i = 1; i<=num/2; i++){
      if(num%i==0){
        answer+=i;
      }
    }
        return answer+num;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        SumDivisor c = new SumDivisor();
        System.out.println(c.sumDivisor(12));
    }
}
```
