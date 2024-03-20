# 기수 변환 - 대응표 사용

## 대응표 사용 방법
주로 10진수를 16진수로 변환하거나 그 반대의 변환을 할 때 사용하며, 각 10진수 값에 대응하는 16진수 값을 표로 만들고 이를 사용해 변환을 하면 된다.

## 코드
```java
import java.util.Scanner;

public class RadixConverter {
    static int convertToRadix(int x, int radix, char[] d) {
        String digitChars = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";
        int digits = 0;

        // 기수 변환
        while(x != 0) {
            d[digits++] = digitChars.charAt(x % radix);
            x /= radix;
        }

        // 역순 정렬
        for (int i = 0; i < digits / 2; i++) {
            char temp = d[i];
            d[i] = d[digits - i - 1];
            d[digits - i - 1] = temp;
        }

        return digits;
    }

    public static void main(String[] args) {
        Scanner input = new Scanner(System.in);
        char[] result = new char[32]; // 변환 결과를 저장할 배열

        System.out.println("10진수를 다른 기수로 변환합니다.");

        System.out.print("변환할 정수(0 이상): ");
        int number = input.nextInt();

        System.out.print("변환할 기수(2~36): ");
        int radix = input.nextInt();

        int digitNum = convertToRadix(number, radix, result);

        System.out.print(number + "가 " + radix + "진수로는 ");
        for (int i = 0; i < digitNum; i++)
            System.out.print(result[i]);
        System.out.println(" 입니다.");
    }
}
```

## 코드 설명
사용자가 입력한 10진수 정수를 사용자가 지정한 기수(2진법부터 36진법까지)로 변환해준다.

- `convertToRadix()` : 실제 변환 작업을 수행하며, 변환된 숫자의 자릿수를 반환한다.
- `main()` : 변환된 결과를 문자 배열 `result`에 저장하고, 이 값을 사용하여 변환된 숫자를 출력한다.

<br>

---

<br>

# 소수

## 문제 설명
- 1,000 이하의 소수를 찾는 문제
- 효율적으로 찾기 위해 첫 번째 소수인 2를 배열에 저장한 후, 2의 배수인 값들은 소수가 아니므로, 3부터 시작하여 2씩 증가시키며 홀수만을 소수인지 아닌지 판별한다.
- 그 해당 홀수들이 소수인지 판별하기 위해, 이미 찾은 소수들로만 나눠서 떨어지지 않는 경우에만 소수로 추가해준다.

## 코드
```java
public class ListPrimes {
    public static void main(String[] args) {

        int[] primes = new int[1000];
        primes[0] = 2;
        int cnt = 1;
        for(int i = 3; i <= 1000; i+=2){
            if(isPrime(primes, i, cnt))     primes[cnt++] = i;
        }

        System.out.println("=============1000까지의 소수들=============");
        for(int i = 0; i < cnt; i++){
            System.out.printf("%d ", primes[i]);
        }
    }

    public static boolean isPrime(int[] primes, int n, int index){
        for(int i = 0; i < index; i++){
            if (n % primes[i] == 0)     return false;
        }
        return true;
    }
}
```

## 코드 설명
- 먼저 소수를 저장할 `primes` 배열을 생성해 첫 번째 소수인 `2`를 추가해준다.
- for문에서 `i`를 3부터 1000까지 2씩 증가하며 `i`를 이용해 `isPrime()` 메소드를 호출해준다.
- `isPrime()` : `i`가 현재 저장된 소수들 중 하나라도 나뉘는지 판별하고 나뉜다면 `false`, 안 나뉜다면 `true`를 반환해준다.
- 그렇게 for문이 종료되면 소수가 저장된 배열 `primes`를 출력해준다.

<br>

---

<br>

# 2차원 배열

## 문제 설명
학생 n명의 m과목 점수를 저장하는 2차원 배열을 선언하고, 총점과 평균을 출력해보자.

## 코드
```java
import java.util.Scanner;

public class TwoDimensionalArrayExample {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("학생 수를 입력해주세요 : ");
        int n = sc.nextInt();

        System.out.print("과목 수를 입력해주세요 : ");
        int m = sc.nextInt();

        int[][] scores = new int[n][m];

        for(int i = 0; i < n; i++){
            for (int j = 0; j < m; j++) {
                System.out.print(i+1 + "번째 학생의 " + (j+1) + "번째 과목 점수를 입력해주세요 : ");
                scores[i][j] = sc.nextInt();
            }
        }

        for (int i = 0; i < n; i++) {
            System.out.print((i+1) + "번째 학생의 총점은 ");
            int total = 0;
            for (int j = 0; j < m; j++) {
                total += scores[i][j];
            }
            System.out.print(total + "점이고, 평균은 " + total/m + "점입니다.\n");
        }

        sc.close();
    }
}
```

<br>

---

<br>

# 클래스
> `클래스` : 객체 지향 프로그래밍의 핵심 구성 요소로, 데이터(속성)와 이 데이터를 조작할 수 있는 메소드를 포함하는 틀을 얘기하며, 코드의 재사용성, 확장성 및 관리 용이성을 향상 시킬 수 있다.
> `객체` : 클래스에 정의된 속성과 동작을 가지며, 클래스에서 생성된다.

<br>

## 클래스 정의 방법
자바에서 클래스를 정의하는 기본 문법은 아래와 같다.
```java
public class ClassName{
    // 필드
    // 메소드
}
```
여기서 `ClassName`은 클래스 이름으로, 자바의 네이밍 컨벤션에 따라 각 단어의 **첫 글자는 대문자**로 시작한다.

<br>

## 구성 요소
1. `필드` : 클래스 내에서 정의된 변수로, 객체의 상태를 나타내며, 객체의 속성, 특성을 저장하기 위해 사용된다.
2. `메소드` : 객체가 수행할 수 있는 동작을 정의한 코드 블록을 의미하며, 특정 작업을 수행하고, 결과를 반환하거나, 단순 작업을 수행할 수 있다.
3. `생성자` : 클래스 이름과 동일하며, 객체가 생성될 때 자동으로 호출되는 특별한 메소드로, 객체 초기화에 사용되며, 명시적으로 생성자를 정의하지 않으면 기본 생성자를 제공한다.

<br>

## 간단한 자바 클래스

### 클래스 생성
```java
class Dog { // 필드
    String breed;
    int age;
    String color;
    // 메서드
    void bark() {
        System.out.println("Woof!");
    }
    void displayInfo() {
        System.out.println("Breed: " + breed + " Age: " + age + " Color: " + color);
    }
}
```

- `Dog` 클래스는 세 개의 필드 `breed`, `age`, `color`와 두 개의 메소드 `bark()`, `displayInfo()`를 가지고 있다.
  - `bark()` : 개가 짖는 소리를 나타내는 메소드
  - `displayInfo()` : 개의 정보를 출력해주는 메소드

### 객체 생성 및 사용
```java
public class DogTest {
    public static void main(String[] args) {
        Dog myDog = new Dog();
        myDog.breed = "Labrador";
        myDog.age = 5;
        myDog.color = "Brown";
        myDog.bark();
        myDog.displayInfo();
    }
}
```
- `Dog 클래스`의 객체를 생성하고, 필드에 값을 할당한 다음, 메소드를 호출한다.
