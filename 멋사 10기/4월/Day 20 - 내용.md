# Object가 오버라이딩하라고 제공하는 메소드

## 1. toString()
Object 클래스의 `toString()` 메소드는 객체의 문자 정보를 리턴하며, 여기서 객체의 문자 정보란 객체를 문자열로 표현한 값을 말한다.

### Object 클래스의 toString()
```java
public String toString() {
    // 결과: package명.클래스명@해시코드값
    return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

### String 클래스의 toString()
우리는 Object의 `toString()` 메소드를 오버라이딩해서 활용할 수 있다.

주로 `toString()` 메소드의 오버라이딩 목적은 `객체의 정보를 문자열 형태로 표현`하는 것이다.

따라서 String 클래스는 아래와 같이 `toString()` 메소드를 오버라이딩해 자기 자신의 값을 리턴하는 코드로 구현되어 있다.
```java
public String toString(){
  return this;
}
```

<br>

## 2. equals()
Object 클래스의 `equals()` 메소드는 두 객체의 값이 같은지 다른지에 대한 boolean 값을 반환해주는 메소드이다.

### Object 클래스의 equals()
```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

메모리 주소만 비교하고 값을 비교하지 않는 것을 볼 수 있다. <br>

이때, 우리는 **물리적으로 다른 메모리 주소를 갖는 객체이더라도, 논리적으로 동일한지 여부**를 반환받고 싶기 때문에, Object 클래스의 `equals()` 메소드를 오버라이딩 해서 사용할 수 있다.

### String 클래스의 equals()
```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    return (anObject instanceof String aString)
            && (!COMPACT_STRINGS || this.coder == aString.coder)
            && StringLatin1.equals(value, aString.value);
}
```
- 만약 전달받은 객체의 메모리 주소가 같다면 `true` 반환
- 그렇지 않다면 값을 비교한 뒤 `true` / `false` 반환

<br>

위와 같이 정의된 String 클래스의 `equals()` 메소드를 사용해보면 물리적으로 메모리 주소가 같지 않더라도, 값이 동일하면 `true`를 반환해주는 것을 확인할 수 있다.
```java
String strA = new String("abc");
String strB = new String("abc");

System.out.println(strA == strB);        // false
System.out.println(strA.equals(strB));   // true
```

<br>

## 3. hashCode()
Object 클래스의 `hashCode()` 메소드는 `객체 해시코드`를 반환해주며, 여기서 객체 해시코드란 `객체를 식별할 하나의 정수값`을 의미한다.<br>
또한, 객체의 메모리 주소를 이용해서 해시코드를 만들어 리턴하기 때문에 객체마다 다른 값을 가지고 있다.

### Object 클래스의 hashCode()
```java
public native int hashCode();
```
여기서 `native` 키워드는 `JNI(Java Native Interface)`라는 `native code`를 이용해 구현되었음을 의미하며,<br>
c/c++ 등 **자바가 아닌 언어로 구현**된 부분을 JNI를 통해 Java에서 이용하고자 할 때 사용한다.<br>

### equals와 hashCode의 관계
동일한 객체는 동일한 메모리 주소를 갖는다는 것을 의미하므로, 동일한 객체는 동일한 해시코드를 가져야 한다. <br>
그렇기 때문에 우리가 `equals()` 메소드를 오버라이드한다면, `hashCode()` 메소드도 함께 오버라이드되어야 한다. <br>
이러한 `equals`와 `hashCode`의 관계를 정의하면 다음과 같다.

- Java 프로그램을 실행하는 동안 equals에 사용된 정보가 수정되지 않았다면, hashCode는 항상 동일한 정수값을 반환해야 한다.
- 두 객체가 `equals()`에 의해 동일하다면, 두 객체의 `hashCode()` 값도 일치해야 한다.
- 두 객체가 `equals()`에 의해 동일하지 않다면, 두 객체의 `hashCode()` 값은 일치하지 않아도 된다.

> 출처 <br>
> [MangKyus-Diary](https://mangkyu.tistory.com/101) <br>
> [heoseungyeon.log](https://velog.io/@heoseungyeon/Object%EA%B0%9D%EC%B2%B4-%ED%83%90%EA%B5%AC%ED%95%98%EA%B8%B0-toString-equals-hashCode)

<br>

---

<br>

# 추상 클래스
`abstract` 키워드를 사용해 선언하고, 확장만을 위한 용도로 정의되는 클래스다. <br>
대부분 하나 이상의 추상 메소드를 가지고, 일반 메소드도 가질 수 있으며, **객체화할 수 없다.** <br>

<br>

## 추상 메소드
추상 메소드는 메소드에 대한 구현을 가지지 않아, 자식 클래스가 해당 메소드를 구현하도록 강요되고, 추상 클래스에만 존재할 수 있다.

<br>

## 추상 클래스의 상속
추상 클래스의 상속에도 `extends` 키워드를 사용한다. <br>
또, 추상 클래스간의 상속에서는 추상 클래스를 구현하지 않아도 되지만, 추상클래스를 상속하는 클래스는 반드시 추상 클래스의 추상 메소드를 구현해야한다.

<br>

---

<br>

# 템플릿 메소드 패턴
`템플릿 메소드 패턴`은 디자인 패턴 중 하나로, 알고리즘의 구조를 메소드에 정의하고, 알고리즘의 일부 단계를 서브클래스에서 구현하도록 하여 알고리즘의 일부 변경을 허용하는 패턴이다.

## 구조
- `추상 클래스`: 알고리즘의 단계를 정의하는 템플릿 메소드와 알고리즘의 일부를 구현하는 하나 이상의 추상 메소드로 구성
- `구체 클래스`: 추상 클래스를 상속받아 추상 메소드를 구현하면서, 알고리즘의 일부 단계를 구체화

<br>

## 예제

### 추상 클래스
```java
public abstract class BeverageRecipe {      // 템플릿 메소드
    public final void prepareRecipe() {
        boilWater();
        brew();
        pourInCup();
        addCondiments();
    }

    abstract void brew();                   // 추상 메소드
    abstract void addCondiments();          // 추상 메소드

    public void boilWater() {
        System.out.println("물을 끓입니다.");
    }

    public void pourInCup() {
      System.out.println("컵에 따릅니다.");
    }
}
```

### 구체 클래스 - Coffee
```java
public class Coffee extends BeverageRecipe {
    @Override
    void brew() {
        System.out.println("필터를 통해 커피를 우려냅니다.");
    }

    @Override
    void addCondiments() {
        System.out.println("설탕과 우유를 추가합니다.");
    }
}
```

### 구체 클래스 - Tea
```java
public class Tea extends BeverageRecipe {
    @Override
    void brew() {
        System.out.println("차를 우려냅니다.");
    }

    @Override
    void addCondiments() {
        System.out.println("레몬을 추가합니다.");
    }
}
```

### 사용 예제
```java
public class TemplateMethodPatternDemo {
    public static void main(String[] args) {
        BeverageRecipe tea = new Tea();
        tea.prepareRecipe();

        System.out.println();

        BeverageRecipe coffee = new Coffee();
        coffee.prepareRecipe();
    }
}
```

### 구현 결과
<img width="357" alt="스크린샷 2024-04-09 13 35 34" src="https://github.com/hamsangjin/TIL/assets/103736614/be8c99ee-7b4e-4952-af27-9975531ad2f4">


<br>

---

<br>

# 인터페이스

`인터페이스`는 `interface` 키워드를 이용해 선언하고, `implements` 키워드로 사용할 수 있으며, `다중 상속`이 가능하다<br>
또한, 인터페이스를 구현한 클래스에서는 추상 메소드를 반드시 정의해야한다.

<br>

## 예제

### A 인터페이스
```java
public interface Ainter {
    public void aMethod();
    public void same(int x);
}
```

### B 인터페이스
```java
public interface Binter extends Ainter{
    public void bMethod();
}
```

### C 인터페이스
```java
public interface Cinter {
    public void cMethod();
    public void same();
}
```

### D 인터페이스
```java
public interface Dinter extends Cinter, Binter{
    public void dMethod();
}
```

### D 인터페이스 구현체
```java
public class Dimpl implements Dinter{
    @Override
    public void aMethod() {
        System.out.println("amethod 구현");
    }

    @Override
    public void same(int x) {
        System.out.println("A 인터페이스 same 구현");
    }

    @Override
    public void bMethod() {
        System.out.println("bmethod 구현");

    }
    @Override
    public void cMethod() {
        System.out.println("cMethod 구현");
    }

    @Override
    public void same() {
        System.out.println("C 인터페이스 same 구현");
    }
    
    @Override
    public void dMethod() {
        System.out.println("dMehtod 구현");
    }
}
```
- `A <- B, (B, C) <- D`
- 위와 같은 식으로 상속이 이루어지고 있음
- 따라서 `D`를 상속 받으면 `D`에서 `B와 C`를 상속받고, `B`에서 `A`를 상속받는다.
- 그러므로 `Dimpl`에서는 `A, B, C, D`의 **추상 메소드를 전부 오버라이딩해서 구현**해야한다.

<br>

## default 메소드
인터페이스가 변경이 되면, 인터페이스를 구현하는 모든 클래스들이 해당 메소드를 구현해야 하는 문제가 있어, 이런 문제를 해결하기 위해 인터페이스에 메소드를 구현할 수 있게 했다. <br>

인터페이스에서 메소드를 구현할 땐 `default` 키워드로 선언하면 되고, 이를 구현하는 클래스는 `default 메소드`를 오버라이딩할 수 있다. <br>
또한, 인터페이스에 `정적(static) 메소드`도 선언해서 보통 정적 메소드처럼 사용이 가능하다.

<br>

