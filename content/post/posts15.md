---
title: "프로그래머스 - 야근 지수"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

좀 지저분하긴해도 시간초과 까지 개선한 코드.. N^2으로 풀어도 5초가 넘어서(정답 처리는 됨) 좀더 최적화 해봄
```java
import java.util.Arrays;

public class NoOvertime {
    public int noOvertime(int no, int[] works) {
        int result = 0;
        Arrays.sort(works);
        int i = works.length-1;
        Loop1: while(i!=0 && no!=0) {
            if(works[i]>works[i-1]) {
                for(int j=i;j<works.length;j++) {
                    if(no==0) {
                        break Loop1;
                    }
                    works[j]--;
                    no--;
                }            
            continue Loop1;
            }
            i--;
        }

        while(no!=0) {
            works[no--%works.length]--;         
        }
        for(int j=0;j<works.length;j++) {
            result+=works[j]*works[j];
        }

        return result;
    }
    public static void main(String[] args) {
        NoOvertime c = new NoOvertime();
        int []test = {4,3,3};
        System.out.println(c.noOvertime(4,test));
    }
}
```
