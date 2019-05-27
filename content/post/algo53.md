---
title: "프로그래머스 - 사칙연산"
date: 2017-12-31T16:20:42+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

완전 초급용은 아닌 dp

이제 자신감이 좀 붙는고만




```
import java.awt.Point;
import java.util.HashMap;

class Solution {
	
	static int[] intarr;
	
	public static int solution(String arr[]) {
		HashMap<Point,Integer> dmax = new HashMap<Point,Integer>();
		HashMap<Point,Integer> dmin = new HashMap<Point,Integer>();
		int len = arr.length;
		int max_temp = 0;
		int min_temp = 0;
		int max = 0;
		int min = 0;
		
		// 숫자로 변환, 및 기본값 세팅
		intarr = new int[arr.length];
		for(int i=0;i<len;i++) {
			if(arr[i].equals("+")) {
				intarr[i] = 1001;	// 플러스는 1001로 치환
			}
			else if (arr[i].equals("-")) {
				intarr[i] = 1002;// 마이너스는 1002로 치환
			}
			else {
				intarr[i] = Integer.parseInt(arr[i]);
				dmax.put(new Point(i,i), intarr[i]);
				dmin.put(new Point(i,i), intarr[i]);
			}
		}
		
		for(int i=2;i<len;i+=2) { //하나의 연산을 포함하는 길이에서 입력의 모든 연산을 포함하는 길이가 될때까지, 길이의 증가는 +2씩(연산자 하나, 숫자하나)			
			
			for(int j=0;j+i<len;j+=2) { // 0자리 부터 시작해서 해당 순서의 길이까지 반복, 최대 길이를 넘지 않아야 함
				
				max_temp = 0;
				min_temp = 0;
				max = Integer.MIN_VALUE;
				min = Integer.MAX_VALUE;
				
				
				for(int k=j;k<i+j;k++) { // +,- 연산이 오는 경우 범위에서의 min과 max를 구한다.
					if(intarr[k]==1001) {
						max_temp = dmax.get(new Point(j,k-1)) + dmax.get(new Point(k+1,i+j));
						min_temp = dmin.get(new Point(j,k-1)) + dmin.get(new Point(k+1,i+j));
						if(max_temp>max)
							max = max_temp;
						if(min_temp<min)
							min = min_temp;
					}
					else if(intarr[k]==1002) {
						max_temp = dmax.get(new Point(j,k-1)) - dmin.get(new Point(k+1,i+j));
						min_temp = dmin.get(new Point(j,k-1)) - dmax.get(new Point(k+1,i+j));
						if(max_temp>max)
							max = max_temp;
						if(min_temp<min)
							min = min_temp;
					}

				}
				dmax.put(new Point(j,j+i), max);
				dmin.put(new Point(j,j+i), min);
			}
			
		}
		
		
		
		return dmax.get((new Point(0,len-1)));
	}
}
```
