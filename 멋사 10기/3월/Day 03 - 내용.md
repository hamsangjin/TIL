# 조건문

## 1. if

> `if` : 순차적인 흐름 안에서 조건에 따라 제어를 할 필요가 있을 때 사용한다.

### if 사용법

```java
if(조건문){
    조건문이 참일 경우 실행될 블록
}
```

```java
if(조건문){
    조건문이 참일 경우 실행될 블록
} else{
    조건문이 거짓일 경우 실행될 블록
}
```

```java
if(조건문 1){
    조건문 1이 참일 경우 실행될 블록
} else if(조건문 2){
    조건문 2이 참일 경우 실행될 블록
} else{
    모든 조건문이 거짓일 경우 실행될 블록
}
```

만약 `if문` 에 중괄호(블록)가 없는 경우 `if문` 의 다음 문장만 조건에 만족할 경우 실행된다.

```java
int a = 10;

if(a < 5)
    System.out.println("a는 10보다 큽니다."); 
    System.out.println("hello");
```

위 경우 `a`가 만족하지 않아서 아무것도 출력되지 않을 거 같지만 `hello` 는 무조건 출력된다.

- 들여쓰기를 잘못 사용한 코드

<br>

### if 사용 예시

```java
public class IfExam1 {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        if(a > b){
            System.out.println("a는 b보다 큽니다");
            System.out.println("두 번째 줄도 출력 됩니다.");
        } else if(a < b)
            System.out.println("a는 b보다 작습니다.");
        else
            System.out.println("a와 b는 같습니다.");
    }
}
```

<br>

## 2. 삼항연산자

> `삼항연산자` : 항이 3개인 연산자로 조건식이 참일 경우 반환값 1이 사용되고, 거짓일 경우 반환값 2가 사용된다.


### 삼항연산자 사용법

```java
조건식 ? 반환값1 : 반환값2
```

<br>

### 삼항연산자 사용 예시

위 `if문`과 조건문이 동일하게 작성해봤다.

```java
public class IfExam1 {
    public static void main(String[] args){
        int a = 10;
        int b = 20;
        int value = (a > b) ? a : b;

        System.out.println(value);    // 20
    }
}
```

<br>

## 3. Switch

> `switch` : 경우에 따라 `if문`보다 가독성이 좋을 수 있다.


### switch 사용법

```java
switch(변수){
    case 값1:
        변수가 값1일 때 실행
        break;
    case 값2:
        변수가 값2일 때 실행
        break;
    default:
        변수의 값이 어떤 case에도 해당되지 않을 경우 실행
}
```

- switch블록 안에는 **여러 개의 case**가 올 수 있다.
- switch블록 안에는 하나의 **default**가 올 수 있다.
- break문은 생략 가능하지만, 생략하게 되면 **변수의 값을 만족하는 case부터 아래 case까지 모두 실행**된다.

<br>

### switch 사용 예시

```java
public class SwitchExam1 {
    public static void main(String[] args) {
        int num = 2;
        switch(num){
            case 1 :
                System.out.println("1입니다.");
                break;
            case 2 :
                System.out.println("2입니다.");
                break;
            case 3 :
                System.out.println("3입니다.");
                break;
            default :
                System.out.println("1,2,3이 아닙니다.");
        }

        char ch = 'b';
        switch(ch){
            case 'a' :
                System.out.println("A입니다.");
                break;
            case 'b' :
                System.out.println("B입니다.");
                break;
            case 'c' :
                System.out.println("C입니다.");
                break;
            default :
                System.out.println("A, B, C가 아닙니다.");
        }

        String str = "고구마";
        switch(str){
            case "감자" :
                System.out.println("감자입니다.");
                break;
            case "고구마" :
                System.out.println("고구마입니다.");
                break;
            default :
                System.out.println("감자와 고구마가 아닙니다.");
        }
    }
}
```

`int`, `char` 타입의 값을 이용해 `switch문`을 작성해봤고, JDK 7이상부터 `switch`에서 `String`타입을 사용할 수 있게 되었다.

<br>

---

<br>

# 반복문

## 1. while

> `while` : 탈출 조건식이 `false`을 반환할 때 종료된다.


### while 사용법

```java
변수의 초기화
while(탈출 조건식){
    탈출 조건식이 참일 경우 실행되는 코드;
    변수의 증감식
}
```

- `while문`에서 `break`를 만나면, 더이상 반복되지 않고 종료되고, `break`는 보통 조건문 `if`와 함께 사용된다.
- `while문`에서 `continue`를 만나면, `continue` 가 작성된 이하 문장들은 실행하지 않고 다음 반복으로 넘어간다.
- `while(true)`인 경우 **무한 루프**라고 부르며, 끝없이 반복한다.

<br>

### while 사용 예시

```java
public class WhileExam1 {
    public static void main(String[] args) {
        int i = 0;
        int total = 0;
        // 1 ~ 100까지의 짝수의 합 출력
        while(i++ <= 100){
            if(i % 2 == 0)
                total += i;
            continue;
            System.out.println("생략되는 코드");
        }
        
        System.out.println(total);  // 2550
    }
}
```

<br>

## 2. do-while

> `do-while` : `while문`과 비슷하지만, 무조건 한 번은 실행된다는 특징이 있다.


### do-while 사용법

```java
변수의 초기화
do{
    무조건 한 번은 실행되며 그 이후, 탈출 조건식이 참일 경우에만 실행되는 코드;
    변수의 증감식
} while(탈출 조건식);
```

<br>

### do-while 사용 예시

```java
public class DoWhileExam1 {
    public static void main(String[] args) {
		    // 1 ~ 10의 숫자 출력
        int i = 1;
        do{
            System.out.println(i++);
        }while(i <= 10);
	
				// 처음 1만 출력
        int j = 1;
        do{
            System.out.println(j++);
        }while(i < 1);
    }
}
```

<br>

## 3. for

> `for` : `while문`은 **변수 선언, 탈출 조건식, 증감식**이 보통 3줄로 이루어 지지만, `for문`은 한 줄에 모두 표현한다.


### for 사용법

```java
for(변수의 초기화; 탈출 조건식; 증감식){
    탈출 조건식이 참인 경우 실행되는 부분
}
```

- `반복문` 안에 `조건문`이 올 수 있는 것처럼, `반복문` 안에 `반복문`이 올 수 있으며 이를 `중첩 반복문`이라고 한다.

<br>

### for 사용 예시

```java
public class forExam1 {
    public static void main(String[] args) {
		    // 1 ~ 10의 숫자 출력
        for(int i = 1; i <= 10; i++){
            System.out.println(i);
        }

				// 구구단 출력
        for(int i = 1; i < 10; i++){
            for(int j = 1; j < 10; j++){
                System.out.println(i + "*" + j + " = " + i*j);
            }
        }
        
        // 무한루프
        for(int i = 1;;){
		        if(i++ > 5)    break;
		        System.out.println("멈춰주세요 ..");
        }
    }
}
```

- 모든 기본형 타입은 문자열과 더해지면 문자열이 된다.

<br>

## 반복문과 label

### break와 continue의 한계

- `break`는 현재 반복문을 빠져나가는데 사용하며, `continue`는 continue문 아래 부분을 실행하지 않고 다시 반복한다.
- 그렇다면 **중첩반복문**을 한번에 빠져나가려면 어떻게 해야할까?
- 이럴 때 `label`을 사용한다.

<br>

### label을 사용한 중첩반복문 탈출

```java
public class LabelExam1 {
    public static void main(String[] args){
        outter:
        for(int i = 0; i< 3; i++){
            for(int k = 0; k < 3; k++){
                if( i == 0 && k == 2)
                    break outter;
                    // continue outter;
                System.out.println(i + ", " + k);
            }
        }
    }
}
```

- 위 코드는 `0, 0` , `0, 1` 만 출력되고 종료된다.
- 저기서 `break` 를 `continue`로 바꿔줘도 그대로 동작한다.
