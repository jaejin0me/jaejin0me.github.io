---
title: "백준알고리즘 1753번, 최단경로"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

다익스트라 알고리즘을 공부하기 위해서 풀어본 문제입니다..
어렵네요 어려웠어요 휴..
구현할때 주의하실 점은 인접리스트랑 우선순위큐 꼭 쓰세요! 
노드의 갯수가 커지면 인접행렬의 경우 메모리엄청 잡아 먹고 순회하는데 시간이 많이 걸립니다!

```
#include <stdio.h>
#include <string.h>
#include <vector>
#include <queue>
#include <functional>

#define INF 0x7f

using namespace std;

typedef pair<int, int> P;

int main(void) {
 int u, v, w;
 int V, E, K;
 bool visited[20001] = { false }; // 방문 여부 확인용
    int dist[20001]; // 노드까지의 거리를 저장하는 배열
 memset(dist, INF, sizeof dist);
    int next, di, curr; // 다음노드, 거리, 현재노드용 변수
    vector<P> adj[20001];   // (노드번호, 거리) 로 인접 노드를 저장
 priority_queue<P, vector<P>, greater<P> > pq; // 거리가 가장짧은 노드를 빼내기 위한 우선순위 큐, (거리, 노드번호) 로 저장됩니다 주의! adj와 다름
 
 scanf("%d %d\n", &V, &E); // V,E,K 입력받기
 scanf("%d\n", &K);

 for (int i = 0; i<E; i++) { // u,v, w입력받기
  scanf("%d %d %d", &u, &v, &w);
  adj[u].push_back(P(v, w));
 }

 dist[K] = 0; // 시작점까지의 거리는 0
 pq.push(P(dist[K], K)); // 시작점 넣음

 while (!pq.empty()) { // pq가 비면 종료

  do {
   curr = pq.top().second; // 거리가 가장 작은노드
   pq.pop(); //접근한 노드는 빼내기
  } while (visited[curr] && !pq.empty()); // curr가 방문한 노드면 다음 최소노드 구하기
            
// 방문한 노드면 종료
  if (visited[curr]) break;

  visited[curr] = true; // 방문으로 바꾸기
  for (int i = 0; i < adj[curr].size(); i++) { // 인접한 노드들에 대해
   next = adj[curr][i].first;
   di = adj[curr][i].second;
   if (dist[next] > dist[curr] + di) { //거리가 더 낮으면 낮은 값으로 갱신
    dist[next] = dist[curr] + di;
    pq.push(P(dist[next], next)); // 큐에 추가해줌
   }
  }
 }

 for (int i = 1; i<V+1; i++) {
  if
   (dist[i] == 0x7f7f7f7f) printf("INF\n"); //도달불가능한 경우
  else 
   printf("%d\n", dist[i]);
 }

 return 0;



}
```
