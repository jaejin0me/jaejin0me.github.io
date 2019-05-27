---
title: "java.lang.Integer parseInt 메소드 분석"
date: 2017-12-23T01:37:56+08:00
draft: false
tags: ["java"]
categories: ["java"]
author: "Jaejin Jang"
---

스트링을 숫자로 변화해주는 parseInt 메소드는 

parseInt(String)
parseInt(String, int)

2가지가 존재합니다. 하지만 parseInt(String)의 경우 내부를 보면 parseInt(String,10)으로 구현되어 있기 때문에 parseInt(String, int)만 분석하면 됩니다.
변환할 문자열이 null이면 NumberFormatException이 납니다.
int형의 인자로 진법을 설정할 수가 있습니다. 최소 2진법에서 최대 36진법까지 변환할 수 있습니다. 범위를 넘어가게 되면 NumberFormatException 이 납니다.

코드를 분석하면서 아이러니 했던 것은 문자열이 양수인 경우에도 결과값을 -값으로 누적하면서 계산하게 됩니다. 그 이유는 max보다 min의 절대값 정수현 표현 범위가 1 크기 때문에 -로 누적하면서 계산하면 음수와 양수인 경우 모두 계산가능하기 때문입니다.

내부적으로 보면 각문자열을 digit = Character.digit(s.charAt(i++),radix); 를 이용해 변환하는 것을 확인할 수 있습니다. 그리고 범위를 벗어나는지를 확인하기 위해 3개의 if문이 옵니다. 
첫번째 if문의 경우 digit의 값을 검사하는 루틴으로 0미만의 값 리턴, 즉 -1 리턴을 검사하기 위함입니다. -1 리턴의 경우 Chracter.digit()메소드에서 실패를 의미합니다.
두번째 if문의 경우 범위 검사입니다. 양수와 음수에 상관없이 limit = -214748364 가 됩니다. 결과값이 limit보다 작아지는경우는 -214748365,-214748366,-214748367,-214748368... 등이 값이 오게 되는 데요 이 경우는 무조건 MAX나 MIN값을 벗어나게 되기 때문에 예외가 일어납니다.
세번째 if문의 경우 결과값이 앞의 자리는 양수인 경우 [-2147483647,-2147483640] 인지 음수인 경우 [-2147483648,-2147483640] 인지를 확인하는 루틴입니다. 2번째 루틴에서는 십의 자리를 통해 범위를 확인했다면 세번째 루틴에서는 일의 자리 범위를 확인하는 과정이라고 보시면 됩니다.

```
     public static int parseInt(String s, int radix)
                 throws NumberFormatException
     {
         if (s == null) { //문자열 인자가 널이면 예외 발생
             throw new NumberFormatException("null");
         }
 
         if (radix < Character.MIN_RADIX) { // 2진법 미만이면 예외 발생
             throw new NumberFormatException("radix " + radix +
                                             " less than Character.MIN_RADIX");
         }
 
         if (radix > Character.MAX_RADIX) {  //36 진법 초과이면 예외 발생
             throw new NumberFormatException("radix " + radix +
                                             " greater than Character.MAX_RADIX");
         }
 
         int result = 0;
         boolean negative = false;
         int i = 0, len = s.length();
         int limit = -Integer.MAX_VALUE;
         int multmin;
         int digit;
 
         if (len > 0) {
             char firstChar = s.charAt(0);
            if (firstChar < '0') { // Possible leading "-"
                 if (firstChar == '-') {
                     negative = true;
                    limit = Integer.MIN_VALUE;
                 } else
                     throw NumberFormatException.forInputString(s);
 
                 if (len == 1) // Cannot have lone "-"
                     throw NumberFormatException.forInputString(s);
                 i++;
             }
            multmin = limit / radix;
             while (i < len) {
                 // Accumulating negatively avoids surprises near MAX_VALUE
                digit = Character.digit(s.charAt(i++),radix);
                if (digit < 0) { // Chrater.digit() 메소드 실패인 경우
                     throw NumberFormatException.forInputString(s);
                }
                if (result < multmin) { // 십의 자리에서 범위를 벗어나는 경우
                     throw NumberFormatException.forInputString(s);
                }
                 result *= radix;
                if (result < limit + digit) { // 일의 자리에서 범위를 벗어나는 경우
                     throw NumberFormatException.forInputString(s);
                }
                 result -= digit;
             }
        } else {
             throw NumberFormatException.forInputString(s);
        }
         return negative ? result : -result;
     }
```
