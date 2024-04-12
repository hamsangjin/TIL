# 내부 클래스
`내부 클래스`는 자바에서 클래스 내부에 또 다른 클래스를 정의하는 개념이며, 주로 외부 클래스와 관련된 작업을 수행하기 위해 사용된다.

## 멤버 내부 클래스
`멤버 내부 클래스`는 다른 클래스 내부에 위치하는 클래스로, 외부 클래스의 인스턴스와 연결되어 있으며, 보통 외부 클래스의 멤버 변수나 메소드가 위치하는 곳에서 정의한다.

```java
public class OuterClass {
    private int outerField = 10;

    public void outerMethod() {
        InnerClass innerClass = new InnerClass();
        innerClass.InnerMethod();
    }

    class InnerClass{
        public void InnerMethod(){
            // private한 멤버 변수도 직접 접근 가능
            System.out.println("outerField = " + outerField);
        }
    }

    public static void main(String[] args) {
        OuterClass outerClass = new OuterClass();
        outerClass.outerMethod();
    }
}
```

<br>

## 로컬 내부 클래스
`로컬 내부 클래스`는 메소드 내부에 정의되는 클래스이며, 주로 특정 메소드에서만 사용되는 기능을 구현하는데 사용된다.

```java
public class LocalOuterClass {
    private int outerField = 10;

    public void outerMethod() {
        class InnerClass{
            public void innerMethod(){
                System.out.println("outerField : " + outerField);
            }

        }
        InnerClass innerClass = new InnerClass();
        innerClass.innerMethod();
    }

    public static void main(String[] args) {
        LocalOuterClass localOuterClass = new LocalOuterClass();
        localOuterClass.outerMethod();
    }
}
```

<br>

## 정적 내부 클래스
`정적 내부 클래스`는 외부 클래스의 정적 멤버처럼 취급되는 내부 클래스로, 주로 외부 클래스와 관련된 공통적인 유틸리티나 헬퍼 클래스로 사용된다.

```java
public class Network {
    private static int totalMessages = 0;

    public static class Message {
        private String content;

        public Message(String content) {
            this.content = content;
            totalMessages++;
        }

        public void send() {
            System.out.println("메시지 전송: " + content);
            System.out.println("전체 메시지 수: " + totalMessages);
        }
    }

    public static int getTotalMessages() {
        return totalMessages;
    }

    public static void main(String[] args) {
        Network.Message msg1 = new Network.Message("안녕하세요!");
        Network.Message msg2 = new Network.Message("반갑습니다!");
        msg1.send();
        msg2.send();
        System.out.println("네트워크를 통해 전송된 전체 메시지 수: " + Network.getTotalMessages());
    }
}
```

<br>

---

<br>

# 내부 인터페이스
`내부 인터페이스`는 클래스나 다른 인터페이스 내부에 정의된 인터페이스를 의미하며, 주로 코드를 더욱 잘 조직화하고, 관련 기능들을 그룹화하기 위해 사용한다.

```java
public class OuterClass {
    // 외부 클래스의 멤버 변수와 메소드
    public interface InnerInterface {
        // 내부 인터페이스의 메소드 선언
        void performAction();
    }
}
```

```java
public interface OuterInterface {
    interface InnerInterface {
        // 내부 인터페이스의 메소드 선언
    }
}
```

## 예제
- 내부 인터페이스와 외부 클래스
```java
public class SmartPhone {
    public interface Camera {
        void takePhoto();
    }

    public class BasicCamera implements Camera {
        @Override
        public void takePhoto() {
            System.out.println("사진을 찍습니다.");
        }
    }

    public void activateCamera() {
        Camera camera = new BasicCamera();
        camera.takePhoto();
    }

    public static void main(String[] args) {
        SmartPhone smartphone = new SmartPhone();
        smartphone.activateCamera();
    }
}
```

<br>

---

<br>

# 익명 객체
`익명 객체`는 이름없이 선언되고 생성되는 객체로, 주로 일회성 작업에 사용된다.

```java
인터페이스명 객체명 = new 인터페이스명() {
    // 인터페이스의 메소드 구현
};
```
```java
추상클래스명 객체명 = new 추상클래스명() {
    // 추상 클래스의 메소드 구현
};
```

## 예제
```java
public class AnonymousObject {
    public interface Greeter {
        void greet();
    }

    public static void main(String[] args) {
        // 익명 객체를 사용하여 Greeter 인터페이스 구현 
        Greeter morningGreeter = new Greeter() {
            @Override
            public void greet() {
                System.out.println("좋은 아침입니다!");
            }
        };

        Greeter eveningGreeter = new Greeter() {
            @Override
            public void greet() { System.out.println("좋은 저녁입니다!");
            }
        };

        morningGreeter.greet(); // 출력: 좋은 아침입니다!
        eveningGreeter.greet(); // 출력: 좋은 저녁입니다!
    }
}
```

<br>

---

<br>

# java.lang의 주요 클래스

## 1. Object
`Object` 클래스는 자바에서 모든 클래스의 조상이며, 모든 자바 객체가 기본적으로 상속받는 클래스고, 이 메소드들은 자바에서 객체를 다룰 때 기본적인 동작을 의미한다.

### 새로 알게된 메소드
- `clone()`: 객체의 복제본을 생성해준다.

<br>

## 2. String
`String` 클래스는 문자열을 표현하고 관리하는 데 사용되며, 자바에서 `String 객체`는 `불변 객체`다.

그리고, 문자열의 연결, 비교, 추출과 같은 다양한 문자열 처리 기능을 제공해준다.

### 새로 알게된 메소드
- `equalsIgnoreCase()`: 대소문자를 구분하지 않고 동일한지 확인해준다.
- `indexOf()`, `lastIndexOf()`: 문자열 내부에서 특정 문자나 문자열의 위치를 찾을 수 있다.

<br>

## 3. StringBuilder, StringBuffer
`StringBuilder`와 `StringBuffer` 클래스는 변경 가능한 문자열을 처리하며, 두 클래스 모두 문자열을 추가, 삭제, 수정하는 등의 기능을 제공해준다.

`StringBuilder`는 동기화되지 않아 성능이 빠르며, 단일스레드 환경에서 적합하다.<br>
`StringBuffer`는 멀티스레드 환경에서 안전하도록 동기화되어 있다.

### 새로 알게된 메소드
- `append()`: 문자열 끝에 주어진 데이터를 추가한다.
- `insert()`: 문자열 특정 위치에 주어진 데이터를 추가한다.
- `delete()`: 문자열의 특정 위치부터 특정 위치까지의 데이터를 삭제한다.
- `deleteCharAt()`: 문자열의 특정 위치의 데이터를 삭제한다.

<br>

## 4. Math
`Math` 클래스는 기본적인 수학적 연산과 관련된 메소드를 제공하는 유틸리티 클래스다.

이 클래스는 최대값, 최소값, 제곱근, 지수 등 수학적 계산에 필요한 정적 메소드들을 포함하고 있다.

<br>

## 5. System
`System` 클래스는 시스템 관련 기능을 제공하며, 표준 입출력, 오류 스트림에 접근하거나, 환경 변수 읽기, 현재 시간 측정, 시스템 프로퍼티 조회 등의 기능을 포함한다.

### 새로 알게된 메소드
- `System.getenV()`: 환경 변수의 값을 얻을 수 있다.
- `System.currentTimeMillis(), System.nanoTime()`: 현재 시간 밀리초 또는 나노초 단위로 측정할 수 있다.
- `System.exit()`: 애플리케이션 종료가 가능하다.

<br>

## 6. Throwable
`Throwable`: 자바에서 모든 오류와 예외의 최상위 클래스이며, 자바에서 예외 처리 메커니즘의 기반을 이룬다.

### 새로 알게된 메소드
- `printStackTrace()`: 예외가 발생한 호출 스택의 추적 정보를 출력한다.
- `getCause()`: 예외의 원인을 반환한다.
- `initCause()`: 예외의 원일을 설정한다.


<br>

---

<br>

# java.util의 주요 클래스

## Date
`Date` 클래스는 날짜와 시간을 나타내는데 사용되며, 날짜와 시간을 쉽게 조작하고 표시할 수 있는 여러 메소드를 제공한다.
> Java 8 이후에는 `java.time` 패키지의 새로운 날짜 및 시간 API가 권장된다.

### 새로 알게된 메소드
- `after(Date date)`, `before(Date date)`: 특정 `Date` 객체가 다른 `Date` 객체보다 이전이나 이후인지 비교한다.
- `getTime()`, `setTime(long time)`: `Date` 객체가 나타내는 시간을 밀리초 단위로 반환하거나 설정한다.

<br>

## Calendar
`Calendar` 클래스는 자바에서 날짜와 시간을 관리하는데 사용되는 추상 클래스이면서, 직접 인스턴스화할 수 없으며, `GregorianCalendar` 와 같은 구현 클래스를 사용한다.

### 새로 알게된 메소드
- `getInstance()` : 현재 시스템의 시간대의 Calendar 객체를 반환한다.
- `set(int year, int month, int date)`: 년, 월, 일을 설정한다.
- `get(int field)`: 지정된 필드의 값을 반환한다.
- `add(int field, int amount)`: 지정된 필드의 값에 주어진 값을 더한다.
- `roll(int field, boolean up)`: 지정된 필드의 값을 증가시키거나 감소시킨다.

<br>

# java.time의 주요 클래스

## LocalDate
`LocalDate` 클래스는 날짜를 나타내며, 연, 월, 일 정보를 포함하며, 시간대 정보는 포함하지 않는다.

<br>

## LocalTime
`LocalTime` 클래스는 시간을 나타내며, 시, 분, 초, 나노초 정보를 포함하며, 날짜 정보는 포함하지 않는다.

<br>

## LocalDateTime
`LocalDateTime`는 날짜와 시간을 모두 표현하며, `LocalDate`와 `LocalTime` 클래스의 조합으로, 시간대 정보 없이 날짜와 시간을 나타낸다.

<br>

### LocalDate/Time, LocalDateTime의 주요 메소드
- `now()`: 현재 날짜나 시간을 반환한다.
- `of()`: 지정된 날짜나 시간으로 객체를 생성한다.
- `plusDays()`, `minusHours()` 등: 날짜나 시간을 더하거나 뺀다.
- `format(DateTimeFormatter)`: 지정된 포맷터를 사용하여 날짜/시간을 문자열로 변환한다.

<br>

## ZonedDateTime
`ZonedDateTime` 클래스는 시간대를 고려한 날짜와 시간을 나타내며, 전세계의 다양한 시간대에 따른 정확한 시간을 표현할 수 있다. 특히 국제적인 애플리케이션에서 중요하다.

### ZonedDateTime의 주요 메소드
- `of(LocalDateTime, ZoneId)`: 주어진 `LocalDateTime`과 시간대를 사용하여 `ZonedDateTime` 객체를 생성한다.
- `withZoneSameInstant(ZoneId)` : 다른 시간대로 시간을 변환한다.

<br>

## Duration
`Duration` 클래스는 두 시간 사이의 지속 기간을 나타내며, 주로 `LocalTime`이나 `LocalDateTime` 객체 간의 차이를 초 단위로 표현할 때 사용된다.

## Period
`Period` 클래스는 두 날짜 사이의 기간을 나타내며, 주로 `LocalDate` 객체 간의 차이를 연, 월, 일 단위로 표현할 때 사용된다.

### Duration, Period의 주요 메소드
- `Duration.ofSeconds()`, `Duration.ofMinutes()` 등: 특정 단위의 시간 길이를 나타내는 `Duration` 객체를 생성한다.
- `Period.ofDays()`, `Period.ofMonths()` 등: 특정 단위의 날짜 길이를 나타내는 `Period` 객체를 생성한다.
- `between()`: 두 날짜나 시간 사이의 간격을 `Duration`이나 `Period` 객체로 반환한다.
