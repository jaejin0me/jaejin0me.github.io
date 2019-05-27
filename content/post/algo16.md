---
title: "프로그래머스 - 정수 내림차순으로 배치하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

toCharArray를 쓰니 간편하군욤
```
import java.util.Arrays;

public class ReverseInt {
    public int reverseInt(int n){
        int answer = 0;
        String a = Integer.toString(n);
        char[] tmp = a.toCharArray();
        Arrays.sort(tmp);
        for(int i=0;i<a.length();i++) {
            answer*=10;
            answer+=tmp[a.length()-1-i]-'0';
        }

        return answer;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void  main(String[] args){
        ReverseInt ri = new ReverseInt();
        System.out.println(ri.reverseInt(118372));
    }
}
```
