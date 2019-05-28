---
title: "프로그래머스 - 땅따먹기 게임"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

dp
```java
import java.util.Arrays;

public class Hopscotch {

    int hopscotch(int[][] board, int size) {

        for(int i=1;i<size;i++) {
            board[i][0] += board[i-1][1] > board[i-1][2] ? (board[i-1][1] > board[i-1][3] ? board[i-1][1] : board[i-1][3]) : board[i-1][2] > board[i-1][3] ? board[i-1][2] : board[i-1][3];
            board[i][1] += board[i-1][0] > board[i-1][2] ? (board[i-1][0] > board[i-1][3] ? board[i-1][0] : board[i-1][3]) : board[i-1][2] > board[i-1][3] ? board[i-1][2] : board[i-1][3];
            board[i][2] += board[i-1][0] > board[i-1][1] ? (board[i-1][0] > board[i-1][3] ? board[i-1][0] : board[i-1][3]) : board[i-1][1] > board[i-1][3] ? board[i-1][1] : board[i-1][3];
            board[i][3] += board[i-1][0] > board[i-1][1] ? (board[i-1][0] > board[i-1][2] ? board[i-1][0] : board[i-1][2]) : board[i-1][1] > board[i-1][2] ? board[i-1][1] : board[i-1][2];
        }
        Arrays.sort(board[size-1]);

        return board[size-1][3];
    }

    public static void main(String[] args) {
        Hopscotch c = new Hopscotch();
        int[][] test = { { 1, 2, 3, 5 }, { 5, 6, 7, 8 }, { 4, 3, 2, 1 } };
        //아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println(c.hopscotch(test, 3));
    }

}
```
