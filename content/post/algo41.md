---
title: "백준알고리즘 7576번, 토마토"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

토마토의 상태가 행렬로 주어질 때, 모든 토마토가 익는데 걸리는 시간을 구하는 문제입니다. 
큐를 이용해서 BFS로 풀었습니다. 공부많이 됐네용.
입력값이 행렬로 주어져서 인접행렬로 풀었는데, 다음에는 인접 리스트로 푸는 문제를 도전해보겠습니다
```
#include <stdio.h>
#include <string.h>
#include <queue>

using namespace std;

int g[1000][1000];
int M;
int N;

int BFS();

pair<int, int> temp;
queue < pair<int,int> > q;

int main(void) {
 memset(g, 0, sizeof g);
 bool check_allzero = true; // 입력값이 모두 0인경우 확인용
 bool check_allone = true; // 입력값이 모두 1또는 -1인 경우 확인용
 bool check_zero = false;
 int n1, n2, min;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d %d\n", &n1,&n2);
 N = n1; M = n2;
 for (int i = 0; i < n2; i++) {
  for (int j = 0; j < n1; j++) {
   scanf("%d ", &g[i][j]);
   if (g[i][j] == 1 || g[i][j] == -1)
    check_allzero = false;
   else
    check_allone = false;
  }
 }

 if (check_allone == true) {
  printf("%d\n", 0);
  return 0;
 }

 if (check_allzero == true) {
  printf("%d\n", -1);
  return 0;
 }

 

 for (int i = 0; i < n2; i++) {
  for (int j = 0; j < n1; j++) {
   if (g[i][j] == 1)
    q.push(pair<int,int>(i,j));
  }
 }
 
 printf("%d\n",BFS());

 
 
#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif
 return 0; 
}

int BFS() {
 pair<int, int> temp;
 while (!q.empty()) {
  temp = q.front();
  q.pop();
  if (temp.first > 0 && g[temp.first - 1][temp.second] == 0) { ?// 위쪽에 안익는 토마토가 있으면, 현재 값의 +1로 할당
//현재 값의 +1로 할당해주는 이유는 같은 값으로 할당해버리면 그 토마토를 접근했을때 그 토마토가 또 주변의 토마토를 익게 만들기 때문입니다.
   g[temp.first - 1][temp.second] = g[temp.first][temp.second] + 1;
   q.push(pair<int, int>(temp.first - 1, temp.second));
  }

  if (temp.first < M - 1 && g[temp.first + 1][temp.second] == 0) { ?// 아래쪽에 안익는 토마토가 있으면, 현재 값의 +1로 할당
    g[temp.first + 1][temp.second] = g[temp.first][temp.second] + 1;
   q.push(pair<int, int>(temp.first + 1, temp.second));
  }

  if (temp.second > 0 && g[temp.first][temp.second - 1] == 0) { // 왼쪽에 안익는 토마토가 있으면, 현재 값의 +1로 할당
   g[temp.first][temp.second - 1] = g[temp.first][temp.second] + 1;
   q.push(pair<int, int>(temp.first, temp.second-1));
  }

  if (temp.second < N - 1 && g[temp.first][temp.second + 1] == 0) { // 오른쪽에 안익는 토마토가 있으면, 현재 값의 +1로 할당
   g[temp.first][temp.second + 1] = g[temp.first][temp.second] + 1;
   q.push(pair<int, int>(temp.first, temp.second+1));
  }
 }
    
    for (int i = 0; i < M; i++) {
  for (int j = 0; j < N; j++) {
   if (g[i][j] == 0)
    return -1;
  }
 }
    
 return g[temp.first][temp.second]-1;
}
```
