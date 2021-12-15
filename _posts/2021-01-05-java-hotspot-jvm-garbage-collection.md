---
title: "Java Garbage Collection"
layout: post
---


# Hotspot JVM Garbage Collection

Generational Collection 방식 사용.

Heap을 object generation 별로 young area와 old area로 구분.  
young area는 다시 Eden area/ survivor area 두개로 구분.
- Eden Area: object allocation 만을 위한 전용 공간.
- Survivor Area: the place to move objects that have survived minor garbage collection. 
  
card table: old area memory를 대표하는 별도의 memory structure.

GC mechanism은 경험적 지식(weak generational hypothesis)으로 두가지 가설을 둠.  
1. 대부분의 객체는 생성된 후 금방 Garbage가 된다.
2. old object가 young object를 참조할 일은 드물다.

Garbage를 추적하는 부분은 tracing algorithm 사용.  
- Root set에서 참조 관계를 추적. 참조되고 있는 객체는 marking.
- marking은 suspend 상태에서 수행되기 때문에 young area에 한해 수행.

## Young Area Garbage Collection

The young area consists of ***an eden area*** and ***two survivor areas***.

most of the newly generated objects are located in the eden area.

Objects survived after GC has occurred on the Eden area move to one of survivor area.

Objects are stacked on one of the survivor areas continuously. when it is full,  move objects that be living to the other survivor area and empty the survivor area which was full.

After repeating this process, the surviving objects are moved to the old area.

### bump-the-pointer

Eden area에 할당된 마지막 객체 추적.
그 다음 삽입할 객체가 빈 공간에 적절한지 확인
multi-thread 환경에 적절치 않음.

### TLABs (Thread-Local Allocation Buffers)

각각의 thread 가 각각의 몫에 해당하는 Eden 영역의 작은 덩어리를 가짐. 각 스레드는 자기 TLAB에만 접근 가능.


## Old Area GC

JDK 7 기준 

### Serial GC

`-XX:+UseSerialGC`

mark-sweep-compact

single thread

### Parallel GC

(=Throughput GC)

`-XX:+UseParallelGC`

mechanism은 serial gc와 동일. 
multi thread로 작동함. 메모리가 충분하고 코어의 갯수가 많을 때 유리.

### Parallel Old GC 
(=Parallel Compacting GC)

`-XX:+UseParallelOldGC`

Parallel GC와 비교하여, old area의 gc algorithm만 다름.

mark-summary-compaction

### Concurrent Mark & Sweep GC 
(=CMS GC)

`-XX:+UseConcMarkSweepGC`

- initial mark : class loader에서 가장 가까운 객체 중 살아있는 객체만 찾음.  
- concurrent mark : initial mark에서 찾은 살아남은 객체에서 창조하고 있는 객체들을 따라가면서 확인. 다른 스레드가 실행중인 상테에서 동시에 실행.  
- remark : concurrent mark에서 새로 추가되거나 창조가 끊긴 객체 확인.  
- sweep : garbage 정리. 다른 스레드가 실행중인 상황에서 진행.

stop-the-world 시간이 매우 짧음.

다른 GC 보다 memory와 cpu를 더 많이 사용.

compaction 단계가 없음 -> compaction을 실행하면 다른 GC의 stop-the-world 시간보다 더 긴 시간 소요.

### G1(Garbage First) GC

matrix의 각 영역에 객체를 할당해두고 GC 실행.  
해당 영역이 꽉 차면 다른 영역에서 객체 할당. GC 실행.  
young area에서 old area로의 promote 단계가 없음.  

---
java 8 - Parallel GC
java 11 - G1 GC

---
### References
- <https://coding-start.tistory.com/206>
- <https://d2.naver.com/helloworld/1329>
