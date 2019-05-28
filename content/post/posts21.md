---
title: "프로그래머스 - 가운데 글자 가져오기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
public class StringExercise{
    String getMiddle(String word){
        int len = word.length();
        if(len%2==0) {//짝수
            return word.substring(len/2-1, len/2+1);
        }
        else {//홀수
            return word.substring(len/2, len/2+1);          
        }    
    }
    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void  main(String[] args){
        StringExercise se = new StringExercise();
        System.out.println(se.getMiddle("power"));
    }
}
```
아래와 같은 풀이도 있다. 짝수와 홀수를 2로 나눴을때의 결과를 잘 활용한 것 같다. 배워야지
```
lass StringExercise{
    String getMiddle(String word){

        return word.substring((word.length()-1)/2, word.length()/2 + 1);    
    }
    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void  main(String[] args){
        StringExercise se = new StringExercise();
        System.out.println(se.getMiddle("power"));
    }
}
```
