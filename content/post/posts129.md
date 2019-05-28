---
title: "뉴럴네트워크"
date: 2017-12-24T01:37:56+09:00
draft: false
tags: ["neural network"]
categories: ["neural network"]
author: "Jaejin Jang"
---

[해커가 알려주는 뉴럴 네트워크](https://tensorflow.blog/2%EC%9E%A5-%EB%A8%B8%EC%8B%A0%EB%9F%AC%EB%8B%9D-%ED%95%B4%EC%BB%A4%EA%B0%80-%EC%95%8C%EB%A0%A4%EC%A3%BC%EB%8A%94-%EB%89%B4%EB%9F%B4-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/)를 공부한 내용

```
import sys
import random
from math import exp, floor


class rgate:

	def mul(x,y):
		return x*y
	def add(x,y):
		return x+y

class unit:
	value = 0.0
	grad = 0.0
	def __init__(self, value=0.0,grad=0.0):
		self.value = value
		self.grad = grad

class u:
	value = 0.0
	grad = 0.0
	def __init__(self, value=0.0,grad=0.0):
		self.value = value
		self.grad = grad

class mg:
	u0 = None
	u1 = None
	ru = None
	def __init__(self, u0=None,u1=None):
		self.u0 = None
		self.u1 = None 
		self.ru = None
	def f(self, u0, u1):
		self.u0 = u0
		self.u1 = u1
		self.ru = unit(self.u0.value*self.u1.value , 0)
		return self.ru
	def b(self):
		self.u0.grad +=  self.ru.grad * self.u1.value
		self.u1.grad +=  self.ru.grad * self.u0.value

class ag:
	u0 = None
	u1 = None
	ru = None
	def f(self,u0, u1):
		self.u0 = u0
		self.u1 = u1
		self.ru = unit(u0.value+u1.value,0)
		return self.ru
	def b(self):
		self.u0.grad +=  self.ru.grad*1
		self.u1.grad +=  self.ru.grad*1

class sg:
	u0 = None
	ru = None
	def sig(self, x):
		return 1/(1+exp(-x))
	def f(self,u0):
		self.u0 = u0
		self.ru = unit(self.sig(self.u0.value), 0)
		return self.ru
	def b(self):
		s = self.sig(self.u0.value)
		self.u0.grad += (s*(1-s))*self.ru.grad

def fc(a,b,c,x,y):
	return 1/(1+exp(-(a*x+b*y+c)))

class sqg:
	u0 = None
	ru = None
	def f(self,u0):
		self.u0 = u0
		self.ru = unit(self.u0.value*self.u0.value,0.0)
		return self.ru
	def b(self):
		self.u0.grad += 2*self.u0.value*self.ru.grad

class dg:
	u0 = None
	u1 = None
	ru = None
	def f(self,u0=unit(1.0,0.0),u1=unit(1.0,0.0)):
		self.u0 = u0
		self.u1 = u1
		self.ru = unit(self.u0.value/self.u1.value,0.0)
		return self.ru
	def b(self):
		self.u0.grad += (-1/(self.u0.value*self.u0.value))

class mag:#max gate
	u0 = None
	u1 = None
	ru = None
	def f(self,u0,u1):
		self.u0 = u0
		self.u1 = u1
		self.ru = unit(self.u0.value > self.u1.value and self.u0.value or self.u1.value , 0.0)
		return self.ru
	def b(self):
		self.u0.grad += self.ru.value == self.u0.value and 1.0*self.ru.grad or 0
		self.u1.grad += self.ru.value == self.u1.value and 1.0*self.ru.grad or 0

class rg:#ReLU gate
	u0 = None
	ru = None
	def f(self,u0):
		self.u0 = u0
		self.ru = unit(self.u0.value > 0 and self.u0.value or 0 , 0.0)
		return self.ru
	def b(self):
		self.u0.grad += self.ru.value > 0 and 1.0*self.ru.grad or 0

def sig(a):
	return 1/(1+exp(-a))

class cir:
	a = None
	b = None
	c = None
	x = None
	y = None
	mg1 = mg()
	mg2 = mg()
	ag1 = ag()
	ag2 = ag()
	r1 = None
	r2 = None
	r3 = None
	r4 = None

	def f(self,x ,y , a ,b ,c):
		self.x = x
		self.y = y
		self.a = a
		self.b = b
		self.c = c
		self.r1 = self.mg1.f(self.a ,self.x)
		self.r2 = self.mg2.f(self.b ,self.y)
		self.r3 = self.ag1.f(self.r1 ,self.r2)
		self.r4 = self.ag2.f(self.r3 ,self.c)
		return self.r4
	def back(self,gt):
		self.r4.grad = gt
		self.ag2.b()
		self.ag1.b()
		self.mg2.b()
		self.mg1.b()

class svm:
	a = u(1.0, 0.0)
	b = u(-2.0, 0.0)
	c = u(-1.0, 0.0)
	cir1 = cir()
	o = None

	def f(self,x,y):
		self.o = self.cir1.f(x, y, self.a ,self.b ,self.c)
		return self.o

	def back(self,label):
		self.a.grad = 0
		self.b.grad = 0
		self.c.grad = 0
		pull = 0.0
		if(label == 1 and self.o.value < 1):
			pull = 1
		if(label == -1 and self.o.value > -1 ):
			pull = -1
		self.cir1.back(pull)

	def lform(self,x,y,label):
		self.f(x,y)
		self.back(label)
		self.p()

	def p(self):
		ssize = 0.01
		self.a.value += ssize*self.a.grad
		self.b.value += ssize*self.b.grad
		self.c.value += ssize*self.c.grad
	def accu(self, datas,labels):
		num_correct = 0
		for i in range(6):
			x = u(datas[i][0],0.0)
			y = u(datas[i][1],0.0)
			true_label = labels[i]
			r = self.f(x,y)
			predicted_label = r.value > 0 and 1 or -1
			if(predicted_label == true_label):
				num_correct+=1

		return num_correct/6.0


if __name__=='__main__':
	#이진분류
	#데이터를 +1,-1로 구분하는것
	#데이터 포인트이 각 값을 특성(feature)라 하고 결과(+1,-1)을 레이블(lable)이라고 한다

	#목표 : 2차원 벡터(2개의 특성)를 입력받아 레이블을 예측하는 함수를 학습하는 것
	#함수의 파라미털ㄹ 조정해 정확한 레이블이 나오도록하는 것이다

	#훈련방법
	#복잡한 수식은 어려우니 어려운 단순한 선형분류로 시작한다
	#f(x,y) = ax+by+c
	#x,y를 입력(2D 벡터,특성) a,b,c를 학습시킬 함수의 파라미터로 생각한다

	#1.무작위로 데이터 포인트를 선택해 입력한다
	#2.회로의 출력을 통해 레이블을 결정한다
	#3.예측값이 레이블에 얼마나 잘맞는지 측정한다
	#4.회로에 힘을 가하고, a,b,c에 역전파 시켜 입력값을 변경시킨다. x,y는 데이터이므로 역전파로 값을 업데이트할 필요가 없다
	#5.반복한다

	#SVM(Support Vector Machine)
	#매우 인기있는 선형분류 알고리즘이다.
	#함수의 형태는 f(x,y) = ax+by+c로 똑같다

	#쉬운이해를 위해 포스 명세서라고 하자. 전통적으로는 사용하지 않는 용어이다.
	#역전파에 의한 출력증가 외에 a,b(c는 제외)에 0의 방향으로 당기는 힘을 추가하는 것이다.
	#0으로 돌아가고자 하는 탄성력을 추가하는 것과 같다. 힘의 크기는 lal, lbl 에 비례한다. 훅의 법칙과 유사하다.
	
	#학습을 위해 여러 차레 반복이 필요하다.
	#모델의 복잡도, 초기화, 데이터의 정규화, 스텝 사이즈에 따라 학습시간이 달라진다.


	# f(x,y) = a*x + b*y +c
	print("f(x,y) = a*x + b*y +c")

	#input def
	a = u(1.0,0.0)
	b = u(2.0,0.0)
	x = u(3.0,0.0)
	y = u(4.0,0.0)
	c = u(5.0,0.0)


	#gate def
	mg1 = mg()
	mg2 = mg()
	ag1 = ag()
	ag2 = ag()
	
	#cal f
	r1 = mg1.f(a,x)
	r2 = mg2.f(b,y)
	r3 = ag1.f(r1,r2)
	r4 = ag2.f(r3,c)

	#cal b
	r4.grad = 1.0
	ag2.b()
	ag1.b()
	mg1.b()
	mg2.b()

	#print derivative
	da = mg1.u0.grad; print(da)
	db = mg2.u1.grad; print(db)
	dc = ag2.u1.grad; print(dc)
	print('------')

	# svm code
	print("SVM code")

	#data def
	data = []
	data.append([1.2, 0.7])
	data.append([-0.3, -0.5])
	data.append([3.0, 0.1])
	data.append([-0.1, -1.0])
	data.append([-1.0, 1.1])
	data.append([2.1, -3])

	label = []
	label.append(1)
	label.append(-1)
	label.append(1)
	label.append(-1)
	label.append(-1)
	label.append(1)

	#svm def
	svm1 = svm()

	for i in range(400):
		r = floor(random.random()*6)
		x = u(data[r][0],0.0)
		y = u(data[r][1],0.0)
		t_label = label[r]
		svm1.lform(x , y, t_label)
		if(i%25 == 0 ):
			print("{0}번째 훈련정확도는 {1}",i,svm1.accu(data,label))

	#이 회로는 선형함수 뿐만 아니라 어떤 수식도 적용될 수 있다.

	# not structed svm code
	print("not structed svm code")

	#data def
	data = []
	data.append([1.2, 0.7])
	data.append([-0.3, -0.5])
	data.append([3.0, 0.1])
	data.append([-0.1, -1.0])
	data.append([-1.0, 1.1])
	data.append([2.1, -3])

	label = []
	label.append(1)
	label.append(-1)
	label.append(1)
	label.append(-1)
	label.append(-1)
	label.append(1)

	a = 1; b = -2; c = -1

	for i in range(400):
		r = floor(random.random()*6)
		x = data[r][0]
		y = data[r][1]
		t_label = label[r]
		score = a*x+b*y+c
		pull = 0
		if(label == 1 and score < 1):
		   pull = 1
		if(label == -1 and score > -1):
		   pull = -1

		step_size = 0.01
		a += step_size*(x*pull-a)
		b += step_size*(y*pull-b)
		c += step_size*(1*pull)

	#힘의 크기 pull이 0, +1, -1 인것을 알 수 있다. 다르게 오차의 크게 비례하여 크기를 다르게 할 수도이다.
	#훈련 데이터의 특징에 따라 비례하여 힘을 주는 것이 좋을 수도 나쁠수도 있다.
	#데이터에 이상치(outlier)가 있다면 비례하여 힘을 줄경우 이상값에 의해 파라미터의 변화가 커진다. 하지만 고정값 -1,+1,0으로 줄경우
	#큰 영향을 받지 않으므로 잘 견딘다고(robustness) 할 수 있다.


	#SVM을 뉴럴 네트워크로 일반화 하기
	#SVM은 매우 간단한 함수 이다. 이 회로를 2개의 레이어를 가진 뉴럴 네트워크로 확장시켜 보자.


	a1 = random.random()-0.5
	a2 = random.random()-0.5
	a3 = random.random()-0.5
	a4 = random.random()-0.5
	b1 = random.random()-0.5
	b2 = random.random()-0.5
	b3 = random.random()-0.5
	b4 = random.random()-0.5
	c1 = random.random()-0.5
	c2 = random.random()-0.5
	c3 = random.random()-0.5
	c4 = random.random()-0.5
	d4 = random.random()-0.5

	for i in range(400):
		r = floor(random.random()*6)
		x = data[r][0]
		y = data[r][1]
		l = label[r]

		n1 = 0 > a1*x+b1*y+c1 and 0 or a1*x+b1*y+c1
		n2 = 0 > a2*x+b2*y+c2 and 0 or a2*x+b2*y+c2
		n3 = 0 > a3*x+b3*y+c3 and 0 or a3*x+b3*y+c3
		score = a4*n1 + b4*n2 + c4*n3 + d4

		pull = 0.0
		if(l == 1 and score < 1):
			pull = 1
		if(l == -1 and score > -1):
			pull = -1

		dscore = pull
		da4 = n1*dscore
		dn1 = a4*dscore
		db4 = n2*dscore
		dn2 = b4*dscore
		dc4 = n3*dscore
		dn3 = c4*dscore
		dd4 = 1.0*dscore

		dn3 = n3 == 0 and 0 or dn3
		dn2 = n2 == 0 and 0 or dn2
		dn1 = n3 == 0 and 0 or dn1

		da1 = x * dn1
		db1 = y * dn1
		dc1 = 1.0 * dn1

		da2 = x * dn2
		db2 = y * dn2
		dc2 = 1.0 * dn2

		da3 = x * dn3
		db3 = y * dn3
		dc3 = 1.0 * dn3

		da1 += -a1; da2 += -a2; da3 += -a3;
		db1 += -b1; db2 += -b2; da3 += -b3;
		da4 += -a4; db4 += -b4; dc4 += -c4;

		step_size = 0.01
		a1 += step_size * da1;
		b1 += step_size * db1; 
		c1 += step_size * dc1;
		a2 += step_size * da2; 
		b2 += step_size * db2;
		c2 += step_size * dc2;
		a3 += step_size * da3; 
		b3 += step_size * db3; 
		c3 += step_size * dc3;
		a4 += step_size * da4; 
		b4 += step_size * db4; 
		c4 += step_size * dc4; 
		d4 += step_size * dd4;


	#전통적인 방법
	#포스명세서라 하지 않고 손실함수(목정함수,비용함수)라고 부른다
	#2d 서포트벡터 머신의 손실함수는 참고사이트에서 확인
	X = [[1.2,0.7],[-0.3,0.5],[3,2.5]]
	y = [1,-1,1]
	w = [0.1,0.2,0.3]
	alpha = 0.1

	total_cost = 0.0
	for i in range(len(X)):
		xi = X[i]
		score = w[0]*xi[0] + w[1]*xi[1]+w[2]
		yi = y[i]
		costi = 0 > -yi*score+1 and 0 or -yi*score+1
		print('example {0} : xi = ({1}) and label = {2}',i,xi,yi)
		print('  score computed to be {0}',score)
		print('  => cost computed to be {0}',costi)
		total_cost += costi

	reg_cost = alpha*(w[0]*w[0]+w[1]*w[1])
	print('regularzization cost for current model is {0}',reg_cost)
	total_cost += reg_cost

	print('total cost is {0}',total_cost)
```
