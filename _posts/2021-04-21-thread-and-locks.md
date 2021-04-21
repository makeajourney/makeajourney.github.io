---
title: "Thread and Locks"
layout: post
---


- 공유되는 변수에 대한 접근을 반드시 동기화.  
- 쓰는 스레드와 읽는 스레드가 모두 동기화 되어야 함.  
- 여러 개의 잠금장치를 미리 정해진 공통의 순서에 따라 요청.  
- 잠금장치를 가진 상태에서 외부 메서드를 호출하지 않음.  
- 잠금장치는 최대한 짧게 보유.  


Java Memory Model [link](http://www.cs.umd.edu/~pugh/java/memoryModel/)
[JSR 133 FAQ](https://www.cs.umd.edu/~pugh/java/memoryModel/jsr-133-faq.html) 
- [번역](https://medium.com/@qwefgh90/jsr-133-java-memory-model-faq-%EB%B2%88%EC%97%AD-128487aebc1e)
