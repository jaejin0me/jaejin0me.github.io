---
title: "백준알고리즘 2748번 - 피보나치 수 2"
date: 2018-01-26T12:20:42+08:00
draft: false
tags: ["algorithm","피보나치수"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

피보나치 수 두번째 입니다.

DP로 푸는 문제입니다.

점화식으로 풀때는 Top-down 방식이지만 DP로 풀때는 Bottom-up 방식입니다.

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
public class Main {
	
	static long[] arr = new long[91];
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n = Integer.parseInt(st.nextToken());
		arr[1] = 1;
		
		for(int i=2;i<=n;i++) {
			arr[i] = arr[i-1] + arr[i-2];
		}
		
		System.out.println(arr[n]);
		
		
	}

}
```
