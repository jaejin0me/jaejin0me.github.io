---
title: "카카오 코드 페스티벌 예선1번, 카카오 프렌즈 컬러링북"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

처음으로 참가한 알고리즘 대회인데, 이 대회 덕분에 알고리즘 공부를 시작하게 되었죠.
본선 진출 1문제 차이로 탈락했었습니다.

이 문제는 1번 문제인데요, 행렬로 주어진 그림에서 영역(상하좌우에서 같은 색으로 된 부분은 같은 영역)의 갯수와 최대 영역의 크기를 구하는 문제입니다. 재귀 문제입니다.
flood_fill 알고리즘을 이용해서 푸시면 됩니다.
```
#include <vector>

using namespace std;

// 전역 변수를 정의할 경우 함수 내에 초기화 코드를 꼭 작성해주세요.
int M;
int N;
vector<vector<int>> Pic;

int flood_fill(int m, int n,int col)
{
 int sum = 0;
 if (col == 0) { // 색상이 다르면 종료
  return 0;
 }
 if (n > 0 && col == Pic[m][n - 1] ) // left, 색상이 같으면 재귀적으로 진행
 {
  Pic[m][n] = 0; // 해당 영역은 색상을 없애고
  sum += flood_fill(m, n - 1, col); // 왼쪽으로 수행
 }
 if (n <  N - 1 && col == Pic[m][n+1] ) // right
 {
  Pic[m][n] = 0;
  sum += flood_fill(m, n + 1, col);
 }
 if (0 < m && col == Pic[m - 1][n]) //above
 {
  Pic[m][n] = 0;
  sum += flood_fill(m - 1, n, col);
 }
 if (m < M - 1 && col == Pic[m+1][n]) // below
 {
  Pic[m][n] = 0;
  sum += flood_fill(m + 1, n, col);
 }
 
 Pic[m][n] = 0;
 return 1+sum;
}

vector<int> solution(int m, int n, vector<vector<int>> picture) {
    
 int cnt_area, max_area, temp;
    Pic.clear();
    Pic = picture;
 M = m;
 N = n;
 cnt_area = 0;
 max_area = 0;
 temp = 0;
 vector<int> answer;

 for (int i = 0; i<m; i++) {

  for (int j = 0; j<n; j++)
  {
   max_area = flood_fill(i, j, Pic[i][j]); // 최대 크기값을 반환
   if (max_area != 0) // 색상 영역이 존재하는 경우
   {
    cnt_area++; // 영역 카운트+
    if (temp < max_area) // max값인지 확인
     temp = max_area;
    max_area=0;
   }
   
  }
 }

 answer.push_back(cnt_area);
 answer.push_back(temp);
    
    return answer;
}
```
