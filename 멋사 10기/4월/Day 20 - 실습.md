# 실습 문제: 음식 주문 처리 시스템 구현하기

## 배경
음식점에서 음식을 주문하고 처리하는 과정은 다양한 단계를 포함합니다.<br>
주문을 받고, 음식을 준비하고, 서빙하고, 결제를 처리하는 것까지 각 단계마다 다른 행동이 요구됩니다.<br>
이 실습 문제에서는 추상 클래스와 인터페이스를 활용하여 간단한 음식 주문 처리 시스템을 구현합니다.

<br>

## 목표
- `Order`라는 추상 클래스를 정의하여 음식 주문과 관련된 공통적인 특성과 메서드를 구현합니다.
- `Payment`라는 인터페이스를 정의하여 결제 방식에 따른 다양한 메서드를 선언합니다.
- `PizzaOrder`와 `BurgerOrder` 클래스를 `Order` 추상 클래스를 상속받아 구현하고, `CreditPayment`와 `CashPayment` 클래스를 `Payment` 인터페이스를 구현하여 작성합니다.

<br>

## 요구사항
1. 추상 클래스 Order 정의
- `prepareFood()`와 `serveFood()`라는 추상 메서드를 포함합니다.
- `final void completeOrder()` 메서드를 통해 주문 완료 과정을 구현합니다.
  - 이 메서드는 `prepareFood()`, `serveFood()`를 순서대로 호출하고, `"주문이 완료되었습니다!"`를 출력합니다.
2. 인터페이스 Payment 정의
- `processPayment()`메서드를 선언합니다.
3. 구체 클래스 구현
- `PizzaOrder`와 `BurgerOrder` 클래스는 `Order` 추상 클래스를 상속받아 각 메서드를 구현합니다.
- `CreditPayment`와 `CashPayment` 클래스는 `Payment` 인터페이스를 구현하여 `processPayment()` 메서드를 구현합니다.

<br>

### 답안 코드를 보지 않고, 요구사항을 분석해서 코드를 작성해 보세요. 
위의 요구 사항을 바탕으로 음식 주문 처리 시스템을 완성해보세요.<br>
`PizzaOrder`와 `BurgerOrder` 클래스를 각각 구현하고, 이들 클래스의 객체를 생성하여 `completeOrder()` 메서드를 호출해보세요.<br>
`CreditPayment`와 `CashPayment` 클래스를 구현하고, 이를 이용해 결제 과정을 시뮬레이션해보세요.<br>
다양한 주문 및 결제 조합을 통해 시스템의 유연성을 확인해보세요.

<br>

## 코드

### Order 추상 클래스
```java
public abstract class Order {
    public abstract void prepareFood();
    public abstract void serveFood();

    final void completeOrder() {
        prepareFood();
        serveFood();
        System.out.println("주문이 완료되었습니다.");
    }
}
```

### Order의 구현 클래스 - BurgerOrder
```java
public class BurgerOrder extends Order{
    @Override
    public void prepareFood() {
        System.out.println("쉿! 햄버거 만드는 중 ~");
    }

    @Override
    public void serveFood() {
        System.out.println("햄버거 나가요 ~");
    }
}
```

### Order의 구현 클래스 - PizzaOrder
```java
public class PizzaOrder extends Order{
    @Override
    public void prepareFood() {
        System.out.println("쉿! 피자 만드는 중 ~");
    }

    @Override
    public void serveFood() {
        System.out.println("피자 나가요 ~");
    }
}
```

<br>

### Payment 인터페이스
```java
public interface Payment {
    public void processPayment();
}
```

### Payment의 구현 클래스 - CashPayment
```java
public class CashPayment implements Payment{
    @Override
    public void processPayment() {
        System.out.println("현금 받았습니다 ~ 결제 되셨어요 ~~");
    }
}
```

### Payment의 구현 클래스 - CreditPayment
```java
public class CreditPayment implements Payment{
    @Override
    public void processPayment() {
        System.out.println("카드 받았습니다 ~ 결제 되셨어요 ~~");
    }
}
```

<br>

### 실행 클래스 - main
```java
public class OrderExam {
    public static void main(String[] args) {
        Order burgerOrder = new BurgerOrder();
        Order pizzaOrder = new PizzaOrder();

        Payment creditPayment = new CreditPayment();
        Payment cashPayment = new CashPayment();

        burgerOrder.completeOrder();
        creditPayment.processPayment();

        System.out.println();

        pizzaOrder.completeOrder();
        cashPayment.processPayment();
    }
}
```

### 결과 사진
<img width="326" alt="스크린샷 2024-04-09 20 05 14" src="https://github.com/hamsangjin/TIL/assets/103736614/20a367f1-0e49-422d-8eae-b1f1aaecd33f">
