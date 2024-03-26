# 상속
> `상속`: "A는 B다." 또는 "A는 B의 종류 중 하나다." 라고 표현할수 있다면 상속관계라고 말할 수 있으며, 상속 관계는 `is a` 관계 또는 `kind of` 관계라고 말하기도 한다.

<br>

## A는 B의 종류 중 하나다.
- 일반화 시킨다.
<img width="382" alt="스크린샷 2024-03-26 09 19 57" src="https://github.com/hamsangjin/TIL/assets/103736614/7d552751-4a2a-44e2-ab47-1b28f963c503">

<br>
<br>

## 상속 = 일반화 + 확장
`상속`이란 `일반화`와 `확장`이라는 개념을 합한 것으로 생각하면 된다. <br>
부모클래스를 상속받는다는 것은 부모가 가지고 있는 것을 자식이 물려받아 사용할 수 있다는 것을 의미한다.

<br>

## 상속 선언 방법
```java
[접근제한자] [abstract | final] class 클래스명 extends 부모클래스명{
}
```

아무것도 상속받지 않는다면, 모든 클래스는 `Object`의 자손이므로 자동으로 `java.lang.Object`를 상속받는다.

<br>

## 부모타입으로 자손 타입 참조

```java
Parent parent = new Child();
Object obj1 = new Child();
Object obj2 = new Parent();

Car car = new Bus();
```

<br>

## 다형성 - 메소드 오버라이딩
> `오버라이딩(Overriding)`: 상위 클래스의 메소드를 하위 클래스가 재정의하는 것으로, 메소드의 이름과 파라미터의 개수 및 타입도 동일해야 한다.

```java
public class Parent{
  int i = 5;
  void run(){
    System.out.println("부모의 run 메소드");
  }
}

public class Child extends Parent{
    int i = 10;
    @Override
    void run() {
        System.out.println("자식의 run 메소드");
    }

    public static void main(String[] args) {
        Parent parent = new Child();

        parent.run();            // "자식의 run 메소드" 출력
        System.out.println(p.i)  // 10 출력
    }
}
```

- 필드는 `type`을 따라가고, 메소드는 `오버라이딩된 자식 메소드`가 실행된다.

<br>

## getter, setter
`정보은닉`은 객체지향의 중요한 기법이다. 중요한 필드는 은닉하고, 필드는 메소드를 통해서만 접근해서 사용하도록 한다.

```java
public class Car {
    private String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}
```

<br>

---

<br>

# 생성자
> `생성자`: 인스턴스를 생성할 때 사용하며, 어떤 값을 가지고 인스턴스가 만들어지게 하고 싶다면 생성자를 사용하고, 생성자를 작성하지 않았더라도 자동으로 기본 생성자가 생성된다.
> - `기본 생성자`: 매개변수를 하나도 받지 않는 생성자

<br>

## 생성자 오버로딩
생성자는 매개변수의 개수가 다르거나, 타입이 다르다면 여러 개를 가질 수 있다.

<br>

## this
`this()`는 인스턴스 자기 자신을 참조할 때 사용하는 키워드로, 자기 자신의 생성자를 말하고, 생성자 안에서만 사용가능하다. <br>
또한, 생성자 안에서 `super()` 생성자를 호출하는 코드 다음이나 첫 번째 줄에 위치해야한다.

```java
public class Car {
    private String name;
    private int speed;

    public Car(int speed, String name) {
        this.speed = speed;
        this.name = name;
    }
    
    public Car(String name) {
        this(0, name);
    }

    public Car(int speed) {
        this(speed, "K3");
    }
}
```

<br>

## super
`super()`는 인스턴스 부모를 참조할 때 사용하는 키워드로, 부모 생성자를 말하고, 생성자 안에서만 사용가능하다. <br>
첫 번째 줄에만 올 수 있으며, 생성자는 무조건 `super()` 생성자를 호출해야하며 호출하지 않았다면, 자동으로 부모의 기본 생성자가 호출된다. <br>
만약 부모 클래스가 기본 생성자를 가지고 있지 않다면, 사용자는 반드시 직접 `super()` 생성자를 호출하는 코드를 작성해야한다.

- Car
```java
package com.example.day11;

public class Car {
    private String name;
    private int speed;

    // 생성자 목록
    public Car(int speed, String name) {
        this.speed = speed;
        this.name = name;
        System.out.println("Car(int speed, String name)");
    }

    public Car(String name) {
        this(0, name);
        System.out.println("Car(String name)");
    }

    public Car(int speed) {
        this(speed, "K3");
        System.out.println("Car(int speed)");
    }

    public Car() {
        System.out.println("Car()");
    }

    // Getter, Setter
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getSpeed() {
        return speed;
    }

    public void setSpeed(int speed) {
        this.speed = speed;
    }
}
```

- Bus
```java
package com.example.day11;

public class Bus extends Car{

    public Bus(){
        // super()가 생략
        System.out.println("버스 생성자");
    }

    public Bus(String name){
        super(name);
        System.out.println("Bus(String name)");
    }
}
```

- CarTest
```java
public class CarTest {
    public static void main(String[] args) {
        Car car = new Car("벤틀리");

        System.out.println(car.getName());

        Bus bus = new Bus("붕붕");
    }
}
```

- 출력
<img width="470" alt="스크린샷 2024-03-26 14 37 32" src="https://github.com/hamsangjin/TIL/assets/103736614/163b146a-b188-485c-b644-8d0f495ba037">
