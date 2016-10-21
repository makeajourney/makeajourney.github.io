---
title : "Apache Spark - Key-Value Pair"
layout : post
---

## Key-Value Pair

집합 연산 등에 빈번하게 쓰임.  

스파크는 키/값 쌍을 가지고 있는 RDD에 대해 특수한 연산 제공.  
pair rdd라고 불림.  


<br/>

### Pair RDD 생성

키를 가지는 데이터로 동작하는 함수들을 고려해 튜플로 구성된 RDD 리턴.  

- python

```py
pairs = lines.map(lambda x: (x.split(" ")[0], x))
```

- scala

```scala
val pairs = lines.map(x => (x.split(" ")(0), x))
```

스칼라나 파이썬에서 메모리에 있는 데이터로부터 페어 RDD를 만들어내려면 페어 데이터세트에 Spark.Context.parallelize() 호출.  


<br/>

### pair rdd transformation

기본 RDD에서 가능한 모든 트랜스포메이션을 사용할 수 있다.  
단, 페어 RDD는 튜플을 가지므로, 개별데이터를 다루는 함수 대신 튜플을 처리하는 함수를 전달해야 한다.  


#### functions

rdd example

```scala
{(1, 2), (3, 4), (3, 6)}
```


`reduceByKey(func)` : 동일 키에 대한 값들을 합친다.  

```scala
rdd.reduceByKey((x, y) => x + y)  // result : {(1, 2), (3, 10)}
```


`groupByKey()` : 동일 키에 대한 값들을 그룹화한다.  

```scala
rdd.groupByKey()    // result : {(1, [2]), (3, [4, 6])}
```


`combineByKey(createConbiner, mergeValue, mergeConbiners, partitioner)` :
다른 결과타입을 써서 동일 키의 값을 합친다.


`mapValues(func)` : 키의 변경 없이 페어 RDD의 각 값에 함수를 적용한다.  

```scala
rdd.mapValues(x => x + 1)   // result : {(1, 3), (3, 5), (3, 7)}
```


`flatMapValues(func)` : pair RDD의 각 값에 대해 반복자를 리턴하는 함수를 적용하고, 리턴받은 값들에 대해 기존 키를 써서 키/값 쌍을 만든다. 종종 token 분리에 쓰인다.  

```scala
rdd.flatMapValues(x => (x to 5))
// {(1, 2), (1, 3), (1, 4), (1, 5), (3, 4), (3, 5)}
```


`keys()` : RDD가 가진 키들만을 되돌려준다.  

```scala
rdd.keys()    //result : {1, 3, 3}
```


`values()` : RDD가 가진 값들만을 되돌려준다.  

```scala
rdd.values()  // result : {2, 4, 6}
```


`sortByKey()` : 키로 정렬된 RDD를 되돌려준다.  

```scala
rdd.sortByKey()   // result : {(1, 2), (3, 4), (3, 6)}
```


#### 두 pair RDD에 대한 functions

rdd = {(1, 2), (3, 4), (3, 6)}, other = {(3, 9)}


`subtractByKey` : 다른 쪽 RDD에 있는 키를 써서 RDD의 데이터를 삭제한다.  

```scala
rdd.subtractByKey(other)  // result : {(1, 2)}
```


`join` : 두 RDD에 대해 이너 조인(inner join)을 수행한다.  

```scala
rdd.join(other)   // result : {(3, (4, 9)), (3, (6, 9))}
```


`rightOuterJoin` : 첫 번째 RDD에 있는 키들을 대상으로 두 RDD간에 조인을 수행한다.  

```scala
rdd.rightOuterJoin(other) // result : {(3, (Some(4), 9)), (3, (Some(6), 9))}
```


`leftOuterJoin` : 다른 쪽 RDD에 있는 키들을 대상으로 두 RDD 간에 조인을 수행한다.  

```scala
rdd.leftOuterJoin(other)  // result : {(1, (2, None)), (3, (4, Some(9))), (3, (6, Some(9)))}
```


`cogroup` : 동일 키에 대해 양쪽 RDD를 그룹화한다.  

```scala
rdd.cogroup(other)    // result : {(1, ([2], [])), (3, ([4, 6], [9]))}
```


#### value에 filter 적용

```scala
pairs.filter{ case (key, value) => value.length < 20 }
```

```scala
map { case(x, y): (x, func(y))} // mapValues(func)와 동일
```


<br/>

## 집합 연산

스칼라에서 reduceByKey()와 mapValues()로 키별 평균 구하기  

```scala
rdd.mapValues(x => (x, 1)).reduceByKey((x, y) => (x._1 + y._1, x._2 + y._2))
```

```scala
// word count 예
val input = sc.textFile("s3://...")
val words = input.flatMap(x => x.split(" "))
val result = words.map(x => (x, 1)).reduceByKey((x, y) => x + y)
```

```scala
// combineByKey()를 사용한 키별 평균
val result = input.combineByKey(
  (v) => (v, 1),    // createConbiner
  (acc: (Int, Int), v) => (acc._1 + v, acc._2 + 1), // mergeValue
  (acc1: (Int, Int), acc2: (Int, Int)) => (acc1._1 + acc2._1, acc1._2 + acc2._2)  // mergeConbiners
).map { case(key, value) => (key, value._1 / value._2.toFloat) }
result.collectAsMap().map(println(_))
```

```scala
// 병렬화 직접 지정을 사용한 reduceByKey()
val data = Seq(("a", 3), ("b", 4), ("a", 1))
sc.parallelize(data).reduceByKey((x, y) => x + y) // 기본 병렬화 수준 사용.
sc.parallelize(data).reduceByKey((x, y) => x + y, 10)   // 병렬화 수준 지정.
```


<br/>

### sorting

```scala
val input: RDD[(Int, Venue)] = ...
implicit val sortIntegersByString = new Ordering[Int] {
  override def compare(a: Int, b: Int) = a.toString.compare(b.toString)
}
rdd.sortByKey(sortIntegersByString)
```


<br/>

### pair RDD Action

rdd example

```scala
{(1, 2), (3, 4), (3, 6)}
```

`countByKey()` : 각 키에 대한 값의 개수를 센다.  

```scala
rdd.countByKey()    // result : {(1, 1), (3, 2)}
```


`collectAsMap()` : 쉬운 검색을 위해 결과를 맵 형태로 모은다.  

```scala
rdd.collectAsMap()    // result : Map{(1, 2), (3, 4), (3, 6)}
```


`lookup(key)` : 들어온 키에 대한 모든 값을 되돌려 준다.  

```scala
rdd.lookup(3)   // result : [4, 6]
```


<br/>

### Data Partitioning

```scala
// code initialization, read user information from hadoop sequence file
val sc = new SparkContext(...)
val userData = sc.sequenceFile[UserID, UserInfo]("hdfs://...").persist()

// 지난 5분간의 이벤트 로그 파일을 처리하기 위해 주기적으로 불리는 함수.
// 여기서 처리하는 시퀀스 파일이 (UserId, LinkInfo) 쌍을 갖고 있다고 가정.
def processNewLogs(logFileName: String) {
  val events = sc.sequenceFile[UserID, LinkInfo](logFileName)
  val joined = userData.join(events)  // (UserID, (UserInfo, LinkInfo))를 페어로 가지는 RDD

  val offTopicVisits = joined.filter {
    // 각 아이템을 그 컴포넌트들로 확장한다.
    case (userId, (userInfo, linkInfo)) => !userInfo.topics.contains(linkInfo.topic)
  }.count()
  println("Number of visits to non-subscribed topics: " + offTopicVisits)
}
```


### 파티셔닝 연산

페이지 랭크 알고리즘  

```scala
// 이웃 리스트는 스파크 오브젝트 파일에 저장되어 있다고 가정.
val links = sc.objectFile[(String, Seq[String])]("links").partitionBy(new HashPartitioner(100)).persist()

// 각 페이지의 랭크를 1.0으로 초기화. mapValues()를 사용하므로 결과 RDD는 links와 동일한 파티셔너를 가진다.
var ranks = links.mapValues(v => 1.0)

// 10회 반복하여 알고리즘 수행
for (i <- 0 until 10) {
  val contributions = links.join(ranks).flatMap {
    case (pageId, (links, rank)) =>
      links.map(dest => (dest, rank / links.size))
  }
  ranks = contributions.reduceByKey((x, y) => x + y).mapValues(v => 0.15 + 0.85 * v)
}

// 결과 기록
ranks.saveAsTextFile("ranks")
```
