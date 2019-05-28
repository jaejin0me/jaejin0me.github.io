---
title: "백준알고리즘 10828번, 스택"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

스택의 기본적인 연산 5가지인 push,pop,top,empty,size를 구현하는 문제이다.
별다른 풀이 전략이 필요하지 않다, 그냥 구현만 해내면 그만인 문제이다
스택을 클래스나 구조체로 만들려고 했으나, 굳이 그렇게 할 필요까진 없어 보여 배열과 위치를 갖은 변수하나로 구성하였다.
앞으로 자주 사용된다면 클래스화하여 작성할 예정이다.
참고로, 온라인저지는 컴파일 옵션에 -DONLINE_JUDGE 가 들어가기 때문에 이것을 활용해 예시문을 테스트하는 것을 간편히 만들 수 있다.
freopen()을 이용해 인풋과 아웃풋의 스트림을 컴파일 옵션에 따라 바꾼다.
```
#include <stdio.h>
#include <string.h>

int main(void) {
 unsigned int size = 0;
 int stack[10000] = { 0 };
 int n;
 char cmd[6] = { 0 };
 cmd[5] = '\0';
 int item;
#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif
 scanf("%d", &n);
 for (int i = 0; i < n; i++) {
  scanf("%s", cmd);
  if (strcmp(cmd, "pop") == 0)
  {
   if (size == 0)
   {
    printf("-1\n");
    continue;
   }
   printf("%d\n", stack[size-1]);
   size--;
  }
  else if (strcmp(cmd, "push") == 0)
  {
   scanf("%d\n", &item);
   stack[size] = item;
   size++;
   
  }
  else if (strcmp(cmd, "top") == 0)
  {
   if (size == 0)
   {
    printf("-1\n");
    continue;
   }
   printf("%d\n", stack[size-1]);
  }

  else if (strcmp(cmd, "empty") == 0)
  {
   printf("%d\n", size == 0);
  }
  else {
   printf("%d\n", size );
  }
  
 }
 
 #ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
 #endif

 return 0;
}
```
