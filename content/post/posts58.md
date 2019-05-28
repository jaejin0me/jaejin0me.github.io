---
title: "백준알고리즘 2609번 - 최대공약수와 최소공배수"
date: 2018-01-26T12:20:42+08:00
draft: false
tags: ["algorithm","최대공약수","최소공배수"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

두 자연수의 최대 공약수와, 최소 공배수를 출력하는 문제입니다.

유클리드 호제법을 이용해 최대공약수를 구하고 최대공야수를 이용해 최소 공배수를 구하면 됩니다.

최소 공배수만 구하라고 하는 경우에도 최대공약수를 구하여 최소공배수를 구하는 것이 쉽습니다.

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


public class Main {
	
	static int euc(int a,int b) {
		if(b>a) {
			int temp = a;
			a = b;
			b= temp;
		}
		
		return a%b == 0 ? b : euc(b , a%b);		
		
	}
	
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n1 = Integer.parseInt(st.nextToken());
		int n2 = Integer.parseInt(st.nextToken());
		
		int gcf = euc(n1,n2);
		int lcm = n1*n2/gcf;
		
		System.out.println(gcf);
		System.out.println(lcm);
		
	}

}
```
