# Enum

> `Enum` : Enumeration의 약자로 JDK 5부터 지원하는 기능


## 상수 사용 시의 문제점

JDK 5 이전에 어떤 상수들을 표현할 때 사용하는 방법

```java
package com.example.enumtype;

public class DayType {
    public final static int SUNDAY = 0;
    public final static int MONDAY = 1;
    public final static int TUESDAY = 2;
    public final static int WEDNESDAY = 3;
    public final static int THURSDAY = 4;
    public final static int FRIDAY = 5;
    public final static int SATURDA = 6;
}
```

`DayType` 클래스는 `final static int`로 정의된 상수를 6개 가지고 있다.

```java
int today = DayType.SUNDAY;

if(today == DayType.SUNDAY){
		System.out.println("일요일 입니다.");
}
```

`today` 는 `SUNDAY` 상수 값을 가지게 되어 `0`이라는 값을 가지게되며, `SUNDAY`인지 검사할 수 있다.

하지만 요일을 나타내는 `today` 가 int형이기 때문에 아무 정수값이 들어갈 수 있어 0~6의 요일값만 들어간다고 보장할 수 없다.

<br>

## Enum 사용하기

먼저 클래스를 생성하는 것처럼 Enum을 생성하자.

```java
public enum Day {
    SUNDAY,
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
}
```

이렇게 Enum 블록 안에 상수를 나타내는 값을 적으며, 보통 대문자로 표현하고, `컴마(,)`로 구분한다.

이제 `Enum` 타입을 필드로 가지는 `Today` 클래스를 다음과 같이 작성한다.

```java
public class Today {
    private Day day;
    
    public Day getDay() {
				return day; 
		}
		
    public void setDay(Day day) {
        this.day = day;
		} 
}
```

이제 `Today`를 사용하는 `TodayTest` 클래스를 다음과 같이 작성한다.

```java
public class TodayTest {
    public static void main(String[] args){
        Today today = new Today();
        today.setDay(Day.SUNDAY);
        // today.setDay(100); -> 오류
        System.out.println(today.getDay());  // SUNDAY
		} 
}
```

<br>

## Enum 타입의 특징

- `Enum`은 `switch` 문에서 사용 가능하며, JDK 7이상부터는 `switch`문에서 `String`도 사용가능하다.

`Enum`을 이용한 `switch`문이 작성되어 있는 클래스를 살펴보자.

```java

public class DaySwitchTest {
    public static void main(String[] args){
        Day day = Day.SUNDAY;
        switch(day){
            case SUNDAY :
								System.out.println("일요일입니다.");
                break;
            case MONDAY :
								System.out.println("월요일입니다.");
                break;
            default :
		            System.out.println("그 밖의 요일");
				} 
		}
}
```

- `day` 가 어떤 상수냐에 따라서 알맞은 `case` 부분이 실행된다.

<br>

---

<br>

# 변수명 규칙

- 하나 이상의 글자로 이루어져야 한다.
- 첫 번째 글자는 문자 이거나 `$`, `_`이어야 한다.
- 두번재 이후의 글자는 `숫자`, `문자`, `$`, `_`이어야 한다.
- `$`, `_` 이외의 특수문자 사용은 불가능하다.
- 길이 제한이 없다.
- 키워드는 변수명으로 사용할 수 없다.
- 상수 값을 표현하는 단어 `true`, `false`, `null`은 변수명으로 사용할 수 없다.

<br>

---

<br>

# 타입 변환

## 명시적 형변환

실수 값을 정수 타입의 변수에 저장하려면, 실수값 앞에 `(int)`를 붙여, 정수타입으로 형변환을 해야한다. 

이때, 소수점 이하 부분은 잘리며, 이를 `명시적 형변환(강제 타입 변환, explicit conversion)`이라고 한다.

```java
int i1 = (int)50.0;
int i2 = (int)25.6f;
```

<br>

## 묵시적 형변환

> 크기가 큰 타입은 작은 타입을 저장할 수 있다**.**
> 
- `long` 타입의 변수는 `byte`, `short`, `int` 타입의 값을 저장할 수 있다.
- `int` 타입의 변수는 `byte`, `short` 타입의 값을 저장할 수 있다.
- `short` 타입의 변수는 `byte` 타입의 값을 저장할 수 있다.
- **`정수형의 자동 타입 변환` : byte형 → short형 → int형 → long형 → float형 → double형**

```java
short s = 5;
int i = s;
long x = i;

System.out.println(i);
System.out.println(x);
```
