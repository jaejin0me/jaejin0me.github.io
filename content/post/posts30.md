---
title: "프로그래머스 - 평균구하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
public class GetMean {
    public int getMean(int[] array) {
        int sum=0;
        for(int i=0;i<array.length;i++) {
            sum+=array[i];
        }

        return sum/array.length;
    }

    public static void main(String[] args) {
        int x[] = {5, 4, 3};
        GetMean getMean = new GetMean();
        // 아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println("평균값 : " + getMean.getMean(x));
    }
}
```
아래와 같은 방법이 있는데 java8부터 추가된 기능이다. 하지만 수행시간의 차이가 40~50배가 났다.
```
import java.util.Arrays;

public class GetMean {
    public int getMean(int[] array) {
        return (int) Arrays.stream(array).average().orElse(0);
    }

    public static void main(String[] args) {
        int x[] = {5, 4, 3};
        GetMean getMean = new GetMean();
        // 아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println("평균값 : " + getMean.getMean(x));
    }
}
```
