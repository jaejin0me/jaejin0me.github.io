---
title: "프로그래머스 - 삼각형 출력하기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
public class PrintTriangle {
    public String printTriangle(int num){
        String ret = "";
        for(int i=0;i<num;i++) {
            for(int j=0;j<=i;j++) {
                ret+="*";
            }
            ret+="\n";
        }

        return ret;     
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        PrintTriangle pt = new PrintTriangle();
        System.out.println( pt.printTriangle(3) );
    }
}
```

아래와 같은 풀이가 있는데 참신하다. 당연히 2중 반복문으로 풀었는데 하나로 풀어내다니.. 배워야겠다.
```
public class PrintTriangle {
    public String printTriangle(int num){
    String result = "";
        String stars = "*";
        for(int i=0; i<num; ++i){
            result += stars+"\n";
            stars += "*";
        }
        return result;
    }

    // 아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String[] args) {
        PrintTriangle pt = new PrintTriangle();
        System.out.println( pt.printTriangle(3) );
    }
}
```
