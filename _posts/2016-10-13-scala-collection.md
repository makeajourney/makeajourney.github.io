---
title: "Scala - Collections"
layout : post
---

## Collections


### immutable

수정 불가능한 콜렉션은 절대 변하지 않아서 안전하게 이에 대한 레퍼런스를 공유하고 멀티스레드 프로그래밍도 가능.  

`scala.collection.mutable.Map` : 수정 가능한 콜렉션  
`scala.collection.immutable.Map` : 수정 불가능한 콜렉션  
위 두개의 Map 모두 `scala.collection.Map`을 가짐.  
`scala.collection` 패키지의 컴패니언 오브젝트는 수정 불가능한 콜렉션을 생성한다.  
`scala`, `Predef` 오브젝트는 수정 불가능한 트레이트를 지칭하는 List, Set, Map 타입 별칭을 갖고 있다.  

```scala
// 정수의 모든 자리 숫자의 집합 계산
def digits(n: Int): Set[Int] =
  if (n < 0) digits(-/n)
  else if (n < 10) Set(n)
  else digits(n / 10) + (n % 10)
```

위 메소드는 하나의 숫자를 포함한 집합으로 시작. 각 단계에서 다른 숫자가 더해짐.  
하지만 숫자 더하기가 집합을 변경하지는 않으며, 대신 각 단계에서 새로운 집합이 생성된다.  


<br/>

### Sequence

- Vector  
ArrayBuffer의 수정 불가능한 버전.  
빠른 무작위 접근이 가능한 인덱스 시퀀스.  
벡터는 각 노드가 32개까지 자식을 가질 수 있는 트리로 구현됨.  


- Range  
정수 시퀀스를 나타냄.  
Range 오브젝트는 모든 시퀀스 값을 저장하지는 않고, 시작, 끝, 증가분만 저장. to와 until 메소드로 Range 오브젝트 생성.  


<br/>

### List

```scala
val digits = List(4, 2)

digits.head     // 4
digits.tail     // List(2)
digits.tail.head  // 2
digits.tail.tail  // Nil

// List(9, 4, 2)
9 :: List(4, 2)
9 :: 4 :: 2 :: Nil
9 :: (4 :: (2 :: Nil))

// 재귀 호출을 이용해 모든 원소의 합 계산
def sum(lst: List[Int]): Int =
  if (lst == Nil) 0 else lst.head + sum(lst.tail)

// 패턴매칭 사용
def sum(lst: List[Int]): Int = lst match {
  case Nil => 0
  case h :: t => h + sum(t) // h는 lst.head, t는 lst.tail
}

// 이미 sum 함수는 있다.
List(9, 4, 2).sum   // 15를 돌려준다.
```


<br/>

### mutable

LinkedList는 elem 레퍼런스에 대입하여 머리를 수정할 수 있고, next 레퍼런스에 대입하여 꼬리를 수정할 수 있다는 점을 제외하면 수정 불가능한 List와 유사하게 동작한다.  

```scala
// 모든 음의 값을 0으로 변경
val lst = scala.collection.mutable.LinkedList(1, -2, 7, -9)
var cur = lst
while (cur != Nil) {
  if (cur.elem < 0)  cur.elem = 0
  cur = cur.next
}
```

```scala
// 리스트에서 매 두 번째 원소를 제거
var cur = lst
while (cur != Nil && cur.next != Nil) {
  cur.next = cur.next.next
  cur = cur.next
}
```

링크드리스트에서 리스트 노드를 리스트의 마지막 노드로 만들려면, next 레퍼런스를 Nil로 설정할 수 없다. 대신 LinkedList.empty로 설정해야 한다. null로 설정하지 말아야 한다.  


<br/>

### Set (집합)

서로 다른 원소의 콜렉션  

```scala
Set(2, 0, 1) + 1    // Set(2, 0, 1)과 동일
```

삽입된 순서를 보존하지 않는다.  
원소가 hashCode 메소드의 값에 따라 정리되는 해시 집합으로 구현된다.  
해시 집합에서 원소를 찾는 것은 배열이나 리스트에서 찾는 것 보다 훨씬 빠르다.  


<br/>

링크드 해시 집합은 원소가 삽입된 순서를 기억한다. 이 용도로 링크드 리스트를 유지한다.  

```scala
val weekdays = scala.collection.mutable.LinkedHashSet("Mo", "Tu", "We", "Th", "Fr")
scala.collection.immutable.SortedSet(1, 2, 3, 4, 5, 6)  // SortedSet : 정렬된 순서로 iterate
```

정렬된 집합은 레드-블랙 트리로 구현  
데이터 구조에 대한 책 <http://www.cs.williams.edu/javastructures/Welcome.html>  


<br/>

비트 집합은 음수가 아닌 정수의 집합을 일련의 비트로 구현한 것.  
i가 집합에 있으면 i번째 비트가 1이 된다.  
`BitSet` 클래스는 수정 가능한 클래스, 수정 불가능한 클래스 모두 제공한다.  

`contains` 메소드는 집합이 주어진 값을 가지고 있는지 확인한다.  
`subsetOf` 메소드는 집합의 모든 원소가 다른 집합에 속하는지 확인한다.  

```scala
val digits = Set(1, 7, 2, 9)
digits contains 0     // false
Set(1, 2) subsetOf digits // true
```

`union`, `intersect`, `diff` 메소드로 집합 연산 수행 가능.  
`|`, `&`, `%~`, `++`, `--` 연산자 사용 가능.


<br/>

### 원소들을 추가 혹은 제거하는 연산자  

| 연산자 | 설명 | 콜렉션 타입 |
|---|---|---|
| coll :+ elem | coll과 같은 타입의 콜렉션에 elem이 뒤에 추가. | Seq |
| elem +: coll | coll과 같은 타입의 콜렉션에 elem이 앞에 추가. | Seq |
| coll + elem <br/>coll + (e1, e2, ...) | coll과 같은 타입의 콜렉션에 주어진 원소가 더해짐. | Set, Map |
| coll - elem <br/>coll - (e1, e2, ...) | coll과 같은 타입의 콜렉션에 주어진 원소가 제거됨. | Set, Map, ArrayBuffer |
| coll ++ coll2<br/>coll2 ++: coll | coll과 같은 타입의 콜렉션에 양쪽 콜렉션의 원소들을 포함. | Iterable |
| coll -- coll2 | coll과 같은 타입의 콜렉션에 coll2의 원소들이 제거됨.<br/>(sequence의 경우, diff를 사용.) | Set, Map, ArrayBuffer |
| elem :: lst <br/>lst2 ::: lst | 원소 혹은 주어진 리스트 lst 앞에 추가된 리스트. +:와 ++:와 같음. | List |
| list ::: list2 | list ++: list2와 같음. | List |
| set <code>|</code> set2 | 합집합. | Set |
| set & set2 | 교집합. | Set |
| set &~ set2 | 차집합. | Set |
| coll += elem<br/>coll += (e1, e2, ...)<br/>coll ++= coll2 | 주어진 원소들을 추가하여 coll을 수정. | Mutable<br/>collections |
| coll -= elem<br/>coll -= (e1, e2, ...)<br/>coll --= coll2 | 주어진 원소들을 제거하여 coll을 수정. | Mutable<br/>collections |
| elem +=: coll<br/>coll2 ++=: coll | 주어진 원소나 콜렉션을 앞에 추가하여 coll을 수정. | ArrayBuffer |


<br/>

### common method

| method | description |
|---|---|
| head | 첫 원소를 리턴. |
| last | 마지막 원소를 리턴. |
| headOption | 첫 원소를 option으로 리턴. |
| lastOption | 마지막 원소를 option으로 리턴. |
| tail | 첫 원소를 제외한 모두를 리턴. |
| init | 마지막 원소를 제외한 모두를 리턴. |
| length | 길이를 리턴. |
| isEmpty | 길이가 0이면 true를 리턴. |
| map(f), foreach(f), flatMap(f), collect(pf) | 함수를 모든 원소에 적용. |
| reduceLeft(op), reduceRight(op), <br/>foldLeft(init)(op), foldRight(init)(op) | 이항 연산을 모든 원소에 주어진 순서로 적용. |
| reduce(op), fold(init)(op),<br/>aggregate(init)(op, conbineOp) | 이항 연산을 임의의 순서로 적용 |
| sum | 합(원소 타입이 암묵적으로 Numeric 트레이트로 변환될 수 있다는 가정하에) 리턴. |
| product | 곱(원소 타입이 암묵적으로 Numeric 트레이트로 변환될 수 있다는 가정하에) 리턴. |
| max | 최대(원소 타입이 Ordered 트레이트로 변환될 수 있다는 가정하에). |
| min | 최소(원소 타입이 Ordered 트레이트로 변환될 수 있다는 가정하에). |
| count(pred) | 조건을 만족하는 원소들의 숫자를 리턴. |
| forall(pred) | 모든 원소가 조건을 만족하면 true. |
| exists(pred) | 하나의 원소가 조건을 만족하면 true. |
| filter(pred) | 조건을 만족하는 모든 원소를 리턴. |
| filterNot(pred) | 조건을 만족하지 않는 모든 원소를 리턴. |
| partition(pred) | 조건을 만족하는 원소들과 만족하지 않는 원소들을 분리하여 pair로 제공. |
| takeWhile(pred) | pred를 만족하는 첫번째 원소를 리턴. |
| dropWhile(pred) | pred를 만족하는 원소 중 첫번째 원소를 제거하고 리턴. |
| span(pred) | takeWhile과 dropWhile의 결과값을 pair로 리턴. |
| take(n) | 첫 n개의 원소를 리턴. |
| drop(n) | 첫 n개를 제외한 모든 원소를 리턴. |
| splitAt(n) | take와 drop의 결과값을 pair로 리턴. |
| takeRight(n) | 마지막 n개의 원소를 리턴. |
| dropRight(n) | 마지막 n개를 제외한 모든 원소 리턴. |
| slice(from, to) | from에서 to 범위의 원소를 리턴. |
| zip(coll2), <br/>zip(coll2, fill, fill2),<br/>zipWithIndex | 이 콜렉션과 다른 콜렉션 원소의 쌍들을 리턴. |
| grouped(n) | 원소를 n개 단위로 묶은 iterator 제공. |
| sliding(n) | 1 to n, 2 to n+1 형태로 원소를 n개 단위로 묶은 이터레이터 제공. |
| mkString(before, between, after) | 주어진 문자열을 첫 번째 전, 각 원소 사이, 마지막 원소 후에 추가하여 모든 원소의 문자열을 만듬. |
| addString(sb, before, between, after) | 모든 원소의 문자열을 만든 후 문자열 빌더에 추가. |
| toIterable, toSeq, toIndexedSeq<br/>toArray, toList, toStream<br/>toSet, toMap| 콜렉션을 지정된 타입의 콜렉션으로 변환. |
| copyToArray(arr), <br/>copyToArray(arr, start, length), <br/>copyToBuffer(buf) | 원소들을 배열 혹은 버퍼로 복사. |


<br/>

### folding

```scala
val freq = scala.collection.mutable.Map[Char, Int]()
for (c <- "Mississippi") freq(c) = freq.getOrElse(c, 0) + 1
// freq : Map(M -> 1, s -> 4, p -> 2, i -> 4)
```

```scala
(Map[Char, Int]() /: "Mississippi") {
  (m, c) => m + (c -> (m.getOrElse(c, 0) + 1))
}
// This returns immutable map : Map(M -> 1, i -> 4, s -> 4, p -> 2)
```


<br/>

### scan

모든 중간 결과의 콜렉션을 얻는다.  

```scala
(1 to 10).scanLeft(0)(_ + _)
// result : Vector(0, 1, 3, 6, 10, 15, 21, 28, 36, 45, 55)
```


<br/>

### zipping

```scala
val prices = List(5.0, 20.0, 9.95)
val quantities = List(10, 2, 1)

prices zip quantities
// result : List[(Double, Int)] = List((5.0,10), (20.0,2), (9.95,1))

(prices zip quantities) map { p => p._1 * p._2 }
// result : List[Double] = List(50.0, 40.0, 9.95)

((prices zip quantities) map { p => p._1 * p._2 }) sum
// result : Double = 99.95
```


<br/>

### iterator

```scala
while (iter.hasNext)
  iter.next()

for (elem <- iter)
  elem
```


<br/>

### stream

스트림은 꼬리가 lazy하게 계산됨.  
`#::` 연산자를 통해 스트림 생성.  

```scala
def numsFrom(n: BigInt) : Stream[BigInt] = n #:: numsFrom(n + 1)
val tenOrMore = numsFrom(10)  // result : tenOrMore: Stream[BigInt] = Stream(10, ?)

```


<br/>

### 자바 콜렉션과의 상호 호환

자바 콜렉션을 사용하려면, 스칼라와 자바 콜렉션 사이 변환을 제공하는 `JavaConversions` 오브젝트를 가져온다.

```scala
import scala.collection.JavaConversions._
val props: scala.collection.mutable.Map[String, String] = System.getProperties()
props("com.horstmann.scala") = "impatient"  // put("com.hosrtmann.scala", "impatient") 호출
```


<br/>

### thread safe Collections

여러 스레드에서 수정 가능한 콜렉션에 접근 할 때, 다른 쓰레드들에서 접근하는 것과 동시에 한 쓰레드에서 콜렉션을 변경하지 않도록 해야 할 필요가 있다.  

```scala
// 동기화된 연산의 맵 생성
val scores = new scala.collection.mutable.HashMap[String, Int] with
  scala.collection.mutable.SynchronizedMap[String, Int]
```

혹은 `java.util.concurrent` 패키지의 클래스 사용. 데이터 구조와 관련있는 부분만 동기화하고, 관련 없는 부분은 다른 스레드에서 접근 가능.


<br/>

### 병렬 콜렉션

`par` 메소드 사용  

```scala
coll.par.sum  // coll: collection name. 합을 병렬로 계산
```

여기서 리턴되는 병렬 콜렉션은 ParIterable의 서브 타입. 순차컬렉션으로 변환하려면 `ser` 메소드 사용.  
