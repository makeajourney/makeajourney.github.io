---
title: "파이썬3 리스트(List)"
layout: post
date: 2016-02-19
categories : python3
---

### 리스트(list)
* 데이터를 순차적으로 파악하는 데 유용
* 변경 가능(mutable)
* 리스트는 0개 이상의 요소와 대괄호([]), 콤마(,)로 구성.  

<br>

### 리스트 생성
`객체이름 = []`  
`객체이름 = list()`  
위 두가지 방법으로 리스트 생성 가능.  
```
empty_list = []
another_empty_list = list()
weekdays = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
```  

<br>

### 다른 데이터 타입을 리스트로 변환
`list()`  
하나의 단어를 한 문자씩 리스트로 변환
```
list('cat')	// ['c', 'a', 't']
```  

튜플을 리스트로 변환  
```
a_tuple = ('ready', 'fire', 'aim')
list(a_tuple) 	// ='ready', 'fire', 'aim']
```  

<br>

### [offset]으로 항목 얻기
문자열과 마찬가지로 offset을 이용해 하나의 특정 값을 추출할 수 있다.  
```
marxes = ['Groucho', 'Chico', 'Harpo']
marxes[0] 		// 'Groucho'
```  

offset을 리스트 크기보다 큰 값으로 입력하면 에러가 발생한다.  
`IndexError: list index out of range`  

### 리스트의 리스트
리스트 내에 다른 리스트나 객체를 포함할 수 있다.  
