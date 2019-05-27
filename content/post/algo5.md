---
title: "프로그래머스 - 가장 큰 정사각형 찾기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

이 문제는 이제 익숙해진 유형
```java
class TryHelloWorld
{
    public int findLargestSquare(char [][]board)
    {
        int max=0;
        int min=0;
        for(int i=0;i<board.length;i++) {
            for(int j=0;j<board[0].length;j++) {
                if(board[i][j]=='O')
                    board[i][j]=1;
                else
                    board[i][j]=0;
            }
        }

        for(int i=0;i<board.length-1;i++) {
            for(int j=0;j<board[0].length-1;j++) {
                if(board[i][j]==0||board[i][j+1]==0||board[i+1][j]==0||board[i+1][j+1]==0)
                    continue;
                else{
                    min = board[i][j] < board[i][j+1] ? (board[i][j] < board[i+1][j] ? board[i][j] : board[i+1][j]) : (board[i][j+1] < board[i+1][j] ? board[i][j+1] : board[i+1][j]);
                    board[i+1][j+1] = (char) (min+1);
                    if(board[i+1][j+1]>max)
                        max=board[i+1][j+1];
                }
            }
        }   

        return max*max;
    }
    public static void main(String[] args)
    {
        TryHelloWorld test = new TryHelloWorld();
                char [][]board ={
                {'X','O','O','O','X'},
                {'X','O','O','O','O'},
                {'X','X','O','O','O'},
                {'X','X','O','O','O'},
                {'X','X','X','X','X'}};

        System.out.println(test.findLargestSquare(board));
    }
}
```
