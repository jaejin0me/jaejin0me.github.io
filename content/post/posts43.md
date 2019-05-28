---
title: "백준알고리즘 11725번, 트리의 부모 찾기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

연결된 두 vertex의 정보가 주어질 때 각 노드의 부모를 찾아서 2번부터 출력해주는 문제입니다.
처음에 접근할때는 행렬로 풀려고 했는데, 다익스트라 알고리즘 공부하면서 인정리스트의 장점을 실감하게 되어서
vector를 사용해 인정리스트를 만들었고, 큐를 사용해 BFS로 풀었습니다.
재귀함수를 이용해 DFS로 풀 수도있겠지만 뭔가 함수를 계속 호출하는건 성능에 안좋을꺼같아 BFS로 풀었습니다.
```
#include <cstdio>
#include <cstring>
#include <vector>
#include <queue>
#include <functional>
#include <utility>

using namespace std;

int main(void) {
 vector<int> adj[100001];  ?//인접행렬
 bool visited[100001] = { false }; //방문확인
 queue<int> q;
 int t;
 int n1, n2;
 int curr, next,parent;
 for (int i = 1; i < 100001; i++) {
  adj[i].push_back(0); //각 노드 벡터의 0번째는 부모노드를 가르킴, 기본값 0 설정
 }

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d\n", &t);
 for (int i = 1; i < t; i++) {
  scanf("%d %d\n", &n1, &n2);
  adj[n1].push_back(n2); //각 노드에 연결 정보저장
  adj[n2].push_back(n1); //각 노드에 연결 정보저장
 }

 q.push(1); //1번 노드부터 시작
 while (!q.empty()) {?//BFS
  curr = q.front();
  q.pop();

  visited[curr] = true;
  for (int i = 1; i < adj[curr].size(); i++) {

    next = adj[curr][i];

    if (visited[next] == true)
     continue;

    else {
     adj[next][0] = curr;
     q.push(next);

    }
   }
  }

 for (int i = 2; i <= t; i++) {
  printf("%d\n", adj[i][0]);
 }
 

 

#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;

}
```
