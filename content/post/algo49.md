---
title: "프로그래머스 - 2016년"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

Date 오브젝트 쓰기!
```
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

public class TryHelloWorld
{
    public String getDayName(int a, int b)
    {
        String answer = "";
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        SimpleDateFormat sdf2 = new SimpleDateFormat("E");
        String dateInString = "2016-"+a+"-"+b;
        Date date;
        try {
            date = sdf.parse(dateInString);
            answer = sdf2.format(date).toUpperCase();

        } catch (ParseException e) {
        } 

        return answer;
    }
    public static void main(String[] args)
    {
        TryHelloWorld test = new TryHelloWorld();
        int a=5, b=24;
        System.out.println(test.getDayName(a,b));      
    }
}
```
