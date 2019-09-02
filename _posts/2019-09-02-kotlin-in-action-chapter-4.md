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

코틀린의 클래스와 메소드는 기본적으로 final이다.  
어떤 클래스의 상속을 허용하려면 클래스 앞에 open modifier를 붙여야 한다.  
override를 허용하고 싶은 메소드나 프로퍼티의 앞에도 마찬가지로 open modifier를 붙여야 한다.  
Base class나 인터페이스의 멤버를 오버라이드하는 경우 그 메소드는 open이 default이므로, 오버라이드하는 메소드의 구현을 derived class에서 override 하지 못하게 하려면 final을 명시해야 한다.  
abstract class의 member는 항상 open이므로 open modifier를 명시할 필요가 없다.  
