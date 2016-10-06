---
title : "Scala - File"
layout: post
---


## File  


### reading lines  

```scala
import scala.io.Source

val source = Source.fromFile("myfile.txt", "UTF-8")
val lineIterator = source.getLines
```  


결과는 iterator로 만들어진다. 이를 반복문을 통해 한줄씩 처리할 수 있다.  

```scala
for (l <- lineIterator) process l
```  


iterator에 `toArray`나 `toBuffer` 메소드를 적용하여 줄의 배열이나 배열 버퍼에 넣을 수 있다.  

```scala
val lines = source.getLines.toArray
```  


하나의 문자열로 읽어올 수 있다.  
```scala
val contents = source.mkString
```


파일을 다 읽은 다음에는 파일을 닫아줘야 한다.  

```scala
source.close()
```


<br/>

### reading characters

개별 문자들을 읽으려면 Source 오브젝트를 직접 이터레이터로 사용.

```scala
for (c <- source) process c
```  


문자를 소모하지 않고 확인만 하려는 경우

```scala
val source = Source.fromFile("myfile.txt", "UTF-8")
val iter = source.buffered
while (iter.hasNext) {
  if (iter.head /* conditions*/)
    process iter.next
  else
    ....
}
source.close()
```


<br/>

### reading tokens

```scala
// 공백으로 구분된 모든 토큰을 읽는다.
val tokens = source.mkString.split("\\s+")
```


숫자로 변환하고 싶을 때는 `toDouble`, `toInt` 메소드를 사용한다.

```scala
val numbers = for (w <- tokens) yield w.toDouble
```

```scala
val numbers = tokens.map(_.toDouble)
```


<br/>

### read numbers from the console

```scala
print("How old are you? ")
val age = readInt() // or use readDouble, readLong
```


<br/>

### reading from URLs

```scala
val source1 = Source.fromURL("http://horstmann.com", "UTF-8")
```


<br/>

### reading from the given String

```scala
val source2 = Source.fromString("Hello, world!")
```


<br/>

### reading from standard input

```scala
val source3 = Source.stdin
```


<br/>

### reading binary files

자바 `FileInputStream`을 사용해야 함.  

```scala
val file = new File(filename)
val in = new FileInputStream(file)
val bytes = new Array[Byte](file.length.toInt)
in.read(bytes)
in.close()
```


<br/>

### Writing text files

자바 `PrintWriter` 사용  

```scala
val out = new PrintWriter("numbers.txt")
for (i <- 1 to 100) out.println(i)
out.close()
```  


<br/>

### visiting directories

```scala
import java.io.File
def subdirs(dir: File): Iterator[File] = {
  val children = dir.listFiles.filter(_.isDirectory)
  children.toIterator ++ children.toIterator.flatMap(subdirs _)
}

for (d <- subdirs(dir)) process d
```


혹은 `java.nio.file.Files` 클래스의 `walkFileTree`를 사용.  

```scala
import java.nio.file._
implicit def makeFileVisitor(f: (Path) => Unit) = new SimpleFileVisitor[Path] {
  override def visitFile(p: Path, attrs: attribute.BasicFileAttributes) = {
    f(p)
    FileVisitResult.CONTINUE
  }
}

Files.walkFileTree(dir.toPath, (f: Path) => println(f))
```


<br/>

### Serialization

```scala
class Person extends Serializable {
  private val friends = new ArrayBuffer[Person] // Ok - ArrayBuffer is serializable
  ...
}
```
