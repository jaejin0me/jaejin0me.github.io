---
title: "백준알고리즘 11495번 - 격자 0 만들기"
date: 2018-02-04T12:20:42+09:00
draft: false
tags: ["algorithm","네트워크플로우"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

시간초과가 계속해서 나는 것은 최적화 하면서 근근이 풀긴했는데, 속도가 많이 느리다. 자바로 풀 사람들 중에서 꼴등임 ㅎ

다른 정답자들의 풀이를 보니, 나는 에드몬드 카프 알고리즘을 이용해 풀었지만 빠르게 푼 사람들은 "디닉 알고리즘"이라는 것을 이용해서 풀었다.

디닉 알고리즘이 더 나은 시간복잡도를 갖기 때문에 시간차이가 나는 것은 당연한 결과!

아무튼 푸는 방법은, 격자의 입력을 두 그룹으로 나눈다. 나누는 방법은 이웃(상,하,좌,우)한 격자점은 다른 그룹으로 두면 된다.

A B A B A  
B A B A B  
A B A B A  

이런 방식으로

한 그룹은 Source->A로 이어 주고, 다른 한 그룹은 B->SinK로 간선을 연결해준다. 간선의 용량은 해당 격자점에 주어진 정수이다.

그리고 이웃한 격자점끼리는 Source에 연결된 그룹에서 Sink에 연결된 그룹으로(A->B) 간선을 이어준다. 용량은 무제한으로 볼 수 있을 만큼 넉넉히 잡아준다.

이후에 최대 유량 F를 찾고, 격자점에 적힌 정수를 모든 합한 S에서 빼주면 답이 된다. S-F 

```
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;
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
		
		int K=0,N=0,M=0;
        BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
        //BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
        StringTokenizer st = new StringTokenizer(br.readLine());
        ArrayList<edge>[] adj = new ArrayList[2502];
        int[] grid = new int[2502];
        int[] prev = new int[2502];
        edge f_edge = null;
        edge b_edge = null;
		
		K = Integer.parseInt(st.nextToken());
		
		for(int i=0;i<K;i++) { //테스트 케이스 K번
			
			st = new StringTokenizer(br.readLine());
			N = Integer.parseInt(st.nextToken());
			M = Integer.parseInt(st.nextToken());
			Arrays.fill(grid, -1);
			int s = 0;
			int S = N*M;
			int E = N*M+1;
			int temp_curr = 0;
			
			for(int j=0;j<N*M+2;j++) {
				adj[j] = new ArrayList<edge>();
			}
			
			for(int j=0;j<N;++j) { // 엣지 입력 받기
				
				st = new StringTokenizer(br.readLine());
				
				for(int k=0;k<M;++k) {
					
					temp_curr = j*M+k;
					grid[temp_curr] = Integer.parseInt(st.nextToken());		
					s+=grid[temp_curr];
					
					//시작점 셋팅
					if((j+k)%2==0) {
					
						f_edge = new edge(temp_curr,0,grid[temp_curr],null);
						b_edge = new edge(S,0,0,f_edge);
						f_edge.rever = b_edge;
						adj[S].add(f_edge);
						adj[temp_curr].add(b_edge);
						
						//오른쪽 엣지
						if(k<M-1) {
							f_edge = new edge(temp_curr+1,0,1000,null);
							b_edge = new edge(temp_curr,0,0,f_edge);
							f_edge.rever = b_edge;
							adj[temp_curr].add(f_edge);
							adj[temp_curr+1].add(b_edge);
						}
						//왼쪽 엣지
						if(k>0) {
							f_edge = new edge(temp_curr-1,0,1000,null);
							b_edge = new edge(temp_curr,0,0,f_edge);
							f_edge.rever = b_edge;
							adj[temp_curr].add(f_edge);
							adj[temp_curr-1].add(b_edge);
						}
					
						//위쪽 엣지
						if(j>0) {
							f_edge = new edge((j-1)*M+k,0,1000,null);
							b_edge = new edge(temp_curr,0,0,f_edge);
							f_edge.rever = b_edge;
							adj[temp_curr].add(f_edge);
							adj[(j-1)*M+k].add(b_edge);
						
						}
					
						//아래쪽 엣지
						if(j<N-1) {
							f_edge = new edge((j+1)*M+k,0,1000,null);
							b_edge = new edge(temp_curr,0,0,f_edge);
							f_edge.rever = b_edge;
							adj[temp_curr].add(f_edge);
							adj[(j+1)*M+k].add(b_edge);
						}
						
					}
						
					// 도착점 셋팅
					else {

							f_edge = new edge(E,0,grid[temp_curr],null);
							b_edge = new edge(temp_curr,0,0,f_edge);
							f_edge.rever = b_edge;
							adj[temp_curr].add(f_edge);
							adj[E].add(b_edge);
						
					}
					
				}
			}
			
			
			int curr = 0;
			edge next =  null;
			int total = 0;
			
			while(true) {
				Arrays.fill(prev, -1);
				edge[] path = new edge[2502];
				Queue<Integer> q = new LinkedList<Integer>();
				q.add(S);
				
				while(!q.isEmpty()) {
					curr = q.poll();
					
					for(int j=0;j<adj[curr].size();j++) {
						
						next = (edge)adj[curr].get(j);
						
						if(next.residu() > 0 && prev[next.to] == -1) {
							
							q.add(next.to);
							prev[next.to] = curr;
							path[next.to] = next;
							if(next.to == E) break;
							
						}
						
					}
					
				}
				
				if(prev[E] == -1) break;
				
				int flow = Integer.MAX_VALUE;
				
				for(int j=E; j!=S; j=prev[j])
					flow = Math.min(flow, path[j].residu());
					
				for(int j=E; j!=S; j=prev[j]) {
					path[j].addflow(flow);
				}
				
				total += flow;
				
			}
			
			System.out.println(s-total);
			
		}
		
	}
}
```
