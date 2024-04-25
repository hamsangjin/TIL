# 실습 문제 1: 숫자 배열 처리
주어진 정수 배열에서 짝수만을 찾아 그 합을 구하시오.

**예시 입력**
```java
int[] numbers = {3, 10, 4, 17, 6};
```

**예시 출력**
```java
20
```

## 풀이 코드
```java
import java.util.Arrays;

public class Exam1 {
    public static void main(String[] args) {
        int[] numbers = {3, 10, 4, 17, 6};

        int sum = Arrays.stream(numbers)
                .filter(n -> n % 2 == 0)
                .sum();
        System.out.println(sum);
    }
}
```

<br>

---

<br>


# 실습 문제 2: 문자열 리스트 필터링
주어진 문자열 리스트에서 길이가 5 이상인 문자열만을 선택하여 대문자로 변환하고, 결과를 리스트로 반환하시오.

**예시 입력**
```java
List<String> words = Arrays.asList("apple", "banana", "cherry", "date");
```

**예시 출력**
```java
["APPLE", "BANANA", "CHERRY"]
```

## 풀이 코드
```java
import java.util.*;

public class Exam2 {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "date");

        List<String> result = words.stream()
                .filter(w -> w.length() >= 5)
                .map(String::toUpperCase)
                .toList();
        System.out.println(result);
    }
}
```
<br>

---

<br>

# 실습 문제 3: 학생 성적 처리
학생 객체의 리스트가 주어졌을 때, 성적(score)이 80점 이상인 학생들만을 선택하고, 이들의 이름을 알파벳 순으로 정렬하여 출력하시오.

**예시 입력**
```java
List<Student> students = Arrays.asList(
    new Student("Alice", 82),
    new Student("Bob", 90),
    new Student("Charlie", 72),
    new Student("David", 76)
);
```

**예시 출력**
```java
["Alice", "Bob"]
```

**필요한 `Student` 클래스**
```java
public class Student {
    private String name;
    private int score;

    public Student(String name, int score) {
        this.name = name;
        this.score = score;
    }

    public String getName() {
        return name;
    }

    public int getScore() {
        return score;
    }
}
```

## 풀이 코드
```java
import java.util.*;

public class Exam3 {
    public static void main(String[] args) {
        List<Student1> students = Arrays.asList(
                new Student1("Alice", 82),
                new Student1("Bob", 90),
                new Student1("Charlie", 72),
                new Student1("David", 76)
        );

        System.out.println(students.stream()
                .filter(s -> s.getScore() >= 80)
                .map(Student1::getName)
                .sorted()
                .toList());
    }
}
```

<br>

---

<br>

# 실습 문제 4: 직원 관리
직원 객체의 리스트에서 각 부서(department)별로 평균 급여를 계산하시오.

**예시 입력**
```java
List<Employee> employees = Arrays.asList(
    new Employee("Alice", "HR", 3000),
    new Employee("Bob", "HR", 2000),
    new Employee("Charlie", "Engineering", 5000),
    new Employee("David", "Engineering", 4000)
);
```

**예시 출력**
```java
HR: 2500.0
Engineering: 4500.0
```

**필요한 `Employee` 클래스**
```java
public class Employee {
    private String name;
    private String department;
    private double salary;

    public Employee(String name, String department, double salary) {
        this.name = name;
        this.department = department;
        this.salary = salary;
    }

    public String getDepartment() {
        return department;
    }

    public double getSalary() {
        return salary;
    }
}
```

## 풀이 코드
```java
import java.util.*;
import java.util.stream.Collectors;

public class Exam4 {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
                new Employee("Alice", "HR", 3000),
                new Employee("Bob", "HR", 2000),
                new Employee("Charlie", "Engineering", 5000),
                new Employee("David", "Engineering", 4000)
        );

        Map<String, Double> mapByDepartment = employees.stream()
                .collect(Collectors.groupingBy(
                        Employee::getDepartment,
                        Collectors.averagingDouble(Employee::getSalary)
                ));

        mapByDepartment.forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```

<br>

---

<br>

# 실습 문제 5: 제품 카테고리별 평균 가격 계산
주어진 제품 리스트에서 각 카테고리별로 평균 가격을 계산하시오.

**예시 입력**
```java
List<Product> products = Arrays.asList(
    new Product("Laptop", "Electronics", 1200.00),
    new Product("Smartphone", "Electronics", 700.00),
    new Product("Desk", "Furniture", 300.00),
    new Product("Chair", "Furniture", 150.00)
);
```

**예시 출력**
```java
Electronics: 950.0
Furniture: 225.0
```

**필요한 `Product` 클래스**
```java
public class Product {
    private String name;
    private String category;
    private double price;

    public Product(String name, String category, double price) {
        this.name = name;
        this.category = category;
        this.price = price;
    }

    public String getCategory() {
        return category;
    }

    public double getPrice() {
        return price;
    }
}
```

## 풀이 코드
```java
import java.util.*;
import java.util.stream.Collectors;

public class Exam5 {
    public static void main(String[] args) {
        List<Product> products = Arrays.asList(
                new Product("Laptop", "Electronics", 1200.00),
                new Product("Smartphone", "Electronics", 700.00),
                new Product("Desk", "Furniture", 300.00),
                new Product("Chair", "Furniture", 150.00)
        );

        Map<String, Double> mapToCategory = products.stream()
                .collect(Collectors.groupingBy(
                        Product::getCategory,
                        Collectors.averagingDouble(Product::getPrice)
                ));

        mapToCategory.forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```

<br>

---

<br>

# 실습 문제 6: 나이대별 평균 점수 계산
학생 리스트에서 나이대별(10대, 20대 등)로 평균 점수를 계산하시오.

**예시 입력**
```java
List<Student> students = Arrays.asList(
    new Student("Alice", 14, 88),
    new Student("Bob", 23, 82),
    new Student("Charlie", 17, 95),
    new Student("David", 21, 73)
);
```

**예시 출력**
```java
10s: 91.5
20s: 77.5
```

**필요한 `Student` 클래스 (새 버전)**
```java
public class Student {
    private String name;
    private int age;
    private int score;

    public Student(String name, int age, int score) {
        this.name = name;
        this.age = age;
        this.score = score;
    }

    public int getAge() {
        return age;
    }

    public int getScore() {
        return score;
    }
}
```

## 풀이 코드
```java
import java.util.*;
import java.util.stream.Collectors;

public class Exam6 {
    public static void main(String[] args) {
        List<Student2> students = Arrays.asList(
                new Student2("Alice", 14, 88),
                new Student2("Bob", 23, 82),
                new Student2("Charlie", 17, 95),
                new Student2("David", 21, 73)
        );

        Map<Integer, Double> mapToAge = students.stream()
                .collect(Collectors.groupingBy(
                        s -> Integer.parseInt(Integer.toString(s.getAge()).charAt(0) + "0"),
                        Collectors.averagingInt(Student2::getScore)
                ));

        mapToAge.forEach((k, v) -> System.out.println(k + "s: " + v));
    }
}
```

<br>

---

<br>

# 실습 문제 7: 도시별 최고 온도 기록
여러 도시의 일일 최고 온도가 주어졌을 때, 각 도시의 최고 온도를 찾으시오.

**예시 입력**
```java
List<Temperature> temperatures = Arrays.asList(
    new Temperature("Seoul", 33),
    new Temperature("New York", 30),
    new Temperature("Seoul", 34),
    new Temperature("New York", 28)
);
```

**예시 출력**
```java
Seoul: 34
New York: 30
```

**필요한 `Temperature` 클래스**
```java
public class Temperature {
    private String city;
    private int maxTemp;

    public Temperature(String city, int maxTemp) {
        this.city = city;
        this.maxTemp = maxTemp;
    }

    public String getCity() {
        return city;
    }

    public int getMaxTemp() {
        return maxTemp;
    }
}
```

## 풀이 코드
```java
import java.util.*;
import java.util.stream.Collectors;

public class Exam7 {
    public static void main(String[] args) {
        List<Temperature> temperatures = Arrays.asList(
                new Temperature("Seoul", 33),
                new Temperature("New York", 30),
                new Temperature("Seoul", 34),
                new Temperature("New York", 28)
        );

        Map<String, Integer> mapToCity = temperatures.stream()
                .collect(Collectors.toMap(
                        Temperature::getCity,
                        Temperature::getMaxTemp,
                        Integer::max
                ));

        mapToCity.forEach((k, v) -> System.out.println(k + ": " + v));
    }
}
```
