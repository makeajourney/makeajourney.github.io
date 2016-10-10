---
title : "Scala - Operator"
layout : post
---


## Operator (연산자)  


<br/>

## identifier (인식자)  

기본적으로 자바와 비슷하다.  

추가로 아래와 같은 연산자 문자들을 사용할 수 있다.  
- 문자, 숫자, 밑줄, 괄호 `()` `[]` `{}`, 혹은 구분자 `.,;'"` 등을 제외한 아스키 문자(`!#$%&*+-/:<=>?@\^|~`).  
- 유니코드 수학 부호나 유니코드 범주 Sm과 So에 속하는 다른 부호들.  


예약어도 사용 가능하다. 예약어를 인식자로 사용하고자 할 때는 백쿼트 \`\`를 사용하면 된다.  

```scala
val `val` = 43
```

보통의 경우 이렇게 사용하지 않겠지만, 스칼라의 예약어와 같은 이름의 자바 메소드에 접근해야 할 경우에 유용하게 사용할 수 있다.  

```scala
Thread.`yield`()
```


<br/>

## infix Operator (삽입 연산자)  

`a identifier b`  

`1 to 10` is actually `1.to(10)`  
`1 -> 10` is actually `1.->(10)`  

연산자가 인자 사이에 있기 때문에 삽입 표현식이라고 부름.  


<br/>

## unary operator (단항 연산자)  

인자가 하나인 연산자.  


### postfix operator (후위 연산자)

연산자가 인자 뒤에 오면 후위 연산.  

`a identifier` is actually `a.identifier()`  
`1 toString` is actually `1.toString()`  


<br/>

### prefix operator (전위 연산자)

인자 앞에 연산자가 오면 전위 연산.  

`-a` is actually `a.unary_-`  


<br/>

## assignment operator (할당 연산자)  

`a operator= b` is actually `a = a operator b`  
`a += b` is actually `a = a + b`  


<br/>

## priority (우선 순위)

할당 연산자를 제외하고, 우선순위는 연산자의 첫 번째 문자가 결정.  

>> highest : 아래 나열되지 않은 연산자  
>> `*` `/` `%`  
>> `+` `-`  
>> `:`  
>> `<` `>`  
>> `!` `=`  
>> `&`  
>> `^`  
>> `|`  
>> 연산자 문자가 아닌 문자  
>> lowest : 할당 연산자  

같은 열에 있는 문자들은 같은 우선순위.  
후위 연산자는 삽입 연산자에 비해 우선순위가 낮다.  


<br/>

## associativity (결합성)  


### 왼쪽 결합  

오른쪽으로 결합하는 연산자를 제외한 모든 연산자.  
예를 들어, `17 - 2 - 9`는 `(17 - 2) - 9`로 계산한다.  


### 오른쪽 결합  

- 콜론(:)로 끝나는 연산자  
- 할당 연산자  


예를 들어, `1 :: 2 :: Nil`은 `1 :: (2 :: Nil)`  
`2 :: Nil` is actually `Nil.::(2)`  


<br/>

## apply와 update 메소드  

함수호출 문법인 `f(arg1, arg2, ...)` 을 함수가 아닌 곳에서도 사용할 수 있다.  

```scala
f.apply(arg1, arg2, ...)
```

`f(arg1, arg2, ...) = value` 는 `f.update(arg1, arg2, ...)`와 대응.  

이 방식은 배열과 맵에 사용된다.  


`apply` 메소드는 new 없이 오브젝트를 생성하는 컴패니언 오브젝트에서 일반적으로 사용.  

```scala
class Fraction(n: Int, d: Int) {
  ...
}

object Fraction {
  def apply(n: Int, d: Int) = new Fraction(n, d)
}
```


<br/>

## extractor (추출자)

`unapply` 메소드가 있는 오브젝트.  
오브젝트를 받아서 오브젝트 생성시 사용된 값을 추출.  

```scala
object Fraction {
  def unapply(input: Fraction) =
    if (input.den == 0) None else Some((input.num, input.den))
}
```

```scala
object Name {
  def unapply(input: String) = {
    val pos = input.indexOf(" ")
    if (pos == -1) None
    else Some((input.substring(0, pos), input.substring(pos + 1)))
  }
}

val author = "Cay Horstmann"
val Name(first, last) = author // Name.unapply(author) 호출
```

모든 케이스 클래스는 자동으로 apply와 unapply를 가진다.  


<br/>

## 인자 하나 혹은 인자 없는 추출자

(잘 모르겠다......)  

```scala
object Number {
  def unapply(input: String): Option[Int] =
    try {
      Some(Integer.parseInt(input.trim))
    } catch {
      case ex: NumberFormatException => None
    }
}

val Number(n) = "1729"
```

```scala
object IsCompound {
  def unapply(input: String) = input.contains(" ")
}

author match {
  case Name(first, last @ IsCompound()) => ...  // 저자가 Peter van der Linden이면 매치
  case Name(first, last) => ...
}
```


<br/>

## unapplySeq  

임의의 일련의 값 추출.  
Option[Seq[A]] 리턴.  

```scala
object Name {
  def unapplySeq(input: String): Option[Seq[String]] =
    if (input.trim == "") None else Some(input.trim.split("\\s+"))
}
```
