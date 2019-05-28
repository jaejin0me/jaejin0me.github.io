---
title: "프로그래머스 - 나누어 떨어지는 숫자 배열"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
import java.util.Arrays;

public class Divisible {
    public int[] divisible(int[] array, int divisor) {
        int[] temp = new int[array.length];
        int idx = 0;
        for(int i=0;i<array.length;i++) {
            if(array[i]%divisor==0) {
                temp[idx++] = array[i];
            }
        }

        int[] ret = new int[idx];
        for(int i=0;i<idx;i++) {
            ret[i] = temp[i];
        }
        return ret;
    }
    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        Divisible div = new Divisible();
        int[] array = {5, 9, 7, 10};
        System.out.println( Arrays.toString( div.divisible(array, 5) ));
    }
}
```
아래의 풀이도 있는데 간결해서 깜짝놀라고 속도 떨어지는 것에 또 놀랐다.
```
import java.util.Arrays;

class Divisible {
    public int[] divisible(int[] array, int divisor) {
        
        return Arrays.stream(array).filter(factor -> factor % divisor == 0).toArray();
    }
    public static void main(String[] args) {
        Divisible div = new Divisible();
        int[] array = {5, 9, 7, 10};
        System.out.println( Arrays.toString( div.divisible(array, 5) ));
    }
}
```
