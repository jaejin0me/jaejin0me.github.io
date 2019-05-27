---
title: "백준알고리즘 1149번, RGB거리"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

RGB 색상별로 가중치가 주어질 때, 가장 값이 낮게 집을 칠하는 문제입니다.

전략 : 특별한 전략은 필요 없네요.
```
#include <stdio.h>
#include <string.h>

using namespace std;

int dp[1000][3];

int main(void) {
 memset(dp, -1, 0);
 int n;
 int t;
 int min = 100000;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d", &t);
 for (int i = 0; i < t; i++) {
  scanf("%d %d %d", &dp[i][0], &dp[i][1], &dp[i][2]);
 }

 for (int i = 0; i < t; i++) {
  dp[i + 1][0] = dp[i+1][0] + (dp[i][1] < dp[i][2] ? dp[i][1] : dp[i][2]);
  dp[i + 1][1] = dp[i+1][1] + (dp[i][0] < dp[i][2] ? dp[i][0] : dp[i][2]);
  dp[i + 1][2] = dp[i+1][2] + (dp[i][0] < dp[i][1] ? dp[i][0] : dp[i][1]);
 }

 for (int i = 0; i < 3; i++) {
  if (dp[t - 1][i] < min)
   min = dp[t - 1][i];
 }

 printf("%d\n", min); 
 

#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;
}
```
