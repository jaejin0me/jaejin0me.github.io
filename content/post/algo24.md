---
title: "프로그래머스 - 스트링을 숫자로 바꾸기"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

```
public class StrToInt {
    public int getStrToInt(String str) {

        return Integer.parseInt(str);
    }
    //아래는 테스트로 출력해 보기 위한 코드입니다.
    public static void main(String args[]) {
        StrToInt strToInt = new StrToInt();
        System.out.println(strToInt.getStrToInt("-1234"));
    }
}
```
직접 변환하는 것이 문제의 의도이거 같지만, 그냥 제공되는 메소드를 사용했다. 다음에 parseInt의 소스코드를 분석해 봐야겠다.
