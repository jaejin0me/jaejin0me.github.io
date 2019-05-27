---
title: "자바 Set, Map"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["java"]
categories: ["java"]
author: "Jaejin Jang"
---

1.Set
```
  HashSet<Integer> abc = new HashSet<Integer>();
  abc.add(1);
  abc.add(2);
  abc.add(3);
  
  Iterator it = abc.iterator();
  Integer temp = 0;
  while(it.hasNext()) {
   temp = (Integer) it.next();
   System.out.println(temp);   
  }
  
  for(Integer item : abc) {
   System.out.println(item);
  }
 ```

2.Map
```
  HashMap<String,Integer> abc = new HashMap<String,Integer>();
  abc.put("a", 0);
  abc.put("b", 1);
  abc.put("c", 2);
  
  for(String temp:abc.keySet()) {
   System.out.println(temp);
  }
  
  if(abc.containsKey("a")) {
   System.out.println(abc.get("a"));
  }
  
  abc.clear();
  System.out.println(abc.size());
 ```
