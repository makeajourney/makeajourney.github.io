---
title: "Kotlin in Action Chapter 5"
layout: post
---

This note is summary of _Kotlin in Action_.

You can access the code samples for this chapter [here](https://github.com/Kotlin/kotlin-in-action/tree/master/src/ch05).

# 람다(lambda)로 프로그래밍

lambda expression or lambda는 기본적으로 다른 함수에 넘길 수 있는 작은 코드 조각을 뜻한다.  

수신 객체 지정 람다 lambda with receiver

## 5.1 람다 식과 멤버 참조

###  5.1.1 람다 소개: 코드 블록을 함수 인자로 넘기기

lambda expression을 사용하면 함수를 선언할 필요가 없고 code block을 직접 function의 arguments로 전달할 수 있다.  

### 5.1.2 람다와 컬렉션

~~~kotlin
>>> val people = listOf(Person("Alice", 29), Person("Bob", 31))
>>> println(people.maxBy { it.age })
Person(name=Bob, age=31)
~~~

`maxBy`는 가장 큰 원소를 찾기 위해 비교에 사용할 값을 돌려주는 함수를 인자로 받는다. 중괄호로 둘러싸인 코드 `{ it.age }`비교에 사용할 값을 돌려주는 함수다. collection의 element를 argument로 받아서 비교에 사용할 값을 반환한다.  

자바 컬렉션에 대해 수행하던 대부분의 작업은 람다나 멤버 참조를 인자로 취하는 라이브러리 함수를 통해 개선할 수 있다.

### 5.1.3 람다 식의 문법

람다는 값처럼 여기저기 전달할 수 있는 동작의 모음이다.  
람다를 따로 선언해서 변수에 저장할 수도 있다.  

~~~kotlin
>>> val sum = { x: Int, y: Int -> x + y}
>>> println(sum(1, 2))
~~~

Kotlin lambda expression은 항상 중괄호로 둘러싸여 있다. 인자 목록 주변에 괄호가 없다는 사실을 꼭 기억하라.  
`->`가 인자 목록과 람다 본문을 구분해준다.  

인자로 받은 람다를 실행해주는 라이브러리 함수인 run을 사용해 실행할 수 있다.  

~~~kotlin
>>> run { println(41) }
42
~~~

코틀린에는 함수 호출시 맨 뒤에 있는 인자가 람다 식이라면 그 람다를 괄호 밖으로 빼낼 수 있다는 문법 관습이 있다.  
둘 이상의 람다를 인자로 받는 함수도 인자 목록의 맨 마지막 람다만 밖으로 뺄 수 있다. 따라서 그런 경우에는 괄호를 사용하는 일반적인 함수 호출 구문을 사용하는 편이 낫다.  


named arguments를 사용  

~~~kotlin
>>> val people = listOf(Person("이몽룡", 29), Person("성춘향", 31))
>>> val names = people.joinToString(separator = " ", transform = { p: Person -> p.name })
>>> println(names)
이몽룡 성춘향
~~~

람다를 괄호 밖에 전달  

~~~ kotlin
people.joinToString(" ") { p: Person -> p.name }
~~~

lambda arguments의 type을 제거하여

~~~kotlin
people.joinToString(" ") { p -> p.name }
~~~

lambda의 argument가 하나뿐이고 그 타입을 컴파일러가 추론할 수 있는 경우 `it`을 쓸 수 있다.  

~~~kotlin
people.joinToString(" ") { it.name }
~~~

