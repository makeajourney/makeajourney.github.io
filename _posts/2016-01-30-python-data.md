---
title: "파이썬3 기본 자료형, 연산자"
layout: post
date: 2016-01-30
categories : python
---

## python

모든 것(bool, integer, floating point, string, data structure, function, program)이 object로 구현됨.


### 변수와 상수의 기본적인 특성
변수 (variable)
: 가변(Mutable). 데이터 값을 변경할 수 있음.  
  
상수 (literal)
: 불변(immutable). 데이터 값을 변경할 수 없음.
  
### 파이썬은 강타입
강타입 (Strong Type) : 객체의 타입을 바꿀 수 없음.
  
### 변수 선언과 값 할당
1. literal  
`a = 7	// a라는 이름의 변수에 정수 값 7을 할당`  
이 때 a는 변수의 이름이다. 왼쪽에 값을 할당할 변수의 이름을 쓰고, 오른쪽에 할당할 값을 쓴다.
1. variable  
`b = a	// b라는 이름의 변수에 a의 값인 7을 할당`  
오른쪽에 변수의 이름을 씀으로써 해당 변수의 할당 값을 왼쪽 변수에 할당한다.
a의 값이 b로 복사된다고 생각하면 이해가 더 편리하다.

### 변수의 타입 알기
`type (a)	// a라는 이름의 변수의 타입을 출력`
