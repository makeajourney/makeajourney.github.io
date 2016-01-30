---
title: "파이썬3 기본 자료형, 연산자"
layout: post
date: 2016-01-30
categories : python
---

## python

모든 것(bool, integer, floating point, string, data structure, function, program)이 object로 구현됨.

<br>

### 변수와 상수의 기본적인 특성
변수 (variable)
: 가변(Mutable). 데이터 값을 변경할 수 있음.  
  
상수 (literal)
: 불변(immutable). 데이터 값을 변경할 수 없음.
  
<br>

### 파이썬은 강타입
강타입 (Strong Type) : 객체의 타입을 바꿀 수 없음.
  
<br>  

### 변수 선언과 값 할당

1. literal  

> a = 7	// a라는 이름의 변수에 정수 값 7을 할당  

이 때 a는 변수의 이름이다. 왼쪽에 값을 할당할 변수의 이름을 쓰고, 오른쪽에 할당할 값을 쓴다.

1. variable  

> b = a	// b라는 이름의 변수에 a의 값인 7을 할당  

오른쪽에 변수의 이름을 씀으로써 해당 변수의 할당 값을 왼쪽 변수에 할당한다.
a의 값이 b로 복사된다고 생각하면 이해가 더 편리하다.
  
<br>  

### 변수의 타입 알기

> type (a)	// a라는 이름의 변수의 타입을 출력  

결과 출력
`<class 'int'>`  
  
<br>

### 변수의 이름
* 소문자(a-z), 대문자(A-Z), 숫자(0-9), 언더스코어(_) 사용 가능.  
* 첫 문자는 숫자로 시작할 수 없음.
* 언더스코어(_)로 시작하는 이름은 특별한 방법으로 처리.
* 예약어(reserved word)는 변수의 이름으로 사용 불가.

<br>

### 할당 가능한 숫자
파이썬은 기본적으로 정수(integer)와 부동소수점수(floating point) 지원.  
변수를 선언하고 초기화 할 때 명시적으로 표기하지 않지만, 어느 한 쪽으로 할당된 이후에는 다른 타입의 수나 데이터로 변경할 수 없음. (강타입)  

<br>

### 연산자(operator)
* \+ 더하기
* \- 빼기
* \* 곱하기
* / 부동소수점 나누기
* // 정수 나누기
* % 나머지
* ** 지수

<br>

### 연산자를 사용한 연산 시 주의사항
* 정수 0을 쓸 수 있다. 05는 쓸 수 없다.  `SyntaxError: invalid token`
* 0으로 나누면 (`5 / 0`) 예외 발생.  `ZeroDivisionError: division by zero` 
* `5 // 0` 역시 예외 발생. `ZeroDivisionError: integer division or modulo by zero`
* `a + 3`은 `a = a + 3`과 다르다.  a가 100으로 초기화 되어 있다고 할 때, `print(a + 3)`은 97을 출력하지만, a는 여전히 100이다.  `a = a + 3`은 두번째 a 변수의 값과 3을 더하기 연산한 후 그 값을 왼쪽의 a에 할당한다.