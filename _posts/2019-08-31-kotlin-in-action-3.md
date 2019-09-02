---
title: "Kotlin in Action Chapter 3"
layout: post
---

This note is summary of _Kotlin in Action_.

You can access the code samples for this chapter [here](https://github.com/Kotlin/kotlin-in-action/tree/master/src/ch03).

# 3장 함수 정의와 호출

## 3.1 코틀린에서 컬렉션 만들기

코틀린은 자체 컬렉션을 제공하지 않고, 표준 자바 컬렉션을 활용  
  - 자바 코드와 상호작용하기가 훨씬 더 쉽다.
  - 자바와 코틀린 컬렉션을 서로 변환할 필요가 없다.

Kotlin

~~~kotlin
>>> val set = hashSetOf(1, 7, 55)
>>> println(set.javaClass)
class java.util.HashSet
~~~

## 3.2 함수를 호출하기 쉽게 만들기

### 3.2.1 이름 붙인 인자 (Named arguments)

Java

~~~java
joinToString(collection, /*separator */ " ", /* prefix */ " ", /* postfix */ " ")
~~~

Kotlin

~~~kotlin
joinToString(collection, separator = " ", prefix = " ", postfix = " ")
~~~

### 3.2.2 디폴트 파라미터 값 (Default arguments)

디폴트 파라미터를 이용해 Overloading 메소드를 늘리지 않고 구현 가능

Kotlin

~~~kotlin
fun <T> joinToString(
	collection: Collection<T>,
	separator: String = ", ",
	prefix: String = "".
	postfix: String =. 
): String
~~~

코틀린 함수를 자바에서 호출하는 경우에는 모든 인자를 명시해야 함. 이 때 `@JvmOverloads` Annotation을 함수에 추가하면 코틀린 컴파일러가 Overloading된 자바 메소드를 추가해준다.  

### 3.2.3 정적인 유틸리티 클래스 없애기: 최상위 함수와 프로퍼티 (Top-level functions and properties)

코틀린은 static methods를 만들기 위해 의미 없는 클래스를 생성할 필요가 없음.

Kotlin

~~~kotlin
// filename: join.kt
package strings

fun joinToString(...): String {...}
~~~

위를 자바코드로 변환하면 

~~~java
public class JoinKt {
	public static String joinToString(...) {...}
}
~~~

코틀린으로 선언된 최상위 함수를 자바에서 호출하려면

~~~java
import strings.JoinKt;
...
JoinKt.joinToString(list, ", ", "", "");
~~~

`@JvmName` Annotation을 파일 맨 앞, 패키지 이름 선언 이전에 사용하면 코틀린 최상위 함수가 포험되는 클래스의 이름 변경 가능.

#### 최상위 프로퍼티 (Top-level properties)

함수와 마찬가지로 프로퍼티도 파일의 최상위 수준에 놓을 수 있다.  
`const` notation을 추가해서 `public static final` 필드로 사용할 수 있다.

Kotlin

~~~kotlin
const val UNIX_LINE_SEPARATOR = "\n"
~~~

Java

~~~java
public static final String UNIX_LINE_SEPARATOR = "\n";
~~~

## 3.3 메소드를 다른 클래스에 추가: 확장 함수(extension functions)와 확장 프로퍼티(extension properties)

extension function: 어떤 클래스의 멤버 메소드인 것처럼 호출할 수 있지만 그 클래스의 밖에 선언된 함수.  

~~~kotlin
package strings

fun String.lastChar(): Char = this.get(this.length - 1)
~~~

수신 객체 타입(receiver type): 함수가 확장할 클래스의 이름. 위 예제에서 `String` 부분.  
수신 객체(receiver object) : extension function이 호출되는 대상이 되는 값(객체). 위 예제에서 `this`.  

바로 위에서 선언한 함수를 호출
 
~~~kotlin
"Kotlin".lastChar()
~~~

receiver object는 생략 가능.

~~~kotlin
package strings

fun String.lastChar(): Char = get(length - 1)
~~~

### 3.3.1 임포트와 확장 함수

한 파일 내에서 다른 여러 패키지에 속한 이름이 같은 함수를 가져와 사용하는 경우 이름을 바꿔서 import 할 수 있다.  

~~~kotlin
// file name: StringUtil.kt 
import strings.lastChar as last

val c = "Kotlin".last()
~~~

### 3.3.2 자바에서 확장 함수 호출

내부적으로 extension function은 receiver object를 첫 번째 인자로 받는 static methods.  
3.3.1에서 정의한 함수를 자바에서 호출한다면,  

~~~kotlin
char c = StringUtilKt.lastChar("Java");
~~~  

### 3.3.3 확장 함수로 유틸리티 함수 정의

extension function은 정적 메소드 호출에 대한 syntactic sugar.  

### 3.3.4 확장 함수는 오버라이드 할 수 없다  

이름과 파라미터가 완전히 같은 base class와 derived class에 정의해도, 수신 객체로 지정한 변수의 **정적 타입** 에 의해 어떤 확장 함수가 호출될지 결정된다.  

### 3.3.5 확장 프로퍼티

기존 클래스 객체에 대한 프로퍼티 형식의 구문 작성은 가능하나, 아무 상태도 가질 수 없기 때문에, 기본적으로 getter를 반드시 직접 구현해야 한다. setter 또한 명시적으로 구현, 호출해야 한다.  

확장 프로퍼티 선언  

~~~kotlin
var StringBuilder.lastChar: Char
    get() = get(length - 1)
    set(value: Char) {
        this.setCharAt(length - 1, value)
    }
~~~

확장 프로퍼티 사용

~~~kotlin
>>> println("Kotlin".lastChar)
n
>>> val sb = StringBuilder("Kotlin?")
>>> sb.lastChar = "!"
>>> println(sb)
Kotlin!
~~~  

Reference about Extensions <https://kotlinlang.org/docs/reference/extensions.html#extensions>

## 3.4 컬렉션 처리: 가변 길이 인자(varargs), 중위 함수 호출(infix calls), 라이브러리 지원(library support)

### 3.4.1 자바 컬렉션 API 확장

자바 컬렉션에 추가된 코틀린의 새로운 기능은 확장함수로 구현됨.  

### 3.4.2 가변 인자 함수: 인자의 개수가 달라질 수 있는 함수 정의

varargs: 메소드를 호출할 때 원하는 개수만큼 값을 인자로 넘기면 자바 컴파일러가 배열에 그 값들을 넣어주는 기능.  
함수 정의 시 argument 앞에 `varang` notation을 추가.  

~~~kotlin
fun listOf<T>(varang values: T): List<t> { ... } 
~~~  

코틀린에서 배열을 varargs에 넘기려면 *(spread operator)를 이용.  

~~~kotlin
fun main(args: Array<String>) {
    val list = listOf("args: ", *args)
    println(list)
}
~~~

### 3.4.3 값의 쌍 다루기: 중위 호출과 구조 분해 선언(destructuring declarations)

argument가 하나뿐인 method나 extension function에 `infix` notation을 함수 선언 앞에 추가하여 중위호출을 사용할 수 있다.  

~~~kotlin
// 실제 to의 구현은 아님 
infix fun Any.to(other: Any) = Pair(this, other)
~~~

위에 선언한 to를 사용한 두 변수 초기화. 이런 기능을 destructuring declarations 라고 부른다.  

~~~kotlin
val (number, name) = 1 to "one"
~~~

루프에서의 destructuring declarations.  

~~~kotlin
for ((index, element) in collection.withIndex()) {
    println("$index: $element")
}
~~~

## 3.5 문자열과 정규식(regular expression) 다루기

코틀린 문자열은 자바 문자열과 같다.  

### 3.5.1 문자열 나누기

코틀린에서는 token으로써 문자열과 Regular expression이 혼동되는 것을 막기 위해 Regular expression을 명시적으로 사용한다.  

~~~kotlin
>>> println("12.345-6.A".split("\\.|-".toRegex()))
[12, 345, 6, A]
~~~

위의 정규식에서 `.`가 wild card가 아닌 literal로 쓰이도록 escape 했다.  

여러가지 구분자를 동시에 이용할 수 있다.  

~~~kotlin
println("12.345-6.A".split(".", "-"))
~~~

### 3.5.2 정규식과 3중 따옴표로 묶은 문자열

정규식을 쓸 때, 3중 따옴표를 사용하면 escape 할 필요가 없다.

~~~kotlin
fun parsePath(path: String) {
    val regex = """(.+)/(.+)\.(.+)""".toRegex()
    val matchResult = regex.matchEntire(path)
    if (matchResult != null) {
        val (directory, filename, extension) = matchResult.destructured
        pritnln("Dir: $directory, name: $filename, ext: $extension")
    }
}
~~~

### 3.5.3 여러 줄 3중 따옴표 문자열

~~~kotlin
val kotlinLogo = """|  //
                   .| //
                   .|/ \"""
println(kotlinLogo.trimMargin("."))
~~~

3중 따옴표 문자열에는 줄 바꿈, 탭 등이 그대로 들어간다.  
여러 줄 문자열을 코드에서 알아보기 쉽게 하기 위해 들여쓰기를 하되, 들여쓰기의 끝부분을 특정한 문자로 표시하고 trimMargin을 사용해 해당 문자와 들여쓴 공백을 제거한다. 위 예제에서는 마침표를 들여쓰기 구분 문자로 사용했다.  

## 3.6 코드 다듬기: 로컬 함수(local functions)와 확장

반복되는 코드(검증 코드 등)를 로컬함수로 분리하여 중복을 없애는 동시에 코드 구조를 깔끔하게 유지할 수 있다.  
로컬함수는 자신이 속한 Outer function의 모든 파라미터와 변수를 사용할 수 있다.  
중첩된 함수의 깊이가 깊어지면 코드를 읽기가 어려워 질 수 있음에 주의하라.  

~~~kotlin
class User(val id: Int, val name: String, val address: String)

fun User.validateBeforeSave() {
    fun validate(value: String, fieldName: String) { // 로컬 함수
        if (value.isEmpty()) {
            throw IllegalArgumentException(
                "Can't save user $id: empty $fieldName")
        }
    }
    validate(name, "Name")
    validate(address, "Address")
}

fun saveUser(user: User) {
    user.validateBeforeSave()
    // user를 데이터베이스에 저장한다.
}
~~~
