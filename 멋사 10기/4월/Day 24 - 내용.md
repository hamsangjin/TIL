# Iterator
`Iterator`는 자바의 컬렉션 프레임워크에서 컬렉션에 저장된 요소를 읽거나 제거하는 방법을 제공하는 인터페이스다.

## 주요 기능
- `순차적 접근`: 컬렉션 내의 요소를 순서대로 접근한다.
- `안전한 요소 제거`: Iterator의 `remove()` 메소드를 사용하면 반복 중에 요소를 안전하게 제거할 수 있다.
- `단방향 이동`: `Iterator는 컬렉션의 요소를 앞에서 뒤로만 이동하며 접근한다.

<br>

## Iterator를 지원하는 클래스
- List 인터페이스: `ArrayList`, `LinkedList` 등 `List` 인터페이스를 구현하는 모든 클래스들은 Iterator를 지원한다.
- Set 인터페이스: `HashSet`, `TreeSet` 과 같은 `Set` 인터페이스를 구현하는 클래스들도 Iterator를 지원한다.
- Map 인터페이스: `HashMap`, `TreeMap` 과 같은 `Map` 인터페이스를 구현하는 클래스들은 `keySet`, `entrySet`, `values` 메소드를 통해 반환된 컬렉션을 `Iterator`를 사용하여 순회할 수 있다.

<br>

## 예제 코드
```java
import java.util.ArrayList;
import java.util.Set;
import java.util.HashSet;
import java.util.Iterator;
public class IteratorExample {
    public static void main(String[] args) {
        // ArrayList 생성 및 요소 추가
        ArrayList<String> list = new ArrayList<>();
        list.add("a");
        list.add("b");
        list.add("c");

        // for문을 사용하여 ArrayList 순회
        for(int i = 0; i < list.size(); i++){
            System.out.println(list.get(i));
        }
        
        // Iterator를 사용하여 ArrayList 순회
        Iterator<String> iterator1 = list.iterator();
        while(iterator1.hasNext()) {
            String str = iterator1.next();
            System.out.println(str);
        }

        // HashSet 생성 및 요소 추가
        Set<String> set = new HashSet<>();
        set.add("a");
        set.add("b");
        set.add("c");

        // set은 순서가 없는 컬렉션은 get과 같이 인덱스를 통한 접근이 불가
        // 따라서 for문이 아닌 Iterator를 사용해 순회
        Iterator<String> iterator2 = set.iterator();
        while(iterator2.hasNext()) {
            String str = iterator2.next();
            System.out.println(str);
        }

        // for-each문은 가능
        for(String str : set){
            System.out.println(str);
        }
    }
}
```

<br>

---

<br>

# List
`List` 인터페이스는 객체의 순서가 있는 시퀀스를 나타내며, 중복을 허용하고, 각 객체는 인덱스를 통해 특정 위치에 접근할 수 있다.

## 주요 구현 클래스
- `ArrayList`: 내부적으로 동적 배열을 사용하여 요소를 저장한다. 인덱스를 통한 빠른 임의 접근이 가능하며, 요소 추가 및 삭제 시 다른 요소들의 이동이 필요할 수 있다.
- `LinkedList`: 이중 연결 리스트를 사용하여 요소를 관리한다. 요소의 추가와 삭제가 빠르지만, 임의 접근에는 상대적으로 시간이 더 걸린다.
- `Vector`: ArrayList와 유사하지만, 스레드에 안전한 방식으로 구현되어 있다. 이로 인해 멀티스레드 환경에서 사용하기에 적합하지만, 성능이 다소 느릴 수 있다.
- `Stack`: Vector를 상속받은 클래스로, 후입선출(LIFO) 방식의 데이터 처리에 사용된다.

<br>

## 예제 코드
```java
import java.util.ArrayList;
import java.util.List;
public class ListExample {
    public static void main(String[] args) {
        // ArrayList 생성
        List<String> fruits = new ArrayList<>();

        // 요소 추가
        fruits.add("Apple");
        fruits.add("Banana");
        fruits.add("Cherry");

        // 기존 리스트 출력
        System.out.println("기존 과일 리스트: " + fruits);

        // 요소 접근
        String firstFruit = fruits.get(0);
        System.out.println("첫 번째 과일: " + firstFruit);

        // 요소 수정
        fruits.set(1, "Blueberry");

        // 요소 삭제
        fruits.remove("Cherry");

        // 연산 후 리스트 출력
        System.out.println("업데이트된 과일 리스트: " + fruits);
    }
}
```

<br>

---

<br>

# Set
`Set` 인터페이스는 중복되지 않는 유일한 요소들을 저장하는 데 사용되며, 순서를 보장하지 않고, 같은 요소의 중복을 허용하지 않는다.

## 주요 구현 클래스
- `HashSet`: 가장 널리 사용되는 `Set` 구현체이며, 내부적으로 `HashMap`을 사용하여 요소를 저장한다. 요소의 순서를 보장하지 않으며, 빠른 검색 속도를 제공한다.
- `LinkedHashSet`: `HashSet` 과 유사하지만, 요소들을 `LinkedList` 의 형태로 데이터를 저장하여 삽입된 순서대로 유지한다.
- `TreeSet`: `레드-블랙 트리(Red-Black tree)` 데이터 구조를 기반으로 하는 `Set` 구현체이며, 이는 요소들을 정렬된 순서대로 저장하고 검색, 삭제, 삽입 작업을 효율적으로 수행한다.

<br>

## 예제 코드
```java
import java.util.HashSet;
import java.util.Set;

public class SetExample {
    public static void main(String[] args) {
        // HashSet 생성
        Set<String> fruitSet = new HashSet<>();
        
        // 요소 추가
        fruitSet.add("사과");
        fruitSet.add("바나나");
        fruitSet.add("키위");
        fruitSet.add("바나나"); // 중복 요소 추가 시도
        
        // 요소 출력
        System.out.println("과일 집합: " + fruitSet);
        
        // 특정 요소 포함 여부 확인
        if (fruitSet.contains("키위")) {
            System.out.println("키위가 있습니다.");
        }
        
        // 요소 제거
        fruitSet.remove("바나나");
        System.out.println("바나나 제거 후: " + fruitSet);
        
        // 집합의 크기
        System.out.println("집합의 크기: " + fruitSet.size());
    }
}
```

<br>

---

<br>

# Map
`Map`은 `키(Key)`와 `값(Value)`의 쌍으로 데이터를 저장하는 자료구조이고, 각 키는 고유하며, 이를 통해 해당 키에 매핑된 값을 검색할 수 있다. 

## 특징
- `Key`: Map 내의 각 키는 중복될 수 없으며, 하나의 키는 오직 하나의 값을 가리킬 수 있다.
- `값의 매핑`: 키를 사용하여 값을 저장하고, 나중에 같은 키로 해당 값을 검색할 수 있다.
- `순서의 무관성`: 대부분의 Map 구현체에서는 요소들의 순서를 보장하지 않는다.

<br>

## 주요 구현 클래스
- `HashMap`: 가장 일반적으로 사용되는 Map 구현체로, 해시 테이블을 사용하여 키와 값을 저장하고, 순서를 보장하지 않는다.
- `TreeMap`: 레드-블랙 트리 기반의 Map 구현체로, 키에 따라 정렬된 순서로 키-값 쌍을 저장한다. 키의 자연 순서에 따라 정렬되거나, 생성자에 제공된 `Comparator`에 의해 정렬됩니다.
- `LinkedHashMap`: HashMap과 유사하지만, 요소들이 추가된 순서 또는 접근 순서를 유지한다.
- `Hashtable`: HashMap과 유사하지만, 동기화를 지원한다. 멀티스레드 환경에서 안전하게 사용할 수 있지만, 일반적으로는 `ConcurrentHashMap`을 사용하는 것이 더 바람직하다.

<br>

## 예제 코드
```java
import java.util.HashMap;
import java.util.Map;
public class PhoneBookExample {
    public static void main(String[] args) {
        // HashMap 생성
        Map<String, String> phoneBook = new HashMap<>();

        // 전화번호 추가
        phoneBook.put("김철수", "010-1234-5678");
        phoneBook.put("박영희", "010-8765-4321");
        phoneBook.put("이민지", "010-5566-7788");
        
        // 특정 키를 이용해 전화번호 검색
        String minjiNumber = phoneBook.get("이민지");
        System.out.println("이민지의 전화번호: " + minjiNumber);
        
        // 전체 전화번호 목록 출력
        System.out.println("전체 전화번호 목록:");
        for (Map.Entry<String, String> entry : phoneBook.entrySet()) {
            System.out.println(entry.getKey() + ": " + entry.getValue());
        }
    }
}
```

<br>

---

<br>

# Collection
`Collections`는 자바 표준 컬렉션 프레임워크에 포함된 클래스로, 다양한 컬렉션 관련 알고리즘과 정적 메소드를 제공한다.

## 메소드 소개
- `shuffle()`: 리스트의 요소를 무작위로 섞어준다.
- `compareTo()`: `Comparable` 인터페이스의 메소드로, 두 개의 값을 비교하여 int형 값을 반환해준다.
  - `반환 방법`: 두 개의 값을 사전순으로 비교하여 같으면 `0`, 사전순으로 이전이면 `음수`, 이후면 `양수`를 반환

<br>

## 예제 
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Person implements Comparable<Person> {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Person other) {
        return this.age - other.age; // 나이를 기준으로 오름차순 정렬
    }

    @Override
    public String toString() {
        return name + ": " + age;
    }

    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();

        people.add(new Person("Alice", 32));
        people.add(new Person("Bob", 24));
        people.add(new Person("Charlie", 28));

        Collections.sort(people); // Comparable을 사용한 정렬

        for (Person person : people) {
            System.out.println(person);
        }
    }
}
```

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

public class Person {
    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + ": " + age;
    }

    public static void main(String[] args) {
        List<Person> people = new ArrayList<>();

        people.add(new Person("Alice", 32));
        people.add(new Person("Bob", 24));
        people.add(new Person("Charlie", 28));

        // Comparator를 사용한 정렬
        Collections.sort(people, new Comparator<Person>() {
            @Override
            public int compare(Person p1, Person p2) {
                return p1.name.compareTo(p2.name); // 이름을 기준으로 오름차순 정렬
            }
        });

        for (Person person : people) {
            System.out.println(person);
        }
    }
}
```
