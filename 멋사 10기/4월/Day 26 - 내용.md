# 멀티스레드

먼저 컴퓨터 프로그램의 실행과 관련된 개념인 프로세스와 멀티스레드가 뭔지 알아보자.

## 프로세스

`프로세스`는 실행중인 프로그램의 인스턴스이다.

`운영체제`는 각 프로세스에 메모리와 시스템 리소스를 할당한다.

프로세스는 독립적인 메모리 공간을 가지며, 다른 프로세스와는 격리되어 있고, 격리함으로써 프로세스 간의 안정성을 보장하지만, 리소스 관리 측면에서 비효율적일 수 있으며, 프로세스 간 통신이 복잡할 수 있다.

<img width="731" alt="스크린샷 2024-04-18 10 12 22" src="https://github.com/hamsangjin/TIL/assets/103736614/2cb0e878-0c24-423e-816b-6c01e37e98c8">

<br>
<br>

## 멀티스레드

`멀티스레드`는 하나의 프로세스 내에서 여러 개의 스레드를 동시에 실행하는 기법이다.

`스레드`는 프로세스 내에서 실제로 작업을 수행하는 최소 단위로, 프로세스의 리소스를 공유한다.

각 스레드는 독립적인 실행 흐름을 가지며, 자신만의 스택을 가지지만, 힙 메모리와 코드 영역은 공유한다.

<img width="734" alt="스크린샷 2024-04-18 10 13 32" src="https://github.com/hamsangjin/TIL/assets/103736614/a3572309-0640-44f3-86ef-45fa02fe1a0e">

<br>
<br>

## 프로세스와 멀티스레드의 차이점

- `메모리 공유`: 멀티스레드 환경에서는 여러 스레드가 같은 메모리 공간(힙)을 공유하는 반면, 각 프로세스는 완전히 독립적인 메모리 공간을 가진다.
- `통신`: 멀티스레드는 같은 프로세스의 메모리를 공유하기 때문에 데이터 공유가 쉽지만, 프로세스간 통신은 IPC라는 방법을 사용하기 때문에 어렵다.
- `오버헤드`: 멀티스레드는 프로세스보다 생성과 관리 측면에서 오버헤드가 적지만, 프로세스는 더 많은 시스템 리소스를 필요로 한다.
- `안정성`: 프로세스가 서로 독립적인 반면, 멀티스레드 환경에서 한 스레드의 오류가 전체 프로세스에 영향을 줄 수 있다.

<br>

## 멀티스레드 작성 문법

### 1. Thread 클래스를 상속받는 방법

`Thread` 클래스를 상속받아서 `run` 메소드를 오버라이드하면 된다.

```java
class MyThread extends Thread {
    public void run() {
        // 스레드가 실행할 작업
    }
}
```

- `장점`: 구현이 간단하고 직관적이며, `Thread` 클래스의 다른 메소드들을 직접 사용할 수 있다.
- `단점`: 자바는 다중 상속을 지원하지 않으므로, `Thread`를 상속받으면 다른 클래스를 상속 받을 수 없고, 스레드 관련 코드가 일반 클래스 코드와 섞여 복잡해질 수 있다.

### 2. Runnable 인터페이스를 구현하는 방법

`Runnable` 인터페이스를 `implements` 받아서, `run` 메소드를 정의하고, `Runnable` 객체를 `Thread` 객체에 전달해 **스레드를 생성**한다.

```java
class MyRunnable implements Runnable {
    public void run() {
        // 스레드가 실행할 작업
    }
}

Runnable runnable = new MyRunnable();
Thread thread = new Thread(runnable);
```

- `장점`: 클래스가 다른 클래스를 상속받을 수 있으므로 유연성이 높아지고, 스레드 코드와 비즈니스 로직 코드를 분리할 수 있어, 코드의 재사용성과 유지보수성이 향상됩니다.
- `단점`: `Runnable` 인터페이스에는 `Thread` 클래스의 다른 유용한 메소드들이 포함되어 있지 않아 명시적으로 `Thread` 객체를 생성해야한다.

일반적으로 `Runnable` 인터페이스를 구현하는 방법이 더 유연하고 권장된다.

<br>

## 멀티스레드 제어 필요성

### 1. 데이터 일관성과 안정성 유지
여러 스레드가 동일한 데이터에 접근하고 변경할 경우, 데이터의 일관성과 안전성이 손상될 수 있다.

따라서, 멀티스레드 환경에서는 데이터의 동시 접근을 제어하고 동기화하는 메커니즘이 필요하다.

### 2. 데드락 방지
<img width="504" alt="스크린샷 2024-04-18 13 26 03" src="https://github.com/hamsangjin/TIL/assets/103736614/5c1c73a2-4019-4cc6-8065-4bd4565ae69e">

`데드락`은 여러 스레드가 서로가 소유한 리소스의 해제를 기다리면서 무한히 대기하는 상태를 말한다. <br>
이러한 상황은 시스템의 작업 처리 능력을 저하시킬 뿐만 아니라, 프로그램의 정상적인 실행을 방해한다.

따라서, 스레드 간 리소스 할당과 해제를 적절히 관리해 데드락을 방지하는 것이 중요하다.

### 3. 자원 공유 최적화
<img width="492" alt="스크린샷 2024-04-18 13 27 24" src="https://github.com/hamsangjin/TIL/assets/103736614/5698156c-49fd-4bd2-85e9-b1c10932f3b8">

멀티 스레드 환경에서는 여러 스레드가 시스템 자원을 공유한다. <br>
스레드의 수가 많아질수록 자원 경쟁이 심화되어 성능 저하가 발생할 수 있다.

따라서, 자원을 효율적으로 공유하고 관리하는 전략이 필요하다.

### 4. 스레드 생명 주기 관리
<img width="570" alt="스크린샷 2024-04-18 13 29 08" src="https://github.com/hamsangjin/TIL/assets/103736614/9ec19a2b-2a92-485b-91b4-c6ff80075f87">

스레드의 생성, 실행, 대기, 종료 등 생명 주기를 적절히 관리하는 것도 중요하다.

예를 들어, 사용하지 않는 스레드는 자원을 낭비할 수 있으므로, 필요에 따라 스레드를 중지하거나 재활용하는 메커니즘이 필요하다.

### 5. 성능 최적화 및 부하 관리
멀티 스레드는 시스템의 멀티코어 프로세서를 효율적으로 활용할 수 있게 해주지만, 스레드를 너무 많이 생성하면, 컨텍스트 스위칭으로 인한 오버헤드가 증가할 수 있다.

따라서, 적절한 스레드 수와 스레드 풀 관리는 시스템의 성능을 최적화하는 데 중요하다.

<br>

## 멀티스레드 제어를 위한 자바 기능

### 1. 동기화(Synchronization)

`synchroniztion` 키워드를 사용하면 메소드나 블록을 동기화해 한 시점에 하나의 스레드만이 해당 코드 영역에 접근할 수 있도록 한다.

이를 통해 공유 자원에 대한 동시 접근을 방지하고, 데이터의 일관성과 안정성을 보장한다.

- 사용 방법
```java
public synchronized void synchronizedMethod() {
    // 동기화된 메소드 내용
}

public void synchronizedBlock() {
    synchronized(this) {
        // 동기화된 블록 내용
    }
}
```

- 예제 코드
<img width="703" alt="스크린샷 2024-04-18 13 32 46" src="https://github.com/hamsangjin/TIL/assets/103736614/cd75bf13-2d66-4e35-bf68-8c481c0c6f49">

```java
public class Robot {
    public synchronized void methodA(){
        for(int i = 1; i <= 5; i++){
            System.out.println("method A : " + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }

    public synchronized void methodB(){
        for(int i = 1; i <= 5; i++){
            System.out.println("method B :" + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }

    public void methodC(){
        for(int i = 1; i <= 5; i++){
            System.out.println("method C : " + i);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
public class RobotPlayer extends Thread{
    private String name;
    private Robot robot;

    public RobotPlayer(String name, Robot robot) {
        this.name = name;
        this.robot = robot;
    }

    public void run(){
        if(name.equals("A")) {
            robot.methodA();
        }else if(name.equals("B")) {
            robot.methodB();
        }else{
            robot.methodC();
        }
    }
}
```

```java
public class RobotMain {
    public static void main(String[] args){
        Robot robot = new Robot();
        RobotPlayer playerA = new RobotPlayer("A", robot);
        RobotPlayer playerB = new RobotPlayer("B", robot);
        RobotPlayer playerC = new RobotPlayer("C", robot);
        playerA.start();
        playerB.start();
        playerC.start();
    }
}
```

<br>

### 2. 스레드 생명 주기 제어

`wait()`과 `notify()/notifyAll()`은 `Object` 클래스에 정의된 메소드로, 스레드 간 통신을 제어하는데 사용된다.
- `wait()`: 스레드를 대기 상태로 만들기
- `notify()/notifyAll()`: 대기 중인 스레드를 깨우기
<br>

- `wait/notify` 예제 코드
```java
public class WaitNotifyExample {
    private static final Object lock = new Object();
    private static boolean itemAvailable = false;

    static class Producer extends Thread {
        public void run() {
            synchronized (lock) {
                System.out.println("생산자가 아이템을 생산 중입니다.");
                itemAvailable = true;
                lock.notify(); // 생산이 끝났으므로 소비자에게 알림
                System.out.println("생산자가 알림을 보냈습니다.");
            }
        }
    }

    static class Consumer extends Thread {
        public void run() {
            synchronized (lock) { // lock의 소유권을 가진다.
                while (!itemAvailable) {
                    try {
                        System.out.println("소비자가 아이템을 기다리고 있습니다.");
                        lock.wait(); // 아이템을 기다림, wait하면 lock의 소유권을 포기한다.
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            System.out.println("소비자가 아이템을 소비했습니다.");
            itemAvailable = false; // 아이템 소비 후 상태 업데이트
            }
        }
    }

    public static void main(String[] args) {
        Producer producer = new Producer();
        Consumer consumer = new Consumer();
        consumer.start(); // 소비자 스레드 시작

        try {
            Thread.sleep(1000); // 생산자 시작 전에 잠시 대기
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        producer.start(); // 생산자 스레드 시작
    }
}
```

- `wait/notifyAll` 예제 코드
```java
public class WaitNotifyAllExample {
    private static final Object lock = new Object();
    private static int itemsAvailable = 0; // 사용 가능한 아이템 수

    static class Producer extends Thread {
        public void run() {
            synchronized (lock) {
                itemsAvailable += 5; // 5개의 아이템을 생산
                System.out.println("생산자가 " + itemsAvailable + "개의 아이템을 생산하였습니다.");
                lock.notifyAll(); // 모든 대기 중인 소비자 스레드에 알림 System.out.println("생산자가 모든 소비자에게 알림을 보냈습니다.");
            }
        }
    }

    static class Consumer extends Thread {
        private int id;

        Consumer(int id) {
            this.id = id;
        }

        public void run() {
            synchronized (lock) {
                while (itemsAvailable <= 0) {
                    try {
                        System.out.println("소비자 " + id + "가 아이템을 기다리고 있습니다.");
                        lock.wait(); // 아이템을 기다림
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                itemsAvailable--; // 아이템 소비
                System.out.println("소비자 " + id + "가 아이템을 소비했습니다. 남은 아이템: " + itemsAvailable);
            }
        }
    }

    public static void main(String[] args) {
        Producer producer = new Producer();
        Consumer consumer1 = new Consumer(1);
        Consumer consumer2 = new Consumer(2);
        Consumer consumer3 = new Consumer(3);

        consumer1.start(); // 소비자 1 스레드 시작
        consumer2.start(); // 소비자 2 스레드 시작
        consumer3.start(); // 소비자 3 스레드 시작

        try {
            Thread.sleep(1000); // 생산자 시작 전에 잠시 대기
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        producer.start(); // 생산자 스레드 시작
    }
}
```

<br>

### 3. 스레드 상태 확인 및 제어

- `interrupt()`: 스레드를 중단시키는 요청을 하며, 스레드에 인터럽트 플래그를 설정한다.
- `isInterrupted()`, `interrupted()`: 스레드가 인터럽트 되었는지 여부를 확인한다.

<br>

- 예제 코드
```java
public class ThreadInterruptExample {
    static class MyThread extends Thread {
        public void run() {
            try {
                for (int i = 0; i < 5; i++) {
                    Thread.sleep(1000); // 1초 동안 일시 정지
                    System.out.println("Processing step " + (i + 1));
                }
            } catch (InterruptedException e) {
                System.out.println("스레드가 중단되었습니다.");
                return; // 스레드를 안전하게 종료
            }
        }
    }
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start(); // 스레드 시작
        try{
            Thread.sleep(2500); //메인스레드를2.5초동안일시정지
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        t.interrupt(); // 스레드에 인터럽트 신호 보내기
    }
}
```

<br>

### 4. Thread 클래스의 기타 메소드
- `join()`: 한 스레드가 다른 스레드의 종료를 기다리게 한다.
  - `t1.join()`은 `t1`이 종료될 때까지 현재 스레드의 실행을 일시정지한다.
- `setPriority()`, `getPriority()`: 스레드의 우선 순위를 설정하거나 가져온다.

<br>

- 예제 코드
```java
class DemonThread extends Thread{
    @Override
    public void run() {
        while(true){
            System.out.println("재생");
            try {
                sleep(500);
            } catch (InterruptedException e) {
                throw new RuntimeException();
            }
        }
    }
}

public class ThreadJoinExample {
    static class TaskThread extends Thread {
        private String taskName;

        public TaskThread(String taskName) {
            this.taskName = taskName;
        }

        public void run() {
            System.out.println(taskName + " 작업 시작");
            try {
                Thread.sleep(2000); // 2초 동안 스레드 일시 정지 (작업 시뮬레이션)
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(taskName + " 작업 완료");
        }
    }
    public static void main(String[] args) {
        TaskThread task1 = new TaskThread("작업 1");
        TaskThread task2 = new TaskThread("작업 2");
        DemonThread demonThread = new DemonThread();


        task1.start();
        task2.start();

        // 데몬 쓰레드 설정
        // 이 메인 메소드가 종료되면 같이 종료됨
        demonThread.setDaemon(true);
        demonThread.start();

        try {
            System.out.println("모든 작업의 완료를 기다립니다.");
            task1.join(); // task1의 완료를 기다림
            task2.join(); // task2의 완료를 기다림
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("모든 작업이 완료되었습니다.");
    }
}
```
