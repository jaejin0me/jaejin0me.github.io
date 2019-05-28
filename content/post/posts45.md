---
title: "백준알고리즘 1167번, 트리의 지름"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

트리의 지름은 두 vertex간의 길이가 가장 긴 값입니다. 
이 문제에서는 간선에 가중치가 있기 때문에 멀어도 길이가 짧은 수 있고 그 반대가 될 수도 있습니다.
트리의 지름을 구하는 방법은 

1. 임의의 정점으로 부터 가장 먼 정점을 구한다.
2. 1에서 나온 정점으로 부터 가장 먼 정점을 구한다.
3. 1과 2에서 나온 정점간의 거리가 트리의 지름!

이 방법은 공식처럼 외우시면 됩니다. 증명은 구글링 하시면 잘 나옵니다.

저는 큐를 이용해 BFS로 풀었습니다. BFS로 계속 풀어서 그런지 이게 편하네요.
임의의 정점을 1번노드로 잡아서 BFS를 한번 수행하고, 거기서 나온 정점으로 한번 더 수행해서 구했습니다.
```
#include <cstdio>
#include <cstring>
#include <vector>
#include <queue>
#include <functional>
#include <utility>

using namespace std;

typedef pair<int, int> P; //노드, 거리

int main(void) {
 int V, u, v, w;
 vector<P> adj[100001];
 bool visited[100001] = { false };
 int cost[100001] = { 0 };
 queue<int> q;
 int curr, next, weight, max_weight=0,max_node;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d\n", &V);
 for (int i = 0; i < V; i++) {
  scanf("%d", &u);
  do
  {
   scanf("%d", &v);
   if (v != -1) {
    scanf("%d", &w);
    adj[u].push_back(P(v, w));
   }

  } while (v!=-1);  
 }

 q.push(1); //1번 노드를 기준으로 BFS
 while (!q.empty()) {
  curr = q.front();
  q.pop();
  visited[curr] = true;
  for (int i = 0; i < adj[curr].size(); i++) {
   next = adj[curr][i].first;
   if (visited[next] == false) {
    weight = adj[curr][i].second;
    cost[next] = cost[curr] + weight;
    if (cost[next] > max_weight) {
     max_weight = cost[next];
     max_node = next;
    }
    q.push(next);
   }
  }
 }

 memset(visited, 0, sizeof visited);
 memset(cost, 0, sizeof cost);

 q.push(max_node); // 위의 BFS수행에서 나온 노드를 바탕으로 한번더 수행
 while (!q.empty()) {
  curr = q.front();
  q.pop();
  visited[curr] = true;
  for (int i = 0; i < adj[curr].size(); i++) {
   next = adj[curr][i].first;
   if (visited[next] == false) {
    weight = adj[curr][i].second;
    cost[next] = cost[curr] + weight;
    if (cost[next] > max_weight) {
     max_weight = cost[next];
     max_node = next;
    }
    q.push(next);
   }
  }
 }

 printf("%d\n", max_weight);


#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;

}
```
