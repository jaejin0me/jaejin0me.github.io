---
title: "백준알고리즘 1929번 - 소수 구하기"
date: 2018-01-26T12:20:42+08:00
draft: false
tags: ["algorithm","소수구하기"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

주어진 두 정수 범위내의 소수를 구하는 문제입니다.

에라토스테네스의 체 라는 것을 사용해 구합니다.

개념적으로 주의해야 할 것은 소수 판별하는 것과 다릅니다. 에라토스테네스의 체 는 특점 범위의 소수를 구할때 쓰는 알고리즘입니다.

소수를 판별할때는 2~n-1로 $$\sqrt{n}$$ 까지 나눠보는 방법이 더 효율적입니다.


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
	
	static int[] arr = new int[1000001];
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n1 = Integer.parseInt(st.nextToken());
		int n2 = Integer.parseInt(st.nextToken());
		
		for(int i=0;i<=n2;i++) {
			arr[i] = i;			
		}
		arr[0] = 0;
		arr[1] = 0;
		
		for (int i=2; i*i<=n2; i++)
	    {
			if(arr[i]!=0) {
				for(int j=i+i;j<=n2;j+=i)
					arr[j] = 0;
			}
	    }
		
		for(int i=n1;i<=n2;i++) {
			if(arr[i]!=0)
				System.out.println(arr[i]);
		}
		
		
	}

}
```
