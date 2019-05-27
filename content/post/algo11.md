---
title: "프로그래머스 - 문자열 내림차순으로 배치하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```java
import java.util.Arrays;

public class ReverseStr {
    public String reverseStr(String str){
        String ans = "";
        char[] temp = str.toCharArray();
        Arrays.sort(temp);

        for(int i=0;i<temp.length;i++) {
            ans+=temp[temp.length-1-i];
        }

        return ans;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        ReverseStr rs = new ReverseStr();
        System.out.println( rs.reverseStr("Zbcdefg") );
    }
}
```
지금 쓰는 방법으로도 풀 수 있긴한데, 스트림버퍼사용법좀 배워야 될듯
