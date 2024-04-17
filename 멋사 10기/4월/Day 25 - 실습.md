# 실습 문제: 전화번호부 관리 프로그램

## 목표
사용자로부터 여러 명의 이름과 전화번호를 입력받아, 이를 파일에 저장하고 저장된 파일의 내용을 읽어 콘솔에 출력하는 프로그램을 작성합니다.

<br>

## 요구사항
1. **사용자 입력 받기**: 사용자로부터 총 3명의 이름과 전화번호를 입력받습니다.
2. **파일에 저장**: 입력받은 이름과 전화번호를 "c:\\temp\\phone.txt" 파일에 저장합니다. 각 라인에는 한 명의 이름과 전화번호가 공백으로 구분되어 저장되어야 합니다.
3. **파일 읽기**: 저장된 파일을 다시 읽어 그 내용을 콘솔에 출력합니다.
4. **예외 처리**: 파일 입출력 과정에서 발생할 수 있는 예외를 적절히 처리합니다. 

<br>

## 상세 구현
- **PrintWriter**와 **FileOutputStream**을 사용하여 파일에 데이터를 쓸 수 있습니다.
- **BufferedReader**와 **InputStreamReader**를 사용하여 사용자로부터 데이터를 입력받을 수 있습니다.
- **BufferedReader**와 **FileReader**를 사용하여 파일의 데이터를 읽을 수 있습니다.
- 입력받는 데이터의 수가 3개로 한정되어 있으며, 파일 작업이 완료된 후에는 사용자에게 파일에 데이터가 저장되었다는 메시지와 함께 파일의 내용을 출력하는 메시지를 표시해야 합니다.
- 파일이 존재하지 않을 때의 예외 처리와 입출력 과정에서의 예외를 처리해야 합니다.

<br>

## 예상 실행 결과
```
이름: 홍길동
전화번호: 010-1234-5678
이름: 이순신
전화번호: 010-9876-5432
이름: 강감찬
전화번호: 010-1111-2222
... 전화번호가 c:\temp\phone.txt에 저장되었습니다.

c:\temp\phone.txt의 내용은 다음과 같습니다...
홍길동 010-1234-5678
이순신 010-9876-5432
강감찬 010-1111-2222
```

<br>

## 풀이 코드
```java
import java.io.*;

public class PhoneBookManagementProgram {
    public static void main(String[] args) {
        try(BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
            PrintWriter pw = new PrintWriter("src/com/example/day25/Example/phone.txt");) {
            for(int i = 0; i < 3; i++){
                System.out.print("이름 : ");
                String name = br.readLine();
                System.out.print("전화번호 : ");
                String number = br.readLine();

                pw.write(name + " " + number + "\n");
            }
            System.out.println("전화번호가 src/com/example/day25/Example/phone.txt에 저장되었습니다.");
        } catch (IOException e) {
            e.printStackTrace();
        }

        System.out.println();

        try(BufferedReader br = new BufferedReader(new FileReader("src/com/example/day25/Example/phone.txt"));) {
            System.out.println("src/com/example/day25/Example/phone.txt의 내용은 다음과 같습니다.");
            for (int i = 0; i < 3; i++) {
                System.out.println(br.readLine());
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

# 실습 예제: 다중 텍스트 관리 시스템

## 목표
- 사용자로부터 여러 줄의 텍스트를 입력받아 파일에 저장하고, 저장된 파일의 내용을 리스트에 저장한 후 출력합니다.
- 이 과정을 클래스와 메소드를 적절하게 사용하여 구현합니다.

<br>

## 설계
1. **`TextFileManager` 클래스** - 파일 쓰기와 읽기 작업을 담당합니다.
2. **`UserInputHandler` 클래스** - 사용자로부터의 입력을 처리합니다.
3. **`Application` 클래스** - 애플리케이션의 주 실행 로직을 관리합니다.

<br>

## 클래스 및 메소드 구현

### 1. TextFileManager 클래스
```java
public class TextFileManager {
    
}
```

### 2. UserInputHandler 클래스
```java
public class UserInputHandler {
    
}
```

### 3. Application 클래스
```java
import java.io.IOException;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        String filePath = "texts.txt";
        TextFileManager fileManager = new TextFileManager(filePath);
        UserInputHandler inputHandler = new UserInputHandler();

        try {
            List<String> userInput = inputHandler.getUserInput();
            fileManager.writeToFile(userInput);

            List<String> fileContent = fileManager.readFromFile();
            System.out.println("Content of the file:");
            fileContent.forEach(System.out::println);
        } catch (IOException e) {
            System.err.println("An error occurred: " + e.getMessage());
        }
    }
}
```

<br>

## 설명
- **`TextFileManager`**: 파일 I/O 작업을 캡슐화합니다. 파일 쓰기와 읽기 기능을 메소드로 제공합니다.
- **`UserInputHandler`**: 사용자 입력을 처리합니다. 사용자로부터 문자열을 입력받아 리스트로 반환하는 메소드를 포함합니다.
- **`Application`**: 메인 클래스로, 애플리케이션의 로직을 조정하며, 사용자 입력을 파일에 저장하고 파일에서 읽은 데이터를 출력합니다.

<br>

## 풀이 코드

### 1. TextFileManager 클래스
```java
import java.io.*;
import java.util.*;

public class TextFileManager {
    String path;

    public TextFileManager(String path) {
        this.path = path;
    }

    public void writeToFile(List<String> contents) throws IOException {

        BufferedWriter bw = new BufferedWriter(new FileWriter(path));

        for(String c : contents) {
            bw.write(c);
        }

        bw.close();
    }

    public List<String> readFromFile() throws IOException {
        List<String> list = new ArrayList<>();
        BufferedReader br = new BufferedReader(new FileReader(path));

        String line;
        while ((line = br.readLine()) != null) {
            list.add(line);
        }

        br.close();

        return list;
    }
}
```

### 2. UserInputHandler 클래스
```java
import java.io.*;
import java.util.*;

public class UserInputHandler {
    public List<String> getUserInput() throws IOException {
        List<String> list = new ArrayList<String>();

        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        for (int i = 0; i < 5; i++) {
            System.out.print("텍스트 입력: ");
            list.add(br.readLine() + "\n");
        }

        br.close();

        return list;
    }
}
```

### 3. Application 클래스
```java
import java.io.IOException;
import java.util.List;

public class Application {
    public static void main(String[] args) {
        String filePath = "src/com/example/day25/Example/texts.txt";
        TextFileManager fileManager = new TextFileManager(filePath);
        UserInputHandler inputHandler = new UserInputHandler();

        try {
            System.out.println(filePath + "에 저장할 내용을 5줄 적어주세요.");

            List<String> userInput = inputHandler.getUserInput();
            fileManager.writeToFile(userInput);

            System.out.println();

            List<String> fileContent = fileManager.readFromFile();
            System.out.println(filePath + "에 저장되어 있는 내용은 아래와 같습니다.");
            fileContent.forEach(System.out::println);
        } catch (IOException e) {
            System.err.println(e.getMessage());
        }
    }
}
```
