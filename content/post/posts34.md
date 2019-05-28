---
title: "백준알고리즘 10845번, 큐"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

큐를 구현하는 문제이다.
push,pop,empty,size,front,back 함수를 구현해야 한다.
배열을 이용해 구현하였다. 

고려해야 할 것은 앞과 끝을 가르키는 위치를 갖고 있어야 된다는 정도이다. 정해진 배열의 크기에서 큐가 원형으로 자라나고 줄어들게 만들었다.

```
#include <stdio.h>
#include <string.h>

#define MAX_QUEUE_SIZE 100000

using namespace std;

class Queue {
private:
 unsigned int first;
 unsigned int last;
 unsigned int size;
 int queue[MAX_QUEUE_SIZE];
public:
 Queue() {
  first = 0;
  last = 0;
  size = 0;
  memset(queue, 0, sizeof queue);
 }
 int Size() {
  return size;
 }
 bool Empty() {
  return size == 0;
 }
 int Front() {
  if (size == 0)
   return -1;
  return queue[first];
 }
 int Back() {
  if (size == 0)
   return -1;
  return queue[(last-1)%MAX_QUEUE_SIZE];
 }

 void Push(int item)
 {
  size++;
  queue[last++] = item;
  if (last == MAX_QUEUE_SIZE)
   last %= MAX_QUEUE_SIZE;
 }

 int Pop() {
  if (size == 0)
   return -1;
  int temp = queue[first];
  size--;
  first++;
  if (first == MAX_QUEUE_SIZE)
   first %= MAX_QUEUE_SIZE;
  return temp;
 }
};

int main(void) {
 Queue q1;
 char sign[6] = { 0 };
 int n = 0;
 int item = 0;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d", &n);
 for (int i = 0; i < n; i++)
 {
  scanf("%s", sign);
  if (strstr(sign, "push"))
  {
   scanf("%d", &item);
   q1.Push(item);
  }
  else if (strstr(sign, "pop"))
  {
   printf("%d\n", q1.Pop());
  }
  else if (strstr(sign, "front"))
  {
   printf("%d\n", q1.Front());
  }
  else if (strstr(sign, "back"))
  {
   printf("%d\n", q1.Back());
  }
  else if (strstr(sign, "size"))
  {
   printf("%d\n", q1.Size());
  }
  else
  {
   printf("%d\n", q1.Empty());
  }  
 }

#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;
}
```
