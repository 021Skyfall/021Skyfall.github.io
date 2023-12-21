---
title: "자바 : 스레드(Thread)와 자바 버츄얼 머신(JVM)"
author: Jeremiah Lee
date: 2023-01-10
categories: [ 자바, 기초 ]
tags: [자바, 부트캠프]
pin: false
math: true
mermaid: true
image: 
  path: /assets/img/Academic_jelly.png
  lqip: data:image/webp;base64,UklGRpoAAABXRUJQVlA4WAoAAAAQAAAADwAABwAAQUxQSDIAAAARL0AmbZurmr57yyIiqE8oiG0bejIYEQTgqiDA9vqnsUSI6H+oAERp2HZ65qP/VIAWAFZQOCBCAAAA8AEAnQEqEAAIAAVAfCWkAALp8sF8rgRgAP7o9FDvMCkMde9PK7euH5M1m6VWoDXf2FkP3BqV0ZYbO6NA/VFIAAAA
  alt: 
---
***

### 스레드

#### 프로세스 (Process)
- 실행 중인 애플리케이션을 뜻함
- 애플리케이션을 실행하면 운영체제로부터 실행에 필요한 만큼의 메모리를 할당 받아 프로세스가 됨
- 데이터, 컴퓨터 자원, 그리고 스레드로 구성

#### 싱글 스레드 프로세스 = 단 하나의 스레드를 가지는 프로세스   
#### 멀티 스레드 프로세스 = 여러 개의 스레드를 가지는 프로세스


### 스레드 (Thread)
- 데이터와 애플리케이션이 확보한 자원을 활용하여 소스 코드를 실행
- 프로세스 내에서 실행되는 소스 코드 실행 흐름

### 메인 스레드 (Main thread)   
=> 자바 애플리케이션을 실행하면 가장 먼저 실행되는 메서드는 main 메서드,   
메인 스레드가 main 메서드를 실행   
메인 스레드는 main 메서드의 코드를 처음부터 끝까지 순차적으로 실행시키며,   
코드의 끝을 만나거나 return문을 만나면 실행을 종료   

![](/assets/img/bootcamp/single_thread_process.png)

<br>

### 멀티 스레드 (Multi-Thread)
- 여러 스레드가 동시에 작업을 수행할 수 있음을 의미 = 멀티 스레딩
- 하나의 애플리케이션 내에서 여러 작업을 동시에 수행하는 멀티 태스킹을 구현하는 데에 핵심적인 역할을 수행

ex)   
일반적인 메신저 애플리케이션은 멀티 스레드로 동작하며, 메세지를 주고 받으며 동시에 파일을 업로드할 수 있음   
만약, 메신저 애플리케이션이 싱글 스레드로 이루어져 있다면, 파일을 업로드하는 동안에는 메세지를 주고 받을 수 없을 것   

![](/assets/img/bootcamp/multi_thread_process.png)

<br>

### 스레드의 생성과 실행

#### 작업 스레드 생성과 실행
- 메인 스레드 외에 별도의 작업 스레드를 활용한다는 것 = 작업 스레드가 수행할 코드를 작성하고, 작업 스레드를 생성하여 실행시키는 것을 의미
- 스레드가 수행할 코드도 클래스 내부에 작성해주어야 하며, run()이라는 메서드 내에 스레드가 처리할 작업을 작성하도록 규정되어져 있음
- run() 메서드는 Runnable 인터페이스와 Thread 클래스에 정의

#### 방법
1. Runnable 인터페이스를 구현한 객체에서 run()을 구현하여 스레드를 생성하고 실행하는 방법
2. Thread 클래스를 상속 받은 하위 클래스에서 run()을 구현하여 스레드를 생성하고 실행하는 방법

##### 1.
ex)

```java
// Runnable 인터페이스를 구현한 객체에서 run()을 구현하여 스레드를 생성하고 실행하는 방법
public class MultiThreading_1 {
  public static void main(String[] args) {
    // Runnable 인터페이스를 구현한 TreadTask 클래스 인스턴스화
//        Runnable runnable = new ThreadTask1();

    // Runnable 구현 객체를 인자로 전달하면서 Thread 클래스를 인스턴스화하여 스레드 생성
//        Thread thread = new Thread(runnable);

    // 위 압축 ~> Thread 클래스 인스턴스화의 인자로 Runnable 인터페이스를 구현한 ThreadTask1 인스턴스화 전달
    // 결국 의미 자체는 구현한 ThreadTask1 에 있기 때문에 가능한 것
    Thread thread = new Thread(new ThreadTask1());
    thread.start(); // 시작 메소드

    for (int i = 0; i < 100; i++) {
      System.out.print("@");
    }// 메인메소드에 추가
  }
}
class ThreadTask1 implements Runnable { // Runnable 인터페이스 구현
  @Override
  public void run() { // 인터페이스 메소드 오버라이드(구현)
    for (int i = 0; i < 100; i++)
      System.out.print("#");
  }
}
// 출력 결과 =
// 매 출력마다 결과가 달라짐 + @와 #이 섞여서 나오는 경우도 있음
// 이유 =
// 작업 스레드인 Thread 와 메인 스레드인 for~ 구문이 동시에 작동(병렬 구동)하기 때문임
// 이게 멀티 스레딩
```

##### 2.
ex)

```java
public class MultiThreading_2 {
  public static void main(String[] args) {
    // 인터페이스인 runnable 을 다룰 때처럼 Thread 클래스를 직접 인스턴스화 안함
    // 상속이니까 뭐 어차피 하위 객체인 ThreadTask2 가 Thread 를 참조하고 있는거는 뻔해서
    // 왜냐고? 자바는 다중 상속이 불가능하거든요
    // 인터페이스는 왜 그러냐고? 다중 구현이 가능하기 때문에 참조 주소 확실히 하는 거임
    ThreadTask2 task2 = new ThreadTask2();
    task2.start(); // 소환!!!!!!!!!

    for (int i = 0; i < 100; i++) {
      System.out.print("*");
    } // 메인 스레드 실행
  }
}
class ThreadTask2 extends Thread {
  @Override
  public void run() { // 메소드 오버라이드
    for (int i = 0; i < 100; i++) {
      System.out.print("!");
    }
  }
}
// 결과는 구현했던 전 단계와 마찬가지로 섞여서 나오기도하고 아니기도하고
// 병렬 구동 가능
```

<br>

### Ex. 익명 객체를 사용하여 스레드 생성 실행
ex)

```java
// 익명 객체를 사용하여 스레드 생성하고 실행하기
public class MultiThreading_3 {
    public static void main(String[] args) {
        // 일회용 익명 객체 구현 / runnable 구현 객체 / 인터페이스
        Thread thread1 = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 30; i++) {
                    System.out.print("1");
                }
            }
        });
        Thread thread2 = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 30; i++) {
                    System.out.print("3");
                }
            }
        });
        Thread thread3 = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 30; i++) {
                    System.out.print("4");
                }
            }
        });
        Thread thread4 = new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 30; i++) {
                    System.out.print("5");
                }
            }
        });
        // Thread 익명 하위 객체 활용 / Thread 바로 호출
        Thread thread = new Thread() {
            public void run() {
                for (int i = 0; i < 30; i++) {
                    System.out.print("!");
                }
            }
        };

        // 익명 구현 객체 람다식 ~> 익명 하위 객체 문법도 같음
        Thread thread5 = new Thread(() -> {
            for (int i = 0; i < 30; i++) {
                System.out.print("*");
            }
        });

        thread1.start();
        thread2.start();
        thread4.start();
        thread3.start();
        thread.start();
        thread5.start();

            for (int i = 0; i < 30; i++) {
                System.out.print("2");
            }
    }
}
```

<br>

### 스레드의 이름
- 메인스레드는 “main”이라는 이름을 가지며, 그 외에 추가적으로 생성한 스레드는 기본적으로 “Thread-n”이라는 이름을 가짐

### 스레드의 이름 조회하기
- 스레드의_참조값.getName()으로 조회
- 코드가 작성된 순서로 번호 매겨짐
- 스레드 클래스로부터 인스턴스화 된 인스턴스 메소드

ex)

```java
System.out.println(thread1.getName());
```

<br>

### 스레드의 이름 설정하기
- 스레드의_참조값.setName()
- 출력때만 바뀜 -> 작성된 코드의 이름이 바뀌진 않음
- 스레드 클래스로부터 인스턴스화 된 인스턴스 메소드

ex)

```java
thread2.setName("hi");
System.out.println(thread2.getName());
```

<br>

### 스레드 인스턴스의 주소값 얻기
- currentThread()
- Thread 클래스의 정적 메서드 ~> 직접 클래스 주소 찍어줘야한다는 것

ex)

```java
public class ThreadExample1 {
public static void main(String[] args) {

        Thread thread1 = new Thread(new Runnable() {
            public void run() {
                System.out.println(Thread.currentThread().getName());
            }
        });

        thread1.start();
        System.out.println(Thread.currentThread().getName());
    }
}
// currentThread() 가 정적 메소드라 각각 위치마다 출력 값이 달라짐
```

<br>

### 스레드의 동기화
- 멀티 스레드 프로세스의 경우, 두 스레드가 동일한 데이터를 공유하게 되어 문제가 발생할 수 있음

오류 ex)

```java
public class MultiTreading_4 {
public static void main(String[] args) {
    Runnable runnable = new TreadTask();
    Thread thread1 = new Thread(runnable);
    Thread thread2 = new Thread(runnable);

        thread1.setName("김씨");
        thread2.setName("박씨");

        thread1.start();
        thread2.start();
    }
}
class Accout {
private int balance = 1000; // 잔액

    public int getBalance() {
        return balance;
    }

    //인출 성공 실패 boolean 으로 반환
    public boolean withDraw(int money) {
        //인출 가능 여부 판단
        if (balance>=money) {
            //if 문에 진입하자마자 해당 스레드 정지 후
            // 다른 스레드로 제어권 강제 변경
            try {Thread.sleep(1000);} catch (Exception e) {}

            // 잔액에서 인출금을 깎아 새로운 잔액 기록
            balance -= money;

            return true;
        }
        return false;
    }
}
class TreadTask implements Runnable {
    Accout accout = new Accout();

    @Override
    public void run() {
        while (accout.getBalance() > 0) {

            //100~300 원 사이로 인출금 랜점 지정
            int money = (int) (Math.random() * 3 + 1) * 100;

            //withDraw 를 실행시키는 동시에 인출 성공 여부 변수에 할당
            boolean denied = !accout.withDraw(money);

            // 인출 결과 확인
            // withDraw 가 false 리턴하면 DENIED 출력
            System.out.println(String.format("WithDraw %d원 by %s. Balance : %d %s",
                    money, Thread.currentThread().getName(), accout.getBalance(), denied ? "-> DENIED" : ""));

        }
    }
}
// 잔액 출력 값이 이상하게 나옴
// 두 스레드 간에 객체가 공유되기 때문에 발생하는 오류
// 조건문 무시하고 음수 잔액 한번 씩 발생
// 음수는 ~> Account 객체를 공유하는 상황에서 한 스레드가 if 조건문을 true로 받는 동안
// 또 다른 스레드가 끼어든 것 << 얘는 false 인거지 원래
// DENIED 도 나왔다가 안나왔다가 함
```

<br>

### 임계 영역(Critical section)과 락(Lock)   
임계 영역 = 오로지 하나의 스레드만 코드를 실행할 수 있는 코드 영역을 의미   
락 = 임계 영역을 포함하고 있는 객체에 접근할 수 있는 권한을 의미   
~> 임계 영역은 멀티 스레드들이 무조건 하나씩 들어가서 작업을 처리할 수 있는 영역인거고   
락은 거기에 들어갈 수 있는 접근 권한을 뜻함   
즉, 임계영역에 A 스레드가 들어가면 B 스레드는 멈춰 있고 A 스레드가 끝나고 락 반납 후 임계영역을 빠져나오게 되면 그제서야 B 스레드가 해당 임계영역에 대한 락을 얻고 진입해 프로세스가 진행되는 것   

쉽게 말해 임계 영역 = 입장권이 필요한 한 명만 들어갈 수 있는 장소   
락 = 그 입장권   

### 방법
1. 메서드 전체를 임계 영역으로 지정하기
2. 특정한 영역을 임계 영역으로 지정하기

##### 1.
- 메서드의 반환 타입 좌측에 synchronized 키워드

ex)

```java
public synchronized boolean withDraw(int money) { // << 임계 영역 설정
        //인출 가능 여부 판단
        if (balance>=money) {
            //if 문에 진입하자마자 해당 스레드 정지 후
            // 다른 스레드로 제어권 강제 변경
            try {Thread.sleep(1000);} catch (Exception e) {}

            // 잔액에서 인출금을 깎아 새로운 잔액 기록
            balance -= money;

            return true;
        }
        return false;
    } // 임계 영역으로 지정해야함 ~> synchronized
```

##### 2.
- synchronized 키워드와 함께 소괄호(()) 안에 해당 영역이 포함된 객체의 참조를 넣고, 중괄호({})로 블럭을 열어, 블럭 내에 코드를 작성

ex)

```java
public boolean withDraw(int money) {
        synchronized (this) { // << 특정 영역을 임계 영역 설정 / 여기부터
            //인출 가능 여부 판단
            if (balance >= money) {
                //if 문에 진입하자마자 해당 스레드 정지 후
                // 다른 스레드로 제어권 강제 변경
                try {
                    Thread.sleep(1000);
                } catch (Exception e) {
                }

                // 잔액에서 인출금을 깎아 새로운 잔액 기록
                balance -= money;

                return true;
            }
            return false;
        } // 여기 까지 임계 영역
    }

```

<br>

### 스레드의 상태와 실행 제어
- 지금까지 start() 메소드로 스레스 실행 시켰는데, 엄밀히 말하면 start()는 스레드를 실행시키는 메서드는 아님   
~> start()는 스레드의 상태를 실행 대기 상태로 만들어주는 메서드, 어떤 스레드가 start()에 의해 실행 대기 상태가 되면 운영체제가 적절한 때에 스레드를 실행   
- 스레드에는 상태라는 것이 존재
- 스레드의 상태를 바꿀 수 있는 메서드가 존재

![](/assets/img/bootcamp/thread_behav_and_method.png)

<br>

### 스레드 실행 제어 메서드

##### 1. sleep(long milliSecond) : milliSecond 동안 스레드를 잠시 멈춤   
   ~> static void sleep(long milliSecond)   
   - sleep()은 스레드의 실행을 잠시 멈출 때 사용
   - 항상 지정한 시간 만큼 정확히 스레드가 중지되는 것은 아니며 약간의 오차 존재
   - sleep()은 Thread의 클래스 메서드 == sleep()을 호출할 때에는 Thread.sleep(1000);과 같이 클래스를 통해서 호출하는 것이 권장
   - sleep()을 호출하는 코드를 실행한 스레드의 상태가 실행 상태에서 일시 정지(TIMED_WAITING) 상태로 전환
   - sleep()에 의해 일시 정지된 스레드는 다음의 경우에 실행 대기 상태로 복귀
      + 인자로 전달한 시간 만큼의 시간이 경과한 경우
      + interrupt()를 호출한 경우 ~> 반드시 try … catch 문을 사용해서 예외 처리(interrupt()가 호출되면 기본적으로 예외가 발생하기 때문)

ex)

```java
try { Thread.sleep(1000); } catch (Exception e) {}
```

<br>

##### 2. interrupt() : sleep(), wait(), join()에 의해 일시 정지 상태에 있는 스레드들을 실행 대기 상태로 복귀시킴   
ex) void interrupt()   
   -> 작동 원리 : 기존에 호출되어 스레드를 멈추게 했던 sleep(), wait(), join() 메서드에서 예외가 발생되며, 그에 따라 일시 정지가 풀리게 됨.   
   즉, interrupt() 메소드는 일부러 예외를 발생시켜 일시정지를 푸는 메소드   

ex)

```java
public class MultiThreading_5 {
  public static void main(String[] args) {
    Thread thread1 = new Thread(() -> {
      try {
        while (true) Thread.sleep(1000);
      } catch (Exception e) {}
      System.out.println("Wake up!");
    });
    System.out.println("thread1.getState() = " + thread1.getState());
    thread1.start(); // 대기 상태
    System.out.println("thread1.getState() = " + thread1.getState());

    while (true) {
      if (thread1.getState() == Thread.State.TIMED_WAITING) {
        System.out.println("thread1.getState() = " + thread1.getState());
        break;
      }
    }
    thread1.interrupt(); // 스레드 강제 실행
    while (true) {
      if (thread1.getState() == Thread.State.RUNNABLE) {
        System.out.println("thread1.getState() = " + thread1.getState());
        break;
      }
    }
    while (true) {
      if (thread1.getState() == Thread.State.TERMINATED) {
        System.out.println("thread1.getState() = " + thread1.getState());
        break;
      }
    }
  }
}
```

<br>

##### 3. yield() : 다른 스레드에게 실행을 양보   
- 다른 스레드에게 자신의 실행 시간을 양보   
- 예를 들어, 운영 체제의 스케줄러에 의해 3초를 할당 받은 스레드 A가 1초 동안 작업을 수행하다가 yield()를 호출하면 남은 실행 시간 2초는 다음 스레드에게 양보

-> 스레드를 활용할 때, 스레드에게 반복적인 작업을 실행시키는 경우가 많음   
그런데 특정 경우에 반복문의 순회가 불필요할 때가 있음   

ex)

```java
public void run() {
  while (true) {
    if (example) {
      ...
    }
  }
}

여기서 example 이 false 여도 while 은 돌아감
이럴 떄 쓰는게 yield() 메소드
ex)
public void run() {
  while (true) {
    if (example) {
						...
    }
    else Thread.yield();
  }
}
```

-> 이런식으로 yield() 메소드를 두면 example의 값이 false 일때 실행 대기 상태로 변경되며   
남은 실행 시간을 대기열 중 높은 우선순위를 가진 다음 스레드에게 양보함   


##### 4. join() : 다른 스레드의 작업이 끝날 때까지 기다림
- 특정 스레드가 작업하는 동안에 자신을 일시 중지 상태로 만드는 상태 제어 메서드
- 인자로 시간을 밀리초 단위로 전달할 수 있으며, 전달한 인자만큼의 시간이 경과하거나, interrupt()가 호출되거나, join() 호출 시 지정했던 다른 스레드가 모든 작업을 마치면 다시 실행 대기 상태로 복귀
- sleep() 과 유사한 메소드 : join()을 호출한 스레드는 일시 중지 상태
- try … catch문으로 감싸서 사용
- interrupt()에 의해 실행 대기 상태로 복귀
- 단, sleep 은 Thread 클래스의 정적 메소드인 반면 join은 특정 스레드에 대해 동작하는 인스턴스 메소드임

ex)

```java
void join()
void join(long milliSecond)
```

ex)

```java
public class MultiThreading_6 {
  public static void main(String[] args) {
    SumThread sumThread = new SumThread();
    sumThread.setTo(10);
    sumThread.start();


    // 메인 스레드가 sumThread의 작업이 끝날 때 까지 기다림
    try {sumThread.join();} catch (Exception e){}
    // 아 이거 끄고 실행 시키니까
    // 당장에 메인 메소드가 먼저 실행되서
    // sumThread 의 run() 메소드 안에서 이뤄지는 작업이 안끝나고 바로 출력되버려서
    // 합이 0 으로 나옴 ㅋㅋ

    System.out.println(String.format("1부터 %d까지의 합 = %d"
      , sumThread.getTo(),sumThread.getSum()));
  }
}
class SumThread extends Thread {
  private long sum;
  private int to;

  public long getSum() {
    return sum;
  }

  public int getTo() {
    return to;
  }

  public void setTo(int to) {
    this.to = to;
  }

  @Override
  public void run() {
    for (int i = 1; i <= to; i++){
      sum += i;
    }
  }
}
```

<br>

#### 5. wait(), notify() : 스레드 간 협업에 사용
- 두 스레드가 교대로 작업을 처리해야할 때 사용

ex)

```java
//스레드A가 공유 객체에 자신의 작업을 완료합니다.
//이 때, 스레드B와 교대하기 위해 notify()를 호출합니다.
//notify()가 호출되면 스레드B가 실행 대기 상태가 되며, 곧 실행됩니다.
//이어서 스레드A는 wait()을 호출하며 자기 자신을 일시 정지 상태로 만듭니다.
//
//이후 스레드B가 작업을 완료하면 notify()를 호출하여 작업을 중단하고 있던 스레드A를 다시 실행 대기 상태로 복귀시킨 후,
//wait()을 호출하여 자기 자신의 상태를 일시 정지 상태로 전환합니다.
//
//이와 같은 과정이 반복되면서,
//두 스레드는 공유 객체에 대해 서로 배타적으로 접근하면서도 효과적으로 협업할 수 있습니다.

public class MultiThreading_7 {
  public static void main(String[] args) {
    WorkObject sharedObject = new WorkObject();
    ThreadA threadA = new ThreadA(sharedObject);
    ThreadB threadB = new ThreadB(sharedObject);

    threadA.start();
    threadB.start();

  }
}
class WorkObject { // 메소드 동기화는 하나씩만 실행되게하려고 검
  public synchronized void methodA() { // 1. 먼저 인자로 받은 스레드 실행 / 9. 교대된 스레드 실행 ~ 반복
    System.out.println("ThreadA의 methodA 작업 중"); // 2. 실행
    notify(); // 3. 교대
    try {wait();} catch (Exception e) {} // 4. 자기는 일시정지 상태로
  }
  public synchronized void methodB() { // 5. 교체된 스레드 실행
    System.out.println("ThreadB의 methodB 작업 중"); // 6. 실행
    notify(); // 7. 교대
    try {wait();} catch (Exception e) {} // 8. 자신은 일시정지
  }
}
// 아래 객체를 실행하고서 끝나지 않는 이유는 위의 wait 메소드로 정시 시켰기 때문임
class ThreadA extends Thread {
  private WorkObject workObject; // 캡슐화

  public ThreadA(WorkObject workObject) {
    this.workObject = workObject;
  } // 인자 받아올 생성자

  @Override
  public void run() {
    for (int i = 0; i < 10; i++) { // 실행될 횟수
      workObject.methodA();
    }
  }
}

class ThreadB extends Thread {
  private WorkObject workObject;

  public ThreadB(WorkObject workObject) {
    this.workObject = workObject;
  }

  @Override
  public void run() {
    for (int i = 0; i < 10; i++) {
      workObject.methodB();
    }
  }
}

// 출력값
// ThreadA의 methodA 작업 중
// ThreadB의 methodB 작업 중
// ThreadA의 methodA 작업 중
// ThreadB의 methodB 작업 중
// ThreadA의 methodA 작업 중
// ThreadB의 methodB 작업 중
// ThreadA의 methodA 작업 중
// ThreadB의 methodB 작업 중
// ThreadA의 methodA 작업 중
// ThreadB의 methodB 작업 중
// ThreadA의 methodA 작업 중
// ThreadB의 methodB 작업 중
// ThreadA의 methodA 작업 중
```

<br>
<br>

### 자바 가상 머신(Java Virtual Machine)
- 자바 이전에는 C++이 프로그래밍 언어를 많이 사용.
- C언어를 기반으로 한 유명한 객체지향 프로그래밍 언어 C++에 큰 문제가 하나있음, 운영체제로부터 독립적이지 못하다는 점.
- Windows를 위해 만든 프로그램은 Windows에서만 작동이 가능했고,
- Mac OS에서 그 프로그램을 실행시키려면 Mac OS에 맞게 새로 프로그램을 만들고 컴파일해야됨
- 프로그래밍 언어가 운영체제에 종속적이기 때문에, 어떤 프로그램을 만들 때 각 운영체제 별로 따로 만들어주어야 했음
- 자바는 이러한 문제점을 해결하고자 탄생한 언어
- 자바로 소스 코드를 한 번만 작성하면 어떤 운영체제에서도 코드를 수정할 필요 없이 프로그램을 실행시킬 수 있음
- 이와 같은 운영체제로부터의 독립성은 JVM이라는 별도의 프로그램을 통해서 구현됨.


### JVM
- JVM(Java Virtual Machine)은 자바 프로그램을 실행시키는 도구   
~> 즉, JVM은 자바로 작성한 소스 코드를 해석해 실행하는 별도의 프로그램

### 자바가 운영체제로부터 독립적인 이유
1. 프로그램이 실행되기 위해서는 CPU, 메모리, 각종 입출력 장치 등과 같은 컴퓨터 자원을 프로그램이 할당받아야 함
2. 프로그램이 자신이 필요한 컴퓨터 자원을 운영체제에게 주문하면, 운영체제는 가용한 자원을 확인한 다음, 프로그램이 실행되는 데에 필요한 컴퓨터 자원을 프로그램에게 할당
3. 2프로그램이 운영체제에게 필요한 컴퓨터 자원을 요청하는 방식이 운영체제마다 다름
4. 3번이 다른 프로그래밍 언어가 운영체제에 대해 종속성을 가지게 되는 이유
5. 자바는 3번, 4번과 달리 JVM을 매개해서 운영체제와 소통, 즉, JVM이 자바 프로그램과 운영체제 사이에서 일종의 통역가 역할을 수행
6. JVM은 각 운영체제에 적합한 버전이 존재
7. JVM은 자바 소스 코드를 운영 체제에 맞게 변환해 실행

<br>

### JVM 구조

![](/assets/img/bootcamp/JVM_structure.png)

<br>

#### 흐름
1. 자바로 소스 코드를 작성하고 실행
2. 컴파일러가 실행되면서 컴파일이 진행
3. 컴파일의 결과로 .java 확장자를 가졌던 자바 소스 코드가 .class 확장자를 가진 바이트 코드 파일로 변환
4. JVM은 운영 체제로부터 소스 코드 실행에 필요한 메모리를 할당 받음 ~> 런타임 데이터 영역(Rumtime Data Area)
5. 클래스 로더(Class Loader)가 바이트 코드 파일을 JVM 내부로 불러들여 런타임 데이터 영역에 적재시킴 ~> 자바 소스 코드를 메모리에 로드시키는 것
6. 실행 엔진(Execution Engine)이 런타임 데이터 영역에 적재된 바이트 코드를 실행시킴
7. 실행 엔진은 두 가지 방식으로 바이트 코드를 실행시킴
   - 인터프리터(Interpreter)를 통해 코드를 한 줄씩 기계어로 번역하고 실행시키기
   - JIT Compiler(Just-In-Time Compiler)를 통해 바이트 코드 전체를 기계어로 번역하고 실행시키기   
   - 기본적으로 a의 방법을 통해 바이트 코드를 실행시키다가, 특정 바이트 코드가 자주 실행되면 해당 바이트 코드를 JIT Compiler를 통해 실행시킴

<br>

### Stack과 Heap

![](/assets/img/bootcamp/JVM_memory_structure.png)

- JVM에 Java 프로그램이 로드되어 실행될 때 특정 값 및 바이트코드, 객체, 변수등과 같은 데이터들이 메모리에 저장되어야 함
- 런타임 데이터 영역이 바로 이러한 정보를 담는 메모리 영역 ~> 크게 5가지 영역으로 구분

<br>

### Stack 영역
- 스택은 일종의 자료구조 ~> 자료구조는 프로그램이 데이터를 저장하는 방식을 의미
- 흔히 LIFO라는 키워드로 설명 ~> “Last In First Out”
- LIFO 자료 구조 == 스택

ex) 프링글스 ~> 만들 때 제일 마지막으로 들어간 과자를 먹을 때 가장 먼저 꺼낼 수 있음

<br>

### JVM안에서 Stack 작동
1. 메서드가 호출되면 그 메서드를 위한 공간인 Method Frame이 생성
2. 메서드 내부에서 사용하는 다양한 값들, 참조변수, 매개변수, 지역변수, 리턴값 및 연산시 일어나는 값들이 임시로 저장
3. Method Frame이 Stack에 호출되는 순서대로 쌓이게 되는데, Method의 동작이 완료되면 역순으로 제거

![](/assets/img/bootcamp/stack_structure.png)

<br>
<br>

### Heap 영역
- JVM에는 단 하나의 Heap 영역이 존재
- JVM이 작동되면 이 영역은 자동 생성
- 객체나 인스턴스 변수, 배열 저장
- Heap 영역은 실제 객체의 값이 저장되는 공간

ex)

```java
Person person = new Person();
```

1. new Person()이 실행되면 Heap 영역에 인스턴스가 생성됨
2. 인스턴스가 생성된 위치의 주소값을 person에게 할당 ~> person은 Stack 영역에 선언된 변수   
즉, 우리가 객체를 다룬다는 것은 Stack 영역에 저장되어 있는 참조 변수를 통해 Heap 영역에 존재하는 객체를 다룬다는 의미   
~> Person 클래스의 전체 객체가 힙 메모리에 저장되고 인스턴스화 하면 Person 타입의 person 변수가 임시로 스택에 저장되고 힙 메모리에 있는 Person 클래스의 전체 객체를 new 연산자로 복제해서 불러온다는 것   
그럼 person 변수는 힙 메모리에 들어가 있는 Person을 복제해서 불러온다는 것이니까 주소를 참조하고 있는 것   
이 주소에서 불러 온 객체가 스택 메모리에 임시 저장됨 << 이걸 복제되었다고 생각하는 것   
이후 실행되는 것   
생성 시점을 따지면 Person 클래스의 객체가 힙 메모리에 저장되는 것이 먼저고   
인스턴스화 해서 해당 객체를 실행 할 때 스택 메모리에 생성하는 것이 나중임   
이후 실행되고 나면 삭제됨   

![](/assets/img/bootcamp/heap_,memory.png)

<br>
<br>

### Garbage Collection
- 자바의 메모리 자동 관리 프로세스
- 프로그램에서 더 이상 사용하지 않는 객체를 찾아 삭제하거나 제거하여 메모리를 확보하는 것

ex)

```java
public class Garbage_Collection {
  public static void main(String[] args) {
    Person person = new Person();
    person.setName("김씨");
    person = null;
    // person 변수가 null 값으로 없는 걸로 바꾸었기 때문에
    // 해당 person 변수는 더 이상 Person 인스턴스와 연결되어있지 않음
    // 즉 쓰지 않는 변수라는 것
    // 이게 가비지 발생
    // 자바서는 가비지가 발생함과 동시에 메모리에서 삭제 해줌
    person = new Person();
    person.setName("박씨");

    System.out.println(person.getName());
  }
}
class Person {
  private String name;

  public void setName(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }
}
```

<br>

### 동작 방식
- JVM의 Heap 영역은 객체는 대부분 일회성이며, 메모리에 남아 있는 기간이 대부분 짧다는 전제로 설계
- 객체가 얼마나 살아있냐에 따라서 Heap 영역 안에서도 영역을 나누게 되는데 Young, Old영역 2가지로 나뉨

![](/assets/img/bootcamp/garbage_collector.png)

<br>

#### Young 영역
- 새롭게 생성된 객체가 할당되는 곳
- 많은 객체가 생성되었다 사라지는 것을 반복
- 이 영역에서 활동하는 가비지 컬렉터를 Minor GC라 함

#### Old 영역
- Young영역에서 상태를 유지하고 살아남은 객체들이 복사되는 곳
- Young 영역보다 크게 할당되고 크기가 큰 만큼 가비지는 적게 발생
- 이 영역에서 활동하는 가비지 컬렉터를 Major GC라 함


Young 영역과 Old 영역은 서로 다른 메모리 구조로 되어 있기 때문에,
세부적인 동작 방식은 다르지만 기본적으로 가비지 컬렉션이 실행될때는 다음의 2가지 단계를 따름

1. Stop The World
   - 가비지 컬렉션을 실행시키기 위해 JVM이 애플리케이션의 실행을 멈추는 작업
   - 가비지 컬렉션이 실행될때 가비지 컬렉션을 실행하는 스레드를 제외한 모든 스레드들의 작업은 중단되고, 가비지 정리가 완료되면 재개

2. Mark and Sweep
   - Mark는 사용되는 메모리와 사용하지 않는 메모리를 식별하는 작업을 의미
   - Sweep은 Mark단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업을 의미

~> Stop The World를 통해 모든 작업이 중단되면, 가비지 컬렉션이 모든 변수와 객체를 탐색해서 각각 어떤 객체를 참고하고 있는지 확인   
이후, 사용되고 있는 메모리를 식별해서(Mark) 사용되지 않는 메모리는 제거(Sweep)하는 과정을 진행

<br>
<br>
<br>

***

[이전 블로그](https://blog.naver.com/021skyfall/222980225640)에서 옮겨짐
