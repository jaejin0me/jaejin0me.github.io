---
title: "백준알고리즘 6086번, 최대 유량"
date: 2018-01-04T18:00:42+09:00
draft: false
tags: ["algorithm","네트워크플로우"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

드디어 네트워크플로우

처음에 목표한 알고리즘이 MCMF까지였기 때문에, 곧 결과에 쫓기듯 공부할 필요는 없을꺼같다.

배울 알고리즘이 참 많지만 사실 그런알고리즘은 내가 앞으로 쓸일이 있을까 하는 의문이 들어서 필요성을 느끼지 못하겠다. 그리고 필요하다면 그 때 배워서 사용하면 될꺼같기도 하다.
시험문제로 나온다면.. 그저 슬플수 밖에

어쨋든 앞으로는 네트워크 플로우를 공부좀 하다가 어려운 dp들을 풀어봐야겠다. 이후에는 새로운 알고리즘은 세그먼트 트리, 문자열 알고리즘, 컨벡스 홀 중에서 선택하지 싶다.

인접행렬을 사용한 풀이

```
/**
 * 
 */


import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;
import java.util.StringTokenizer;


/**
 * @author jaeji
 *
 */
public class Main {
	
	static int atoi(String a) {
		if(a.charAt(0)<='Z') {
			return a.charAt(0)-'A';
		}
		else {
			return a.charAt(0)-'a'+26;
		}
	}
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());		
		int n = Integer.parseInt(st.nextToken());
		
		int[][] f = new int[52][52];
		int[][] c = new int[52][52];
		int[] prev =  new int[52];
		ArrayList[] adj = new ArrayList[52];
		Queue<Integer> q =  new LinkedList<Integer>();
		int S = atoi("A");
		int E = atoi("Z");
		
		
		int u=0,v=0,w=0;
		int total = 0;
		
		for(int i=0;i<52;i++) {
			
			adj[i] = new ArrayList<Integer>();
			
		}
		
		for(int i=0;i<n;i++) {
			
			st =  new StringTokenizer(br.readLine());
			u = atoi(st.nextToken());
			v = atoi(st.nextToken());
			w = Integer.parseInt(st.nextToken());
			c[u][v] += w;
			adj[u].add(v);
			adj[v].add(u);
			
		}
		
		int curr = 0;
		int next = 0;
		int adj_size = 0;
		int flow = 0;
		
		while(true) {
			Arrays.fill(prev, -1);
			q.clear();
			q.add(S);
			
			curr = 0;
			next = 0;
			adj_size = 0;
			
			
			while(!q.isEmpty()) { // 소스에서 흐를수 있는 유량찾기
				curr = q.poll();
				adj_size = adj[curr].size();
				for(int i=0;i<adj_size;i++) {
					next = (int) adj[curr].get(i);
					if(c[curr][next] - f[curr][next] > 0 && prev[next] == -1) {
						q.add(next);
						prev[next] = curr;
						if(next == E) break;
					}
				}
			}
			
			if(prev[E] == -1) break; // 경로를 탐색 한 후 싱크로 가는 경로가 없는 경우
            
            flow = Integer.MAX_VALUE;		
			for(int i=E;i!=S;i=prev[i]) {
				flow =  Math.min(flow, c[prev[i]][i]-f[prev[i]][i]);
			}
		
			for(int i=E;i!=S;i=prev[i]) {
				f[prev[i]][i]+=flow;
				f[i][prev[i]]-=flow;
			}
		
			total +=flow;
			
		}
		
		System.out.println(total);
		
	}

}

```

인접리스트를 사용한 풀이

```
/**
 * 
 */

import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
import java.util.Scanner;
import java.util.StringTokenizer;


/**
 * @author jaeji
 *
 */

class edge{
	int to;
	int flow;
	int cap;
	edge rever;
	edge(){
		this(0,0,0,null);		
	}
	edge(int ar_to, int ar_flow, int ar_cap, edge ar_rever){
		to = ar_to;
		flow = ar_flow;
		cap = ar_cap;
		rever =  ar_rever;
	}
	int residu() {
		return cap-flow;
	}
	void addflow(int ar_flow) {
		flow+=ar_flow;
		rever.flow -=ar_flow;
	}
}

public class Main {
	
	
	
	static int atoi(String a) {
		if(a.charAt(0)<='Z') {
			return a.charAt(0)-'A';
		}
		else {
			return a.charAt(0)-'a'+26;
		}
	}
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());		
		int n = Integer.parseInt(st.nextToken());
		
		int[] prev =  new int[52];
		ArrayList[] adj = new ArrayList[52];
		Queue<Integer> q =  new LinkedList<Integer>();
		int S = atoi("A");
		int E = atoi("Z");
		
		
		int u=0,v=0,w=0;
		int total = 0;
		
		for(int i=0;i<52;i++) {
			
			adj[i] = new ArrayList<edge>();
			
		}
		
		for(int i=0;i<n;i++) {
			
			st =  new StringTokenizer(br.readLine());
			u = atoi(st.nextToken());
			v = atoi(st.nextToken());
			w = Integer.parseInt(st.nextToken());
			edge temp_edge = new edge(v,0,w,null);
			edge temp_rever_edge = new edge(u,0,0,temp_edge);
			temp_edge.rever = temp_rever_edge;
			adj[u].add(temp_edge);
			adj[v].add(temp_rever_edge);
			
		}
		
		int curr = 0;
		edge next = null;
		int adj_size = 0;
		int flow = 0;
		
		while(true) {
			edge[] path = new edge[52];
			Arrays.fill(prev, -1);
			q.clear();
			
			q.add(S);
			
			curr = 0;
			next = null;
			adj_size = 0;
			
			while(!q.isEmpty()) { // 소스에서 흐를수 있는 유량찾기
				curr = q.poll();
				adj_size = adj[curr].size();
				for(int i=0;i<adj_size;i++) {
					next = (edge) adj[curr].get(i);
					if(next.residu() > 0 && prev[next.to] == -1) {
						q.add(next.to);
						prev[next.to] = curr;
						path[next.to] = next;
						if(next.to == E) break;
					}
				}
			}
			
			if(prev[E] == -1) break; // 경로를 탐색 한 후 싱크로 가는 경로가 없는 경우
		
			flow = Integer.MAX_VALUE;
			for(int i=E;i!=S;i=prev[i]) {
				flow =  Math.min(flow, path[i].residu());
			}
		
			for(int i=E;i!=S;i=prev[i]) {
				path[i].addflow(flow);
			}
		
			total +=flow;
			
		}
		
		System.out.println(total);
		
	}

}
```
