---
title: "백준알고리즘 2747번 - 피보나치 수"
date: 2018-01-26T12:20:42+08:00
draft: false
tags: ["algorithm","피보나치수"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

피보나치 수를 구하는 가장 기초적인 알고리즘입니다.

$$ F\_n = F\_{n-1} + F\_{n-2} (n>=2) $$

점화식을 그대로 구현하여 재귀적으로 풀어내면 됩니다.

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
	
	static int fibonacci(int no) {
		if(no==0)
			return 0;
		else if(no==1)
			return 1;
		else
			return fibonacci(no-1)+fibonacci(no-2);
	}
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n = Integer.parseInt(st.nextToken());
		
		int result = fibonacci(n);
		System.out.println(result);		
		
		
		
	}

}
```
