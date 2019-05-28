---
title: "디자인 패턴 - 싱글턴 패턴"
date: 2018-01-01T11:31:42+09:00
draft: false
tags: ["디자인 패턴"]
categories: ["디자인 패턴"]
author: "Jaejin Jang"
---

## 싱글턴 패턴

인스턴스가 하나 만들어지고 어디서든지 인스턴스에 접근하기 위한 패턴

간단히 인스턴스에 접근하기 위한 get메소드, 기본적인 생성자, 인스턴스 멤버변수로 구성된다고 가정해보자.

```
public class singleton {

	private static singleton instance;

	private singleton(){}

	public static singleton getinstance(){

		if (instance == null){

			instance = new singleton();

		}

		return instance;

	}

 }
```

get메소드를 통해 인스턴스가 만들어져 있으면 리턴하고, 없으면 생성한다. 미리 만들어 놓지 않고 get메소드가 호출될 때 인스턴스가 만들어진다. 이것은 lazy instantiation 이라고 한다.
하지만 이 코드는 멀티스터드 상황에서 문제가 발생한다. 여러개의 쓰레드에서 동시에 get메소드를 호출하는 경우 동기화가 되지 않아 인스턴스가 여러개 생성된다.

이 문제를 해결하기 위한 방법은 다음과 같다.

1. get메소드 동기화

get메소드를 동기화 하는 것이다. 동기화하는 경우 메소드의 성능이 아주 느려지기 때문에 문제가 되지 않는 경우에만 사용해야 한다.

```
public static synchronized singleton getinstance(){
```

성능개선을 위한 방법으로 DCL(Double-Checking Locking)을 통해 동기화 부분을 get메소드의 더 내부로 넣는 방법이 있다.

```
if (instance == null){
                        synchronized (singleton.class){
                                  if(uinstance == null){
                                             instance = new singleton();
                                  }
                        }
		}
		return uniqueInstance;
```

널체크를 한번한후에 수행하기 때문에 get메소드에 동기화를 한 것보다는 동기화코드가 적게 불리게 된다.
동기화를 쓰면서 조금이나마 성능개선을 원하는 경우에 적합하다.

2. 인스턴스를 미리 만들어 둔다.

함수가 호출될때 만들지 말고 미리 만들어 두는 것이다. 나는 이 방법이 가장 괜찮다고 본다.
```
private static singleton instance = new singleton();
```
