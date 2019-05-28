---
title: "프로그래머스 - 폰켓몬"
date: 2017-12-27T16:42:42+08:00
draft: false
tags: ["algorithm"]
categories: ["algorithm"]
author: "Jaejin Jang"
---

문제를 잘 파악하면 어렵지 않은 문제

```
import java.util.HashSet;
import java.util.Iterator;

public class Solution {
    public int solution(int[] nums) {
    	HashSet<Integer> abc = new HashSet<Integer>();
    	
    	for(int i=0;i<nums.length;i++) {
    		abc.add(nums[i]);
    	}
    	  
    	if(abc.size()>nums.length/2)
    		return nums.length/2;
    	else
    		return abc.size();
        
    }
}
```
