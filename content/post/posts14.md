---
title: "프로그래머스 - 시저 암호"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```java
public class Caesar {
    String caesar(String s, int n) {
    n%=26;
        String result = "";
        char[] temp = s.toCharArray();
        for(int i=0;i<temp.length;i++) {
            if(temp[i]>='a' &&temp[i]<='z')
                temp[i] = (char) ((temp[i]+n)>'z' ? (temp[i]+n-1)%'z'+'a' : (temp[i]+n));  
            else if(temp[i]>='A' &&temp[i]<='Z')
                temp[i] = (char) ((temp[i]+n)>'Z' ? (temp[i]+n-1)%'Z'+'A' : (temp[i]+n));
            result+=temp[i];
        }

        return result;
    }

    public static void main(String[] args) {
        Caesar c = new Caesar();
        System.out.println("s는 'a B z', n은 4인 경우: " + c.caesar("a B z", 4));
    }
}
```
알파벳의 대소문자를 조건문의 범위로 확인하는 방법 말고 Character.isLowerCase()가 있음<br>
삼항 연산자를 사용한 라인에서<br>
temp[i] = (char) ((temp[i] - 'a' + n) % 26 + 'a'); <br>
로 하면 분기를 줄일 수 있음
