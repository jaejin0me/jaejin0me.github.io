---
title: "백준알고리즘 1874번, 스택 수열"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

이번 문제에서는 스택을 클래스로 만들어 사용했다.
1~n까지의 수로 이루어진 수열을 스택을 이용해 재현해내는 문제이다.
push를 할때는 1부터 순서대로 진행된다. 
1~n까지 순서대로 push가 되기 때문에 수열을 재현해내기 불가능한 경우도 나온다.
만약 수열이 일부분이 5, 3 일 경우 5를 pop한 후에 바로 3을 pop하는 것은 불가능하다.(이전 단계에서 4를 pop해 놓지 않은 경우)
일반적으로 고려를 하자면 임의의 k를 pop한 경우 그 다음에 k를 넘어서는 값들이 올 경우 그만큼 push한 후에 pop을 하면 문제가 되지 않지만
k-1보다 작은 값들이 온다면 재현이 불가능한 경우가 된다.
아래 코드의 특이한 점은 변수를 이용해 1부터 n까지 순서대로 push한것이 아니라,
1~n까지의 값을 스택에 넣어 넣고 하나씩 pop하면서 사용했다.
스택 2개를 만들어 사용하는 것을 확인할 수 있을것이다.  
전략:
m을 pop해야 하는 경우 
top이 m보다 작다면 m까지 push한후 pop.
               크다면 불가능.
```
#include <stdio.h>
#include <string.h>
#define MAX_STACK_SIZE 1000000

class Stack {
private:
 unsigned size;
 int stack[MAX_STACK_SIZE];
public:
 Stack() {
  size = 0;
  memset(stack, 0, sizeof stack);
 }
 int Size() {
  return size;
 }
 bool Empty(){
  return size == 0;
 }
 int Top(){
  if (size == 0)
   return 0;
  return stack[size-1];
 }
 void Push(int item)
 {
  stack[size++] = item;
 }
 int Pop() {
  return stack[--size];
 }

};


int main(void) {
 Stack st1, st2;
 int n;
 char sign[1000000] = { 0 };
 unsigned cnt = 0;
 int item;
#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif
 scanf("%d", &n);
 for (int i = n; i > 0; i--)
 {
  st2.Push(i);
 }
 for (int i = 0; i < n; i++) {
  scanf("%d", &item);
  if (st1.Top() <= item)
  {
   while (st1.Top() < item)
   {
    st1.Push(st2.Pop());
    sign[cnt++] = '+';
   }
   st1.Pop();
   sign[cnt++] = '-';
  }
  else
  {
   printf("NO\n");
   return 0;
  }
 }
 
 #ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
 #endif

 for (int i = 0; i < cnt; i++) {
  printf("%c\n", sign[i]);
 }

 return 0;
}
```
