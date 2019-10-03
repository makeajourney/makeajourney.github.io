---
title: "Kotlin in Action Chapter 7"
layout: post
---

This note is summary of _Kotlin in Action_.

You can access the code samples for this chapter [here](https://github.com/Kotlin/kotlin-in-action/tree/master/src/ch07).


# 연산자 오버로딩과 기타 관례
reference : https://kotlinlang.org/docs/reference/operator-overloading.html  

## 7.1 산술 연산자 오버로딩

### 7.1.1 이항 산술 연산 오버로딩

~~~kotlin
data class Point(val x: Int, val y: Int) {
    operator fun plus(other: Point): Point {
        return Point(x + other.x, y + other.y)
    }
}
~~~

연산자를 오버로딩하는 함수 앞에는 operator가 있어야 한다.  

연산자를 확장 함수로 정의할 수도 있다.

~~~kotlin
operator fun Point.plus(other: Point): Point {
    return Point(x + other.x, y + other.y)
~~~

코틀린에서는 프로그래머가 직접 연산자를 만들어 사용할 수 없고 언어에서 미리 정해둔 연산자만 오버로딩할 수 있으며, 관례에 따르기 위해 클래스에서 정의해야 하는 이름이 연산자별로 정해져 있다.  

직접 정의한 함수를 통해 구현하더라도 연산자 우선순위는 언제나 표준 숫자 타입에 대한 연산자 우선순위와 같다.

두 피연산자가 같은 타입일 필요는 없다.

~~~kotlin
operator fun Point.times(scale: Double): Point {
    return Point((x * scale).toInt(), (y * scale).toInt())
}
~~~ 

코틀린 연산자가 자동으로 교환법칙(commutativity)을 지원하지 않음에 유의하라.

결과 타입이 피연산자 타입과 다른 연산자도 정의할 수 있다.

~~~kotlin
operator fun Char.times(count: Int): String {
    return toString().repeat(count)
}
~~~

### 7.1.2 복합 대입 연산자(compound assignment operator) 오버로딩

plus와 같은 연산자를 오버로딩하면 코틀린은 + 뿐만 아니라 그와 관련된 += 연산자도 함께 지원한다.  
`point += Point(3, 4)`는 `point = point + Point(3, 4)`와 같다.  

객체 내부의 상태를 변경하고 싶은 경우 복합 대입 연산자 오버로딩을 사용한다.

~~~kotlin
operator fun <T> MutableCollection<T>.plusAssign(element: T) {
    this.add(element)
}
~~~

plus와 plusAssign 연산을 동시에 정의하지 않도록 주의하라. 이 경우 컴파일러가 오류를 보고한다.

### 7.1.3 단항(unary) 연산자 오버로딩

~~~kotlin
operator fun Point.unaryMinus(): Point {
    return Point(-x, -y)
}
~~~

## 7.2 비교 연산자 오버로딩

### 7.2.1 동등성 연산자: equals

코틀린은 == 연산자 호출을 equals 메소드 호출로 컴파일한다.  
!= 연산자를 사용하는 식도 equals 호출로 컴파일 된다.  
==와 !=는 내부에서 인자가 널인지 검사하므로 nullable 값에도 적용할 수 있다.

`a == b` -> `a?.equals(b) ?: (b == null)`

equals는 Any에 정의되어 있는 메소드이므로 override가 붙는다.

식별자 비교(identity equals) 연산자 `===`를 사용해 equals의 파라미터가 수신 객체와 같은지 살펴본다. `===`를 오버로딩 할 수는 없다.

### 7.2.2 순서 연산자: compareTo

코틀린에서 비교 연산자는 Comparable 인터페이스의 compareTo 호출로 컴파일된다.  
`p1 < p2`는 `p1.compareTo(p2) < 0`과 같다.

~~~kotlin
class Person(
    val firstName: String, val lastName: String
) : Comparable<Person> {
    override fun compareTo(other: Person): Int {
        return compareValuesBy(this, other
            Person::lastName, Person::firstName)
    }
}
~~~

코틀린의 `compareValuesBy` 함수를 사용해 `compareTo`를 쉽고 간결하게 정의할 수 있다.


