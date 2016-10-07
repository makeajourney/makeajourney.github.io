---
title : "Scala - Regular Expression"
layout: post
---


## Regular Expression

Regex 오브젝트를 만들려면 String 클래스의 r 메소드를 사용.  

```scala
val numPattern = "[0-9]+".r

// 역슬래시나 쌍따옴표가 포함되어 있는 경우, 아래처럼 하면 편리
val wsnumwsPattern = """\s+[0-9]+\s+""".r // "\\s+[0-9]+\\s+"
```


매치에 대한 이터레이터 제공  

```scala
for (matchString <- numPattern.findAllIn("99 bottles, 98 bottles"))
  process matchString
```


```scala
// 이터레이터를 배열로 바꿈.
val matches = numPattern.findAllIn("99 bottles, 98 bottles").toArray
```


첫번째 매치를 찾으려면 `findFirstIn` 사용. Option[String] 반환.  

```scala
val m1 = wsnumwsPattern.findFirstIn("99 bottles, 98 bottles")
```


문자열 시작이 매치되는지 확인.  

```scala
numPattern.findPrefixOf("99 bottles, 98 bottles")
wsnumwsPattern.findPrefixOf("99 bottles, 98 bottles")
```


첫번째 매치나 전체 매치 치환.  

```scala
numPattern.replaceFirstIn("99 bottles, 98 bottles", "XX")
wsnumwsPattern.replaceAllIn("99 bottles, 98 bottles", "XX")
```
