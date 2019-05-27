---
title: "프로그래머스 - 게임 맵 최단거리"
date: 2017-12-27T16:42:42+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

BFS로 구현했는데, 효율성 테스트에서 자꾸 시간초과가 나서 고민했던 문제

알고보니 중복방문을 방지하기 위한 코드가 없었음.. 방문 확인의 중요성!!


```
import java.util.LinkedList;
import java.util.Queue;
import java.awt.Point;

class Solution {

    public static int solution(int[][] maps) {
        int X = maps[0].length;
        int Y = maps.length;
        boolean[][] visited = new boolean[Y][X];
        Queue<Point> q = new LinkedList<Point>();
        int x = 0;
        int y = 0;
        int size = 0;
        int cnt = 0;
        Point p = new Point();
        q.add(new Point(Y-1,X-1));

        while(q.isEmpty()==false) {
            size = q.size();
            cnt++;
            for(int i=0;i<size;i++)
            {
                p = q.peek();
                x = p.y;
                y = p.x;
                q.remove();
                if(visited[y][x]==true)
                    continue;
                maps[y][x] = 0;
                visited[y][x] = true;
                if(x==0 && y==0) {
                    return cnt;
                }

                if(x-1>-1 && maps[y][x-1]==1) { //왼쪽 한칸
                    q.add(new Point(y,x-1));
                }

                if(x+1<X && maps[y][x+1]==1) { //오른쪽 한칸
                    q.add(new Point(y,x+1));
                }

                if(y-1>-1 && maps[y-1][x]==1) { //위쪽 한칸
                    q.add(new Point(y-1,x));
                }

                if(y+1<Y && maps[y+1][x]==1) { //아래쪽 한칸
                    q.add(new Point(y+1,x));
                }

            }

        }

        return -1;
    }
}

```
