# 스트림
`스트림(Stream)`은 자바 8에서 도입된 중요한 개념 중 하나로, 데이터의 추상화된 연속된 흐름을 의미하며, 데이터 컬렉션을 더 간결하고 직관적인 방식으로 처리할 수 있게 해준다. 

## 스트림과 컬렉션의 차이점

### 1. 데이터 처리 방식
- `컬렉션`: 데이터를 저장하고 관리하는 구조
- `스트림`: 데이터의 흐름을 나타내며, 데이터를 저장하지 않고, 필요에 따라 요소를 계산(지연 계산)한다.

### 2. 사용 목적과 특성
- `컬렉션`: 데이터의 전체적인 관리와 접근에 중점을 둔다.
- `스트림`: 데이터의 변환과 처리에 초점을 맞춘다.

### 3. 병렬 처리
- `컬렉션`: 병렬 처리를 직접 구현해야 한다.
- `스트림`: 내장된 병렬 처리 기능을 제공한다.

### 4. 데이터 처리의 효율성
- `컬렉션`: 대량의 데이터를 처리할 때, 전체 데이터가 메모리에 올라가야 하므로, 메모리 사용량이 많을 수 있다.
- `스트림`: 지연 계산 방식을 사용하기 때문에, 필요한 데이터만 처리하고 나머지는 무시할 수 있어 메모리 효율성이 더 높다.

<br>

## 스트림 생성 방법

### 1. 컬렉션에서 스트림 생성
```java
List<String> myList = Arrays.asList("a", "b", "c");
Stream<String> myStream = myList.stream();
```

### 2. 배열에서 스트림 생성
```java
String[] myArray = new String[]{"a", "b", "c"};
Stream<String> arrayStream = Arrays.stream(myArray);
```

### 3. 스트림의 정적 메소드 사용
```java
Stream<Integer> numberStream = Stream.of(1, 2, 3);
```

- `of()`, `iterate()`, `generate()`와 같은 정적 메소드를 제공해줌

### 4. 파일로부터 스트림 생성
```java
Stream<String> lines = Files.lines(Paths.get("myFile.txt"));
```

### 5. 빌더를 사용한 스트림 생성
```java
Stream<String> builtStream = Stream.<String>builder().add("a").add("b").add("c").build();
```

<br>

## 스트림의 중간 연산과 최종 연산
<img width="629" alt="스크린샷 2024-04-25 10 16 16" src="https://github.com/hamsangjin/TIL/assets/103736614/091a3ff6-9f10-4060-851e-d48d20d2dfb2">

<br>


### 중간 연산
`중간 연산`은 스트림을 다른 스트림으로 변환하는 연산으로, 여러 개의 중간 연산이 될 수 있다.
- `연쇄 가능`: 여러 중간 연산을 연결하여 복잡한 데이터 처리 파이프라인을 구성할 수 있다.
- `지연 실행`: 중간 연산은 최종 연산이 호출될 때까지 실제로 실행되지 않으므로, 데이터 처리의 효율성을 높인다.
- `상태 없음 혹은 상태 보유`: 일부 중간 연산은 상태를 유지하지 않지만, 정렬 또는 중복 제거와 같이 상태를 유지하는 연산도 있다.

### 최종 연산
`최종 연산`은 스트림의 요소를 소모하여 결과를 도출하거나 부작용을 발생시키는 연산이다.
- `스트림의 소비`: 최종 연산은 스트림의 요소를 소비하여, 단일 값이나 콜렉션을 반환하거나, 아무값도 반환하지 않을 수도 있다.
- `스트림 파이프라인의 실행`: 최종 연산은 중간 연산들이 처리된 데이터에 적용되며, 그 결과를 반환한다.

### 중간 연산과 최종 연산 예제 코드

- 예제 코드 1
```java
import java.util.*;
import java.util.stream.Collectors;

public class StreamExam1 {
    public static void main(String[] args) {
        List<String> myList = Arrays.asList("aefa", "badqd", "dqdc", "bteb", "rtwer", "aafqw");

        // 스트림 사용 X
        List<String> filteredList1 = new ArrayList<>();
        for(String str : myList){
            if(str.startsWith("a")) filteredList1.add(str);
        }
        System.out.println(filteredList1);

        // 스트림 사용 O
        List<String> filteredList = myList.stream()
                .filter(s -> s.startsWith("a")) // 중간 연산
                .collect(Collectors.toList());  // 최종 연산
        System.out.println(filteredList);

        System.out.println();

        String[] names = {"kang", "kim", "hong", "lee", "son",};

        // 스트림 사용 X
        for(String name:names){
            System.out.println(name);
        }

        // 스트림 사용 O
        Arrays.stream(names).forEach(System.out::println);

        System.out.println();

        int[] iarr = {1, 2, 3, 4, 5, 6, 7, 8};

        // 스트림 사용 X
        for(int i : iarr){
            if(i % 2 == 0) System.out.println(i);
        }

        // 스트림 사용 O
        Arrays.stream(iarr).filter(i -> i % 2 == 0).forEach(System.out::println);
    }
}
```

- 예제 코드 2
```java
import java.nio.file.*;
import java.util.stream.Stream;

public class StreamExam2{
    public static void main(String[] args) throws Exception {
        Path path = Paths.get("src/com/example/");
        // 디렉토리를 얻어올 땐 list 메소드 사용
        Stream<Path> stream1 = Files.list(path);
        stream1.forEach(p -> System.out.println(p.getFileName()));
        stream1.close();

        // 파일을 얻어올 땐 lines 메소드 사용
        Stream<String> stream2 = Files.lines(Paths.get("src/com/example/day30/StreamExam1.java"));
        stream2.forEach(p -> System.out.println(p));
    }
}
```

<br>

## 데이터 필터링

### filter 메소드
- `기능`:  주어진 조건에 맞는 요소만을 스트림에서 추출한다.
- `사용 방법`: `Predicate` 인터페이스를 파라미터로 받는다. -> `반환 값 : boolean 타입`
- `예시`: `stream.filter(element -> element.contains("특정 문자열"))`은 스트림 내 요소 중 '특정 문자열'을 포함하는 요소만을 선택한다.

### distinct 메소드
- `기능`: 스트림에서 중복을 제거하며, 주로 `filter` 메소드와 함께 사용되어, 중복되지 않은 요소들만을 필터링하는데 사용된다.
- `동작 원리`: `equals` 메소드를 기반으로 요소들의 동등성을 판단한다.
- `예시`: `stream.distinct()`는 스트림 내에서 중복된 요소를 제거해, 각 요소가 유일하게 존재하도록 한다.

### 데이터 필터링 예제 코드
```java
import java.util.*;

public class StreamExam1 {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("Apple", "Banana", "Cherry", "Apple", "Cherry", "Date");

        // 스트림 사용 X
        List<String> filteredWords1 = new ArrayList<>();
        for(String w : words){
            if(w.length() > 5){
                if(!filteredWords1.contains(w)) filteredWords1.add(w);
            }
        }
        System.out.println(filteredWords1);

        // 스트림 사용 O
        List<String> filteredWords2 = words.stream()
                .filter(w -> w.length() > 5)
                .distinct()
                .toList();
        System.out.println(filteredWords2);
    }
}
```

<br>

## 데이터 변환
<img width="628" alt="스크린샷 2024-04-25 11 41 43" src="https://github.com/hamsangjin/TIL/assets/103736614/5b2022dc-13d8-4186-8514-858ed770d977">

### map 메소드
- `기능`: 스트림의 각 요소를 주어진 함수에 따라 다른 형태로 변환한다.
- `사용 방법`: `Function` 인터페이스를 인자로 받는다.
- `예시`: `stream.map(element -> element.toUpperCase())`는 스트림의 각 문자열 요소를 대문자로 변환한다.

### flatMap 메소드
- `기능`: 각 요소를 스트림으로 변환하고, 그 스트림들을 하나의 스트림으로 합치며, 이는 중첩된 구조를 평탄화하는 데 사용된다.
- `사용 방법`: 중첩된 스트림 구조를 단일 스트림으로 통합할 때 사용되며, map과 동일하게 `Function` 인터페이스를 인자로 받는다.
- `예시`: `stream.flatMap(list -> list.stream())`는 각 요소(리스트 등의 컬렉션)를 스트림으로 변환하고, 이러한 스트림들을 하나의 스트림으로 결합한다.

### 데이터 변환 예제 코드

- map 메소드 예제 코드
```java
package com.example.day30;

import java.util.*;

public class StreamExam3 {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("Apple", "Banana", "Cherry", "Apple", "Cherry", "Date");

        // 스트림 사용 X
        for(String w : words){
            System.out.println(w.toLowerCase());
        }

        // 스트림 사용 O
        words.stream().map(w -> w.toUpperCase()).forEach(System.out::println);

        System.out.println();

        int[] intArr = {3, 6, 3, 76, 4, 2};

        // 스트림 사용 X
        for(int i : intArr){
            System.out.println(i*3);
        }

        // 스트림 사용 O
        Arrays.stream(intArr).map(i -> i*3).forEach(System.out::println);
    }
}
```

- flatmap 메소드 예제 코드
```java
import java.util.*;
import java.util.stream.Collectors;

public class StreamExam4 {
    public static void main(String[] args) {
        List<List<String>> nestedList = Arrays.asList(
                Arrays.asList("Apple", "Banana"),
                Arrays.asList("Cherry", "Date")
        );

        System.out.println(nestedList);

        // 스트림 사용 X
        List<String> flattenedList1 = new ArrayList<>();
        for(List<String> list : nestedList){
            for(String s : list)  flattenedList1.add(s);
        }
        System.out.println(flattenedList1);

        // 스트림 사용 O
        List<String> flattenedList2 = nestedList.stream()
                .flatMap(Collection::stream)
                .collect(Collectors.toList());
        System.out.println(flattenedList2);
    }
}
```

<br>

## 스트림 정렬

### sorted 메소드
- `기능 설명`: 스트림의 요소들을 주어진 기준에 따라 정렬한다.
- `기본 정렬`: 기본적으로 오름차순으로 데이터를 정렬하며, 이는 요소들이 `Comparable` 인터페이스를 구현했을 때 사용할 수 있다.
- `사용자 지정 정렬`: `sorted(Comparator<T>)` 메소드는 사용자 정의 비교자를 사용해 정렬하며, 이를 통해 내림차순 정렬, 특정 속성 기준 정렬 등을 수행할 수 있다.

### 스트림 정렬 예제 코드
```java
import java.util.*;

public class StreamExam5 {
    public static void main(String[] args) {
        List<String> fruits = Arrays.asList("Banana", "Apple", "Cherry", "Date");

        List<String> sortedFruits = fruits.stream()
                .sorted()
                .toList();
        System.out.println(sortedFruits);

        List<String> reverseSortedFruits = fruits.stream()
                .sorted(Comparator.reverseOrder())
                .toList();
        System.out.println(reverseSortedFruits);

        int[] iarr = {5, 3, 27, 9, 5, 9, 0, 4, 34};

        Arrays.stream(iarr)
                .sorted()
                .forEach(System.out::println);

        // 자바 제네릭 타입엔 기본형 타입인 int타입이 올 수 없으므로
        // 상위 타입인 Object 타입으로 변경한 후 내림차순 정렬하거나 -> mapToObj(i -> i)
        // 박싱해서 정렬 -> boxed()
        // - 오토 박싱: 기본형 타입 -> 객체
        // - 오토 언박싱: 객체 -> 기본형 타입
        Arrays.stream(iarr)
                .boxed()
                .sorted(Comparator.reverseOrder())
                .forEach(System.out::println);
    }
}
```

<br>

## 스트림 요소 순회

### forEach 메소드
- `기능 설명`: 스트림의 각 요소에 대해 주어진 작업을 수행한다.
- `사용 상황`: 최종 연산에 사용하며, 스트림의 모든 요소에 대해 주어진 작업을 실행하고 스트림 처리를 종료한다.
- `예시`: 데이터 출력, 요소에 대한 조작 등에 사용된다.

### peek 메소드
- `기능 설명`: 스트림의 각 요소에 대해 작업을 수행하지만, 스트림 자체는 변화시키지 않는다.
- `사용 상황`: 중간 연산에 사용하며, 디버깅이나 요소에 대한 임시 처리를 위해 사용된다.
- `차이점`: forEach와 달리 peek는 스트림의 흐름을 중단하지 않는다.

### 스트림 요소 순회 예제 코드
```java
import java.util.*;

public class StreamExam6 {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
        
        // forEach 사용 예시
        numbers.stream()
                .forEach(n -> System.out.println("Number: " + n));

        // peek 사용 예시
        List<Integer> doubledNumbers = numbers.stream()
                .peek(n -> System.out.println("Processing 1: " + n))
                .map(n -> n * 2)
                .peek(n -> System.out.println("Processing 2: " + n))
                .toList();
    }
}
```

<br>

## 조건 매칭

### allMatch 메소드
- `기능 설명`: 스트림의 모든 요소가 주어진 조건을 만족하는지 검사한다.
- `사용 상황`: 데이터 전체가 특정 조건을 충족해야 할 때 사용된다.
- `예시`: 모든 요소가 양수인지 확인하는 경우에 사용된다.

### anyMatch 메소드
- `기능 설명`: 스트림의 어떤 요소라도 주어진 조건을 만족하는지 검사한다.
- `사용 상황`: 하나 이상의 요소가 조건을 충족하는지 확인할 때 사용된다.
- `예시`: 적어도 하나의 요소가 특정 값을 가지는지 확인하는 경우에 사용된다.

### noneMatch 메소드
- `기능 설명`: 스트림의 모든 요소가 주어진 조건을 만족하지 않는지 검사한다.
- `사용 상황`: 어떤 요소도 특정 조건을 충족하지 않아야 할 때 사용된다.
- `예시`: 모든 요소가 특정 범위 밖에 있는지 확인하는 경우에 사용된다.

> 전부 `최종 연산`에 해당

### 조건 매칭 예제 코드
```java
import java.util.*;

public class StreamExam7 {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        boolean allPositive = numbers.stream().allMatch(n -> n > 0); // 모든 숫자가 양수인지
        boolean anyNegative = numbers.stream().anyMatch(n -> n < 0); // 어떤 숫자라도 음수인지
        boolean noneAboveTen = numbers.stream().noneMatch(n -> n > 10); // 모든 숫자가 10 이하인지

        System.out.println(allPositive);
        System.out.println(anyNegative);
        System.out.println(noneAboveTen);

        int[] intArr = {2, 4, 6, 8};
        boolean result = false;

        result = Arrays.stream(intArr).allMatch(n -> n % 3 == 0);
        System.out.println("모두 3의 배수 입니까??" + result);

        result = Arrays.stream(intArr).anyMatch(n -> n % 3 == 0);
        System.out.println("하나라도 3의 배수 입니까??" + result);

        result = Arrays.stream(intArr).noneMatch(n -> n % 3 == 0);
        System.out.println("모두 3의 배수가 아닙니까??" + result);
    }
}
```

<br>

## 집계 연산

### count 메소드의 기능
- `기능 설명`: 스트림 내 요소의 개수를 반환한다.
- `사용 상황`: 데이터의 총 개수가 필요할 때 사용된다.
- `예시`: 데이터 셋 내 특정 조건을 만족하는 요소의 수를 세는 데 사용된다.

### max 와 min 메소드의 기능
- `기능 설명`: 스트림 내 최대값이나 최소값을 찾는다. 
- `사용 상황`: 데이터 중 가장 크거나 작은 값을 찾을 때 사용된다. 
- `예시`: 최고 점수 또는 최저 점수를 찾는 데 사용된다.

### average 메소드의 기능
- `기능 설명`: 스트림 내 요소들의 평균 값을 계산한다. 
- `사용 상황`: 데이터의 평균적인 특성을 파악할 때 사용된다. 
- `예시`: 평균 점수나 평균 가격을 계산하는 데 사용된다.

### sum 메소드의 기능
- `기능 설명`: 스트림 내 요소들의 합을 계산한다. 
- `사용 상황`: 데이터의 총합이 필요할 때 사용된다. 
- `예시`: 총 매출액이나 총 점수를 계산하는 데 사용된다.

### 집계 연산 예제 코드
```java
import java.util.*;

public class StreamExam8 {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(12, 36, 53, 35, 66, 57);

        long count = numbers.stream()
                .count(); // 요소 개수

        int max = numbers.stream()
                .max(Integer::compareTo)
                .orElse(0); // 최대 값, List가 비어있으면 0 반환

        int min = numbers.stream()
                .min(Integer::compareTo)
                .orElse(0); // 최소 값
        
        double average = numbers.stream()
                .mapToInt(Integer::intValue)
                .average()
                .orElse(0.0); // 평균 값

        int sum = numbers.stream()
                .mapToInt(Integer::intValue)
                .sum(); // 총합

        System.out.println(count);
        System.out.println(max);
        System.out.println(min);
        System.out.println(average);
        System.out.println(sum);
    }
}
```

`max`와 `min` 연산을 했을 때 `Optional`이라는 클래스가 반환된다.
- `Optional 클래스`: `null`에 대한 처리를 하기 위한 클래스
  - `orElse()`와 같이 `null`인 경우 처리할 수 있는 메소드들이 존재한다.
  - `get()`을 이용해서 가져오면 `null`에 대한 처리를 하지 않고 값을 반환받는다.

<br>

## 데이터 축소

### 리듀싱 연산
리듀싱 연산은 스트림 내의 여러 데이터를 특정 기준에 따라 하나의 결과로 결합하는 과정이다. 

스트림 내 요소들을 순차적/병렬적으로 처리하여, 하나의 결론을 도출하는 데 사용된다.

- 특징
  - `결합성`: 연산이 결합 법칙을 만족해야 병렬 처리에 적합하다.
  - `상태 없음`: 각 요소의 처리가 다른 요소들에 의존하지 않아야 힌다.
  - `결과의 단일성`: 모든 입력 요소들이 하나의 결과로 수렴한다.

### 데이터 축소 예제

- Member 클래스 코드
```java
public class Member {
    public static final int MALE=0;
    public static final int FEMALE=1;
    private String name;
    private int score;
    private int age;
    private int gender;

    public Member(String name, int score) {
        this.name = name;
        this.score = score;
    }

    @Override
    public String toString() {
        return "Member{" +
                "name='" + name + '\'' +
                ", score=" + score +
                ", age=" + age +
                ", gender=" + gender +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getGender() {
        return gender;
    }

    public void setGender(int gender) {
        this.gender = gender;
    }
}
```

- 메인 코드
```java
import java.util.*;

public class StreamExam9 {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // 초기에 0이 a가 되고 b는 numbers에서 하나 꺼낸 값이 된다.
        // 그 후 연산이 진행되면 1이 a가 되고 b는 그 다음 numbers에서 하나 꺼낸 값이 되고 이걸 반복하게 된다.
        int n = numbers.stream().reduce(0, (a, b) -> a + b);
        System.out.println(n);

        // Member타입의 리스트 생성
        List<Member> members = Arrays.asList(
                new Member("ham", 100),
                new Member("kim", 80),
                new Member("kang", 90),
                new Member("lee", 70)
        );

        // Member들의 score 총합 구하는 방식 1
        int scoreSum1 = members.stream()
                .mapToInt(Member::getScore)
                .reduce(0, (a, b) -> a + b);
        System.out.println(scoreSum1);

        // Member들의 score 총합 구하는 방식 2
        int scoreSum2 = members.stream()
                .mapToInt(Member::getScore)
                .sum();
        System.out.println(scoreSum2);
        
        // filter를 사용해서 조건에 맞는 Member객체의 점수만 더할 수도 있음
    }
}
```

<br>

## 스트림 결과 수집

### Collectors
- `목록, 세트, 맵 수집`: 스트림의 요소들을 `List`, `Set`, 또는 `Map`과 같은 컬렉션으로 수집하고 예를 들어, `toList()`, `toSet()`, `toMap()` 등의 메소드가 있다.
- `데이터 요약`: `summarizingInt()`, `summarizingDouble()`, `summarizingLong()`과 같은 메소드를 사용해 스트림의 숫자 데이터를 요약할 수 있다.
- `문자열 결합`: `joining()` 메소드는 스트림의 문자열 요소들을 하나의 문자열로 결합하며 구분자, 접두사, 접미사를 선택적으로 사용할 수 있다.
- `그룹화와 분할`: `groupingBy()`와 `partitioningBy()` 메소드는 스트림의 요소들을 특정 기준에 따라 그룹화하거나 분할한다.

### Collectors 예제 코드
- Member 클래스 코드
```java
public class Member {
    public static final int MALE=0;
    public static final int FEMALE=1;
    private String name;
    private int score;
    private int age;
    private int gender;

    public Member(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public Member(String name, int age, int gender) {
        this.name = name;
        this.age = age;
        this.gender = gender;
    }

    @Override
    public String toString() {
        return "Member{" +
                "name='" + name + '\'' +
                ", score=" + score +
                ", age=" + age +
                ", gender=" + gender +
                '}';
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public int getGender() {
        return gender;
    }

    public void setGender(int gender) {
        this.gender = gender;
    }
}
```

- 메인 코드
```java
import java.util.*;
import java.util.stream.Collectors;

public class StreamExam10 {
    public static void main(String[] args) {
        List<Member> members = Arrays.asList(
                new Member("ham", 24, Member.MALE),
                new Member("kim", 32, Member.FEMALE),
                new Member("kang", 56, Member.FEMALE),
                new Member("lee", 15, Member.MALE)
        );

        // key: 성별
        // value: 성별 별로 그룹화된 멤버 리스트
        Map<Integer, List<Member>> mapByGender = members.stream()
                .collect(Collectors.groupingBy(Member::getGender));

        System.out.println(mapByGender);

        System.out.print("남자: ");
        mapByGender.get(Member.MALE).stream()
                .forEach(m -> System.out.print(m.getName() + " "));

        System.out.println();

        System.out.print("여자: ");
        mapByGender.get(Member.FEMALE).stream()
                .forEach(m -> System.out.print(m.getName() + " "));
    }
}
```
