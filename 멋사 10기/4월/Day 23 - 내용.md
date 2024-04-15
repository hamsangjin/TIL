# 제네릭
`제네릭`이란 프로그래밍 언어에서 타입을 파라미터로 사용할 수 있게 하는 기능이다.

## 제네릭 클래스
제네릭 클래스는 클래스 이름 뒤에 `꺽쇠(<, >)` 안에 타입 파라미터를 선언하면 되고, 클래스나 인터페이스는 둘 이상의 타입 파라미터를 가질 수 있다. <br>
이 타입 파라미터는 클래스 내부에서 변수 타입, 메소드의 리턴 타입, 파라미터 타입 등으로 사용될 수 있다.

### 제네릭을 사용하지 않은 클래스
```java
public class NonGenericPair {
    private Object first;   // first는 Object타입이다.
    private Object second;  // second는 Object타입이다.

    public NonGenericPair(Object first, Object second) {
        this.first = first;
        this.second = second;
    }

    public Object getFirst() {
        return first;
    }
    public Object getSecond() {
        return second;
    }
    public void setFirst(Object first) {
        this.first = first;
    }
    public void setSecond(Object second) {
        this.second = second;
    }
}
```
```java
public class NonGenericPairExam{
    public static void main(String[] args){
        NonGenericPair pair = new NonGenericPair("Hello", 5);
        String first = (String) pair.getFirst();      // 캐스팅 필요
        Integer second = (Integer) pair.getSecond();  // 캐스팅 필요
    }
}
```

### 제네릭을 사용한 클래스
```java
public class GenericPair<T1, T2> {
    private T1 first;
    private T2 second;

    public GenericPair(T1 first, T2 second) {
        this.first = first;
        this.second = second;
    }

    public T1 getFirst() {
        return first;
    }
    public T2 getSecond() {
        return second;
    }
    public void setFirst(T1 first) {
        this.first = first;
    }
    public void setSecond(T2 second) {
        this.second = second;
    }
}
```
```java
// 사용 예시
public class NonGenericPairExam{
    public static void main(String[] args){
        GenericPair<String, Integer> pair = new GenericPair<>("Hello", 5);
        String first = pair.getFirst(); // 캐스팅 불필요
        Integer second = pair.getSecond(); // 캐스팅 불필요
    }
}
```

<br>

## 제네릭 메소드
`제네릭 메소드`는 메소드 반환 타입 앞에 타입 파라미터 섹션(`<T>`)을 포함하며, 이 타입 파라미터는 매소드 내에서 타입으로 사용된다.
```java
public <T> void genericMethod(T data) {
    // 메소드 본문
}
```

### 예제
```java
public class GenericMethodExample {
    public static <T> void printArrayElements(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
    public static void main(String[] args) {
        // Integer 배열
        Integer[] intArray = {1, 2, 3, 4, 5};
        printArrayElements(intArray);

        // String 배열
        String[] stringArray = {"Hello", "World", "Java"};
        printArrayElements(stringArray);
    }
}
```

<br>

### 바운디드 타입 파라미터
`바운디드 타입 파라미터`는 제네릭 메소드에서 사용되는 타입 파라미터에 특정 상한을 설정하는 기능이다.<br>
예를 들어 `<T extends Number>`라고 선언하면, Number 클래스나 그 자손 클래스만 타입 파라미터로 들어올 수 있다는 뜻이다.

사용하는 방법은 일반 제네릭 메소드와 비슷하지만, 타입 파라미터 뒤에 `extends` 키워드를 사용해 제한된 타입을 명시해주고, 인터페이스를 사용하는 경우에도 `extends` 키워드를 사용한다. <br>
또, 여러 개의 바운드를 가질 수 있고, 이 경우 `&` 기호를 사용해 연결한다.
- `<T extends Comparable<S>>`: T타입이 Comparable 인터페이스를 구현해야 함을 의미
- `<T extends Number & Comparable<S>>`는 T가 Number를 상속받고, 동시에 Comparable 인터페이스를 구현해야 함을 의미

#### 예제
```java
public class GenericMethodExample {
    // 바운디드 타입 파라미터를 사용하는 제네릭 메소드
    public static <T extends Number> T max(T x, T y, T z) {
        T max = x; // 초기 최댓값 설정
        if (y.doubleValue() > max.doubleValue()) {
            max = y; // y가 더 크면, max를 y로 업데이트
        }

        if (z.doubleValue() > max.doubleValue()) {
            max = z; // z가 더 크면, max를 z로 업데이트
        }
        return max; // 최댓값 반환
    }

    public static void main(String[] args) {
        // 제네릭 메소드 호출
        System.out.println("최댓값: " + max(3, 4, 5));             // 정수
        System.out.println("최댓값: " + max(6.6, 8.8, 7.7));       // 실수
        System.out.println("최댓값: " + max(15.5f, 11.1f, 9.9f));  // float
    }
}
```

<br>

### 상한(extends)
`상한`은 제네릭 타입이 특정 클래스의 하위 타입만을 허용하도록 제한하는 것을 의미하며, `extends` 키워드를 사용해 구현한다.

특정 클래스의 메소드나 속성을 안전하게 사용하고자 할 때, 이를 확장하는 클래스들만을 대상으로 제네릭 타입을 제한하기 위해 사용한다.
- `class Box<T extends Number>`: Box가 Number 혹은 그 하위 타입만을 제네릭 타입으로 받을 수 있음을 의미 

### 하한(super)
`하한`은 제네릭 타입이 특정 클래스의 상위 타입만을 허용하도록 제한하는 것을 의미하며, `super` 키워드를 사용해 구현한다.

특정 클래스에 데이터를 전달하거나 조작할 때, 이 클래스의 상위 타입들만을 허용하고자 할 때 사용한다.
- `void process(Box<T super Integer> box)`: box가 Integer의 상위 타입을 제네릭 타입으로 가질 수 있음을 의미

### 상한과 하한의 차이
- `상한`: 타입이 특정 클래스나 인터페이스의 `하위 클래스`에 제한되며, 주로 **읽기 작업**에 사용된다.
- `하한`: 타입이 특정 클래스의 `상위 클래스`에 제한되며, 주로 **쓰기 작업**에 사용된다.

<br>

## 와일드 카드 타입
`와일드 카드 타입`은 제네릭 프로그래밍에서 사용되는 일종의 표현 방법으로, `?` 기호를 사용해 표현되며, 제네릭 타입의 불확정성을 나타낼 때 사용한다.


### 예제
```java
package org.example;
    // 상위 클래스 Box 정의 class Box<T> {
    private T content;
    public Box(T content) {
        this.content = content;
    }
    public void setContent(T content) {
        this.content = content;
    }
    public T getContent() {
        return content;
    }
    @Override
    public String toString() {
        return "Box containing: " + content;
    }
}
```
```java
// Box를 상속받는 SpecialBox 정의
class SpecialBox<T> extends Box<T> {
    public SpecialBox(T content) {
        super(content);
    }
    // 특별한 기능을 추가하는 메소드 public void printContent() {
        System.out.println("Special box contains: " + getContent());
    }
}
```
```java
// Box를 상속받으면서 새로운 타입 파라미터 C를 추가하는 ColoredBox 정의
class ColoredBox<T, C> extends Box<T> {
    private C color;
    public ColoredBox(T content, C color) {
        super(content);
        this.color = color;
    }
    public C getColor() {
        return color;
    }
    @Override
    public String toString() {
        return "Colored box with color " + color + " containing: " + getContent();
    }
}
```

<br>

### 제한 없는 와일드 카드
`?` 기호만 사용되며, 모든 타입을 허용해준다.

예를 들어 `List<?>`는 어떤 타입이든 될 수 있는 객체의 리스트를 의미한다.

### 제한 있는 와일드 카드
- 상한 제한 와일드 카드: `? extends Type` 형식으로 사용되며, 지정된 타입 또는 그 하위 타입만을 허용한다. 예를 들어, `List<? extends Number>`는 Number 또는 Number의 하위 클래스 타입의 객체를 포함하는 리스트를 의미한다.
- 하한 제한 와일드 카드: `? super Type` 형식으로 사용되며, 지정된 타입 또는 그 상위 타입만을 허용한다. 예를 들어, `List<? super Integer>`는 Integer 또는 Integer의 상위 클래스 타입의 객체를 포함하는 리스트를 의미한다.

<br>

---

<br>

# 컬렉션 프레임워크
`컬렉션 프레임워크`는 자바에서 데이터 집합을 효율적으로 처리할 수 있도록 설계된 표준화된 방법을 제공해준다.

<img width="744" alt="스크린샷 2024-04-15 14 24 43" src="https://github.com/hamsangjin/TIL/assets/103736614/7cd01133-5ab2-4dc2-acf9-5e36432ca2c9">

## 주요 인터페이스 및 상속 클래스
1. `List 인터페이스`: 순서가 있는 컬렉션을 다루며, 동일한 요소의 중복을 혀용한다.
    - 구현 클래스: `ArrayList`, `LinkedList` 등
2. `Set 인터페이스`: 중복을 허용하지 않는 컬렉션으로, 순서에 관계없이 요소를 관리한다.
    - 구현 클래스: `HashSet`, `TreeSet` 등
3. `Queue 인터페이스`: 특정 순서로 요소를 처리할 때 사용한다.
    - 구현 클래스: `LinkedList`, `PriorityQueue` 등

