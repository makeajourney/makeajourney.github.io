---
title: "Java - Concurrency - Thread object"
layout: post
comments: true
---

http://docs.oracle.com/javase/tutorial/essential/concurrency/threads.html  



# concurrency  


concurrent programming의 기본 실행단위로 process와 thread가 있음.    
Java에서는 주로 thread와 관련이 있음.  


## thread  


thread를 새로 만드는 것은 새 process를 만드는 것보다 적은 자원이 필요.  
프로세스 내에 존재. 메모리 및 열린 파일을 포함하여 프로세스의 리소스를 공유.  
주 thread라는 하나의 thread로 시작하여, 추가 스레드를 생성하는 형태.  

스레드의 생성 및 관리를 직접 제어하려면 비동기 작업이 필요할 때 마다 스레드를 인스턴스화.  
스레드 관리를 추상화하려면 태스크를 실행 프로그램에 전달.  


1. Runnable object를 제공하여 thread 생성.

```java
public class HelloRunnable implements Runnable {

    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new Thread(new HelloRunnable())).start();
    }

}
```

Runnable 인터페이스는 스레드에서 실행되는 코드를 포함하는 단일 메소드 run을 정의.


2. thread 상속하여 thread 생성.

```java
public class HelloThread extends Thread {

    public void run() {
        System.out.println("Hello from a thread!");
    }

    public static void main(String args[]) {
        (new HelloThread()).start();
    }

}
```

Thread 클래스 자체는 Runnable의 구현체이지만 run 메서드는 아무 것도 수행하지 않음.  


두 예제 모두 새 스레드를 시작하기 위해 Thread.start를 호출.  
thread 클래스를 상속하는 경우, 다른 클래스의 상속이 제한되기 때문에 주로 Runnable의 implements하는 방식을 사용.  
Runnable 작업과 해당 작업을 실행하는 Thread 개체를 구분하여 사용.


### Thread.sleep  


Thread.sleep은 현재 스레드가 지정된 기간 동안 실행을 일시 중단하게함. 이는 컴퓨터 시스템에서 실행될 수있는 응용 프로그램이나 다른 응용 프로그램의 다른 스레드에서 프로세서 시간을 사용할 수있게하는 효율적인 방법입니다. sleep 메서드는 다음 예제와 같이 페이싱에 사용할 수도 있고 이후 섹션의 SimpleThreads 예제와 같이 시간 요구 사항이있는 것으로 이해되는 의무를 가진 다른 스레드를 기다릴 수도 있습니다.

밀리초와 나노초로 슬립시간 지정 가능. 기본 OS에서 제공하는 기능으로 인해 제한되기 때문에 정확하지 않음.
인터럽트에 의해 종료 될 수 있음.

SleepMessages 예제  
sleep을 사용하여 4 초 간격으로 메시지를 인쇄.

```java
public class SleepMessages {
    public static void main(String args[]) throws InterruptedException {
        String importantInfo[] = {
            "Mares eat oats",
            "Does eat oats",
            "Little lambs eat ivy",
            "A kid will eat ivy too"
        };

        for (int i = 0; i < importantInfo.length; i++) {
            //Pause for 4 seconds
            Thread.sleep(4000);
            //Print a message
            System.out.println(importantInfo[i]);
        }
    }
}
```

`InterruptedException` : sleep가 활성화되어있는 동안 다른 스레드가 현재 스레드를 인터럽트 할 때 sleep이 throw하는 예외.  


### interrupt

스레드가 인터럽트에 어떻게 반응하는지는 프로그래머가 결정하지만 스레드가 종료되는 경우가 가장 일반적.  

```java
for (int i = 0; i < importantInfo.length; i++) {
    // Pause for 4 seconds
    try {
        Thread.sleep(4000);
    } catch (InterruptedException e) {
        // We've been interrupted: no more messages.
        return;
    }
    // Print a message
    System.out.println(importantInfo[i]);
}
```


### join

join method는 한 스레드가 다른 스레드의 완료를 기다리는 것을 허용.

```java
t.join();
```

t의 스레드가 종료 될 때까지 현재 스레드의 실행을 일시 중지.  
