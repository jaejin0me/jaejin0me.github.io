---
title: "실수치 회로, 역전파"
date: 2017-12-23T01:37:56+09:00
draft: false
tags: ["neural network"]
categories: ["neural network"]
author: "Jaejin Jang"
---

[해커가 알려주는 뉴럴 네트워크](https://tensorflow.blog/1%EC%9E%A5-%EC%8B%A4%EC%88%98%EC%B9%98-%ED%9A%8C%EB%A1%9C-%ED%95%B4%EC%BB%A4%EA%B0%80-%EC%95%8C%EB%A0%A4%EC%A3%BC%EB%8A%94-%EB%89%B4%EB%9F%B4-%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/)를 공부한 내용

```
import sys
import random
from math import exp

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
		self.ru = unit(u0.value*u1.value,0)
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

if __name__=='__main__':
	"""
	#Random Local Search,출력을 임의로 변화시켜 더 나은 출력을 찾는다
	print(rgate.mul(1,2))
	x = -2
	y = 3
	best_x = x
	best_y = y
	x_try=0
	y_try=0
	tweak_amout = 0.01
	out = 0;
	best_out = -sys.maxsize-1
	for each in range(100):
		print(each)
		x_try = x + tweak_amout*(random.random()*2-1)
		y_try = y + tweak_amout*(random.random()*2-1)
		out = rgate.mul(x_try,y_try)
		if(out>best_out):
			best_out = out
			best_x = x_try
			best_y = y_try
	print(best_x,best_y,best_out)
	"""
	"""
	#Numerical Gradient, 간단한 계산을 통해 기울기를 찾고 더 나은 출력을 도출해낸다
	#기울기는 더 나은 출력을 위한 최선의 방향을 의미한다
	#단일 뉴런으로 볼때는 큰 스텝이 좋은 출력을 내지만, 복잡하게 꼬여있는 경우 스텝이 크면 예상을 벗어나는 값이 나올 수 있다.
	#스텝의 크기는 눈을 가리고 언덕을 오를때의 보폭의 크기로 비유할 수 있다. 작으면 느리지만 확실하게 언덕을 오를 수 있지만, 크면 빠를수 있지만 다칠수 있다.
	x =-2
	y = 3
	out = rgate.mul(x,y)
	h = 0.0001
	x_derivative = (rgate.mul(x+h,y)-out)/h
	y_derivative = (rgate.mul(x,y+h)-out)/h
	print(x_derivative)
	print(y_derivative)
	step_size = 0.01
	out = rgate.mul(x,y)
	print(out)
	x = x + step_size*x_derivative
	y = y + step_size*y_derivative
	new_out = rgate.mul(x,y)
	print(new_out)
	"""
	"""
	#Analytic Gradient
	#기울기를 입,출력의 변화로 부터 계산할 경우 입력을 개수에 따라 계산하는 비용이 선형적으로 증가한다.
	#수백만 수억개가 있을때는 큰 비용이 들게 된다.
	#이 방법을 입출력에 변화를 주어 계산할 필요 없이, 미분공식으로 기울기는 구한다
	x =-2
	y = 3
	out = rgate.mul(x,y)
	x_derivative = y #미분 결과에 의해
	y_derivative = x #미분 결과에 의해
	step_size = 0.01
	out = rgate.mul(x,y)
	print(out)
	x = x + step_size*x_derivative
	y = y + step_size*y_derivative
	new_out = rgate.mul(x,y)
	print(new_out)
	"""
	"""	
	뉴럴네트워크 라이브러리를 기울기를 구할때 #3 공식기울기를 사용하지만, 검증은 계산기울기를 통해서 한다.
	공식기울기는 효율적이지만 때로는 틀릴수도 있는 반면, 계산기울기는 비용은 크지만 확실한 값이다.
	"""
	"""
	#Backpropagation
	#연결된 게이트에서 #3공식기울기를 구할때는 체인룰을 적용한다. 체인룰은 곱셈으로 연결시키는 것이다.
	x = -2; y = 5; z =-4
	q = rgate.add(x,y)
	f = rgate.mul(q,z)
	#print(q)
	print(f)
	d_f_wrt_q = z
	d_f_wrt_z = q
	d_q_wrt_x = 1.0
	d_q_wrt_y = 1.0
	d_f_wrt_x = d_q_wrt_x*d_f_wrt_q
	d_f_wrt_y = d_q_wrt_y*d_f_wrt_q
	g = [d_f_wrt_x,d_f_wrt_y,d_f_wrt_z]
	step = 0.01
	x=x+step*g[0]
	y=y+step*g[1]
	z=z+step*g[2]
	q = rgate.add(x,y)
	f = rgate.mul(q,z)
	#print(q)
	print(f)
	"""
	
	a = unit(1.0, 0.0)
	b = unit(2.0, 0.0)
	c = unit(-3.0, 0.0)
	x = unit(-1.0, 0.0)
	y = unit(3.0 ,0.0)

	mg0 = mg()
	mg1 = mg()
	ag0 = ag()
	ag1 = ag()
	sg0 = sg()

	m1 = mg0.f(a, x)
	m2 = mg1.f(b, y)
	a1 = ag0.f(m1, m2)
	a2 = ag1.f(a1, c)
	s = sg0.f(a2)

	print(s.value)

	sg0.ru.grad = 1.0
	sg0.b()
	ag0.b()
	ag1.b()
	mg0.b()
	mg1.b()

	step = 0.01
	a.value += step*a.grad
	b.value += step*b.grad
	c.value += step*c.grad
	x.value += step*x.grad
	y.value += step*y.grad

	ax = mg0.f(a, x)
	by = mg1.f(b, y)
	ag0 = ag0.f(ax, by)
	ag1 = ag1.f(ag0, c)
	s = sg0.f(ag1)

	print(s.value)
	
	h = 0.001
	a = 1
	b = 2
	c = -3
	x = -1
	y = 3
	ga = (fc(a+h,b,c,x,y)-fc(a,b,c,x,y))/h
	gb = (fc(a,b+h,c,x,y)-fc(a,b,c,x,y))/h
	gc = (fc(a+h,b,c+h,x,y)-fc(a,b,c,x,y))/h
	gx = (fc(a+h,b,c,x+h,y)-fc(a,b,c,x,y))/h
	gy = (fc(a+h,b,c,x,y+h)-fc(a,b,c,x,y))/h
	
	print(ga)
	print(gb)
	print(gc)
	print(gx)
	print(gy)

	# * gate
	print()
	print("* gate")
	a = u(11.0,0.0)
	b = u(22.0,0.0)
	mg1 = mg()
	r1 = mg1.f(a,b)
	r1.grad = 1.0
	mg1.b()
	da = mg1.u0.grad; print(da)
	db = mg1.u1.grad; print(db)
	print('------')
	
	# + gate
	print("+ gate")
	a = u(11.0,0.0)
	b = u(22.0,0.0)
	ag1 = ag()
	r1 = ag1.f(a,b)
	r1.grad = 1.0
	ag1.b()
	da = ag1.u0.grad; print(da)
	db = ag1.u1.grad; print(db)
	print('------')

	# + gate, 3var
	print("+ gate, 3th var")
	#input def
	a = u(11.0,0.0)
	b = u(22.0,0.0)
	c = u(33.0,0.0)
	#gate def
	ag1 = ag()
	ag2 = ag()
	#cal f
	r1 = ag1.f(a,b)
	r2 = ag2.f(r1,c)
	#cal b
	r2.grad = 1.0
	ag2.b()
	#r1.grad = ag2.u0.grad
	ag1.b()
	#print derivative
	dc = ag2.u0.grad; print(dc)
	db = ag1.u1.grad; print(db)
	da = ag1.u0.grad; print(da)
	print('------')
	
	# mixed gate
	print("mixed gate")

	#input def
	a = u(1.0,0.0)
	b = u(2.0,0.0)
	c = u(3.0,0.0)

	#gate def
	mg1 = mg()
	ag1 = ag()

	#cal f
	r1 = mg1.f(a,b)
	r2 = ag1.f(r1,c)

	#cal b
	r2.grad = 1.0
	ag1.b()
	#r1.grad = ag1.u0.grad
	mg1.b()

	#print derivative
	dc = ag1.u0.grad; print(dc)
	db = mg1.u1.grad; print(db)
	da = mg1.u0.grad; print(da)
	print('------')

	# square gate
	print("square gate")

	#input def
	a = u(11.0,0.0)

	#gate def
	s1 = sqg()

	#cal f
	r1 = s1.f(a)

	#cal b
	r1.grad = 1.0
	s1.b()

	#print derivative
	da = s1.u0.grad; print(da)
	print('------')

	# single neuron,ax+by+c
	print("single neuron,ax+by+c")

	#input def
	a = u(1.0,0.0)
	b = u(2.0,0.0)
	c = u(3.0,0.0)
	x = u(4.0,0.0)
	y = u(5.0,0.0)

	#gate def
	mg1 = mg()
	mg2 = mg()
	ag1 = ag()
	ag2 = ag()
	sg1 = sg()
	

	#cal f
	r1 = mg1.f(a,x)
	r2 = mg2.f(b,y)
	r3 = ag1.f(r1,r2)
	r4 = ag2.f(r3,c)
	r5 = sg1.f(r4)

	#cal b
	r5.grad = 1.0
	sg1.b()
	#r4.grad = sg1.u0.grad
	ag2.b()
	#r3.grad = ag2.u0.grad
	ag1.b()
	#r2.grad = ag1.u1.grad
	mg2.b()
	#r1.grad = ag1.u0.grad
	mg1.b()

	#print derivative
	da = mg1.u0.grad; print(da)
	db = mg2.u0.grad; print(db)
	dc = ag2.u1.grad; print(dc)
	dx = mg1.u1.grad; print(dx)
	dy = mg2.u1.grad; print(dy)
	print('------')

	# Math.pow(((a*b+c),2); 
	print("Math.pow(((a*b+c),2) gate")

	#input def
	a = u(3.0,0.0)
	b = u(2.0,0.0)
	c = u(1.0,0.0)

	#gate def
	mg1 = mg()
	ag1 = ag()
	sq1 = sqg()

	#cal f
	r1 = mg1.f(a,b)
	r2 = ag1.f(r1,c)
	r3 = sq1.f(r2)

	#cal b
	r3.grad = 1.0
	sq1.b()
	ag1.b()
	mg1.b()

	#print derivative
	da = mg1.u0.grad; print(da)
	db = mg1.u1.grad; print(db)
	dc = ag1.u1.grad; print(dc)
	print('------')

	# division gate
	print("division gate")

	#input def
	a = u(3.0,0.0)

	#gate def
	dg1 = dg()

	#cal f
	r1 = dg1.f(a)

	#cal b
	r1.grad = 1.0
	dg1.b()

	#print derivative
	da = dg1.u0.grad; print(da)
	print('------')
	# max gate
	print("max gate")

	#input def
	a = u(1.0,0.0)
	b = u(2.0,0.0)

	#gate def
	mag1 = mag()

	#cal f
	r1 = mag1.f(a,b)

	#cal b
	r1.grad = 1.0
	mag1.b()

	#print derivative
	da = mag1.u0.grad; print(da)
	db = mag1.u1.grad; print(db)
	print('------')

	# ReLU gate
	print("ReLU gate")

	#input def
	a = u(1.0,0.0)

	#gate def
	rg1 = rg()

	#cal f
	r1 = rg1.f(a)

	#cal b
	r1.grad = 1.0
	rg1.b()

	#print derivative
	da = rg1.u0.grad; print(da)
	print('------')

	# mixed gate, (a+b)/(c+d)
	print("mixed gate, (a+b)/(c+d)")

	#input def
	a = u(1.0,0.0)
	b = u(2.0,0.0)
	c = u(3.0,0.0)
	d = u(4.0,0.0)

	#gate def
	ag1 = ag()
	ag2 = ag()
	dg1 = dg()
	mg1 = mg()

	#cal f
	r1 = ag1.f(a,b)
	r2 = ag2.f(c,d)
	r3 = dg1.f(r2)
	r4 = mg1.f(r1,r3)

	#cal b
	r4.grad = 1.0
	mg1.b()
	dg1.b()
	ag1.b()
	ag2.b()

	#print derivative
	da = ag1.u0.grad; print(da)
	db = ag1.u1.grad; print(db)
	dc = ag2.u0.grad; print(dc)
	dd = ag2.u1.grad; print(dd)
	print('------')
```
