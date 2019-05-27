---
title: "백준알고리즘 1003번, 피보나치 함수"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

(n)을 했을때  fib(0),와 fib(1)이 몇번 콜되는지 구하는 문제입니다.

전략 : 전역 변수 2개를 할당해, fib(0)을 부를때 증가시키고, fib(1)을 부를때 증가시키는 것으로 해서 풀었습니다.
```
#include <stdio.h>
#include <string.h>

#define MAX_ARRAY_SIZE 1000

using namespace std;

int n_0;
int n_1;

int fibonacci(int n) {
 if (n == 0) {
  n_0++;
  return 0;
 }
 else if (n == 1) {
  n_1++;
  return 1;
 }
 else {
  return fibonacci(n-1) + fibonacci(n-2);
 }
}

int main(void) {
 int n;
 int t;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d", &t);
 for (int i = 0; i < t; i++) {
  scanf("%d", &n);
  n_0 = 0;
  n_1 = 0;
  fibonacci(n);
  printf("%d %d\n", n_0, n_1);
 }
 

#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;
}
```
