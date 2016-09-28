---
title: "Apache Spark - RDD Transformation"
layout : post
---


* `map(func)`	 
RDD의 각 요소에 함수를 적용하고, 결과를 새 RDD로 리턴한다.


* `filter(func)`  
func가 true를 반환하는(함수의 조건을 통과한 값) element들을 새 RDD로 리턴한다.


* `flatMap(func)`  
map과 유사. 입력되는 element는 0이상의 output과 mapping된다.
(func가 단일 아이템보다는 시퀀스를 리턴한다.)

* `mapPartitions(func)`  
Similar to map, but runs separately on each partition (block) of the RDD, so func must be of type Iterator<T> => Iterator<U> when running on an RDD of type T.


* `mapPartitionsWithIndex(func)`  
Similar to mapPartitions, but also provides func with an integer value representing the index of the partition, so func must be of type (Int, Iterator<T>) => Iterator<U> when running on an RDD of type T.


* `sample(withReplacement, fraction, seed)`  
복원(비복원)추출로 RDD에서 표본을 추출한다.


* `union(otherDataset)`  
source RDD와 otherDataset(RDD)를 합친 새 RDD를 리턴한다.


* `intersection(otherDataset)` 	
source RDD와 otherDataset(RDD)의 교집합인 RDD를 리턴한다.


* `distinct([numTasks]))`	 
source RDD의 중복을 제거한 새 RDD 반환.


* `groupByKey([numTasks])`	 
When called on a dataset of (K, V) pairs, returns a dataset of (K, Iterable<V>) pairs.
Note: If you are grouping in order to perform an aggregation (such as a sum or average) over each key, using reduceByKey or aggregateByKey will yield much better performance.
Note: By default, the level of parallelism in the output depends on the number of partitions of the parent RDD. You can pass an optional numTasks argument to set a different number of tasks.


* `reduceByKey(func, [numTasks])` 	
When called on a dataset of (K, V) pairs, returns a dataset of (K, V) pairs where the values for each key are aggregated using the given reduce function func, which must be of type (V,V) => V. Like in groupByKey, the number of reduce tasks is configurable through an optional second argument.


* `aggregateByKey(zeroValue)(seqOp, combOp, [numTasks])` 	
When called on a dataset of (K, V) pairs, returns a dataset of (K, U) pairs where the values for each key are aggregated using the given combine functions and a neutral "zero" value. Allows an aggregated value type that is different than the input value type, while avoiding unnecessary allocations. Like in groupByKey, the number of reduce tasks is configurable through an optional second argument.


* `sortByKey([ascending], [numTasks])` 	
When called on a dataset of (K, V) pairs where K implements Ordered, returns a dataset of (K, V) pairs sorted by keys in ascending or descending order, as specified in the boolean ascending argument.


* `join(otherDataset, [numTasks])` 	
When called on datasets of type (K, V) and (K, W), returns a dataset of (K, (V, W)) pairs with all pairs of elements for each key. Outer joins are supported through leftOuterJoin, rightOuterJoin, and fullOuterJoin.


* `cogroup(otherDataset, [numTasks])` 	
When called on datasets of type (K, V) and (K, W), returns a dataset of (K, (Iterable<V>, Iterable<W>)) tuples. This operation is also called groupWith.


* `cartesian(otherDataset)`  
When called on datasets of types T and U, returns a dataset of (T, U) pairs (all pairs of elements).


* `pipe(command, [envVars])` 	
Pipe each partition of the RDD through a shell command, e.g. a Perl or bash script. RDD elements are written to the process's stdin and lines output to its stdout are returned as an RDD of strings.


* `coalesce(numPartitions)` 	
Decrease the number of partitions in the RDD to numPartitions. Useful for running operations more efficiently after filtering down a large dataset.


* `repartition(numPartitions)` 	
Reshuffle the data in the RDD randomly to create either more or fewer partitions and balance it across them. This always shuffles all data over the network.


* `repartitionAndSortWithinPartitions(partitioner)` 	
Repartition the RDD according to the given partitioner and, within each resulting partition, sort records by their keys. This is more efficient than calling repartition and then sorting within each partition because it can push the sorting down into the shuffle machinery.
