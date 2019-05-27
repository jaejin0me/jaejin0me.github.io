---
title: "프로그래머스 - 하샤드 수"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

나의 풀이
```
import java.util.Arrays;

public class TryHelloWorld
{

    public int getMinSum(int []A, int []B)
    {
        int answer = 0;
        Arrays.sort(A);
        Arrays.sort(B);

        for(int i=0;i<B.length;i++) {
            answer += A[i]*B[B.length-1-i];
        }

        return answer;
    }
    public static void main(String[] args)
    {
        TryHelloWorld test = new TryHelloWorld();
        int []A = {1,2};
        int []B = {3,4};
        System.out.println(test.getMinSum(A,B));
    }
}
```
문자열로 형변환 없이 풀어낸 코드
```
public class HarshadNumber {
    public boolean isHarshad(int num){
/*
        String n = Integer.toString(num);
        // System.out.println(n);
          String arr[] = n.split("");
       int result = 0;
        for (int i=0; i<arr.length;i++){
        System.out.println(arr[i]);
          result += Integer.parseInt(arr[i]);
          // System.out.println(result);
        }
        if(num%result==0){
            return true;
            }else{
                return false;
            }
        */
int a = num;
  int b = 0;
    while(a!=0){
    b+=a%10;
      a=a/10;

    }


   return num%(a+b)==0; // a는 무조건 0이기 때문에 b만 적어도 될듯
  }

           // 아래는 테스트로 출력해 보기 위한 코드입니다.
        public static void  main(String[] args){
            HarshadNumber sn = new HarshadNumber();
            System.out.println(sn.isHarshad(18));
        }
}
```
