---
title: "백준알고리즘 1546번 - 평균"
date: 2018-11-26T00:23:42+09:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

요즘 다시 시작한 알고리즘 공부..
쉬운거 부터 풀어 봅시다

```
#include <iostream>
#include <vector>
#include <algorithm>
#include <iomanip>

#pragma warning(disable:4996)

using namespace std;

int main(void) {
	int num, max, score;
	double sum = 0;
	vector<int> arr;
	vector<int>::iterator it;

	cin.tie(NULL);
	ios_base::sync_with_stdio(false);

#ifndef ONLINE_JUDGE
	freopen("input.txt", "r", stdin);
	freopen("output.txt", "w", stdout);
#endif

	cin >> num;
	for (int i = 0; i < num; i++) {
		cin >> score;
		arr.push_back(score);
	}

	max = *max_element(arr.begin(), arr.end());
	for (it = arr.begin(); it != arr.end(); it++) {
			sum += (*it) / (double)max * 100;
	}
	cout << fixed << setprecision(2);
	cout << sum / num;

#ifndef ONLINE_JUDGE
	fclose(stdin);
	fclose(stdout);
#endif

	return 0;
}
```
