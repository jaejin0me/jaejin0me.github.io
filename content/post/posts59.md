---
title: "자바에서 입력 빠르게 받기"
date: 2018-01-26T12:20:42+09:00
draft: false
tags: ["algorithm","입력빠르게받기"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

Scanner는 성능이 떨어지기 때문에 입력이 많은 경우 문제가 생깁니다.

BufferedReader와 StringTokenizer를 이용해 입력받는 것을 습관화 시키는게 좋습니다.

## 1. 정수 입력 하나 받는 경우

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;


public class Main {

	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n1 = Integer.parseInt(st.nextToken());	
		// To do //
	}
}
```

## 2. 한 줄에 임의 개의 입력을 받는 경우

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;


public class Main {

	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		StringTokenizer st = new StringTokenizer(br.readLine());

		while(st.hasMoreTokens()) {
			
			int temp = Integer.parseInt(st.nextToken());
			// To do //
		}
	}
}
```

## 3. 첫줄에 테스트 횟수, 이후에 N번 입력을 받는 경우 (1번과 2번을 조합하면 됨)

```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;


public class Main {

	public static void main(String[] args) throws IOException {

		BufferedReader br = new BufferedReader( new InputStreamReader( System.in ) );
		StringTokenizer st = new StringTokenizer(br.readLine());
		int n1 = Integer.parseInt(st.nextToken());
		int temp = 0;

		for(int i=0;i<n1;i++) {
			st = new StringTokenizer(br.readLine());
			while(st.hasMoreTokens()) {
				temp = Integer.parseInt(st.nextToken());
				// To do //
			}
		}
	}
}
```
