---
title: "Kotlin in Action Chapter 4"
layout: post
---

This note is summary of _Kotlin in Action_.

You can access the code samples for this chapter [here](https://github.com/Kotlin/kotlin-in-action/tree/master/src/ch04).

# 4장 클래스, 객체, 인터페이스

코틀린의 클래스, 인터페이스 선언은 기본적으로 final이며 public.  

## 4.1 클래스 계층 정의

### 4.1.1 코틀린 인터페이스

~~~ kotlin
interface Clickable {
    fun click()
}
~~~

코틀린 인터페이스에는 추상메소드와 구현이 있는 메소드도 정의 가능하다.  
위 예제는 click이라는 추상 메소드가 있는 인터페이스를 정의한다.  

~~~kotlin
class Button : Clickable {
    override fun click() = println("I was clicked")
}
~~~

코틀린에서는 클래스 이름 뒤에 콜론(:)을 붙이고 인터페이스와 클래스 이름을 적는 것으로 클래스 확장과 인터페이스 구현을 모두 처리한다.  

한 클래스에서 같은 이름을 가진 메소드가 존재하는 인터페이스를 구현할 경우, Derived class에서 overriding method를 implement하지 않으면 컴파일 에러가 발생한다.  

~~~kotlin
class Button : Clickable, Focusable {
    override fun click() = println("I was clicked")
    override fun showOff() {
        super<Clickable>.showOff()
        super<Focusable>.showOff()
    }
}
~~~

`super<BASE_TYPE_CLASS_NAME>.methodname()` 과 같은 방식으로 Base class의 구현을 호출할 수 있다.  

코틀린은 자바 6와 호환되도록 설계되었기 때문에 인터페이스의 디폴트 메소드를 지원하지 않는다. (코틀린의 디폴트 메소드와 자바의 디폴트 메소드는 다르다는 얘기)  

### 4.1.2 open, final, abstract 변경자: 기본적으로 final

코틀린의 클래스와 메소드는 **기본적으로 final**이다.  
어떤 클래스의 **상속을 허용**하려면 클래스 앞에 **open** modifier를 붙여야 한다.  
override를 허용하고 싶은 메소드나 프로퍼티의 앞에도 마찬가지로 open modifier를 붙여야 한다.  
Base class나 인터페이스의 멤버를 오버라이드하는 경우 그 메소드는 open이 default이므로, 오버라이드하는 메소드의 구현을 derived class에서 override 하지 못하게 하려면 final을 명시해야 한다.  
abstract class의 member는 항상 open이므로 open modifier를 명시할 필요가 없다.  

### 4.1.3 가시성 변경자(visible modifier): 기본적으로 공개

아무 변경자도 없는 경우 public이 기본이다.  
코틀린은 패키지를 네임스페이스를 관리하기 위한 용도로만 사용한다. 그래서 패키지를 가시성 제어에 사용하지 않는다.  

modifier | class member | top-level declaration
---|---|---
public | 모든 곳에서 볼 수 있다. | 모든 곳에서 볼 수 있다.
internal | 같은 모듈 안에서만 볼 수 있다. | 같은 모듈 안에서만 볼 수 있다.
protected | 하위 클래스 안에서만 볼 수 있다. | (최상위 선언에 적용할 수 없음)
private | 같은 클래스 안에서만 볼 수 있다. | 같은 파일 안에서만 볼 수 있다.

### 4.1.4 내부 클래스(inner class)와 중첩된 클래스(nested class): 기본적으로 중첩 클래스

코틀린의 nested class는 명시적으로 요청하지 않는 한 Outer class instance에 대한 접근 권한이 없다.  

클래스 B 안에 정의된 클래스 A | 자바에서는 | 코틀린에서는
---|---|---
nested class(바깥쪽 클래스에 대한 참조를 저장하지 않음) | static class A | class A
inner class(바깥쪽 클래스에 대한 참조를 저장함) | class A | inner class A

inner class인 Inner 안에서 outer class인 Outer에 의 참조에 접근하려면 this@Outer라고 써야 한다.  

~~~kotlin
class Outer {
    inner class Inner {
        fun getOuterReference(): Outer = this@Outer
    }
}
~~~

### 4.1.5 봉인된 클래스(sealed class): 클래스 계층 정의 시 계층 확장 제한  

`sealed` modifier를 붙여서 그 Base class를 상속한 derived class의 정의를 제한할 수 있다.  
sealed class의 derived class를 정의할 때는 반드시 base class 안에 중첩시켜야 한다.  
sealed class는 기본적으로 open.  
when 식에서 sealed class를 처리하는 경우, 모든 하위 클래스를 처리하면 디폴트 분기가 필요 없다.  

~~~kotlin
sealed class Expr {
    class Num(val value: Int) : Expr()
    class Sum(val left: Expr, val right: Expr) : Expr()
}
fun eval(e: Expr) : Int =
    when (e) {
        is Expr.Num -> e.value
        is Expr.Sum -> eval(e.right) + eval(e.left)
    }
~~~

## 4.2 뻔하지 않은 생성자와 프로퍼티를 갖는 클래스 선언

코틀린은 주 생성자(primary constructor)와 부 생성자(secondary constructor)를 구분한다.  
초기화 블록(initializer block)을 통해 초기화 로직을 추가할 수 있다.  

### 4.2.1 클래스 초기화(initialization): 주 생성자(primary constructor)와 초기화 블록(initializer block)

~~~kotlin
class User(val nickname: String)
~~~

클래스 이름 뒤에 오는 괄호로 둘러싸인 코드가 primary constructor이다.
primary constructor는 constructor arguments를 지정하고 그 constructor arguments에 의해 초기화되는 프로퍼티를 정의한다.  

~~~kotlin
class User constructor(_nickname: String) {
    val nickname: String
    init {
        nickname = _nickname
    }
}
~~~

`constructor` : primary constructors나 secondary constructor 정의를 시작할 때 사용한다.    
`init` : initializer block을 시작한다.  
primary constructor는 별도의 코드를 포함할 수 없으므로 initializer block이 필요하다.
primary constructor 앞의 `constructor`는 별다른 annotation이나 visible modifier가 없으면 생략할 수 있다.  
primary constructor의 arguments의 앞에 `val` 혹은 `var`를 붙여서 property 생성과 초기화를 간략하게 작성할 수 있다.  

함수 파라미터 와 마찬가지로 constructor arguments에도 default value를 정의할 수 있다.  

~~~kotlin
class User(val nickname: String, val isSubscribed: Boolean = true)
~~~

클래스 인스턴스 생성은 `new`키워드 없이 생성자를 호출하면 된다.  

~~~kotlin
>>> val soyoun = User("소연")
>>> println(soyoun.isSubscribed)
true
>>> val gye = User("계영", false)
>>> println(gye.isSubscribed)
false
>>> val hey = User("혜원", isSubscribed = false)
>>> println(hey.isSubscribed)
false
~~~

모든 constructor arguments에 default value를 지정하면 컴파일러가 자동으로 파라미터가 없는 생성자를 만들어준다. 파라미터 없는 생성자는 디폴트 값을 사용해 클래스를 초기화한다.  

derived class가 base class의 생성자를 호출해야 할 경우, base class 이름 뒤에 괄호를 치고 constructor parameter를 넘긴다.  

~~~kotlin
open class User(val nickname: String) { ... }
class TwitterUser(nickname: String) : User(nickname) { ... }
~~~

클래스를 정의할 때 생성자를 정의하지 않으면 컴파일러가 인자가 없는 디폴트 생성자를 만들어준다.  

~~~kotlin
open class Button
~~~

button의 생성자는 아무 일도 하지 않지만, button을 상속하는 derived class는 button의 생성자를 호출해야 한다.  

~~~kotlin
class RadioButton: Button()
~~~

이런 특성을 통해 클래스와 인터페이스를 쉽게 구분할 수 있다.  

모든 생성자를 private으로 정의하면 해당 클래스를 클래스 외부에서 인스턴스화 하지 못하게 할 수 있다.  

~~~kotlin
class Secretive private constructor() {}
~~~ 
