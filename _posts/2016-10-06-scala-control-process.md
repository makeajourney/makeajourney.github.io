---
title : "Scala - Control process"
layout: post
---


### process


`scala.sys.process`를 통해 쉘 프로그램과 연동 가능.  

```scala
import sys.process._
"ls -al" !
```  

위 구문 실행의 결과는 표준 출력으로 제공한다.  


`!!` : 출력이 문자열로 리턴.  

```scala
val result = "ls -al .." !!
```  


`#|` : 한 프로그램의 출력을 다른 프로그램으로 연결.  

```scala
"ls -al .." #| "grep sec" !
```  


`#>` : 출력을 파일로 리다이렉트.  

```scala
"ls -al .." #> new File("output.txt") !
```  


`#>>` : 기존 파일 뒤에 붙여서(append) 쓰기.  

```scala
"ls -al .." #>> new File("output.txt") !
```  


`#<` : 입력을 파일로부터 리다이렉트.  

```scala
"grep sec" #< new File("output.txt") !
```  


url로부터 입력을 리다이렉트.  

```scala
"grep Scala" #< new URL("http://horstmann.com/index.html") !
```  


프로세스를 다른 디렉토리나 환경변수로 실행할 필요가 있을 경우, Process 오브젝트의 apply 메소드로 ProcessBuilder 생성.

```scala
val p = Process(cmd, new File(dirName), ("LANG", "en_US"))
"echo 42" #| p !
```
