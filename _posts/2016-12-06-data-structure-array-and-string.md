---
title : "자료구조 : 01 배열과 문자열"
layout : post
---


# Array and String  


<br/>

## hash table  

```java
public HashMap<Integer, Student> buildMap(Student[] students) {
  HashMap<Integer, Student> map = new HashMap<Integer, Student>();
  for (Student s : students) map.put(s.getId(), s);
  return map;
}
```  


<br/>

## ArrayList  

```java
public ArrayList<String> merge(String[] words, String[] more) {
  ArrayList<String> sentence = new ArrayList<String>();
  for (string w : words) sentence.add(w);
  for (string w : more) sentence.add(w);
  return sentence;
}
```


<br/>

## StringBuffer

```java
public String joinWords(String[] words) {
  StringBuffer sentence = new StringBuffer();
  for (String w : words) {
    sentence.append(w);
  }
  return sentence.toString();
}
```
