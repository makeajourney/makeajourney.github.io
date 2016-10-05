---
title: "Scala - Inheritance"
layout: post
---

기본적으로 자바와 거의 동일  


### override  

추상 메소드가 아닌 메소드를 오버라이드 할 때 반드시 `override` 수정자를 사용해야 한다.  


### protected  

scala에서도 `protected`를 사용할 수 있다.  
자바와 다른 점은 protected 멤버가 클래스가 속한 패키지 전체에 보이지는 않는다.  
현재 오브젝트로 접근을 제한하는 `protected[this]`도 있다.  


### superclass

서브클래스와 슈퍼클래스 생성자를 호출하는 기본 생성자를 정의.  

```scala
class Employee(name: String, age: Int, val salary: Double) extends
  Person(name, age)
```  


### overriding fields  

```scala
abstract class Person {
  def id: Int // each person has an ID that is computed in some way
  ...
}

// a student ID is simply provided in the constructor
class Student(override val id: Int) extends Person
```


- def는 오직 다른 def만 오버라이드 할 수 있다.  
- val은 다른 val이나 인자 없는 def만 오버라이드 할 수 있다.   
- var은 오직 추상 var만 오버라이드 할 수 있다.  


| | val로 | def로 | var로 |
|---|---|---|---|
| overriding `val` | * 서브클래스가 비공개 필드를 가짐 (슈퍼클래스 필드와 같은 이름은 괜찮음).<br/> * 게터가 슈퍼클래스 게터를 오버라이드함. | error. | error. |
| overriding `def` | * 서브클래스가 비공개 필드를 가짐.<br/> * 게터가 슈퍼클래스 게터를 오버라이드함. | error. | error. |
| overriding `var` | error. | error. | 슈퍼클래스 var이 추상일 때만. |


<br/>

### anonymous subclass  

```scala
val alien = new Person("Fred") {
  def greeting = "Greetings, Earthling! My name is Fred."
}

def meet(p: Person{def greeting: String}) {
  println(p.name + " says: " + p.greeting)
}
```  


<br/>

### abstract classes / fields  

```scala
abstract class Person(val name: String) {
  val id: Int // No initializer - this is an abstract field with an abstract getter method
  def id: Int // No method body - this is abstract method
}
```

abstract field나 method를 sub클래스에 정의할 때는 override 키워드는 생략한다.  


<br/>

### early definition syntax

서브클래스의 val 필드를 슈퍼클래스가 실행되기 전에 초기화할 수 있게 해준다.

```scala
class Bug extends {
  override val range = 3
} with Creature // Creature :  superclass name
```
