---
title: "프로그래머스 - 행렬의 곱셈"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

머리속에서는 쉽지만 막상 구현하면 헷갈려서 잘 틀리는 행렬곱...

```java
class ProductMatrix {
    public int[][] productMatrix(int[][] A, int[][] B) {
        if(A[0].length!=B.length)
            return null;
        int[][] answer = new int[A.length][B[0].length];
        for(int i=0;i<A.length;i++) {
            for(int j=0;j<B[0].length;j++) {
                for(int k=0;k<B.length;k++) {
                    answer[i][j]+=(A[i][k]*B[k][j]);
                }
            }
        }

        return answer;
    }

    public static void main(String[] args) {
        ProductMatrix c = new ProductMatrix();
        int[][] a = { { 1, 2 }, { 2, 3 } };
        int[][] b = { { 3, 4 }, { 5, 6 } };
      // 아래는 테스트로 출력해 보기 위한 코드입니다.
      System.out.println("행렬의 곱셈 : " + c.productMatrix(a, b));
    }
```
