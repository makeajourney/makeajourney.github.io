---
title: "Scala - Trait"
layout: post
---


## Trait


<br/>

### trait as interface

```scala
trait Logger {
  def log(msg: String)  // abstract method
}
```

트레이트에서 구현되지 않은 메소드는 자동으로 추상메소드로 인식된다.  


`extends` 키워드를 사용해 트레이트를 사용할 수 있다.  
트레이트의 추상메소드를 서브클래스에서 구현할 때 `override`가 필요 없다.  

```scala
class ConsoleLogger extends Logger {
  def log(msg: String) { println(msg) }
}
```


하나 이상의 트레이트를 사용할 경우, `with` 키워드를 사용한다.

```scala
class ConsoleLogger extends Logger with Cloneable with Serializable
```


<br/>

### trait  implemented

```scala
trait ConsoleLogger {
  def log(msg: String) { println(msg) }
}

class SavingsAccount extends Account with ConsoleLogger {
  def withdraw(amount: Double) {
    if (amount > balance) log("Insufficient funds")
    else balance -= amount
  }
}
```


<br/>

### trait in object

```scala
trait Logged {
  def log(msg: String) { }
}

class SavingsAccount extends Account with Logged {
  def withdraw(amount: Double) {
    if (amount > balance) log("Insufficient funds")
    else ...
  }
  ...
}

trait ConsoleLogger extends Logged {
  override def log(msg: String) { println(msg) }
}

val acct = new SavingsAccount with ConsoleLogger
```

SavingsAccount 객체인 acct에서 log를 실행하면 ConsoleLogger의 log가 실행된다.  


<br/>

### layered trait

모든 로깅 메시지에 타임스탬프를 찍는다.  

```scala
trait TimestampLogger extends Logged {
  override def log(msg: String) {
    super.log(new java.util.Date() + " " + msg)
  }
}
```


너무 긴 로그메시지를 잘라낸다.  

```scala
trait ShortLogger extends Logged {
  val maxLength = 15
  override def log(msg: String) {
    super.log(
      if (msg.length <= maxLength) msg else msg.subString(0, maxLength - 3) + "...")
    )
  }
}
```


<br/>

### overriding abstract method about trait

trait의 추상메소드를 오버라이드 할 경우에는 `override`라는 키워드로는 충분치 않다.  
반드시 `abstract`까지 기술해주어야 한다.  

```scala
trait Logger {
  def log(msg: String)
}

trait TimestampLogger extends Logger {
  abstract override def log(msg: String) {
    super.log(new java.util.Date() + " " + msg)
  }
}
```


<br/>

### trait for rich interface

```scala
trait Logger {
  def log(msg: String)
  def info(msg: String) { log("INFO: " + msg) }
  def warn(msg: String) { log("WARN: " + msg) }
  def severe(msg: String) { log("SEVERE: " + msg) }
}

class SavingsAccount extends Account with Logger {
  def withdraw(amount: Double) {
    if (amount > balance) severe("Insufficient funds")
    else ...
  }
  ...
  override def log(msg: String) { println(msg) }
}
```


<br/>

### trait의 구체 필드

트레이트의 필드에 초기값을 제공할 수 있다.  

```scala
trait ShortLogger extends Logged {
  val maxLength = 15
}
```


```scala
class Account {
  var balance = 0.0
}

class SavingsAccount extends Account with ConsoleLogger with ShortLogger {
  var interest = 0.0
  def withdraw(amount: Double) {
    if (amount > balance) log("Insufficient funds")
    else ...
  }
}
```

위와 같이 클래스를 구성하면, ShortLogger의 maxLength는 서브클래스의 필드로 여겨진다. balance는 슈퍼클래스의 필드다.  


<br/>

### abstract field of trait

추상 필드가 있는 트레이트는 구체 서브클래스에서 반드시 오버라이드 되어야 한다.

```scala
trait ShortLogger extends Logged {
  val maxLength: Int  // an abstract field
  override def log(msg: String) {
    super.log(
      // maxLength 필드가 구현에 사용된다.
      if (msg.length <= maxLength) msg else msg.substring(0, maxLength - 3) + "...")
    )
  }
}
```

위와 같은 트레이트를 구체 클래스에서 사용할 때는 반드시 maxLength 필드의 값을 명시해주어야 한다.  

```scala
class SavingsAccount extends Account with ConsoleLogger with ShortLogger {
  val maxLength = 20   // override 키워드는 필요없음.
}

val acct = new SavingsAccount with ConsoleLogger with ShortLogger {
  val maxLength = 20
}
```


<br/>

### 트레이트 생성 순서

```scala
trait FileLogger extends Logger {
  val out = new PrintWriter("app.log")  // 트레이트 생성자의 일부
  out.println("# " + new Date().toString) // 트레이트 생성자의 일부

  def log(msg: String) { out.println(msg); out.flush() }
}
```

생성자들의 실행 순서  
1. 슈퍼클래스 생성자가 처음으로 호출된다.  
2. 트레이트 생성자는 슈퍼클래스 생성자 이후 클래스 생성자 전에 실행된다.  
3. 트레이트는 왼쪽에서 오른쪽으로 생성된다.  
4. 각 트레이트에서 부모가 먼저 생성된다.  
5. 여러 트레이트가 공통의 부모를 공유하고 부모가 이미 생성되었으면 다시는 생성하지 않는다.  
6. 모든 트레이트가 생성된 후에, 서브클래스가 생성된다.  

```scala
class SavingsAccount extends Account with FileLogger with ShortLogger
```

위의 경우 생성자는 다음 순서로 실행된다. (클래스 선형화의 반대 순서)  
1. Accoount (슈퍼클래스)  
2. Logger (첫 번째 트레이트의 부모)  
3. FileLogger (첫 번째 트레이트)  
4. ShortLogger (두 번째 트레이트)  
5. SavingsAccount (클래스)  


<br/>

### 트레이트 필드 초기화  

트레이트는 생성 인자를 가질 수 없고, 인자 없는 생성자를 가진다.  
트레이트와 클래스는 생성 인자 유무의 차이만 존재한다.  
트레이트를 사용하는 서브클래스 생성시 인자를 줄 수 없으므로 다른 방법을 사용해야 한다.  


* 조기 정의 사용  

```scala
val acct = new {  // 조기정의 블록
  val filename = "myapp.log"
} with SavingsAccount with FileLogger
```

혹은 클래스에서 할 필요가 있을때,  

```scala
class SavingsAccount extends {
  val filename = "savings.log"
} with Account with FileLogger {
  ... // SavingsAccount 구현
}
```


* 레이지 값 사용

```scala
trait FileLogger extends Logger {
  val filename: String
  lazy val out = new PrintStream(filename)
  def log(msg: String) { out.println(msg) }   // override는 필요하지 않음
}
```

out 필드는 처음 사용될 때 마다 초기화된다.  
레이지 값은 사용할 때 마다 초기화를 확인하므로 효울이 떨어질 수 있다.  


<br/>

### 클래스를 확장한 트레이트  

```scala
trait LoggedException extends Exception with Logged {
  def log() { log(getMessage()) }
}

class UnhappyException extends LoggedException {
  override def getMessage() = "arggh!"
}
```

LoggedException 트레이트의 슈퍼클래스 Exception은 UnhappyException의 슈퍼클래스가 된다.  



서브클래스가 클래스를 상속하고 이미 클래스를 확장한 트레이트를 사용하는 경우,  
서브클래스가 상속한 클래스와 트레이트가 상속한 클래스가 관련 있는 클래스이면 가능하다.  

```scala
class UnhappyException extends IOException with LoggedException
```

위에서는 IOException이 이미 Exception을 확장한 클래스이다. 이런 경우에만 사용 가능하다.  
아래와 같은 경우는 사용할 수 없다.  

```scala
class UnhappyFrame extends JFrame with LoggedException // error
```


<br/>

### 셀프 타입  

- 클래스를 지정하는 셀프타입  

  트레이트가 `this: type =>` 으로 시작하는 경우, 해당 type의 서브클래스와 믹스인 가능.  

  ```scala
  trait LoggedException extends Logged {
    this: Exception =>
      def log() { log(getMessage()) }
  }
  ```

  위 트레이트는 Exception을 확장하는 것이 아니라 셀프 타입을 가짐. -> Exception의 서브클래스에만 믹스 될 수 있음.  


- 메소드를 지정하는 셀프타입  

  ```scala
  trait LoggedException extends Logged {
    this: { def getMessage() : String } =>
      def log() { log(getMessage()) }
  }
  ```  

  위 트레이트는 getMessage 메소드를 가진 어떤 클래스나 믹스인 가능하다.  
