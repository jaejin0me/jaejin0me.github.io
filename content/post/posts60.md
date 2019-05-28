---
title: "백준알고리즘 2188번 - 축사배정"
date: 2018-01-28T12:20:42+09:00
draft: false
tags: ["algorithm","네트워크플로우"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

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
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());		
		int n = Integer.parseInt(st.nextToken());
		int m = Integer.parseInt(st.nextToken());
		
		int[] prev =  new int[402];
		ArrayList[] adj = new ArrayList[402];
		Queue<Integer> q =  new LinkedList<Integer>();
		int total = 0;
		
		for(int i=0;i<402;i++) {
			adj[i] = new ArrayList<edge>();
		}
		
		for(int i=0;i<n;i++) {
			edge temp_edge = new edge(i,0,1,null);
			edge temp_rever_edge = new edge(400,0,0,temp_edge);
			temp_edge.rever = temp_rever_edge;
			
			adj[400].add(temp_edge);
			adj[i].add(temp_rever_edge);
		}
		
		for(int i=200;i<200+m;i++) {
			edge temp = new edge(401,0,1,null);
			edge temp_rever_edge = new edge(i,0,0,temp);
			temp.rever = temp_rever_edge;
			adj[i].add(temp);
			adj[401].add(temp_rever_edge);			
		}
		
		int no = 0;
		int temp = 0;
		
		for(int i=0;i<n;i++) {
			
			st =  new StringTokenizer(br.readLine());
			no = Integer.parseInt(st.nextToken());
			for(int j=0;j<no;j++) {
				temp = Integer.parseInt(st.nextToken())+199;
				edge temp_edge = new edge(temp,0,1,null);
				edge temp_rever_edge = new edge(i,0,0,temp_edge);
				temp_edge.rever = temp_rever_edge;
				adj[i].add(temp_edge);
				adj[temp].add(temp_rever_edge);
			}
		}
		
		
		int curr = 0;
		edge next = null;
		int S = 400;
		int E = 401;
		
		while(true) {
			edge[] path = new edge[402];
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
		
			for(int i=E;i!=S;i=prev[i]) {
				path[i].addflow(1);
			}
		
			total += 1;
			
		}
		
		System.out.println(total);
		
	}

}
```
