# 실습 문제 1: 기본 상속 구현하기

## 문제 설명
자동차를 모델링하는 기본 클래스 `Car`가 있습니다. <br>
이 클래스는 다음과 같은 `필드`와 `메소드`를 가지고 있습니다.

- `필드`: brand(브랜드), model(모델), year(연식)
- `메소드`: displayInfo() - 자동차의 정보를 출력

`Car` 클래스를 상속받아 `ElectricCar` 클래스를 생성하고, `ElectricCar`에는 배터리 용량을 나타내는 `batteryCapacity` 필드와 이를 포함한 정보를 출력하는 `displayInfo()` 메소드를 오버라이딩하여 추가합니다.

<br>

## 요구 사항
- `Car` 클래스를 정의하고 기본 정보를 출력하는 `displayInfo()` 메소드 구현
- `Car` 클래스를 상속받는 `ElectricCar` 클래스를 정의
- `ElectricCar` 클래스에 `batteryCapacity` 필드 추가 및 `displayInfo()` 메소드 오버라이딩
- `main` 메소드에서 `Car`와 `ElectricCar` 객체를 생성하여 각각의 `displayInfo()` 메소드를 호출하여 테스트

<br>

## main 예시
```java
public static void main(String[] args) {
    // Car 클래스의 인스턴스 생성
    Car car = new Car("Hyundai", "Sonata", 2020);
    car.displayInfo();
    
    // ElectricCar 클래스의 인스턴스 생성
    ElectricCar electricCar = new ElectricCar("Tesla", "Model S", 2021, 75);
    electricCar.displayInfo();
}
```

<br>

## 문제 해결

### Car 클래스 - 부모
```java
public class Car {
    public String brand;
    public String model;
    public int year;

    public void displayInfo(){
        System.out.println("해당 모델은 " + this.brand + "의 " + this.model + " 차량으로, " + this.year + "년식입니다.");
    }

    public Car(String brand, String model, int year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
    }

    public Car() {}
}
```

### ElectricCar 클래스
```java
public class ElectricCar extends Car{
    public int batteryCapacity;

    @Override
    public void displayInfo() {
        System.out.println("해당 모델은 " + this.brand + "의 " + this.model + " 차량으로, " + this.year + "년식이며, 배터리 용량은 " + this.batteryCapacity + "입니다.");
    }

    public ElectricCar(String brand, String model, int year, int batteryCapacity) {
        super(brand, model, year);
        this.batteryCapacity = batteryCapacity;
    }
}
```

### 메인 메소드
```java
public class CarExam {
    public static void main(String[] args) {
        Car car = new Car("Hyundai", "Sonata", 2020);
        car.displayInfo();

        ElectricCar electricCar = new ElectricCar("Tesla", "Model S", 2021, 75);
        electricCar.displayInfo();
    }
}
```

### 출력 결과
<img width="619" alt="스크린샷 2024-03-26 15 30 00" src="https://github.com/hamsangjin/TIL/assets/103736614/6dfe3e73-18fa-434d-9fd7-3a680eedf877">

<br>

---

<br>

# 실습 문제 2: 다형성과 메서드 오버라이딩 활용하기

## 문제 설명
동물을 모델링하는 `Animal` 클래스가 있습니다. <br>
이 클래스에는 동물이 내는 소리를 출력하는 `makeSound()` 메소드가 있습니다.  <br>
`Animal` 클래스를 상속받는 여러 서브 클래스(예: Dog, Cat)를 만들고, 각 서브 클래스에서 `makeSound()` 메소드를 오버라이딩하여 해당 동물의 소리를 출력합니다.

<br>

## 요구 사항
- `Animal` 클래스와 `makeSound()` 메소드 정의
- Animal 클래스를 상속받는 `Dog`, `Cat` 등의 클래스 생성
- 각 서브 클래스에서 `makeSound()` 메소드 오버라이딩
- `main` 메소드에서 다형성을 활용해 `Animal` 타입의 배열을 생성하고, 각 동물의 `makeSound()` 메소드를 호출하여 테스트

<br>

## main 예시
```java
public static void main(String[] args) {
    // 다형성을 활용하여 Animal 타입의 배열 생성
    Animal[] animals = new Animal[3];
    animals[0] = new Dog();
    animals[1] = new Cat();
    animals[2] = new Animal(); // 기본 Animal 인스턴스도 포함
    
    // 각 동물의 makeSound() 메서드 호출
    for(Animal animal : animals) {
        animal.makeSound();
    }
}
```

<br>

## 문제 해결

### Animal 클래스 - 부모
```java
public class Animal {
    public void makeSound(){
        System.out.println("동물이 내는 소리");
    }
}
```

### Dog 클래스 - 자식
```java
public class Dog extends Animal{
    @Override
    public void makeSound() {
        System.out.println("멍멍");
    }
}
```

### Cat 클래스 - 자식
```java
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("냐옹냐옹");
    }
}
```

### Duck 클래스 - 자식
```java
public class Duck extends Animal {
    @Override
    public void makeSound() {
        System.out.println("꽥꽥");
    }
}
```

### 메인 메소드
```java
public class AnimalExam {
    public static void main(String[] args) {
        Animal[] animals = new Animal[3];

        animals[0] = new Dog();
        animals[1] = new Cat();
        animals[2] = new Duck();

        for (int i = 0; i < animals.length; i++) {
            animals[i].makeSound();
        }
    }
}
```

### 출력 결과
<img width="524" alt="스크린샷 2024-03-26 15 20 03" src="https://github.com/hamsangjin/TIL/assets/103736614/fd3fb075-c06a-4844-8cdd-b091c3be76b5">
