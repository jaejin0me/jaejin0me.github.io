---
title: "백준알고리즘 5651번 - 완전 중요한 간선"
date: 2018-02-11T12:20:42+09:00
draft: false
tags: ["algorithm","네트워크플로우"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

중요한 간선을 찾는 코드에서 prev2[next2.to] 가 되어있어야 하는데 prev2[curr2]로 되어 있는 것을 발견하지 못해 푸는데 오래 걸린 문제이다.

중요한 간선의 여부는 최대유량은 찾은 후에 입력 간선들 중에 u->v로 유량을 흘릴수 없는 경우 중요한 간선이라고 판단하였다. 

```
import java.awt.Point;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;
import java.util.StringTokenizer;

class edge {
	int to;
	int flow;
	int cap;
	edge rever;

	edge() {
		this(0, 0, 0, null);
	}

	edge(int ar_to, int ar_flow, int ar_cap, edge ar_rever) {
		to = ar_to;
		flow = ar_flow;
		cap = ar_cap;
		rever = ar_rever;
	}

	int residu() {
		return cap - flow;
	}

	void addflow(int ar_flow) {
		flow += ar_flow;
		rever.flow -= ar_flow;
	}
}

public class Main {

	public static void main(String[] args) throws IOException {

		// BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		// BufferedReader br = new BufferedReader(new FileReader("input.txt"));
		// StringTokenizer st = new StringTokenizer(br.readLine());
		Scanner sc = new Scanner(System.in);
		int K = sc.nextInt();

		// 테스트 케이스 K번 반복한다
		for (int i = 0; i < K; i++) {

			int N = sc.nextInt();
			int M = sc.nextInt();
			edge f_edge = null;
			edge b_edge = null;
			int u = 0, v = 0, c = 0;
			int S = 0;
			int E = N - 1;
			int mf = 0;
			int cnt = 0;
			ArrayList<Point> edge_list = new ArrayList<Point>();

			// 정점의 수만큼 인접리스트 생성
			ArrayList<edge>[] adj = new ArrayList[N];
			for (int j = 0; j < N; j++) {
				adj[j] = new ArrayList<edge>();
			}

			// 간선 입력 받기
			for (int j = 0; j < M; j++) {
				u = sc.nextInt() - 1;
				v = sc.nextInt() - 1;
				edge_list.add(new Point(u,v));
				c = sc.nextInt();

				f_edge = new edge(v, 0, c, null);
				b_edge = new edge(u, 0, 0, f_edge);
				f_edge.rever = b_edge;

				adj[u].add(f_edge);
				adj[v].add(b_edge);
			}

			// 최대 유량 찾기, 에드몬드 카프
			while (true) {
				int[] prev = new int[N];
				edge[] path = new edge[N];
				Arrays.fill(prev, -1);
				Queue<Integer> q = new LinkedList<Integer>();
				q.add(S);

				label1: while (!q.isEmpty() && prev[E] == -1) {
					int curr = q.poll();
					for (int j = 0; j < adj[curr].size(); j++) {
						edge next = adj[curr].get(j);
						if (next.residu() > 0 && prev[next.to] == -1) {
							q.add(next.to);
							prev[next.to] = curr;
							path[next.to] = next;
							if(next.to == E) break label1;
						}
					}
				}

				// 입력 간선 리스트 중에서 중요한 간선을 확인함
				if (prev[E] == -1) {
					
					for(int j=0;j<edge_list.size();j++) {
						Point temp = new Point();
						temp = edge_list.get(j);
						int S2 = temp.x;
						int E2 = temp.y;
						int[] prev2 = new int[N];
						Arrays.fill(prev2, -1);
						Queue<Integer> q2 = new LinkedList<Integer>();
						q2.add(S2);
						label2: while (!q2.isEmpty()) {
							int curr2 = q2.poll();
							for (int l = 0; l < adj[curr2].size(); l++) {
								edge next2 = adj[curr2].get(l);
								if ( next2.residu() > 0 && prev2[next2.to] == -1) {
									q2.add(next2.to);
									prev2[next2.to] = curr2;
									if(prev2[E2] == next2.to) break label2;
								}
							}
						}

						if (prev2[E2] == -1)
							cnt++;
						
					}
					
					System.out.println(cnt);
					break;

				}

				int flow = Integer.MAX_VALUE;
				for (int k = E; k != S; k = prev[k])
					flow = Math.min(flow, path[k].residu());

				for (int k = E; k != S; k = prev[k])
					path[k].addflow(flow);

				mf += flow;

			}

		}

	}
}

```
