---
title: "백준알고리즘 1932번, 숫자삼각형"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

피라미드 형태의 삼각형이 주어질때 아래로 내려가면서 최대의 값을 찾는 문제입니다

전략 : 별다른 전략 필요가 없네요
```
#include <stdio.h>
#include <string.h>

using namespace std;

int dp[500][500];

int main(void) {
 memset(dp, -1, 0);
 int t;
 int max = -1;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d", &t);
 for (int i = 0; i < t; i++) {
  for(int j=0;j<i+1;j++)
   scanf("%d", &dp[i][j]);
 }

 for (int i = 1; i < t; i++) {
  for (int j = 0; j < i + 1; j++) {
   if (j == 0) {
    dp[i][j] = dp[i][j] + dp[i-1][j];
   }
   else if (j == i) {
    dp[i][j] = dp[i][j] + dp[i-1][j-1];
   }
   else {
    dp[i][j] = dp[i][j] + (dp[i - 1][j] > dp[i - 1][j - 1] ? dp[i - 1][j] : dp[i - 1][j - 1]);
   }
  }
 }
 for (int i = 0; i < t; i++) {
  if (dp[t - 1][i] > max)
   max = dp[t - 1][i];
 }
 printf("%d\n", max);


 

#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;
}
```
