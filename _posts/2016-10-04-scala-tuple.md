---
title: "Scala - Tuple"
layout: post
---


## Tuple

aggregates of values of different types  

```scala
val t = (1, 3.14, "Fred")

val second = t._2    // 3.14
// start with 1, not 0

val (first, second, third) = t
val (first, second, _) = t

"New York".partition(_.isUpper)   // ("NY", "ew ork")
```


<br/>


## Zipping

```scala
val symbols = Array("<", "-", ">")
val counts = Array(2, 10, 2)
val pairs = symbols.zip(counts)

// Array(("<", 2), ("-", 10), (">", 2))
```
