---
title: "백준알고리즘 2316번 - 도시 왕복하기"
date: 2018-01-30T12:20:42+09:00
draft: false
tags: ["algorithm","네트워크플로우"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

어제 못 풀고 오늘에서야 푼 문제

정점을 1번만 지나는 조건을 만족하기 위해 정점을 분할하고 분할된 정점사이에 용량의 1의 간선을 추가해야 한다.

주의해야 할 것은 입력되는 간선들을 분할될 정점을 고려해 잘 이어주는 것이다.

```
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
	
	public static void main(String[] args) throws IOException {
		
		//정점 분할 필요
		//0~399 까지는 나눴을 떄 간선이 들어오는 정점, 400~799까지는 나눴을 때 간선이 나가는 정점

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());		
		int n = Integer.parseInt(st.nextToken());
		int p = Integer.parseInt(st.nextToken());
		
		int[] prev =  new int[800];
		ArrayList[] adj = new ArrayList[800];
		Queue<Integer> q =  new LinkedList<Integer>();
		int total = 0,u=0,v=0,u1=0,v1=0,temp=0,v2=0,u2=0;
        int flow =  0;
		
		for(int i=0;i<800;i++) {
			adj[i] = new ArrayList<edge>();
		}
		
		for(int i=0;i<n;i++) {
			u = i;
			v = i+400;

			edge temp_edge = new edge(v,0,1,null);
			edge temp_rever_edge = new edge(u,0,0,temp_edge);
			temp_edge.rever = temp_rever_edge;
			
			adj[u].add(temp_edge);
			adj[v].add(temp_rever_edge);
		}
		
				
		for(int i=0;i<p;i++) {
			
			st =  new StringTokenizer(br.readLine());
			u1 = Integer.parseInt(st.nextToken())-1+400;
			v1 = Integer.parseInt(st.nextToken())-1;
			
			u2 = u1-400;
			v2 = v1+400;
			
			edge temp_edge1 = new edge(v1,0,1,null);
			edge temp_rever_edge1 = new edge(u1,0,0,temp_edge1);
			temp_edge1.rever = temp_rever_edge1;
			
			adj[u1].add(temp_edge1);
			adj[v1].add(temp_rever_edge1);
			
			edge temp_edge2 = new edge(v2,0,0,null);
			edge temp_rever_edge2 = new edge(u2,0,1,temp_edge1);
			temp_edge2.rever = temp_rever_edge2;
			
			adj[u2].add(temp_edge2);
			adj[v2].add(temp_rever_edge2);		
			
		}
		
		int curr = 0;
		edge next = null;
		int S = 400; // 분할된 1번 정점의 나가는 곳
		int E = 1; // 분할된 2번 정점의 들어오는 곳
		
		while(true) {
			edge[] path = new edge[800];
			Arrays.fill(prev, -1);
			q.clear();
			q.add(S);
			curr = 0;
			next = null;
			
			while(!q.isEmpty()) { // 소스에서 흐를수 있는 유량찾기
				curr = q.poll();
				
				for(int i=0;i<adj[curr].size();i++) {
					
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
            
            flow =  Integer.MAX_VALUE;
            for(int i=E; i!=S; i=prev[i]) {
				flow = Math.min(flow,path[i].residu());
			}
		
			for(int i=E; i!=S; i=prev[i]) {
				path[i].addflow(flow);
			}
		
			total += flow;
			
		}
		
		System.out.println(total);
		
	}

}

```
