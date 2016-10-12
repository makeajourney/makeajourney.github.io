---
title: "Scala - Higher Order Function"
layout : post
---


## Higher Order Function (고차함수)  

스칼라에서는 함수의 인자로 변수와 같은 값 뿐만 아니라 함수도 넘길 수 있다. 혹은 함수를 리턴받을수도 있다.  


### function as a value (값으로서 함수)

함수를 변수에 저장.  

```scala
import scala.math._
val num = 3.14
val fun = ceil _ // fun을 ceil 함수로 설정. _는 함수를 뜻함.

// call fun
fun(num)  // 4.0

// 함수를 다른 함수에 전달
Array(3.14, 1.42, 2.0).map(fun)   // Array(4.0, 2.0, 2.0)
```


<br/>

### Anonymous Function (익명함수)

이름을 지정하지 않은 함수.  

```scala
// 인자에 3을 곱한다.
(x: Double) => 3 * x

// 변수에 저장
val triple = (x: Double) => 3 * x

//익명함수를 다른 함수에 넘김.
Array(3.14, 1.42, 2.0).map((x: Double) => 3 * x)  // Array(9.42, 4.26, 6.0)
```

맨 아래 줄 함수는 아래와 같이 작성할 수도 있다.  

```scala
Array(3.14, 1.42, 2.0).map{ (x: Double) => 3 * x }
Array(3.14, 1.42, 2.0) map { (x: Double) => 3 * x }
```


<br/>

### 함수 인자를 받는 함수

`(parameterType) => resultType`  

```scala
// 인자로 들어오는 함수에 0.25를 인자로 줌.
def valueAtOneQuarter(f: (Double) => Double) = f(0.25)

valueAtOneQuarter(ceil _) // 1.0
valueAtOneQuarter(sqrt _) // 0.5
```

위 valueAtOneQuarter 함수의 타입은 `((Double) => Double) => Double`  


<br/>

### 함수를 리턴

```scala
def mulBy(factor : Double) = (x : Double) => factor * x

val quintuple = mulBy(5)
quintuple(20) // 100
```

mulby 함수는 `(Double) => ((Double) => Double)`  


<br/>

### parameter inference (인자 추론)

```scala
valueAtOneQuarter((x: Double) => 3 * x) // 0.75
valueAtOneQuarter((x) => 3 * x)
valueAtOneQuarter(x => 3 * x)   // 인자가 하나이면 괄호 생략 가능
valueAtOneQuarter(3 * _)    // 인자가 화살표 오른쪽에 하나만 나오면 이를 밑줄로 변경 가능
```

인자 타입을 미리 알 때만 단축 사용 가능.  

```scala
val fun = 3 * _ // error: 타입 추론 불가
val fun = 3 * (_: Double) // OK
val fun: (Double) => Double = 3 * _ // fun의 type을 지정했기 때문에 ok
```


<br/>

### useful Higher order Function  

```scala
// 삼각형을 출력하는 map
(1 to 9).map("*" * _).foreach(println _)

// sequence에서 짝수만 얻는 방법
(1 to 9).filter(_ % 2 == 0)   // 2, 4, 6, 8

// 이항 함수를 받아서 왼쪽에서 오른쪽 순으로 시퀀스의 모든 원소에 적용.
(1 to 9).reduceLeft(_ * _)    // (...((1 * 2) * 3) * ... * 9)
"Mary had a little lamb".split(" ").sortWith(_.length < _.length)
// return : Array("a", "had", "Mary", "lamb", "little")
```


<br/>

### closure

```scala
def mulBy(factor: Double) = (x: Double) => factor * x
```

클로저는 코드와 함께 코드를 사용하는 비지역 변수의 정의들로 구성.  
이 함수들은 실제로는 인스턴스 변수 factor와 함수의 바디를 표함하고 있는 apply 메소드가 있는 클래스의 오브젝트로 구현된다.  


<br/>

### SAM (Single Abstract Method)

예를 들어 버튼이 클릭되었을 때 카운터를 증가시키길 원한다고 가정.

```scala
val counter = 0
val button = new JButton("Increment")
button.addActionListener(new ActionListener {
  override def actionPerformed(event: ActionEvent) {
    counter += 1
  }
})
```

를 아래와 같이 변경 가능.  

```scala
button.addActionListener((event: ActionEvent) => counter += 1)

// 암묵 변환 제공
implicit def makeAction(action: (ActionEvent) => Unit) =
  new ActionListener {
    override def actionPerformed(event: ActionEvent) { action(event) }
  }
```


<br/>

### Curring

인자 두개를 받는 함수를 인자 하나를 받는 함수로 바꾸는 프로세스.  

```scala
def mul(x: Int, y: Int) = x * y   // 두개의 인자를 받는 함수
def mulOneAtATime(x: Int) = (y: Int) => x * y   // 하나의 인자를 받아 인자 하나를 받는 함수 리턴.
mulOneAtATime(6)(7)   // 위 함수 호출
```

```scala
def mulOneAtATime(x: Int)(y: Int) = x * y // 커리 함수
```


```scala
val a = Array("Hello", "World")
val b = Array("hello", "world")
a.corresponds(b)(_.equalsIgnoreCase(_))
```


<br/>

### 제어 추상화

```scala
def runInThread(block: () => Unit) {
  new Thread {
    override def run() { block() }
  }.start()
}

runInThread { () => println("Hi"); Thread.sleep(10000); println("Bye") }
```

위 코드는 `() => Unit` 타입의 함수로 주어진다. 그러므로 호출 시에 () =>를 써주어야 한다.  
이를 피하려면 아래처럼 이름으로 호출 표기법을 사용한다. 인자 선언과 인자 함수 호출에서 ()를 생략하고, => 는 생략하지 않는다.

```scala
def runInThread(block: => Unit) {
  new Thread {
    override def run() { block }
  }.start()
}

runInThread { println("Hi"); Thread.sleep(10000); println("Bye") }
```

위 대신 until문을 정의해서 사용할 수도 있다.

```scala
def until(condition: => Boolean)(block: => Unit) {
  if (!condition) {
    block
    until(condition)(block)
  }
}
```

```scala
var x = 10
until (x == 0) {
  x -= 1
  println(x)
}
```


<br/>

### return 표현식

보통 스칼라는 값을 리턴하기 위해 `return`문을 사용하지 않는다.  
익명함수에서 바깥 이름 있는 함수에 값을 리턴하기 위해 return을 사용할 수 있다. 제어추상화에 유용하다.  

```scala
def indexof(str: String, ch: Char): Int = {
  var i = 0
  until (i == str.length) {
    if (str(i) == ch) return i
    i += 1
  }
  return -1
}
```

이름 있는 함수에서 return을 사용할 때는 리턴 타입 지정 필요.  
