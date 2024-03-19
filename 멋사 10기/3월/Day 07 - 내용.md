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
