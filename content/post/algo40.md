---
title: "백준알고리즘 1991번, 트리 사용하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

그래프와 트리쪽 알고리즘이 약해서 요즘 이 파트를 공부하고있습니다.
2진 트리의 원소를 입력받아 순회 종류별로 출력해주는 문제입니다.
재귀적으로 깔끔하게 푸는 것이 중요하겠죠!
```
#include <stdio.h>
#include <string.h>
#include <vector>


using namespace std;


class BTreeNode {
public:
 char elem;
 int lindex;
 int rindex;
 BTreeNode()
 {
  BTreeNode(0, 0, 0);
 }

 BTreeNode(char c, int n1, int n2):elem(c),lindex(n1),rindex(n2)
 {
 }
};

void Preorder(BTreeNode node);

void Inorder(BTreeNode node);

void Postorder(BTreeNode node);

BTreeNode bt[26];

int main(void) {
 memset(bt, 0, sizeof bt);
 int t;
 char c1, c2, c3;

#ifndef ONLINE_JUDGE
 freopen("input.txt", "r", stdin);
 freopen("output.txt", "w", stdout);
#endif

 scanf("%d\n", &t);
 for (int i = 0; i < t; i++) {
   scanf("%c %c %c\n", &c1, &c2, &c3);
   bt[c1-'A'].elem = c1; bt[c1 - 'A'].lindex = c2 - 'A'; bt[c1 - 'A'].rindex = c3 - 'A';
   if (c2 == '.')
    bt[c1 - 'A'].lindex = -1;
   if (c3 == '.')
    bt[c1 - 'A'].rindex = -1;
 }

 Preorder(bt[0]);
 puts("");
 Inorder(bt[0]);
 puts("");
 Postorder(bt[0]);
 
#ifndef ONLINE_JUDGE
 fclose(stdin);
 fclose(stdout);
#endif

 return 0;
 
}

void Preorder(BTreeNode node) {
 printf("%c", node.elem); 
 if (node.lindex != -1)
  Preorder(bt[node.lindex]);
 if (node.rindex != -1)
  Preorder(bt[node.rindex]);
}

void Inorder(BTreeNode node) {
 if (node.lindex != -1)
  Inorder(bt[node.lindex]);
 printf("%c", node.elem);
 if (node.rindex != -1)
  Inorder(bt[node.rindex]);
}

void Postorder(BTreeNode node) {
 if (node.lindex != -1)
  Postorder(bt[node.lindex]);
 if (node.rindex != -1)
  Postorder(bt[node.rindex]);
 printf("%c", node.elem);
}
```
