# 알고리즘

> `알고리즘` : 어떤 문제를 해결하기 위한 명확한 절차나 일련의 단계

## 특징
1. `명확성` : 알고리즘의 각 단계는 명확하고 이해하기 쉬워야한다.
2. `입력` : 하나 이상의 입력이 있어야 한다.
3. `출력` : 하나 이상의 출력이 있으며, 이는 주어진 입력에 대한 해답이다.
4. `유한성` : 알고리즘은 유한한 수의 단계를 거쳐서 종료되어야 한다.
5. `효율성` : 알고리즘은 실행 시간과 사용하는 자원 측면에서 효율적이어야 한다.
6. `일반성` : 알고리즘은 특정 문제뿐만 아니라 문제의 범주에 속하는 다양한 문제를 해결할 수 있어야 한다.

<br>

알고리즘은 일상 생활의 다양한 문제를 해결하는데 사용될 수 있고, `수학적 계산`, `데이터 처리`, `자동화된 추론`, `컴퓨터 프로그래밍` 등 다양한 분야에서 중요한 역할을 한다.

<br>

---

<br>

# 세 값의 최댓값 구하기

## 문제 설명
세 개의 정수 값을 입력받아 그 중 가장 큰 값을 찾아내는 간단한 알고리즘으로, 자바에선 주로 비교 연산자를 사용해 각 값을 서로 비교하고 최대값을 결정해준다.

## 코드
```java
public int max(int a, int b, int c){
    if(a > b){
        if(a > c) return a;
        else return c;
    } else{
        if(b > c) return b;
        else return c;
    }
}
```

## 코드 설명
1. 세 개의 정수 `a`, `b`, `c`를 매개변수로 받는다.
2. `a`와 `b`를 비교한다.
  2-1. `a가 큰 경우` : `a`와 `c`를 비교해 그 중 큰 값을 리턴해준다.
  2-2. `b가 큰 경우` : `b`와 `c`를 비교해 그 중 큰 값을 리턴해준다.

<br>

---

<br>

# 숫자와 문자열 입력하기 1

## 자바에서 Scanner를 이용한 숫자와 문자열 입력하기
자바에서 사용자로부터 입력을 받는 가장 기본적인 방법 중 하나는 `Scanner` 클래스를 사용하는 것이다.
<br>
`Scanner` 클래스는 `java.util` 패키지에 포함되어 있으며, 다양한 형태의 입력을 처리할 수 있다.

1. `import java.util.Scanner` : Scanner 클래스 import
2. `Scanner sc = new Scanner();` : Scanner 객체 생성
3. `nextInt()`, `nextDouble()`, `nextLine()` 등 : 이 메소드들로 원하는 입력을 받기
4. `sc.close` : Scanner 객체 닫기

## 코드
```java
import java.util.Scanner;

public class InputExample {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        // 사용자로부터 정수 입력받기
        System.out.print("정수를 입력하세요: ");
        int number = sc.nextInt();

        // 입력 버퍼 비우기 (줄바꿈 문자 제거)
        sc.nextLine();

        // 사용자로부터 문자열 입력받기
        System.out.print("문자열을 입력하세요: ");
        String text = scanner.nextLine();

        // 입력받은 정수와 문자열 출력
        System.out.println("입력받은 정수: " + number);
        System.out.println("입력받은 문자열: " + text);

        // Scanner 객체 닫기
        sc.close();
    }
}
```

## 코드 설명
1. `Scanner scanner = new Scanner(System.in);` : 사용자로부터 입력받는 Scanner 객체 생성
2. `int number = scanner.nextInt();` : 정수를 입력 받음
3. `scanner.nextLine();` : 정수를 입력하고 줄바꿈을한 내용을 `nextLine()`을 통해 입력 버퍼를 비워준다.
4. `String text = scanner.nextLine();` : 한 줄의 문자열을 입력받음

`nextInt()` 뒤에 `nextLine()`을 바로 사용할 때는 `nextInt()`가 남긴 줄바꿈 문자를 `nextLine()` 이 읽어버리는 문제가 발생할 수 있으므로, `nextInt()` 후에 `nextLine()`을 한 번 더 호출하여 입력 버퍼를 비워주는 것이 좋다.

<br>

---

<br>

# Scanner 클래스의 메소드
1. `nextInt()` : 정수(int) 타입의 데이터를 읽어들인다.
2. `nextLong()` : 긴 정수(long) 타입의 데이터를 읽어들인다.
3. `nextFloat()` : 부동 소수점(float) 타입의 데이터를 읽어들인다.
4. `nextDouble()` : 부동 소수점(double) 타입의 데이터를 읽어들인다.
5. `nextBoolean()` : 불리언(boolean) 타입의 데이터를 읽어들인다.
6. `nextByte()` : 바이트(byte) 타입의 데이터를 읽어들인다.
7. `nextShort()` : 짧은 정수 (short) 타입의 데이터를 읽어들인다.
8. `next()` : 다음 토큰을 문자열로 읽어들이며, `공백`으로 구분된 단어를 반환한다.
9. `nextLine()` : 줄의 끝까지 문자열을 읽어들이며, 한 줄 전체를 입력으로 받으며, 줄바꿈 문자를 읽은 후에 제거한다.

<br>

---

<br>

# 세 값의 대소 관계와 중앙값

## 문제 설명
중앙값은 주어진 값들을 순서대로 배열했을 때 가운데 위치하는 값을 말하며, 세 개의 숫자가 주어졌을 때, 이들의 중앙값을 찾는 알고리즘은 비교 연산을 통해 각 숫자의 대소 관계를 판단하고, 그 결과를 바탕으로 중앙값을 결정한다.

## 코드
```java
static int med3(int a, int b, int c) {
    if (a >= b)
        if (b >= c) return b;
        else if (a <= c)    return a;
        else    return c;
    else if (a > c) return a;
    else if (b > c) return c;
    else    return b;
}
```

## 코드 설명
- a와 b 비교
  - `a가 큰 경우` : b와 c 비교
    - `b가 큰 경우` : 중앙값은 b
    - `a가 큰 경우` : a와 c를 비교해 더 작은 값이 중앙값
  - `b가 큰 경우` : a와 c 비교
    - `a가 큰 경우` : 중앙값은 a
    - `b가 큰 경우` : b와 c를 비교해 더 작은 값이 중앙값

<br>

---

<br>

# 다중 루프 - 구구단

중첩 for문을 사용한 구구단 곱셈표를 출력하는 간단한 코드를 작성해보자.

## 코드
```java
public class Multi99Table {
    public static void main(String[] args) {
        System.out.println("-------------구구단 곱셈표-------------");

        for(int i = 1; i <= 9; i++){
            for(int j = 1; j <= 9; j++)     System.out.printf("%3d ", i*j);
            System.out.println();
        }
    }
}
```

## 코드 설명
- `구구단 곱셈표 출력` : 프로그램은 먼저 `구구단 곱셈표`라는 제목을 출력하여 사용자에게 표의 내용을 알린다.
- `중첩된 for 반복문`
    - `바깥쪽 for문` :`1단부터 9단까지를 나타내는 i의 값`을 1부터 9까지 증가
    - `안쪽 for문` : 각 단에 대해 1부터 9까지 곱하는 j의 값`을 증가시키면서, i와 j의 곱을 출력
- `출력 형식 지정` : `System.out.printf()` 메소드를 사용하여 출력된 숫자가 일정한 폭(세 자리)을 가지도록 한다.
- `줄바꿈 처리` : 각 단의 곱셈이 끝날 때마다 `System.out.println()` 을 호출하여 줄바꿈을 추가해준다.

<br>

---

<br>

# 다중 루프 - 직각 이등변 삼각형 출력
사용자로부터 입력받은 단 수에 해당하는 왼쪽 아래가 직각인 이등변삼각형을 출력하는 코드를 작성해보자.

## 코드
```java
import java.util.Scanner;

public class TriangleLB {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("단 수를 입력해주세요 : ");
        int n = sc.nextInt();

        for(int i = 0; i < n; i++){
            for(int j = 0; j <= i; j++){
                System.out.print("*");
            }
            System.out.println();
        }
    }
}
```

## 코드 설명
- `양수 입력 받기` : 사용자에게 삼각형을 그리기 위한 단수를 입력받는다.
- `삼각형 그리기` : 입력받은 단수 `n`에 따라 왼쪽 아래가 직각인 이등변 삼각형을 그린다.
    - `외부 for문` : 삼각형의 단을 의미
    - `내부 for문` : 각 단에서의 별표를 출력하는 횟수를 의미
- `줄바꿈 처리` : 내부 for 반복문이 종료될 때마다 `System.out.println()` 을 호출하여 줄바꿈을 해줌

<br>

---

<br>

# 배열

## 배열 복제

```java
import java.util.Arrays;
class DuplicateArray {
    public static void main(String[] args) {
        int[] original = {10, 20, 30, 40, 50};
        int[] copied = original.clone(); // copied는 original의 복제본을 참조

        copied[2] = 0; // copied 배열의 세 번째 요소를 0으로 변경

        System.out.println("original = " + Arrays.toString(original));
        System.out.println("copied = " + Arrays.toString(copied));
    }
}
```
`original` 배열이 변경되지 않고 `copied` 배열에서만 변경이 이루어졌음을 확인할 수 있다.

- `clone()`: 배열의 복사본을 생성할 수 있는 메소드
- `Arrays.toString()` : 배열의 주소가 아닌 내용을 출력해주는 메소드

<br>

## 난수로 배열 초기값 설정

```java
import java.util.Random;
import java.util.Scanner;

class MaxOfWeightsRand {
    static int maxOf(int[] weights) {
        int max = weights[0];

        for(int i = 1; i < weights.length; i++){
            if(weights[i] > max)  max = weights[i];
        }

        return max;
    }

    public static void main(String[] args) {
        Random rand = new Random();
        Scanner sc = new Scanner(System.in);

        System.out.println("몸무게의 최댓값을 구합니다.");
        System.out.print("사람 수: ");
        int n = sc.nextInt();

        int[] weights = new int[n];

        for (int i = 0; i < n; i++){
            weights[i] = 40 + (rand.nextInt(60) + 1);
            System.out.println("weights[" + i + "] : " + weights[i]);
        }

        System.out.println("최댓값은 " + maxOf(weights) + "입니다.");
    }
}
```

- `Random` 클래스의 `nextInt(int range)` 메소드 : 예를 들어 `random.nextInt(40)`으로 작성한 경우 `0 ~ 39`의 범위로 동작하기 때문에 `+1`을 해줘야 한다.

<br>

## 배열 역순 정렬
```java

import java.util.Arrays;
import java.util.Scanner;

public class ArrayReverseOrder {
    public void swap(int[] a, int i, int j){
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }

    public void reverse(int[] a){
        int n = a.length;
        for(int i = 0; i < n / 2; i++){
            swap(a, i, n-1-i);
        }
    }

    public static void main(String[] args) {
        ArrayReverseOrder arrayReverseOrder = new ArrayReverseOrder();
        Scanner sc = new Scanner(System.in);

        System.out.print("배열의 크기 : ");
        int n = sc.nextInt();
        int[] a = new int[n];

        for(int i = 0; i < n; i++){
            System.out.print(i+1 + "번째 요소의 값 : ");
            a[i] = sc.nextInt();
        }

        System.out.println("역순 정렬 전 : " + Arrays.toString(a));
        arrayReverseOrder.reverse(a);
        System.out.println("역순 정렬 후 : " + Arrays.toString(a));
    }
}
```

- `swap()` : 배열과 바꿀 인덱스들이 주어지면 해당 배열의 인덱스끼리 교환해주는 메소드
- `reverse()` : 배열을 역순으로 정렬해주는 메소드로, 앞의 요소는 `i`로, 뒷 요소는 `n-1-i`로 swap 메소드를 호출한다.

<br>

## 기수 변환

### 분할과 나머지를 이용한 방법
1. 원래의 수를 새 기수로 나누기
2. 나눈 결과의 나머지를 저장
3. 몫이 0이 될 때까지 1과 2의 과정을 반복
4. 저장한 나머지들을 역순으로 저장하면 새로운 기수의 수가 된다.

- 코드
```java
public class DecimalToBinaryExam {
    public static void main(String[] args) {
        int decimal = 29;

        StringBuilder sb = new StringBuilder();

        // 10진수 -> 2진수
        // 몫이 0이 될 때까지 1과 2의 과정을 반복
        while (decimal > 0) {
            // 나눈 결과의 나머지 저장
            int remainder = decimal % 2;
            // 원래의 수를 새 기수로 나누기
            decimal /= 2;
            sb.append(remainder);
        }
        // 저장한 나머지들을 역순으로 출력
        System.out.println(sb.reverse());
    }
}
```
