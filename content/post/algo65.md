---
title: "백준알고리즘 4344번 - 평균은 넘겠지"
date: 2018-11-26T00:30:42+09:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

제목 재밌네

```
#include <iostream>
#include <vector>
#include <algorithm>
#include <iomanip>

#pragma warning(disable:4996)

using namespace std;

int main(void) {
	int num, casenum, score, cnt;
	double sum, avg;
	
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
		cin >> casenum;
		sum = 0;
		arr.clear();
		avg = 0;
		cnt = 0;
		for (int j = 0; j < casenum; j++) {
			cin >> score;
			arr.push_back(score);
			sum += score;
		}
		avg = sum / casenum;
		for (it = arr.begin(); it != arr.end(); it++) {
			if ((*it) > avg) cnt++;
		}
		cout << fixed << setprecision(3);
		cout << (double)cnt/casenum*100 << "%\n";
	}

#ifndef ONLINE_JUDGE
	fclose(stdin);
	fclose(stdout);
#endif

	return 0;
}
```
