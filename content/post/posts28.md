---
title: "프로그래머스 - 최대값과 최소값"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
import java.util.Vector;

public class GetMinMaxString {
    public String getMinMaxString(String str) {
        Integer min=Integer.MAX_VALUE,max=Integer.MIN_VALUE;
        String[] sepstr = str.split(" ");
        Vector<Integer> vec = new Vector<Integer>();
        for(int i=0;i<sepstr.length;i++) {
            vec.addElement(Integer.parseInt(sepstr[i]));
        }
        for(int i=0;i<vec.size();i++) {
            if(vec.get(i)<min) {
                min = vec.get(i);
            }
            if(vec.get(i) > max) {
                max = vec.get(i);
            }
        }

        return min+" "+max;
    }

    public static void main(String[] args) {
        String str = "1 2 3 4";
        GetMinMaxString minMax = new GetMinMaxString();
        //아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println("최대값과 최소값은?" + minMax.getMinMaxString(str));
    }
}
```
아래와 같은 풀이가 있다. 좋은 방법인듯
```
public class GetMinMaxString {
    public String getMinMaxString(String str) {
        String[] tmp = str.split(" ");
        int min, max, n;
        min = max = Integer.parseInt(tmp[0]);
        for (int i = 1; i < tmp.length; i++) {
                n = Integer.parseInt(tmp[i]);
            if(min > n) min = n;
            if(max < n) max = n;
        }

        return min + " " + max;

    }

    public static void main(String[] args) {
        String str = "1 2 3 4";
        GetMinMaxString minMax = new GetMinMaxString();
        //아래는 테스트로 출력해 보기 위한 코드입니다.
        System.out.println("최대값과 최소값은?" + minMax.getMinMaxString(str));
    }
}
```
