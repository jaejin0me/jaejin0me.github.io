---
title: "백준알고리즘 1110번 - 더하기 사이클"
date: 2018-11-26T00:33:42+09:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
mathjax : true
---

```
#include <iostream>

#pragma warning(disable:4996)

using namespace std;

int main(void) {
	int num, tmp, cnt = 0;

	cin.tie(NULL);
	ios_base::sync_with_stdio(false);

#ifndef ONLINE_JUDGE
	freopen("input.txt", "r", stdin);
	freopen("output.txt", "w", stdout);
#endif

	cin >> num;
	tmp = num;
	do {
		tmp = ((tmp % 10) * 10) + (tmp%10 + tmp/10)%10;
		cnt++;
	} while (tmp != num);

	cout << cnt;

#ifndef ONLINE_JUDGE
	fclose(stdin);
	fclose(stdout);
#endif

	return 0;
}
```
