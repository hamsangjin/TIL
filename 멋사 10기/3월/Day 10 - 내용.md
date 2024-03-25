# 객체지향 프로그래밍
> `객체지향 프로그래밍` : 컴퓨터 프로그래밍의 패러다임 중 하나로, 컴퓨터 프로그램을 명령어 목록으로 보는 시각에서 벗어나 여러 개의 독립된 단위, 즉 `객체`들의 모임으로 파악하고자 하는 것

<br>

## 클래스

클래스는 `필드`와 `메소드`를 가진다.
- `필드`: 클래스의 속성
- `메소드`: 클래스의 기능

### 클래스 선언 방법
1. 첫 문자가 문자나 `_`, `$`의 특수문자로 시작되어야 하고, 숫자로 시작할 수 없다.
2. 첫 문자가 아니라면, 문자나 `''`, `$`의 특수문자, 숫자로 구성될 수 있다.
3. 자바의 예약어는 식별자로 사용할 수 없다.
4. 자바의 식별자는 대소문자를 구분한다.
5. 식별자 길이는 제한이 없고 공백은 포함할 수 없다.

> 메소드 선언 방법은 이름이 소문자로 시작하는 걸 제외하고는 같다.

### 프로그래머들간의 관례
1. 클래스 명은 대문자로 시작한다.
2. 단어와 단어가 만날경우 2번째 단어의 시작은 대문자로 시작한다.

<br>

## 메소드

- `매개변수(parameter)`: 메소드의 정의 부분에 나열되어 있는 변수들
- `전달인자(argument)`: 메소드를 호출할 때 전달되는 실제 값

메소드는 매개변수와 반환값의 유무를 정할 수 있다.

<br>

## 예제: MathBean 클래스

### MathBean 클래스 정의
<img width="626" alt="스크린샷 2024-03-25 11 14 25" src="https://github.com/hamsangjin/TIL/assets/103736614/ca0de47b-6cf9-4b96-a7c8-affe82c5d5a0">

<br>

### MathBean 클래스 정의 코드
```java
public class MathBean {
    public void printClassName() {
        System.out.println(this.getClass().getName());
    }

    public void printNumber(int x) {
        System.out.println(x);
    }

    public int getOne() {
        return 1;
    }

    public int plus(int x, int y) {
        return x + y;
    }
}
```

<br>

### MathBean 클래스 - UML 표기법
<img width="200" alt="스크린샷 2024-03-25 11 15 03" src="https://github.com/hamsangjin/TIL/assets/103736614/9d924ea6-5fd8-4391-a69c-a7f54390c79b">
<br>

<br>

## 클래스 메소드 vs 인스턴스 메소드

- 클래스 메소드: 객체 생성이나 유틸리티 관련에서 사용될 때가 있다.
- 인스턴스 메소드: 인스턴스 별로 다르게 동작해야 한다면 사용하며, 되도록이면 인스턴스 메소드를 사용한다.

<br>

## 필드

> `필드`: 클래스가 가지는 속성을 말하며, 어떤 키워드와 함께 사용하냐에 따라서 사용방법이 달라진다.
- `클래스 필드`: static 키워드와 함께 사용되는 필드
- `인스턴스 필드`: static 키워드가 사용되지 않는 필드

### 필드 선언 방법
```java
[접근제한자] [static] [final] 타입 필드명 [= 초기값];
```
- 대괄호 안에 있는 내용은 생략이 가능하다.
- 필드명은 식별자 규칙을 다르며, 첫 번째 글자는 소문자로 시작하는 게 프로그래머 관례다.
- 타입은 기본형과 참조타입 등이 올 수 있다.
- 초기값이 없을 경우 참조형은 `null`, boolean형은 `false`, 나머지 기본형은 `0`으로 초기화된다.

### 필드 선언 코드
```java
public class ClassA {
    public static int fieldA;

    public int fieldB;

    int count;

    protected int number;

    private String address;

    public String name;

    public static void main(String[] args) {
        ClassA a = new ClassA();

        // 클래스 필드
        System.out.println(ClassA.fieldA);
        
        // 인스턴스 필드
        System.out.println(a.fieldB);
        System.out.println(a.count);
        System.out.println(a.number);
        System.out.println(a.address);
        System.out.println(a.name);
    }
}
```
