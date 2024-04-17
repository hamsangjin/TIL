# Byte 단위 입출력 Stream

## InputStream 클래스들
1. `FileInputStream`: 파일로부터 바이트 단위로 데이터를 읽는다.
2. `ByteArrayInputStream`: 바이트 배열로부터 데이터를 읽는다.
3. `BufferedInputStream`: 버퍼링을 사용하여 입력 효율을 높이고, 데이터를 일정량 모아두었다가 한 번에 처리해, 효율적으로 데이터를 읽는다.
4. `DataInputStream`: 기본 데이터 타입을 바이트 스트림으로부터 직접 읽는다.

<br>

## OutputStream 클래스들
1. `FileOutputStream`: 파일에 바이트 단위로 데이터를 쓴다.
2. `ByteArrayOutputStream`: 바이트 배열에 데이터를 쓴다.
3. `BufferedOutputStream`: 버퍼링을 사용하여 출력 효율을 높이고, 데이터를 일정량 모아두었다가 한 번에 처리해, 효율적으로 데이터를 쓴다.
4. `DataOutputStream`: 기본 데이터 타입을 바이트 스트림으로 쓰게 해준다.

<br>

## 예제 코드
```java
import java.io.*;

public class IOExam2{
    public static void main(String[] args) {
        // Closeable 인터페이스를 구현하고 있는 클래스들은 자동 리소스 닫기에 넣을 수 있음.
        try (FileInputStream in = new FileInputStream("src/com/example/day12/ReadFile.java");
             FileOutputStream out = new FileOutputStream("outputFile.txt");) {

            int c;
            while ((c = in.read()) != -1) {
                out.write(c);
            }
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

<br>

# char 단위 입출력 Stream

## Reader 클래스들
1. `FileReader`: 텍스트 파일로부터 데이터를 읽는다.
2. `StringReader`: 문자열로부터 데이터를 읽는다.
3. `BufferedReader`: 데이터를 버퍼에 저장하는 식으로 데이터를 일정량 모아두었다가 한 번에 데이터를 읽는다.
4. `InputStreamReader`: byte 스트림을 문자 스트림으로 변환하는데 사용된다.

<br>

## Writer 클래스들
1. `FileWriter`: 텍스트 파일에 데이터를 쓴다.
2. `StringWriter`: 문자열에 데이터를 쓴다.
3. `BufferedWriter`: 데이터를 버퍼에 저장하는 식으로 데이터를 일정량 모아두었다가 한 반에 데이터를 쓴다.
4. `OutputStreamReader`: 문자 스트림을 byte 스트림으로 변환하는데 사용된다.

<br>

## 예제 코드
```java
import java.io.*;

public class IOExam2{
    public static void main(String[] args) throws IOException {
        String path1 = "src/com/example/day25/a.txt";
        String path2 = "src/com/example/day25/b.txt";

        try(FileReader fr = new FileReader(path1); FileWriter fw = new FileWriter(path2);) {
            int c;

            while((c=fr.read())!=-1){
                fw.write(c);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
```

<br>

---

<br>

# System

## System.in
`System.in`은 표준 입력 스트림을 나타내며, 주로 콘솔로부터 사용자의 입력을 받는 데 사용한다.

이 스트림은 `InputStream` 형태로 제공되므로, 데이터를 읽기 위해서는 추가적인 처리가 필요하다.

<br>

## System.out
`System.out`은 표준 출력 스트림을 나타내며, 이 스트림을 통해 콘솔에 데이터를 출력할 수 있다.

주로 텍스트 메시지를 사용자에게 표시하는데 사용하며, `PrintStream` 클래스의 인스턴스다.

<br>

## System.err
`System.err`은 표준 오류 출력 스트림을 나타내며, 이 스트림은 오류 메시지나 경고를 출력하는 데 사용한다.

`System.out`과 같이 `PrintStream` 클래스의 인스턴스지만, 주로 오류 메시지를 다루는데 쓰인다.

<br>

## 예제 코드

- 사용자 입력을 파일에 쓰기
```java
import java.io.*;

public class SystemIOExample {
    public static void main(String[] args) {
        try(BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            PrintWriter fw = new PrintWriter("src/com/example/day25/b.txt");){
            String userInput = br.readLine();
            System.out.println("입력받은 문자열: " + userInput);
            fw.write(userInput);
        } catch (IOException e) {
            System.err.println("입력 오류가 발생했습니다: " + e.getMessage());
        }
    }
}
```

- 사이트 소스 불러와서 파일에 쓰기
```java
import java.io.*;
import java.net.*;

public class WWWRead {
    public static void main(String[] args){
        URL url = null;
        try {
            url = new URL("https://sangjin-mbti-test.netlify.app/");
        } catch (MalformedURLException e) {
            throw new RuntimeException(e);
        }

        BufferedReader reader;
        try (InputStream urlInput = url.openStream()) {
            reader = new BufferedReader(new InputStreamReader(urlInput));
            String msg;
            while ((msg = reader.readLine()) != null){
                System.out.println(msg);
            }
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

<br>

---

<br>

# Scanner
`Scanner` 클래스는 자바에서 사용자의 입력을 쉽게 읽을 수 있도록 해주는 유틸리티 클래스다.

주로 `System.in`으로 데이터를 읽는데 사용된다.

## 사용법
- `Scanner` 객체를 생성할 때 System.in`을 파라미터로 전달하여 초기화한다.
- `nextInt()`, `nextDouble()`, `nextLine()` 등의 메소드를 사용하여 각각 정수, 실수, 문자열 등을 입력받을 수 있다.
- 사용자가 입력을 완료하면 `close()` 메소드를 호출하여 `Scanner` 객체를 닫아야 한다.

<br>

## 예제 코드
```java
import java.util.Scanner;
public class ScannerExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        try {
            System.out.print("이름을 입력하세요: ");
            String name = scanner.nextLine();
            System.out.print("나이를 입력하세요: ");
            int age = scanner.nextInt();

            System.out.println("당신의 이름은 " + name + "이고, 나이는 " + age + "살입니다.");
        } finally {
            scanner.close();
        }
    }
}
```

<br>

---

<br>

# 보조 스트림

`기본 스트림`은 파일이나 네트워크 소켓과 같은 데이터 소스나 대상과 직접적으로 연결되지만, `보조 스트림`은 기본 스트림 위에 덧붙여져서 추가적인 기능을 수행한다. 

## 보조 스트림 종류

### Buffered Streams
- `BufferedReader`, `BufferedWriter`: 텍스트 데이터를 처리할 때 사용되며, 내부적으로 버퍼를 사용해 입출력 효율을 높인다.
- `BufferedInputStream`, `BufferedOutputStream`: 바이트 데이터를 처리할 때 사용되며, 버퍼링을 통해 데이터 읽기 및 쓰기의 성능을 향상시킨다.

### Data Streams
- `DataInputStream`, `DataOutputStream`: 바이너리 데이터를 처리할 때 사용되며, 기본 자료형의 데이터를 쉽게 읽고 쓸 수 있게 해준다.

### Object Streams
- `ObjectInputStream`, `ObjectOutputStream`: 객체의 직렬화와 역직렬화를 위해 사용되며, 이를 통해 객체를 바이트 형태로 변환하거나 바이트에서 객체로 복원할 수 있다.

### Print Streams
- `PrintWriter`, `PrintStream`: 형식화된 데이터를 출력할 때 사용되며, `System.out`이 `PrintStream`의 한 예다.

### Character Streams to Byte Streams
- `InputStreamReader`와 `OutputStreamWriter`: 바이트 스트림과 문자 스트림 간의 변환을 위해 사용되며, 이는 바이트 데이터와 문자 데이터 간의 변환을 가능하게 한다.

<br>

## 예제 코드
```java
import java.io.*;

public class DataTypesIOExample {
    public static void main(String[] args) {
        try (DataOutputStream dos = new DataOutputStream(new FileOutputStream("data.bin"));
             DataInputStream dis = new DataInputStream(new FileInputStream("data.bin"))) {

            // 데이터 쓰기
            dos.writeInt(123);
            dos.writeDouble(123.45);
            dos.writeBoolean(true);
            dos.writeUTF("Hello Java!");

            // 데이터 읽기
            int intData = dis.readInt();
            double doubleData = dis.readDouble();
            boolean booleanData = dis.readBoolean();
            String stringData = dis.readUTF();

            // 출력
            System.out.println("Int data: " + intData);
            System.out.println("Double data: " + doubleData);
            System.out.println("Boolean data: " + booleanData);
            System.out.println("String data: " + stringData);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
