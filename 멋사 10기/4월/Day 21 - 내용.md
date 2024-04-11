# final

## final 클래스
`final 클래스`는 다른 클래스가 상속받을 수 없게 하는 특별한 종류의 클래스다.

### final 클래스의 필요성
1. `불변성 보장`: 클래스가 생성된 후 내용이 변경되지 않도록 하여, 안정성과 보안을 향상시킨다.
2. `상속 방지`: final 클래스는 상속받을 수 없으며, 특정 클래스의 설계와 구현이 그대로 유지되어야 할 때 중요하다.
3. `불변 클래스 생성`: 불변성을 보장하므로, 여러 면에서 프로그램의 신뢰성을 높일 수 있다.
4. `성능 최적화`: final 클래스의 메소드는 오버라이딩되지 않기 떄문에, 컴파일 타임에 메소드 호출이 결정될 수 있어, 실행시간이 단축될 수 있다.

<br>

### final 클래스의 특성
1. `확장 불가능`: 다른 클래스는 final 클래스를 상속받을 수 없으며, 이는 모든 메소드가 상속되거나 오버라이딩될 수 없음을 의미한다.
2. `메소드 오버라이딩 방지`: final 클래스 내의 모든 메소드는 자동으로 final로 간주하기 때문에, 메소드들은 하위클래스에서 오버라이딩 될 수 없다.

> final 클래스는 안정성과 유지보수성을 높일 수 있고, 특정한 사용 사례에서 중요한 역할을 한다. <br>
> 클래스의 설계 의도를 명확히 하고, 불필요한 상속과 변경을 방지하여, 프로그램의 견고성을 높이고자 할 때 final 클래스를 사용하는 것이 좋다.

<br>

### Java JDK에서 final 클래스
1. `java.lang.String`: JDK에서 가장 널리 알려진 final 클래스 중 하나로, 문자열을 표현하는데 사용되는 이 클래스는 불변성을 유지해야 하므로, 한번 생성된 String 객체의 내용은 변경될 수 없다.
2. `java.lang.System`: 시스템 관련 기능을 제공하며, 주로 시스템의 입력, 출력 및 오류 출력을 관리하는 정적 메소드들을 포함한다.
3. `java.util.Collections`: Collections 클래스 내부에는 final로 선언된 여러 내부 클래스들이 존재하며, 이 클래스들은 불변의 컬렉션을 생성하는데 사용된다.

<br>
<br>

## final 필드
`final 필드`는 한 번 초기화되면 그 값을 변경할 수 없는 필드를 의미하며, 상수를 정의하거나, 객체의 불변성을 보장하는 데 사용된다.

### final 필드의 필요성
1. `상수 정의`: final 필드는 변경되지 않는 고정된 값을 상수를 정의하는데 사용한다.
2. `불변 객체 생성`: final 필드를 사용하면 객체의 핵심 속성이 한 번 설정된 후에는 변경되지 않도록 할 수 있으며, 이는 객체의 예측 가능성과 신뢰성을 높인다.
3. `스레드 안정성`: 멀티 스레딩 환경에서 final 필드는 동기화없이 스레드간의 안전한 읽기 작업을 보장한다.
4. `메모리 가시성 보장`: final 필드는 객체가 생성될 때 한 번만 쓰여지고, 이후에는 변경되지 않기 때문에 JVM은 메모리 가시성을 보장한다.

<br>

### final 필드의 특성
1. `초기화 제한`: final 필드는 선언 시 또는 생성자에서만 초기화할 수 있다.
2. `객체의 핵심 속성 보호`: 객체가 특정한 상태를 유지해야할 때 즉, 설정값이나 중요한 데이터를 final 필드로 선언함으로써 해당 값의 무결성을 유지할 수 있다.

> final 필드는 프로그램의 안정성과 무결성을 높이는 중요한 방법이다.<br>
> 이는 프로그램이 더욱 견고하고 안정적으로 동작하도록 돕고, 예측 가능한 동작을 보장하는데 기여한다.

<br>

### Java JDK에서 final 필드 사례
1. `java.lang.Math 클래스의 상수`: Math 클래스는 여러 수학적 상수를 final 필드로 정의하여 전역적으로 일관된 값을 제공해준다.
2. `java.lang.System 클래스의 입출력 필드`: System 클래스는 여러 final 필드를 포함하는데 이들은 프로그램 실행 중에 이 필드들의 참조가 변경되지 않음을 보장한다.
3. `java.util.Collections의 불변 컬렉션`: Collections 클래스는 불변 컬렉션을 생성하기 위한 여러 메소드를 제공하며, 이러한 불변 컬렉션 내부에서 final 필드가 사용된다.

<br>
<br>

## final 메소드
`final 메소드`는 자바에서 특별한 유형의 메소드로, 한 번 선언되면 하위 클래스에서 오버라이딩할 수 없고, 이는 클래스의 핵심적인 기능을 안정적으로 유지하기 위해 사용된다.

### final 메소드의 필요성
1. `메소드의 무결성 보장`: final 메소드는 변경되거나 재정의될 수 없으며, 이는 트겆ㅇ 메소드의 기능이 그대로 유지되어야 할 때 중요하다.
2. `오버라이딩 방지`: final 메소드는 하위 클래스에서 오버라이딩될 수 없으며 이는 클래스의 동작을 일관되게 유지하는 데 도움이 된다.
3. `보안 강화`: 보안이 중요한 메소드를 final로 선언함으로써, 외부에서 해당 메소드를 변경하거나 악용하는 것을 방지할 수 있다.
4. `예측 가능한 동작`: final 메소드는 프로그램의 동작을 예측 가능하게 만든다.

<br>

### final 메소드의 특성
1. `재정의 불가능`: final 메소드는 상속받은 클래스에서 다시 정의할 수 없으며, 이는 메소드의 원래 기능과 동작이 보존됨을 의미한다.
2. `클래스의 동작 보호`: 특정 클래스의 핵심적인 동작이나 로직이 final 메소드로 정의되면, 해당 내용이 변경되지 않는다.

> final 메소드는 객체 지향 프로그래밍에서 클래스의 기능을 보호하고, 안정적인 동작을 보장하는 중요한 도구이다.<br>
> 이는 프로그램의 견고성을 높이고, 예상치 못한 동작으로부터 시스템을 보호하는데 중요한 역할을 한다.

<br>

### Java JDK에서 final 메소드 사례
1. `java.lang.Object 클래스의 getClass 메소드`: getClass 메소드는 객체가 속한 클래스의 Class 객체를 반환한다.
2. `java.lang.System 클래스의 입출력 필드`: length 메소드는 문자열의 길이를 반환한다.
3. `java.util.Collections의 불변 컬렉션`: unmodifiableList메소드는 수정할 수 없는 리스트를 반환한다.

<br>

---

<br>

# 예외 처리
`예외처리`란 프로그램 실행 중에 발생할 수 있는 예상치 못한 상황에 대비하여 프로그램의 정상적인 흐름을 유지하고 예외 상황을 안전하게 처리하는 프로그래밍 기법

<br>

## 기본 구조
자바에서 예외처리는 주로 `try-catch-finally` 블록을 사용하여 구현된다.
- `try 블록`: 예외가 발생할 수 있는 코드를 포함한다.
- `catch 블록`: `try블록`에서 발생한 예외를 처리한다.
- `finally 블록(선택적)`: 예외 발생 여부와 관계없이 실행되는 코드를 포함한다.

```java
try{
    예외 발생할 가능성이 있는 코드들 ..
} catch(발생할 예외클래스 예외변수){
    try 블럭에서 예외가 발생한다면 실행할 코드 ...
} finally{
    예외 발생 여부와 상관없이 실행시킬 코드 ...
}
```

<br>
<br>

## RuntimeException
`RuntimeException`이란 자바의 예외처리 체계에서 중요한 부분을 차지하는 클래스로, 주로 자바 런타임 시스템에서 발생하는 예외를 나타낸다.<br>
`java.lang` 패키지에 속하며, `Exception` 클래스의 서브 클래스다.

### RuntimeException 특징
- `비검사형 예외`: RuntimException과 그 서브클래스는 비검사형 예외로 분류되며, 이는 컴파일 시점에서 체크되지 않으며, 주로 프로그래머의 실수로 인해 발생한다.
- `자동으로 전파`: 메소드 시그니처에 예외를 명시적으로 선언하지 않아도 자동으로 호출 스택을 통해 전파된다.

### RuntimeException 사용 사례
1. `ArrayIndexOutOfBoundsException`: 배열의 인덱스를 잘못 접근하는 경우
2. `NullPointerException`: null 참조에 대한 연산 시도
3. `ClassCastException`: 잘못된 형 변환 시도
4. `ArithmeticException`: 수학적으로 불가능한 연산 시도(예: 0으로 나누기)

<br>
<br>

## CheckedException
`CheckedException`은 자바 프로그래밍에서 컴파일 시점에 체크되는 예외 유형으로, 주로 프로그램의 외부 요인에 의해 발생할 수 있는 예외 상황을 나타내며, Exception의 서브클래스이다.

### CheckedException 특징
- `컴파일 시점에서 체크`: CheckedException은 컴파일러에 의해 체크되며 이 예외를 처리하지 않으면 컴파일 오류가 난다.
- `예측 가능한 예외`: CheckedException은 프로그램의 외부 요인, 예를 들어 파일 시스템 오류, 네트워크 문제 등에 의해 발생하는 것이 일반적이다.

### CheckedException 사용 사례
1. `IOException`: 파일 입출력 작업 시 발생 
2. `SQLException`: 데이터베이스 연결 시 발생 
3. `FileNotFoundException`: 존재하지 않는 파일에 접근할 때 발생

<br>
<br>

## 예외 떠넘기기
`예외 떠넘기기`는 메소드가 발생시킨 예외를 처리하지 않고, 그 메소드를 호출한 상위 메소드에 예외 처리를 위임하는 방법이다. <br>
방법은 `throws` 키워드를 메소드 선언에 사용하면 된다.

### 예외 떠넘기기 사례
1. `java.io.InputStream의 read 메소드`: InputStream 클래스의 read메소드는 데이터를 읽을 때 발생할 수 있는 IOException을 떠넘김으로써, 메소드 사용자에게 예외 처리의 책임을 전가한다.
```java
public abstract int read() throws IOException;
```
2. `java.net.Socket의 생성자`: Socket의 생성자는 호스트 주소와 포트를 사용하여 소켓 연결을 시도하는 과정에서, UnknownHostException과 IOException와 같은 여러 네트워크 관련 예외가 발생할 수 있다.
```java
public Socket(String host, int port) throws UnknownHostException, IOException;
```
<br>
<br>

## 예외 발생시키기
`예외 발생시키기`는 특정 조건에서 프로그램의 흐름을 인위적으로 중단하고, 해당 상황을 호출자에게 알리는 데 필요합니다.

### 예외 발생시키기 예제
```java
public class NumberRangeChecker {
    public void checkNumber(int number) {
        // 숫자가 1과 100 사이에 있어야 한다고 가정
        if (number < 1 || number > 100) {
            throw new IllegalArgumentException("숫자는 1과 100 사이에 있어야 합니다: " + number);
        }
        System.out.println("입력한 숫자는 규칙에 맞습니다: " + number);
    }
    public static void main(String[] args) {
        NumberRangeChecker checker = new NumberRangeChecker();
        try {
            checker.checkNumber(150); // 범위를 벗어난 숫자
        } catch (IllegalArgumentException e) {
            System.err.println("오류 발생: " + e.getMessage());
        }
    }
}
```
- checkNumber 메소드가 입력된 숫자가 1~100 범위에 있는지 검사하고, 조건을 만족하지 않을 경우 오류 메시지를 포함해 `IllegalArgumentException`을 발생시킨다.
- main메소드에서 범위가 벗어난 숫자를 가지고 `checkNumber`를 호출해서 예외를 발생시키고, 그 예외를 `catch문`에서 처리했다.

<br>
<br>

## 사용자 정의 예외, 예외 발생, 예외 떠넘기기
`사용자 정의 예외`는 특정 애플리케이션의 비즈니스 규칙과 로직에 맞춰 설계된 예외이며, 자바가 제공하는 표준 예외들로 특정 상황을 명확하게 표현하기 어려울 때 사용한다.

### 사용자 정의 예외 예제

#### 사용자 정의 예외 클래스 생성
```java
class MyCustomException extends Exception {
    public MyCustomException(String message) {
        super(message);
    }
}
```

#### 예외 발생 클래스
```java
public class MyResource {
    public void processResource(String value) throws MyCustomException {
        if (value == null) {
            throw new MyCustomException("Resource value cannot be null");
        }
        // 리소스 처리 로직
        System.out.println("Processing resource with value: " + value);
    }
}
```

#### 예외 떠넘기는 클래스
```java
public class ResourceHandler {
    public void handleResource() {
        MyResource resource = new MyResource();
        try {
            resource.processResource(null);
        } catch (MyCustomException e) {
            System.err.println("Error occurred: " + e.getMessage());
            // 여기서 추가적인 예외 처리 로직을 수행할 수 있음
        }
    }
}
```

#### 실행 클래스
```java
public class Main {
    public static void main(String[] args) {
        ResourceHandler handler = new ResourceHandler();
        handler.handleResource();
    }
}
```

- `MyCustomException`이라는 사용자 정의 예외 클래스 생성
- 예외 발생 클래스인 `MyResource`에서 매개변수 값이 null인경우 `MyCustomException`예외를 발생시키는 `processResource` 메소드를 구현
- 예외 떠넘기는 클래스인 `ResourceHandler`의 `handleResource` 메소드에서 `try-catch`문을 통해 예외를 처리했다.

<br>
<br>

## 자동 리소스 닫기
`자동 리소스 닫기`는 java에서 리소스를 관리하는 효율적인 방법으로, 파일 입출력, 데이터베이스 연결, 네트워크 연결과 같이 시스템 리소스를 사용하는 객체들을 자동으로 닫아주는 기능을 제공해준다.

또한, 기존엔 리소스를 사용한 후에 `finally 블록`에 또 `try-catch`문을 통해 리소스를 닫아줬었지만 이러한 방법은 코드가 복잡해지고, 리소스를 닫는 것을 잊는 실수가 할 수 있어 문제가 발생할 수 있었다.

### 자동 리소스 닫기 예제
`try-with-resources` 구문을 사용해준다.
```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;
public class FileReadExample {
    public static void main(String[] args) {
        // 파일 경로 지정
        String filePath = "example.txt";
    
        // try-with-resources 구문을 사용하여 자동으로 리소스를 닫음
        try (FileReader fileReader = new FileReader(filePath);
              BufferedReader bufferedReader = new BufferedReader(fileReader)) {
            String line;
            while ((line = bufferedReader.readLine()) != null) {
                System.out.println(line); // 파일의 각 줄을 출력
            }
        }catch (IOException e) {
            System.err.println("파일 읽기 중 오류 발생: " + e.getMessage());
        }
    }
}
```
