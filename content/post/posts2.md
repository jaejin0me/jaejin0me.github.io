---
title: "프로그래머스 - 콜라츠 추측"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

연산도중에 int형의 범위를 넘어갈 수 있기 때문에 long으로 바꿔주는 것이 포인트!<br>
시간 많이 날림 ㅜ
```
class Collatz {
    public int collatz(long num) {
        int answer = 0;
        int i;
        for(i=0;i<500;i++) {
            if(num==1)
                return answer;          
            if(num%2==0)
                num/=2;         
            else
                num = (num*3)+1;            
            answer++;
        }

        if(num==1)
            return answer;
        else
            return -1;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        Collatz c = new Collatz();
        int ex = 6;
        System.out.println(c.collatz(ex));
    }
}
```
