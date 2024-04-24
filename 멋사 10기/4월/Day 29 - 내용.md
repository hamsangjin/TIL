# 람다식
`람다식`은 간단히 말해 익명 함수의 일종이며, 익명함수는 이름이 없는 함수를 정의하는 방법으로, 빠르고 간결한 함수 작성을 가능하게 해준다.

<br>

## 람다식의 특징
- `익명성`: 람다식은 이름이 없기 때문에 익명으로 사용되며, 이는 코드의 간결성을 높이고, 재사용성을 감소시킨다.
- `함수형 인터페이스와의 연결`: 람다식은 함수형 인터페이스의 구현체로 사용되며, 함수형 인터페이스는 오직 하나의 추상 메소드를 가진 인터페이스로, 람다식을 통해 간결하게 표현할 수 있다.
- `간결성과 표현력`: 람다식은 코드를 간결하게 만들고, 명확한 표현을 가능하게 하며, 이를 통해 개발자는 보다 직관적이고 간단한 코드 작성에 집중할 수 있다.

<br>

## 사용 방법

### 기본 구조
```java
(매개변수 목록) -> { 표현식; }
```
여기서 중요한 것은 `매개변수 목록`, `화살표 기호(->)`, `표현식`이다.


### 매개변수 목록
- 매개변수가 없을 경우: `() -> { 동작 }`
- 한 개의 매개변수만 있는 경우: `(a) -> { 동작 }` or `a -> { 동작 }`
- 두 개 이상의 매개변수가 있는 경우: `(a, b) -> { 동작 }`


### 표현식
- 단일 표현식: `(a, b) -> a + b`
- 복잡한 동작 또는 여러 줄의 코드: `(a, b) -> { 동작 }`


### 반환 값
- 반환 값이 있는 경우: `(a, b) -> a + b` or `(a, b) -> { return a + b }`
- 반환 값이 없는 경우: `(a, b) -> { System.out.println(a+b) }`

<br>

## 람다식 예제 코드
- 예제코드 1
```java
import java.util.*;
import java.util.function.Consumer;

public class LambdaExam1 {
    public static void main(String[] args) {
        List<String> items = Arrays.asList("Apple", "Banana", "Cherry");

        // 람다식을 이용하여 리스트의 각 요소 출력
        items.forEach(item -> System.out.println(item));

        // 익명 객체를 이용해 리스트의 각 요소 출력
        Consumer<String> lambdaTest = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };

        items.forEach(lambdaTest);

        // 익명 객체를 이용해 스레드 생성 및 실행
        new Thread(new Runnable(){
             public void run(){
                 for (int i = 0; i < 5; i++) {
                     System.out.println("Thread: " + i);
                 }
             }
        }).start();

        // 람다식을 이용하여 스레드 생성 및 실행
        new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("Thread: " + i);
            }
        }).start();
    }
}
```

- 예제코드 2
```java
import java.util.*;

public class LambdaExam2 {
    public static void main(String[] args) {
        // 리스트 정의
        List<Integer> numbers = Arrays.asList(5, 2, 3, 1, 4);

        // 익명 객체를 사용해 정렬 기준 정의(오름차순)
        Collections.sort(numbers, new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {
                return a.compareTo(b);
            }
        });

        // 정렬된 리스트 출력
        numbers.forEach(n -> System.out.println(n));

        System.out.println();

        // 람다식을 사용해 정렬 기준 정의(내림차순)
        Collections.sort(numbers, (Integer a, Integer b) -> b.compareTo(a));

        // 정렬된 리스트 출력
        numbers.forEach(n -> System.out.println(n));
    }
}
```

<br>

## 사용자 정의 함수적 인터페이스
- `함수적 인터페이스`: 자바에서 함수적 인터페이스는 오직 하나의 추상 메소드를 가진 인터페이스를 말한다.
- `사용자 정의 함수적 인터페이스`: 자바 표준 API에 포함되지 않은, 사용자가 필요에 따라 만드는 함수적 인터페이스다.
- `@FunctionalInterface 애노테이션`: 인터페이스가 함수적 인터페이스임을 명시적으로 표시할 수 있으며, 인터페이스에 두 개 이상의 추상 메서드가 있는지 검사한다.

<br>

### 함수적 인터페이스 예제 코드
- 예제 코드 1
```java
public interface MyFunctionalInterface {
    public void method1();
    public void method2();
}
```

```java
public class MyFunctionalInterfaceTest {
    public static void main(String[] args) {

        // 1. 직접 구현해 구현 객체를 이용해 메소드 호출
        MyFunctionalIntegerImpl myFunctionalIntegerImpl = new MyFunctionalIntegerImpl();
        myFunctionalIntegerImpl.method1();
        myFunctionalIntegerImpl.method2();

        // 2. 인터페이스를 익명객체로 구현해서 메소드 호출
        MyFunctionalInterface myFunctionalInterface1;
        myFunctionalInterface1 = new MyFunctionalInterface(){
            @Override
            public void method1() {
                System.out.println("method1 호출");
            }

            @Override
            public void method2() {
                System.out.println("method2 호출");
            }
        };
        myFunctionalInterface1.method1();
        myFunctionalInterface1.method2();

        // 3. 람다를 이용해 메소드 호출
        // -> 추상 메소드가 2개 이상이라 오류 발생
        // -> 따라서 인터페이스의 method2를 지우고 호출하면 가능 !
        MyFunctionalInterface myFunctionalInterface2;
        // myFunctionalInterface2 = () -> System.out.println("method1 호출");
    }
}
```

- 예제 코드 2
```java
@FunctionalInterface
public interface MyFunctionalInterface2 {
    public void method1(int x);

    // 아래 메소드를 추가하면 추상 메소드가 2개 이상이 되므로 컴파일 오류 발생
    // public void method2(int x);
}
```

```java
public class MyFunctionalInterface2Test {
    public static void main(String[] args) {

        MyFunctionalInterface2 myFunctionalInterface2;

        // 익명 객체로 메소드 정의 및 실행
        myFunctionalInterface2 = new MyFunctionalInterface2(){
            @Override
            public void method1(int x) {
                System.out.println(x+x);
            }
        };
        myFunctionalInterface2.method1(5);

        // 람다식으로 메소드 정의 및 실행
        myFunctionalInterface2 = (x -> System.out.println(x*x));
        myFunctionalInterface2.method1(5);
    }
}
```

<br>

### 사용자 정의 함수적 인터페이스 예제 코드
```java
@FunctionalInterface
public interface IntBinaryOperation {
    int apply(int a, int b);
}
```
```java
public class IntBinaryOperationTest {
    public static void main(String[] args) {
        // 인터페이스의 4개의 구현체
        IntBinaryOperation add = (a, b) -> a + b;
        IntBinaryOperation subtract = (a, b) -> a - b;
        IntBinaryOperation multiply = (a, b) -> a * b;
        IntBinaryOperation divide = (a, b) -> a / b;
        
        // 구현체별 apply 메소드 실행
        System.out.println("10 + 5 = " + add.apply(10, 5));
        System.out.println("10 - 5 = " + subtract.apply(10, 5));
        System.out.println("10 * 5 = " + multiply.apply(10, 5));
        System.out.println("10 / 5 = " + divide.apply(10, 5));
    }
}
```

<br>

## 람다식의 변수 접근 규칙
람다식은 자신을 둘러싸는 영역의 지역 변수에 접근할 수 있지만, 사용되는 지역 변수는 반드시 `final` 이거나 `사실상 final`인 변수여야 한다. 
- `사실 상 final`: 변수가 final로 선언되지 않았어도 해당 변수에 대한 재할당이 발생하지 않는 경우를 의미한다.

### 람다식의 변수 접근 규칙 예제 코드
```java
public class LambdaVariableAccess {
    public static void main(String[] args) {
        // 외부 지역 변수 선언
        int num = 10;
        // 람다식 정의
        Runnable r = () -> System.out.println("외부 변수 num의 값은: " + num);

        // 람다식 실행
        new Thread(r).start();

        // 아래처럼 재할당하는 경우 컴파일 오류를 발생시킴
        // num = 20; // 람다식 내에서 사용되는 변수는 final 또는 사실상 final이어야 함.
    }
}
```

<br>

## 람다와 자바 표준 API

### 주요 함수적 인터페이스
1. `Consumer<T>` : 하나의 입력을 받고 반환 값이 없는 작업을 수행하며, `accept(T t)` 메소드를 정의한다.
2. `Supplier<T>` : 아무런 입력 없이 값을 반환하며, `get()` 메소드를 정의한다.
3. `Function<T, R>` : 하나의 입력을 받고 결과를 반환하며, `apply(T t)` 메소드를 정의한다.
4. `Predicate<T>` : 하나의 입력에 대해 boolean 값을 반환하며, `test(T t)` 메소드를 정의한다.

<img width="693" alt="스크린샷 2024-04-24 13 37 26" src="https://github.com/hamsangjin/TIL/assets/103736614/a3d7d3a2-7061-4df1-9c7d-ef787f323104">

<br>

### 주요 함수적 인터페이스 예제 코드
```java
import java.util.function.*;

public class LambdaExam3 {
    public static void main(String[] args) {
        // Consumer: 입력을 받고 반환값이 없는 연산을 수행
        Consumer<String> priter = s -> System.out.println(s);
        priter.accept("Hello");

        // Function: 입력을 받아서 값을 반환
        Function<String, Integer> lengthFunc = x -> x.length();
        System.out.println(lengthFunc.apply("Hello"));

        // Supplier: 아무런 입력없이 값을 반환
        Supplier<Double> randomSup = () -> Math.random();
        System.out.println(randomSup.get());

        // Predicate: 조건의 테스트를 수행
        Predicate<Integer> isPositive = x -> x > 0;
        System.out.println(isPositive.test(5));
        System.out.println(isPositive.test(-1));
    }
}
```

<br>

## 람다식 확장

### 메소드 참조의 형태
1. `정적 메소드 참조`: `클래스::정적메소드` 형태로 사용되고 예를 들어, `Math::max`는 `Math` 클래스의 정적 메소드 `max`를 참조한다.
2. `인스턴스 메소드 참조`: 특정 객체의 인스턴스 메소드를 참조하며, 형태는 `객체참조 변수::인스턴스메소드`이고 예를 들어, `str::length`는 `String` 객체의 `str`에 대해 `length` 메소드를 참조한다.
3. `임의 객체의 인스턴스 메소드 참조`: 클래스 타입의 임의 객체에 대해 해당 메소드를 참조하며, 형태는 `클래스::인스턴스메소드`로, `String::length` 는 모든 `String` 객체의 `length` 메소드를 참조한다.
4. `생성자 참조`: 클래스의 생성자를 참조하는 형태이고, `클래스::new` 형태로 사용되며, `ArrayList::new`는 `ArrayList`의 생성자를 참조한다.

### 메소드 참조 예제 코드
```java
import java.util.*;
import java.util.function.*;

public class LambdaExam4 {
    public static void main(String[] args) {
        // 1. 정적 메소드 참조
        // BiFunction<T, U, R>: 2개의 인자(T, U)를 받고 1개의 결과(R)를 리턴하는 함수형 인터페이스
        // TriFuncton<A, B, C, R>: 3개의 인자(A, B, C)를 받고 1개의 결과(R)를 리턴하는 함수형 인터페이스
        BiFunction<Integer, Integer, Integer> maxFunc1 = (a, b) -> Math.max(a, b);
        BiFunction<Integer, Integer, Integer> maxFunc2 = Math::max;
        System.out.println(maxFunc1.apply(1, 2));
        System.out.println(maxFunc2.apply(1, 2));

        // 2. 인스턴스 메소드 참조
        // 입력값이 곧 특정 객체(str)가 되고, 반환값은 특정 객체의 길이가 된다.
        String str = "Hello world!";
        Supplier<Integer> lengthFunc1 = () -> str.length();
        Supplier<Integer> lengthFunc2 = str::length;
        System.out.println(lengthFunc1.get());
        System.out.println(lengthFunc2.get());

        // 3. 임의 객체의 인스턴스 메소드 참조
        // 입력값으로 임의 객체 s가 들어오면 반환값은 s의 길이가 된다.
        List<String> words = Arrays.asList("hello", "world", "!");
        List<Integer> lengths1 = new ArrayList<>();

        Function<String, Integer> lengthFunc3 = (s) -> s.length();
        for (String word : words) {
            lengths1.add(lengthFunc3.apply(word));
        }
        System.out.println(lengths1);

        List<Integer> lengths2 = new ArrayList<>();
        Function<String, Integer> lengthFunc4 = String::length;
        for (String word : words) {
            lengths2.add(lengthFunc4.apply(word));
        }
        System.out.println(lengths2);

        // 4. 생성자 참조
        // 입력값은 없으며 ArrayList 객체를 반환한다.
        Supplier<List<String>> listSupplier1 = () -> new ArrayList<>();
        List<String> list1 = listSupplier1.get();
        list1.add("Hello");
        list1.add("World");
        list1.add("!");
        System.out.println(list1);

        Supplier<List<String>> listSupplier2 = ArrayList::new;
        List<String> list2 = listSupplier2.get();
        list2.add("Hello");
        list2.add("World");
        list2.add("!");
        System.out.println(list2);
    }
}
```
