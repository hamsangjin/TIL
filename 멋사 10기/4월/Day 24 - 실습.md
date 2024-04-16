# 실습 예제 : 사원 관리 시스템

## 목표
- `ArrayList`와 `HashSet`을 이용하여 사원 관리 시스템을 구현합니다. 이 시스템은 사원의 정보를 추가, 삭제, 검색할 수 있어야 하며, 중복된 사원 정보는 추가되지 않도록 처리합니다.

<br>

## 과제
1. `Employee` 클래스를 생성합니다. 사원은 이름(`name`), 아이디(`id`), 부서(`department`)를 속성으로 갖습니다.
2. `HashSet`을 사용하여 사원 정보를 관리하는 `EmployeeManager` 클래스를 구현합니다. `addEmployee`, `removeEmployee`, `findEmployee` 메소드를 구현합니다.
3. `Employee`의 `equals`와 `hashCode` 메소드를 오버라이드하여 아이디가 다르면 중복으로 간주되지 않도록 합니다.

<br>

## 예제 코드
```java
import java.util.HashSet;
import java.util.Set;

class Employee {
  
}

class EmployeeManager {
    
}

public class EmployeeManagerDemo {
    public static void main(String[] args) {
        EmployeeManager manager = new EmployeeManager();
        manager.addEmployee(new Employee("홍길동", "E01", "HR"));
        manager.addEmployee(new Employee("김철수", "E02", "Marketing"));
        manager.addEmployee(new Employee("홍길동", "E01", "HR")); // 중복 추가 시도

        manager.findEmployee("E01");
        manager.removeEmployee(new Employee("홍길동", "E01", "HR"));
        manager.findEmployee("E01");
    }
}
```

<br>

## 풀이 코드

### Employee
```java
import java.util.Objects;

public class Employee {
    private String name;
    private String id;
    private String department;

    public Employee(String name, String id, String department) {
        this.name = name;
        this.id = id;
        this.department = department;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Employee employee = (Employee) o;
        return Objects.equals(id, employee.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }

    @Override
    public String toString() {
        return "Employee{" +
                "name='" + name + '\'' +
                ", id='" + id + '\'' +
                ", department='" + department + '\'' +
                '}';
    }

    public String getName() {
        return name;
    }

    public String getId() {
        return id;
    }

    public String getDepartment() {
        return department;
    }
}
```

### EmployeeManager
```java
import java.util.HashSet;
import java.util.Set;

public class EmployeeManager {
    private Set<Employee> employees = new HashSet<>();

    public void addEmployee(Employee employee){
        if(employees.add(employee)) System.out.println("사원을 추가했습니다.");
        else System.out.println("이미 존재하는 사원입니다.");

    }

    public void removeEmployee(Employee employee){
        if(employees.remove(employee)) System.out.println("사원을 삭제했습니다.");
        else System.out.println("존재하지 않은 사원입니다.");
    }

    public void findEmployee(String id){
        for (Employee employee : employees) {
            if(employee.getId().equals(id)){
                System.out.println("검색한 사원 정보: " + employee);
                return;
            }
        }
    }
}
```

### Main
```java
public class EmployeeManagerDemo {
    public static void main(String[] args) {
        EmployeeManager manager = new EmployeeManager();
        manager.addEmployee(new Employee("홍길동", "E01", "HR"));
        manager.addEmployee(new Employee("김철수", "E02", "Marketing"));
        manager.addEmployee(new Employee("홍길동", "E01", "HR")); // 중복 추가 시도

        manager.findEmployee("E01");
        manager.removeEmployee(new Employee("홍길동", "E01", "HR"));
        manager.findEmployee("E01");
    }
}
```

<br>

---

<br>

# 실습 예제 : 도시의 인구 관리 시스템

## 목표
- 도시 이름을 키로, 해당 도시의 인구를 값으로 사용하는 Map을 구성합니다.
- 도시의 인구 정보를 추가, 수정, 삭제, 조회할 수 있는 기능을 구현합니다.

<br>

## 과제
1. `HashMap`을 사용하여 여러 도시의 이름과 인구를 관리합니다.
2. 사용자 입력을 받아 도시 인구 정보를 추가하거나 수정합니다.
3. 특정 도시의 인구를 조회하거나 전체 도시의 인구 정보를 출력합니다.
4. 도시의 인구 정보를 삭제하는 기능을 추가합니다.

<br>

## 예제 코드
```java
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

public class PopulationManager {
    //알맞게 구현해 주세요.

    public static void main(String[] args) {
 PopulationManager manager = new PopulationManager();
        Scanner scanner = new Scanner(System.in);

        manager.addOrUpdateCity("서울", 10000000);
        manager.addOrUpdateCity("부산", 3500000);

        while (true) {
            System.out.println("명령을 입력하세요 (1: 추가/수정, 2: 삭제, 3: 조회, 4: 전체 조회, 5: 종료): ");
            int command = scanner.nextInt(); // 사용자가 명령을 숫자로 입력
            if (command == EXIT) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }

            String city;
            switch (command) {
                case ADD_OR_UPDATE:
                    System.out.print("도시 이름을 입력하세요: ");
                    city = scanner.next();
                    System.out.print("인구를 입력하세요: ");
                    int population = scanner.nextInt();
                    manager.addOrUpdateCity(city, population);
                    break;
                case REMOVE:
                    System.out.print("도시 이름을 입력하세요: ");
                    city = scanner.next();
                    manager.removeCity(city);
                    break;
                case DISPLAY:
                    System.out.print("도시 이름을 입력하세요: ");
                    city = scanner.next();
                    manager.displayPopulation(city);
                    break;
                case DISPLAY_ALL:
                    manager.displayAll();
                    break;
                default:
                    System.out.println("알 수 없는 명령입니다.");
            }
        }
        scanner.close();
    }
}
```

<br>

## 풀이 코드
```java
package com.example.day24.Example.Exam2;

import java.util.HashMap;
import java.util.Scanner;

public class PopulationManager {
    HashMap<String,Integer> cityPopulation = new HashMap<>();

    public void addOrUpdateCity(String city, int population) {
        if(cityPopulation.containsKey(city)) {
            System.out.println(city + "의 인구 수를 " + population + "명으로 수정했습니다.");
        } else{
            System.out.println("입력받은 값 : [" + city + ", " + population + "] -> " + "입력된 정보의 도시를 새롭게 추가했습니다.");
        }

        cityPopulation.put(city, population);
    }

    public void removeCity(String city) {
        if(cityPopulation.containsKey(city)) {
            cityPopulation.remove(city);
            System.out.println("입력받은 값 : [" + city + "] -> " + "입력된 정보의 도시를 삭제했습니다.");
        } else{
            System.out.println("입력하신 도시는 존재하지 않습니다.");
        }
    }

    public void displayPopulation(String city) {
        if(cityPopulation.containsKey(city)) {
            System.out.println(city + "의 인구 수: " + cityPopulation.get(city) + "명");
        } else{
            System.out.println("입력하신 도시는 존재하지 않습니다.");
        }
    }

    public void displayAll(){
        for(String city : cityPopulation.keySet()) {
            System.out.println(city + "의 인구 수: " + cityPopulation.get(city) + "명");
        }
    }

    public static void main(String[] args) {
        PopulationManager manager = new PopulationManager();
        Scanner sc = new Scanner(System.in);

        manager.addOrUpdateCity("서울", 10000000);
        manager.addOrUpdateCity("부산", 3500000);

        while (true) {
            System.out.println("명령을 입력하세요 (1: 추가/수정, 2: 삭제, 3: 조회, 4: 전체 조회, 5: 종료): ");
            int command = sc.nextInt(); // 사용자가 명령을 숫자로 입력
            if (command == 5) {
                System.out.println("프로그램을 종료합니다.");
                break;
            }

            String city;
            switch (command) {
                case 1:
                    System.out.print("도시 이름을 입력하세요: ");
                    city = sc.next();
                    System.out.print("인구를 입력하세요: ");
                    int population = sc.nextInt();
                    manager.addOrUpdateCity(city, population);
                    break;
                case 2:
                    System.out.print("도시 이름을 입력하세요: ");
                    city = sc.next();
                    manager.removeCity(city);
                    break;
                case 3:
                    System.out.print("도시 이름을 입력하세요: ");
                    city = sc.next();
                    manager.displayPopulation(city);
                    break;
                case 4:
                    manager.displayAll();
                    break;
                default:
                    System.out.println("알 수 없는 명령입니다.");
            }
        }
        sc.close();
    }
}
```

<br>

---

<br>

# 실습 예제 : 도서 관리 시스템

## 목표
- 도서 정보(도서명, 저자, 출판년도)를 관리하는 시스템을 구현합니다.
- 사용자는 도서를 추가, 삭제, 검색할 수 있어야 하며, 도서 목록을 특정 기준으로 정렬하여 조회할 수 있습니다.

<br>

## 과제
1. `Book` 클래스를 생성하고, 필요한 속성과 메서드를 정의합니다.
2. 도서의 추가, 삭제, 검색 및 목록 정렬 기능을 구현합니다.

<br>

## 예제 코드
```java
import java.util.*;

class Book {
   
}

class BookManager {
   
}

public class Main {
    public static void main(String[] args) {
        BookManager manager = new BookManager();
        manager.addBook(new Book("모두의 자바", "강경미", 2015));
        manager.addBook(new Book("이거이 자바다", "신용권", 2018));
        manager.addBook(new Book("자바의 정석", "남궁성", 2013)); // 중복 추가 시도

        manager.displayBooks();
        manager.sortBooksByYear();
    }
}
```

<br>

## 풀이 코드

### Book
```java
import java.util.Objects;

public class Book {
    private String title;
    private String author;
    private int publicationYear;

    public Book(String title, String author, int publicationYear) {
        this.title = title;
        this.author = author;
        this.publicationYear = publicationYear;
    }

    public int getPublicationYear() {
        return this.publicationYear;
    }

    @Override
    public String toString() {
        return "Book{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", publicationYear=" + publicationYear +
                '}';
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Book book = (Book) o;
        return publicationYear == book.publicationYear && Objects.equals(title, book.title) && Objects.equals(author, book.author);
    }

    @Override
    public int hashCode() {
        return Objects.hash(title, author, publicationYear);
    }
}
```

### BookManager
```java
import java.util.*;

public class BookManager {
    Set<Book> books = new HashSet<>();

    public void addBook(Book book){
        if(books.add(book)) System.out.println("추가할 도서 정보 : " + book.toString() + " -> " + "입력된 정보의 도서를 새롭게 추가했습니다.");
        else  System.out.println("이미 존재하는 도서입니다.");
    }

    public void removeBook(Book book){
        if(books.remove(book)) System.out.println("삭제할 도서 정보 : " + book.toString() + " -> " + "입력된 정보의 도서를 삭제했습니다.");
        else System.out.println("해당 도서는 존재하지 않습니다.");
    }

    public void displayBooks(){
        if(books.isEmpty()) System.out.println("도서가 존재하지 않습니다.");
        else {
            for (Book book : books) {
                System.out.println(book.toString());
            }
        }
    }

    public void sortBooksByYear(){

        if(books.isEmpty()) System.out.println("도서가 존재하지 않습니다.");
        else {
            System.out.println("출판년도를 기준으로 오름차순 정렬합니다.");

            // set -> list
            ArrayList<Book> list = new ArrayList<>(books);

            // 기본타입이나 String 타입이 아니라, 사용자 정의 클래스 타입이기 때문에 직접 Comparator 인터페이스를 구현해야함
            list.sort(new Comparator<Book>() {
                @Override
                public int compare(Book o1, Book o2) {
                    // 결과가 음수인 경우 'o1'이 'o2'보다 먼저 온다는 의미
                    // 결과가 양수인 경우 'o2'가 'o1'보다 먼저 온다는 의미
                    // 결과가 0인 경우 순서는 변경 X
                    return o1.getPublicationYear() - o2.getPublicationYear();
                }
            });

            for (Book book : list) {
                System.out.println(book.toString());
            }
        }
    }

    public void reverseSortBooksByYear(){
        if(books.isEmpty()) System.out.println("도서가 존재하지 않습니다.");
        else {
            System.out.println("출판년도를 기준으로 내림차순 정렬합니다.");

            // set -> list
            ArrayList<Book> list = new ArrayList<>(books);

            list.sort(new Comparator<Book>() {
                @Override
                public int compare(Book o1, Book o2) {
                    return o2.getPublicationYear() - o1.getPublicationYear();
                }
            });

            for (Book book : list) {
                System.out.println(book.toString());
            }
        }
    }
}
```

### Main
```java
public class Main {
    public static void main(String[] args) {
        BookManager manager = new BookManager();
        manager.addBook(new Book("모두의 자바", "강경미", 2015));
        manager.addBook(new Book("이것이 자바다", "신용권", 2018));
        manager.addBook(new Book("자바의 정석", "남궁성", 2013)); // 중복 추가 시도
        manager.addBook(new Book("모두의 자바", "강경미", 2015));
        System.out.println("----------");

        manager.displayBooks();

        System.out.println("----------");

        manager.sortBooksByYear();

        System.out.println("----------");

        manager.reverseSortBooksByYear();

        System.out.println("----------");

        manager.removeBook(new Book("모두의 자바", "강경미", 2015));
        manager.displayBooks();

        manager.removeBook(new Book("모두의 자바", "강경미", 2015));
    }
}
```

<br>

---

<br>

# 실습 예제: 영화 평가 시스템

## 목표
- 영화 객체를 관리하고, 다양한 기준으로 정렬할 수 있는 시스템을 구현합니다.
- `ArrayList`를 사용하여 영화 목록을 관리하고, `Collections.sort()` 메서드를 이용하여 영화를 정렬합니다.
- `Comparable`과 `Comparator`를 구현하여 영화를 제목, 출시 연도, 평점에 따라 정렬할 수 있게 합니다.

<br>

## 과제
1. `Movie` 클래스를 생성하고, 제목, 출시 연도, 평점을 속성으로 갖습니다.
2. `Movie` 클래스에서 `Comparable` 인터페이스를 구현하여 기본적으로 제목에 따라 정렬하도록 합니다.
3. 제목, 출시 연도, 평점에 따라 정렬할 수 있도록 별도의 `Comparator`를 구현합니다.

<br>

## 예제 코드

```java
class Movie 
}

// Comparator 인터페이스 구현
class RatingComparator{
}

public class MovieManager {
    public static void main(String[] args) {
        List<Movie> movies = new ArrayList<>();
        movies.add(new Movie("The Shawshank Redemption", 1994, 9.3));
        movies.add(new Movie("The Godfather", 1972, 9.2));
        movies.add(new Movie("The Dark Knight", 2008, 9.0));

        // 제목 기준 정렬 (Comparable)
        Collections.sort(movies);
        System.out.println("Sorted by title:");
        movies.forEach(System.out::println);

        // 평점 기준 정렬 (Comparator)
        Collections.sort(movies, new RatingComparator());
        System.out.println("Sorted by rating:");
        movies.forEach(System.out::println);

        // 출시 연도 기준 정렬 (Comparator)
        Collections.sort(movies, new ReleaseYearComparator());
        System.out.println("Sorted by release year:");
        movies.forEach(System.out::println);
    }
}
```

1. `Movie` 클래스는 `Comparable` 인터페이스를 구현하여 제목에 따라 영화를 기본적으로 정렬합니다.
2. `RatingComparator`와 `ReleaseYearComparator`는 `Comparator` 인터페이스를 구현하여 평점과 출시 연도에 따라 영화를 정렬합니다.
3. `Collections.sort()` 메서드는 정렬 기준을 변경하기 위해 오버로드된 버전을 사용하여 Comparator를 인자로 받을 수 있습니다.

<br>

## 풀이 코드

### Movie
```java
public class Movie implements Comparable<Movie> {
    private String title;
    private int year;
    private double grade;

    public Movie(String title, int year, double grade) {
        this.title = title;
        this.year = year;
        this.grade = grade;
    }

    public String getTitle() {
        return this.title;
    }

    public double getGrade() {
        return grade;
    }

    public int getYear() {
        return year;
    }

    @Override
    public int compareTo(Movie o) {
        return this.title.compareTo(o.getTitle());
    }

    @Override
    public String toString() {
        return "Movie{" +
                "title='" + title + '\'' +
                ", year=" + year +
                ", grade=" + grade +
                '}';
    }
}
```

### MovieManager
```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class MovieManager {
    public static void main(String[] args) {
        List<Movie> movies = new ArrayList<>();
        movies.add(new Movie("The Shawshank Redemption", 1994, 9.3));
        movies.add(new Movie("The Godfather", 1972, 9.4));
        movies.add(new Movie("The Dark Knight", 2008, 9.0));

        // 제목 기준 정렬 (Comparable)
        Collections.sort(movies);
        System.out.println("Sorted by title:");
        movies.forEach(System.out::println);

        // 평점 기준 정렬 (Comparator)
        Collections.sort(movies, new RatingComparator());
        System.out.println("Sorted by rating:");
        movies.forEach(System.out::println);

        // 출시 연도 기준 정렬 (Comparator)
        Collections.sort(movies, new ReleaseYearComparator());
        System.out.println("Sorted by release year:");
        movies.forEach(System.out::println);
    }
}
```

### RatingComparator
```java
import java.util.Comparator;

class RatingComparator implements Comparator<Movie> {
    @Override
    public int compare(Movie o1, Movie o2) {
        return o1.getGrade() - o2.getGrade() > 0 ? 1 : o1.getGrade() - o2.getGrade() < 0 ? -1 : 0;
    }
}
```

### ReleaseYearComparator
```java
import java.util.Comparator;

class ReleaseYearComparator implements Comparator<Movie> {
    @Override
    public int compare(Movie o1, Movie o2) {
        return o1.getYear() - o2.getYear();
    }
}
```
