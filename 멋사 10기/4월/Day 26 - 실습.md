# 실습 문제: 동시 카운터

## 목표
- Java의 `Thread`와 `Runnable` 인터페이스를 사용하여 멀티스레딩을 구현합니다.
- 두 개의 스레드를 사용하여 동시에 숫자를 세고, 각 스레드의 작업을 콘솔에 출력합니다.

<br>

## 요구사항
1. **스레드 구현**
    - `IncrementCounter` 클래스를 구현하여, 1부터 100까지 숫자를 증가시키면서 각 숫자를 콘솔에 출력합니다.
    - `DecrementCounter` 클래스를 구현하여, 100부터 1까지 숫자를 감소시키면서 각 숫자를 콘솔에 출력합니다.
    - 각 클래스는 `Runnable` 인터페이스를 구현해야 합니다.

2. **스레드 실행**
    - 메인 메소드에서 `IncrementCounter`와 `DecrementCounter`를 각각의 스레드로 실행합니다.
    - 각 스레드는 시작되자마자 자신의 카운트 작업을 시작해야 합니다.

3. **출력**
    - 각 스레드의 출력은 다음 형식을 따라야 합니다: `Increment: {number}` 또는 `Decrement: {number}`.
    - 스레드가 숫자를 출력할 때마다 약간의 시간 지연(`sleep`)을 둡니다. 이는 `Thread.sleep()`을 사용하여 구현하며, 지연 시간은 0~10밀리초 사이의 무작위 시간입니다.

4. **동기화 및 경합 조건 처리**
    - 이 기본 실습에서는 각 스레드가 독립적으로 동작하므로, 동기화 메커니즘은 필요하지 않습니다.
    - 출력 순서에 대한 일관성은 보장되지 않으며, 실행할 때마다 결과가 달라질 수 있습니다.

<br>

## 예상되는 결과
- 프로그램을 실행할 때마다 "Increment" 스레드와 "Decrement" 스레드가 동시에 실행되며, 출력 순서는 실행할 때마다 달라집니다.
- 숫자가 순차적으로 증가하고 감소하는 것을 볼 수 있지만, 두 스레드의 출력이 서로 섞여 나타납니다.

<br>

## 작성할 클래스
- `IncrementCounter.java`: 숫자를 1에서 100까지 증가시키는 스레드 구현 파일.
- `DecrementCounter.java`: 숫자를 100에서 1까지 감소시키는 스레드 구현 파일.
- `CounterApp.java`: 메인 메소드를 포함하며, 두 스레드를 생성하고 실행하는 메인 클래스 파일.

```java
public class CounterApp {
    public static void main(String[] args) {
        Thread incrementThread = new Thread(new IncrementCounter());
        Thread decrementThread = new Thread(new DecrementCounter());

        incrementThread.start();
        decrementThread.start();
    }
}
```

<br>

## 풀이 코드
- `IncrementCounter`
```java
import static java.lang.Thread.sleep;

public class IncrementCounter implements Runnable{
    @Override
    public void run() {
        for(int i=1;i<=100;i++){
            try{
                sleep((int) (Math.random() * 10) + 1);
                System.out.println("Increment: {" + i + "}");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- `DecrementCounter`
```java
import static java.lang.Thread.sleep;

public class DecrementCounter implements Runnable{
    @Override
    public void run() {
        for(int i=100; i>=1; i--){
            try{
                sleep((int) (Math.random() * 10) + 1);
                System.out.println("Decrement: {" + i + "}");
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

- `CounterApp`
```java
public class CounterApp {
    public static void main(String[] args) {
        Thread it = new Thread(new IncrementCounter());
        Thread dt = new Thread(new DecrementCounter());

        it.start();
        dt.start();
    }
}
```


<br>

---

<br>

# 실습 문제: 동시 파일 읽기 및 쓰기

## 목표
- 자바의 `Thread`와 `Runnable` 인터페이스를 사용하여 멀티스레딩을 구현합니다.
- 하나의 스레드가 파일에서 데이터를 읽는 동안 다른 스레드가 다른 파일에 데이터를 쓰는 작업을 동시에 수행합니다.

<br>

## 요구사항
1. **스레드 구현**
    - `FileReaderTask` 클래스를 구현하여, 지정된 파일에서 텍스트 데이터를 읽고 콘솔에 출력합니다.
    - `FileWriterTask` 클래스를 구현하여, 사용자로부터 입력받은 데이터를 파일에 쓰는 작업을 수행합니다.
    - 각 클래스는 `Runnable` 인터페이스를 구현해야 합니다.

2. **스레드 실행**
    - 메인 메서드에서 `FileReaderTask`와 `FileWriterTask`를 각각의 스레드로 실행합니다.
    - 각 스레드는 시작되자마자 자신의 작업을 시작해야 합니다.

3. **파일 작업**
    - `FileReaderTask`는 `input.txt`라는 파일에서 데이터를 읽어서 콘솔에 출력합니다.
    - `FileWriterTask`는 콘솔에서 사용자의 입력을 받아 `output.txt`라는 파일에 계속해서 추가합니다.

4. **동기화 및 경합 조건 처리**
    - 이 실습에서는 각 스레드가 서로 다른 파일을 다루기 때문에 파일에 대한 경합 조건은 고려하지 않아도 됩니다.
    - 사용자 입력과 파일 출력 간의 동기화는 고려할 필요가 없습니다.

<br>

## 예상되는 결과
- 프로그램을 실행하면 하나의 스레드가 입력 파일에서 데이터를 읽어 콘솔에 출력하고, 동시에 다른 스레드가 사용자의 입력을 받아 출력 파일에 데이터를 쓰게 됩니다.
- 두 작업이 서로 간섭 없이 독립적으로 실행됩니다.

<br>

## 작성할 클래스
- `FileReaderTask.java`: 파일에서 데이터를 읽어 콘솔에 출력하는 스레드 구현 파일.
- `FileWriterTask.java`: 콘솔에서 데이터를 읽어 파일에 쓰는 스레드 구현 파일.
- `FileReadWriteApp.java`: 메인 메소드를 포함하며, 두 스레드를 생성하고 실행하는 메인 클래스 파일.

<br>

## 풀이 코드

- `FileReaderTask`
```java
import java.io.*;

public class FileReaderTask implements Runnable{
    private String path;

    public FileReaderTask(String path) {
        this.path = path;
    }

    @Override
    public void run(){
        try(BufferedReader br = new BufferedReader(new FileReader(new File(path)));){
            String line;
            while ((line = br.readLine()) != null) {
                System.out.println("input.txt : " + line);
            }

            System.out.println("input.txt 파일을 다 읽었습니다.");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- `FileWriterTask`
```java
import java.io.*;

public class FileWriterTask implements Runnable {
    private String path;

    public FileWriterTask(String path) {
        this.path = path;
    }

    @Override
    public void run(){
        try(
            BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            PrintWriter pw = new PrintWriter(path);){

            String line;
            System.out.print("입력: ");
            while((line = br.readLine()) != null){
                if(line.equals("q")) break;
                System.out.print("입력: ");
                pw.println(line);
            }

            System.out.println("입력하신 내용을 output.txt 파일에 저장했습니다.");

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

- `FileReadWriteApp`
```java
public class FileReadWriteApp {
    public static void main(String[] args) {
        Thread rt = new Thread(new FileReaderTask("src/com/example/day26/Example/input.txt"));
        Thread wt = new Thread(new FileWriterTask("src/com/example/day26/Example/output.txt"));

        rt.start();
        wt.start();
    }
}
```
