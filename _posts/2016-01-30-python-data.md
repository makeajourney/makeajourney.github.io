---
title: "파이썬3 기본 자료형, 연산자"
layout: post
modified: 2019-02-08
categories : [python]
comments: true
---

## python

모든 것(bool, integer, floating point, string, data structure, function, program)이 object로 구현됨.

<!--more-->

### 변수와 상수의 기본적인 특성

변수 (variable)
: 가변(Mutable). 데이터 값을 변경할 수 있음.  
  
상수 (literal)
: 불변(immutable). 데이터 값을 변경할 수 없음.

### 파이썬은 강타입

강타입 (Strong Type) : 객체의 타입을 바꿀 수 없음.

### 변수 선언과 값 할당

#### literal  

{% highlight python %}
a = 7   // a라는 이름의 변수에 정수 값 7을 할당
{% endhighlight %}

이 때 a는 변수의 이름이다. 왼쪽에 값을 할당할 변수의 이름을 쓰고, 오른쪽에 할당할 값을 쓴다.

#### variable  

{% highlight python %}
b = a  // b라는 이름의 변수에 a의 값인 7을 할당
{% endhighlight %}

오른쪽에 변수의 이름을 씀으로써 해당 변수의 할당 값을 왼쪽 변수에 할당한다.
a의 값이 b로 복사된다고 생각하면 이해가 더 편리하다.

### 변수의 타입 알기

{% highlight python %}
type (a)   // a라는 이름의 변수의 타입을 출력
{% endhighlight %}  

결과 출력
`<class 'int'>`  

### 변수의 이름

* 소문자(a-z), 대문자(A-Z), 숫자(0-9), 언더스코어(_) 사용 가능.  
* 첫 문자는 숫자로 시작할 수 없음.
* 언더스코어(_)로 시작하는 이름은 특별한 방법으로 처리.
* 예약어(reserved word)는 변수의 이름으로 사용 불가.

### 할당 가능한 숫자

파이썬은 기본적으로 정수(integer)와 부동소수점수(floating point) 지원.  
변수를 선언하고 초기화 할 때 명시적으로 표기하지 않지만, 어느 한 쪽으로 할당된 이후에는 다른 타입의 수나 데이터로 변경할 수 없음. (강타입)  

### 연산자(operator)

* \+ 더하기
* \- 빼기
* \* 곱하기
* / 부동소수점 나누기
* // 정수 나누기
* % 나머지
* ** 지수

### 연산자를 사용한 연산 시 주의사항

* 정수 0을 쓸 수 있다. 05는 쓸 수 없다.  `SyntaxError: invalid token`
* 0으로 나누면 (`5 / 0`) 예외 발생.  `ZeroDivisionError: division by zero` 
* `5 // 0` 역시 예외 발생. `ZeroDivisionError: integer division or modulo by zero`
* `a + 3`은 `a = a + 3`과 다르다.  
  a가 100으로 초기화 되어 있다고 할 때, `print(a + 3)`은 97을 출력하지만, a는 여전히 100이다.  
  `a = a + 3`은 두번째 a 변수의 값과 3을 더하기 연산한 후 그 값을 왼쪽의 a에 할당한다.
* divmod() 함수를 사용하면 몫과 나머지를 튜플로 리턴한다.  

{% highlight python %}
divmod(9, 5)   // 결과는 (1, 4)
{% endhighlight %}

### 연산자 우선순위

기본적으로 *,/ 가 +, -보다 우선순위가 높아서 먼저 연산된다. 모든 연산자에 각각의 우선순위가 존재한다. 하지만 이를 모두 외울 필요는 없고, 먼저 연산해야 하는 계산식에 괄호()를 사용하여 연산하는 것이 좋다. 이렇게 연산식을 작성하면, 작성하기에도 편리하고 가독성도 높아진다.  

{% highlight python %}
2 + (3 * 4)
{% endhighlight %}

### 진수 (base)

파이썬에서는 10진수, 2진수, 8진수, 16진수를 표현 할 수 있다.  
이는 비트단위의 연산에서 유용하게 사용할 수 있다.  

* 10진수 (decimal)
* 2진수 (binary) : 0b or 0B
* 8진수 (octat) : 0o or 0O
* 16진수 (hexadecimal) : 0x or 0X  

(앞의 0은 모두 숫자, 8진수의 두번째 O는 영문)

#### 10진수 10을 각각의 진수로 표시

2진수 : `0b10`  
8진수 : `0o10`  
16진수 : `0x10`  

### 형 변환 (type casting)

다른 데이터타입을 정수로 변환 : int() 함수 사용.

{% highlight python %}
int(True)   // 결과는 1
int(False)  // 결과는 0
int(98.6)   // 결과는 99
{% endhighlight %}

int() 함수에서 숫자가 아닌 다른 뭔가를 변환하면 에러가 발생함.

{% highlight python %}
int('99 bottles of beer on the wall')  
int('')
int('98.6')
int('1.0e4')
// 위 모두 같은 에러 발생 (ValueError: invalid literal for int() with base 10)
{% endhighlight %}

### int의 크기

Python 3에서는 int의 크기 제한이 없다.  
64bit보다 더 큰 수도 담을 수 있다.  

### 부동소수점수

부동소수점수로 형 변환하기 위해서 float() 함수 사용.

{% highlight python %}
float('99')  // 결과는 99.0
{% endhighlight %}
