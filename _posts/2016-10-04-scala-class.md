---
title: "Scala - Class"
layout: post
---


## Class  


```scala
class Counter {
  private var value = 0   // 필드는 반드시 초기화해야 한다.
  def increment() { value += 1 }  // 메소드는 기본으로 public이다.
  def current() = value
}

val myCounter = new Counter // 또는 new Counter()
myCounter.increment()
println(myCounter.current)  // 인자가 없는 메소드는 괄호 없이 호출할 수 있다.
```  


오브젝트 상태를 변경하는 메소드(mutator)는 ()를 사용하여 호출하고, 상태를 변경하지 않는 메소드(accessor)는 ()를 생략하는 것이 좋은 스타일.  


`def current = value`로 작성할 수도 있는데, 이 때는 반드시 괄호 생략한 스타일로 호출해야 한다.  


<br/>

### property getter / setter  

scala에서는 기본적으로 var로 선언된 클래스 필드에 대해 getter/setter 메소드를 제공한다.  

- method override  

```scala
class Person {
  private var privateAge = 0

  def age = privateAge
  def age_= (newValue: Int) {
    if (newValue > privateAge) privateAge = newValue;
  }
}
```  


필드가 비공개이면, 게터와 세터는 비공개다.  
val로 선언된 클래스 필드는 getter만 제공한다.  
게터나 세터가 필요 없으면, 필드를 private[this]로 선언한다. (오브젝트-비공개)  


### bean property  

```scala
import scala.reflect.BeanProperty

class Person {
  @BeanProperty var name: String = _
}
```  

class field에 @BeanProperty annotation을 명시하면 아래 4개의 메소드를 생성한다.   
1. `name: String`   
2. `name_=(newValue: String): Unit`  
3. `getName(): String`  
4. `setName(newValue: String): Unit`  


기본 생성자 필드에 자바 게터/세터가 필요하면 아래와 같이 하면 된다.  

```scala
class Person(@BeanProperty var name: String)
```  


<br/>

### sub constructor  

- 생성자의 이름은 this  
- 여러개의 보조 생성자를 가질 수 있지만, 각 보조 생성자는 기본 생성자나 이전에 정의한 보조 생성자 호출로 시작해야 한다.  

```scala
class Person {
  private var name = ""
  private var age = ""

  def this(name: String) {  // 보조 생성자
    this()  // 기본 생성자 호출
    this.name = name
  }

  def this(name: String, age: Int) {  // 보조 생성자
    this(name)    // 앞서 정의된 보조 생성자 호출
    this.age = age
  }
}
```  


<br/>

### default constructor  

기본 생성자 인자들은 클래스 이름 바로 뒤에 온다.  
클래스 정의에 있는 모든 문을 실행한다.  

```scala
class Person(val name: String, val age: Int) {
  println("Just constructed another person")
  def description = name + " is " + age + " years old"
}
```  

val이나 var 없이 사용할 수도 있는데, 이럴 때는 private[this] val 필드와 동일하게 생각하면 된다.  
기본 생성자를 비공개로 만들려면 클래스 이름 뒤 괄호 앞에 private 키워드를 입력한다. 이 때 클래스를 생성하려면 보조 생성자를 반드시 사용해야 한다.  
