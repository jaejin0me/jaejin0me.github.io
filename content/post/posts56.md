---
title: "백준알고리즘 1978번 - 소수 찾기"
date: 2018-01-26T12:20:42+08:00
draft: false
tags: ["algorithm","소수판별"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

주어지는 수들 중에서 소수의 개수를 출력하는 문제입니다.

소수를 판별하는 가장 기초적인 방법인 2~n-1 까지 나눠지는지 확인해서 판별하면 됩니다.

또한 $$\sqrt{n}$$ 까지 나눠지는 약수가 없으면 소수 라는 성질을 이용해 계산량을 줄일 수 있습니다.

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
	
	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		//BufferedReader br = new BufferedReader( new FileReader("input.txt" ) );
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n = Integer.parseInt(st.nextToken());
		int temp = 0;
		int cnt=0;
		st = new StringTokenizer(br.readLine());
		
		loop1: while(st.hasMoreTokens()==true) {
			temp = Integer.parseInt(st.nextToken());
			if(temp==1 || temp==0)
				continue loop1;
			for(int i=2;i*i<=temp;i++) { // N^1/2 까지만 계산
				if(temp%i==0)
					continue loop1;
			}
			cnt++;
		}
		
		System.out.println(cnt);
		
		
	}

}
```
