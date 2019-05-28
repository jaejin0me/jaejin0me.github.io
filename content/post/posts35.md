---
title: "백준알고리즘 2750번, 수 정렬하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

1~N 까지의 수를 임의로 입력 받아 정렬하는 문제 입니다.
버블, 삽입, 선택 정렬 3가지를 이용해 풀어봤습니다.
버블 정렬만 알고 있었는데 이번 기회로 삽입, 선택 정렬에 대해서 알게 되었네요.
Bin-O로 표기할 경우 시간복잡도는 모두 n^2 으로 동일합니다.

```
#include <stdio.h>
#include <string.h>

#define MAX_ARRAY_SIZE 1000

using namespace std;

void selection(int *arr, int N)
{
 int min;
 int p;
 int temp;
 for (int i = 0; i <N; i++)
 {
  min = 1000000;
  p = i;
  for (int j = i; j < N; j++)
  {
   if (arr[j] < min)
   {
    min = arr[j];
    p = j;
   }
  }
  if (p != i)
  {
   temp = arr[p];
   arr[p] = arr[i];
   arr[i] = temp;
  }
 }
}

void insert(int *arr, int N)
{
 int temp;
 for (int i = 1; i < N; i++)
 {
  for (int j = i; j >0; j--)
  {
   if (arr[j] < arr[j-1])
   {
    temp = arr[j];
    arr[j] = arr[j-1];
    arr[j-1] = temp;
   }

  }
 }
}

void bubble(int *arr,int N)
{
 int temp;
 for (int i = N - 1; i > -1; i--)
 {
  for (int j = 0; j < i; j++)
  {
   if (arr[j] > arr[j + 1])
   {
    temp = arr[j];
    arr[j] = arr[j + 1];
    arr[j + 1] = temp;
   }
  }
 }
}

int main(void) {
 int arr[1000] = { 0 };
 int temp;
 int n;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif
 scanf("%d", &n);
 for (int i = 0; i < n; i++) {
  scanf("%d", &temp);
  arr[i] = temp;
 }

 insert(arr,n);
    #bubble(arr,n);
    #selection(arr,n);

 for (int i = 0; i < n; i++)
 {
  printf("%d\n", arr[i]);
 }

#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;
```
