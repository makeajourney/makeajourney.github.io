---
title: "Scala - Object"
layout: post
---


## Object  

클래스의 단독 인스턴스 정의.  
오브젝트의 생성자는 오브젝트가 처음 사용될 때 실행.  
java나 C++ 에서 싱글톤 오브젝트를 사용했을 상황에 사용하면 된다. (ex. 싱글톤 디자인 패턴)  


### Companion object  

정적 메소드를 갖기 위해, 클래스와 같은 이름의 오브젝트를 한 소스 파일 내 기술해서 사용할 수 있다.  
이 때 클래스와 컴패니언 오브젝트는 서로의 private member or function(method)에 접근 가능하다.  

```scala
class Account {
  val id = Account.newUniqueNumber()
  private var balance = 0.0
  def deposit(amount: Double) { balance += amount}
  ...
}

object Account {    // 컴패니언 오브젝트
  private var lastNumber = 0
  private def newUniqueNumber() = { lastNumber += 1; lastNumber }
}
```  

이 때 클래스에서 컴패니언 오브젝트 메소드를 호출하기 위해서는 `Account.newUniqueNumber()` 라고 호출해야 한다.  


<br/>  

### apply  

오브젝트는 생성자 대신 apply()를 사용한다.  
Array 오브젝트는 다음 식과 같은 배열 생성을 허용하는 apply 메소드를 지원한다.  
`Array("Mary", "had", "a", "little", "lamb")`  

```scala
class Account private (val id: Int, initialBalance: Double) {
  private var balance = initialBalance
  ...
}

object Account {    // 컴패니언 오브젝트
  def apply(initialBalance: Double) =
    new Account(newUniqueNumber(), initialBalance)
    ...
}


val acct = Account(1000.0)  // account 생성
```  


<br/>

### application object  

scala 프로그램은 Array[String] => Unit 타입의 오브젝트 main 메소드로 시작해야 한다.  

```scala
object Hello {
  def main(args: Array[String]) {
    println("Hello, world!")
  }
}
```  


main 메소드를 사용하는 대신 App 트레이트를 확장할 수 있다.  

```scala
object Hello extends App {
  if (args.length > 0)
    println("Hello, " + args(0))  // 명령줄 인자 사용
  else
    println("Hello, world!")
}
```  


### Enumeration  

헬퍼 클래스 Enumeration 이용.  

```scala
object TrafficLightColor extends Enumeration {
  type TrafficLightColor = Value
  val Red, Yellow, Green = Value
}

import TrafficLightColor._
def doWhat(color: TrafficLightColor) = {
  if (color == Red) "stop"
  else if (color == Yellow) "hurry up"
  else "go"
}

// TrafficLightColor.values : 모든 값 리턴
for (c <- TrafficLightColor.values) println(c.id + ": " + c)

// TrafficLightColor.Red object 리턴  
TrafficLightColor(0)    // Enumeration.apply 호출
TrafficLightColor.withName("Red")
```  
