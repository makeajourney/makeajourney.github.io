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
