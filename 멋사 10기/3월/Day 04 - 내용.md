# 배열

> `배열` : 참조 타입이며, 같은 타입의 변수가 여러 개 필요할 때 사용하고, `기본형 배열` 과 `참조형 배열` 로 나뉜다.

## 기본형 배열

> `boolean`, `byte`, `short`, `char`, `int`, `long`, `float`, `double` 타입의 변수를 여러 개 선언할 필요가 있을 때 사용한다.

### 기본형 배열 선언

```java
// 초기값 X
기본형타입[] 변수명;
기본형타입 변수명[];

// 초기값 O
기본형타입[] 변수명 = new 기본형타입[배열 크기];
변수명[index] = 값;

기본형타입[] 변수명 = new 기본형타입[]{값1, 값2, ....};
기본형타입[] 변수명 = {값1, 값2, ....};
```

<br>

### 기본형 배열 예시

```java
public class ArrayExam1 {
    public static void main(String[] args) {
        // 초기값 X
        int[] iarr1;
        int[] iarr2, iarr3;
        int iarr5[], iarr6;
        iarr5 = new int[3];
        // iarr6 = new int[3]; -> int형 배열이 아닌 int형으로 선언됨

        // 초기값 O
        int[] array1 = new int[5];
        for(int i = 0; i < 5; i++)  array1[i] = i+1;

        int[] array2 = new int[]{1,2,3,4,5};
        int[] array3 = {1,2,3,4,5};

        System.out.println("array1의 값 출력");
        for(int i = 0; i < 5; i++)  System.out.println(array1[i]);

        System.out.println("array2의 값 출력");
        for(int i = 0; i < 5; i++)  System.out.println(array2[i]);

        System.out.println("array3의 값 출력");
        for(int i = 0; i < 5; i++)  System.out.println(array3[i]);
    }
}
```

<br>

## 참조형 배열

> `참조형 배열` : 배열의 타입이 기본형이 아닌 경우를 말하며, 배열 변수가 참조하는 배열의 공간이 값을 저장하는 것이 아니라 **값을 참조한다는 것**을 의미

### 참조형 배열 예시

```java
public class ItemForArray {
    private int price;
    private String name;

    public ItemForArray(int price, String name) {
        this.price = price;
        this.name = name;
    }

    public int getPrice() {
        return price;
    }

    public String getName() {
        return name;
    }
}
```

```java
public class ArrayExam2 {
    public static void main(String[] args) {
        // 선언 방법 1
        ItemForArray[] arr1 = new ItemForArray[3];
        arr1[0] = new ItemForArray(500, "사과");
        arr1[1] = new ItemForArray(300, "바나나");
        arr1[2] = new ItemForArray(900, "수박");
        
        // 선언 방법 2
        ItemForArray[] arr2 = new ItemForArray[]{
                new ItemForArray(500, "사과"),
                new ItemForArray(300, "바나나"),
                new ItemForArray(900, "수박")
        };

        // 선언 방법 3
        ItemForArray[] arr3 = {
                new ItemForArray(500, "사과"),
                new ItemForArray(300, "바나나"),
                new ItemForArray(900, "수박")
        };

        // arr1 배열에 저장된 ItemForArray 인스턴스들의 Name과 Price 출력
        for(int i = 0; i < arr1.length; i++) {
            System.out.println(arr1[i].getName());
            System.out.println(arr1[i].getPrice());
        }
    }
}
```

<br>

## 배열의 길이

위에서 `arr1` 의 인스턴스들을 출력할 때 `i`의 범위를 `arr1.length`라고 지정해 `for`문을 반복했다.

```java
ItemForArray[] arr1 = new ItemForArray[3];
arr1[0] = new ItemForArray(500, "사과");
arr1[1] = new ItemForArray(300, "바나나");
arr1[2] = new ItemForArray(900, "수박");

// arr1.length : 3   
for(int i = 0; i < arr1.length; i++) {
    System.out.println(arr1[i].getName());
    System.out.println(arr1[i].getPrice());
}
```

- 배열의 모든 요소를 접근하고 싶을 때, 선언부를 매번 확인할 수 없으므로 배열이 가지고 있는 `length` 필드를 통해 배열의 길이를 반환받자.
- 배열의 길이만큼 반환받을 때, 배열의 인덱스는 0부터 시작하기에 이 점을 유의해 배열에 접근하도록 하자.

<br>

## ArrayIndexOutOfBoundsException

선언된 배열의 범위가 아닌 부분을 접근하려고 할 때 발생하는 오류이다.

```java
ItemForArray[] arr1 = new ItemForArray[3];
arr1[0] = new ItemForArray(500, "사과");
arr1[1] = new ItemForArray(300, "바나나");
arr1[2] = new ItemForArray(900, "수박");
// arr1[3] = new ItemForArray(700, "배");
```

![스크린샷 2024-03-14 10.47.56.png](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/9543a083-1881-4ac1-a199-e8d4dc608b5a/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-14_10.47.56.png?id=a0a86e1a-7cf9-4a4d-af3e-acd62c657a87&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=ciyvo8hOgsjjRKqDDNdCLfUV05VRYP3AeOFlxpMPCNs&downloadName=%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2024-03-14+10.47.56.png)

- 배열의 크기는 3이므로 인덱스는 0부터 2까지 존재하지만, 주석처리된 줄에선 `인덱스 3`에 접근하려고 시도했으므로 오류가 발생한다.

<br>

## 이차원 배열

### 이차원 배열 선언

```java
// 기본
타입[][] 변수명 = new 타입[행의수][열의수]; 
변수명[행인덱스][열인덱스] = 값;

// 가변
타입[][] 변수명 = new 타입[행의수][]; 
변수명[행의인덱스] = new 타입[열의수];

int[][] arr = new int[3][];
arr[0] = new int[20];
arr[0] = new int[19];
arr[0] = new int[21];
```

<br>

### 이차원 배열 예시

```java
public class ArrayExam3 {
    public static void main(String[] args) {
        // 기본
        int[][] iarr1 = new int[2][3];
        iarr1[0][0] = 0;
        iarr1[0][1] = 1;
        iarr1[0][2] = 2;
        iarr1[1][0] = 3;
        iarr1[1][1] = 4;
        iarr1[1][2] = 5;

        int[][] iarr2 = {{0,1,2}, {3,4,5}};

        for(int i = 0; i < iarr1.length; i++){
            for(int j = 0; j < iarr1[i].length; j++){
                System.out.print(iarr1[i][j] + "\t");
            }
            System.out.println();
        }

        System.out.println("========================");
				
        // 가변
        int[][] iarr3 = new int[2][];
        iarr3[0] = new int[2];
        iarr3[1] = new int[3];
        iarr3[0][0] = 0;
        iarr3[0][1] = 1;
        iarr3[1][0] = 2;
        iarr3[1][1] = 3;
        iarr3[1][2] = 4;

        int[][] iarr4 = {{0,1}, {2,3,4}};

        for(int i = 0; i < iarr3.length; i++){
            for(int j = 0; j < iarr3[i].length; j++){
                System.out.print(iarr3[i][j] + "\t");
            }
            System.out.println();
        }
    }
}
```

<br>

## for-each

### for-each 사용법

```java
for(타입 변수명 : 배열명){
    ...
}
```

<br>

### for-each 사용 예시

```java
public class ArrayExam4 {
    public static void main(String[] args) {
        int[] arr1 = {1,2,3,4,5};

        for(int i : arr1){
            System.out.println(i);
        }

        ItemForArray[] arr2 = new ItemForArray[3];
        arr2[0] = new ItemForArray(500, "사과");
        arr2[1] = new ItemForArray(300, "바나나");
        arr2[2] = new ItemForArray(900, "수박");

        for(ItemForArray item : arr2){
            System.out.println(item.getName());
            System.out.println(item.getPrice());
        }
    }
}
```

<br>

---

<br>

# Arrays

> `Arrays` : 배열을 다룰 때 사용하는 유틸리티


## Arrays.copyOf()

> `Arrays.copyOf(복사 대상 배열, 복사할 길이)` : 특정 배열을 원하는 길이만큼 복사할 수 있는 메소드이며, 배열의 길이보다 긴 경우 배열의 타입 기본값으로 설정되어 복사된다.
> 

```java
import java.util.Arrays;

public class ArraysExam1 {
    public static void main(String[] args) {
        int[] copyFrom = {1,2,3};
        int[] copyTo = Arrays.copyOf(copyFrom, copyFrom.length);

        // 1 2 3 출력
        for(int c : copyTo){
            System.out.println(c);
        }

        System.out.println("----------------------------------");

        int[] copyTo2 = Arrays.copyOf(copyFrom, 5);
				
        // 1 2 3 0 0 출력
        for(int c : copyTo2){
            System.out.println(c);
        }
    }
}
```

<br>

## Arrays.copyOfRange()

> `Arrays.copyOfRange(복사 대상 배열, 복사 시작 인덱스, 복사 끝 인덱스)` : 특정 배열의 특정부분만 복사할 수 있는 메소드


```java
import java.util.Arrays;

public class ArraysExam2 {
    public static void main(String[] args) {
        char[] copyFrom = {'h', 'e', 'l', 'l', 'o', '!'};
        char[] copyTo = Arrays.copyOfRange(copyFrom, 1, 3);

        // e l 출력
        for(char c : copyTo){
            System.out.println(c);
        }
    }
}
```

<br>

## Arrays.compare()

> `Arrays.compare(비교 배열 1, 비교 배열 2)` : 두 개의 배열이 같은지 다른지 비교하고, 같다면 `0`, 다르다면 `1`을 반환해주는 메소드

```java
import java.util.Arrays;

public class ArraysExam3 {
    public static void main(String[] args) {
        int[] array1 = {1,2,3,4,5};
        int[] array2 = {1,2,3,4,5};
        int compare1 = Arrays.compare(array1, array2);
        System.out.println(compare1);  // 0 출력

        int[] array3 = {1,2,3,4,6};
        int[] array4 = {1,2,3,4,4};
        int compare2 = Arrays.compare(array3, array4);
        System.out.println(compare2);  // 1 출력
    }
}
```

<br>

## Arrays.sort()

> `Arrays.sort()` : 배열의 요소들을 오름차순으로 정렬해주는 메소드


```java
public class ArraysExam4 {
    public static void main(String[] args) {
        int[] array = {5,4,3,1,2};
        Arrays.sort(array);
        
        // 1 2 3 4 5 출력
        for(int i : array){
            System.out.println(i);
        }
    } 
}
```

<br>

## Arrays.binarySearch()

> `Arrays.binarySearch(정렬이 된 배열, key)` : 이미 정렬이 된 배열에서 **이진 탐색 알고리즘**을 통해 `key` 값의 인덱스를 반환해주고, `key`값이 존재하지 않다면 음수를 반환해주는 메소드


```java
import java.util.Arrays;

public class ArraysExam5 {
    public static void main(String[] args) {
        int[] array = {5,4,3,1,2};
        Arrays.sort(array);

        int i = Arrays.binarySearch(array, 4);

        System.out.println(i); // 3 출력
    }
}
```

- 음수 반환 규칙

![Untitled](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/7abf55e7-f051-4d16-9957-fd9d16e0481c/Untitled.png?id=f5066997-e0ce-4734-860d-e35ba08c7254&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=bH47_6KF6VsB8UKi9r06iHsShSP5Yur1Wix3Tc5RDmY&downloadName=Untitled.png)

즉, `N번째 요소 뒤에 위치하는 key`는 `-N` 으로 반환된다고 생각하면 된다. 

배열에 없는 `12`를 탐색하게 된다면 `11` 보단 크고, `15` 보단 작으며, `15`는 배열에서 `6`번째 값이기 때문에  **`-6`** 이라는 값이 반환된다.

<br>

---

<br>

# Arguments

## 명령 행 아규먼트(Command-Line Arguments)

- 우리가 흔하게 접하는 명령 행 아규먼트는 `main 메소드`에 있는 `String[] args` 이다.
- `main 메소드`는 JVM이 실행하는 메소드이며, `JVM`이 `main 메소드`를 실행할 때 `String[]`을 `Arguments`로 넘겨 준다는 것을 의미한다.

### 명령 행 아규먼트 예시

```java
public class CommandLineArgumentExam {
    public static void main(String[] args){
        System.out.println(args.length);
        
        if(args.length == 0){
            System.out.println("사용법 : CommandLineArgumentExam 값 값 ...."); 
            System.exit(0); // return; 으로 변겯 가능
        }
				
        // args의 길이가 0이므로 System.exit(0)이 실행되기 때문에 아래 코드는 실행 안 됨
        for(String arg : args){
          System.out.println(arg);
        }
    }
}
```

<br>

## 제한 없는 아규먼트(unlimited arguments)

- 경우에 따라서 `메소드 아규먼트`를 가변적으로 전달하고 싶은 경우가 있다.
- 예를 들어 경우에 따라서 메소드에 정수값을 3개, 어떤 경우엔 5개를 넘기고 싶다면 이때 `제한없는 아규먼트`를 사용하면 된다.

### 제한 없는 아규먼트 선언

```java
리턴타입 메소드이름(타입... 변수명){ 
    ......
}
```

<br>

### 제한 없는 아규먼트 예시

```java
public class UnlimitiedArgumentsExam {
    public static void main(String[] args){
        System.out.println(sum(5,10));
        System.out.println(sum(1,2,4,2));
        System.out.println(sum(3,1,2,3,4,1));
    }

    public static int sum(int... args) {
        System.out.println("print1 메소드 - args 길이 : " + args.length);

        int sum = 0;
        for (int i = 0; i < args.length; i++) {
            sum += args[i];
        }
        return sum;
    }
}
```

![스크린샷 2024-03-14 13.49.31.png](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/4fb374e5-a821-4f88-bbe8-a4da610b6d6f/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-14_13.49.31.png?id=91d944bf-be6d-4ed8-bbc2-c5e3cec9701a&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=goE-Z3FCfEIqJpNFFDBF38Qsq7qsLIpFJNz_WiQC2_w&downloadName=%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2024-03-14+13.49.31.png)

<br>

---

<br>

# Git & GitHub

## Git & Github를 익혀야하는 이유

### Git

> [git](https://ko.wikipedia.org/wiki/%EA%B9%83_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)) : 컴퓨터 파일의 **변경사항을 추적**하고 여러 명의 사용자들 간에 해당 **파일들의 작업을 조율**하기 위한 `분산 버전 관리 시스템`

- [Git 구조](https://namu.wiki/w/Git#s-3)

![Untitled](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/ffd34dce-2421-47b7-836d-5553d97890b7/Untitled.png?id=5b9c5b3c-8e24-4037-a5da-55c6b66b9e8d&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=Fqv-3I2DAHr-_5AGxa4owIF_Yi7G_YHE3i1tUxzCynw&downloadName=Untitled.png)

### GitHub

> [GitHub](https://ko.wikipedia.org/wiki/%EA%B9%83%ED%97%88%EB%B8%8C) : [루비 온 레일스](https://ko.wikipedia.org/wiki/%EB%A3%A8%EB%B9%84_%EC%98%A8_%EB%A0%88%EC%9D%BC%EC%8A%A4)로 작성된 분산 버전 관리 툴인 [깃](https://ko.wikipedia.org/wiki/%EA%B9%83_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)) 저장소 호스팅을 지원하는 웹 서비스

### 버전관리

> [버전 관리](https://ko.wikipedia.org/wiki/%EB%B2%84%EC%A0%84_%EA%B4%80%EB%A6%AC) : 일반적인 소프트웨어의 소스 코드 내역을 관리하는 것

### 프로젝트

> `프로젝트` : 2인 이상이 함께 어떤 주제를 정해진 기간 안에 만들어야 하는 것

다수의 사람이 프로젝트를 함께 한다면, 소스 코드를 어떻게 관리하는 것이 좋을까 ?

바로 이런 버전 관리 시스템을 이용하면 편리하다 !

<br>

## Git 설치

~~이미 `git` 이 설치되어 있으므로 생략~~

<br>

## Visual Studio Code 설치

~~이미 Visual Studio Code가 설치되어 있으므로 생략~~

<br>

## Fork 설치하기

해당 [링크](https://git-fork.com/)에서 다운로드 후 설치한다.

![스크린샷 2024-03-14 14.10.42.png](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/3c8b3dad-a06b-4dc5-8fc5-4bae8c76f905/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-14_14.10.42.png?id=d43114b4-fa43-481c-87a9-94868662b5b1&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=afrMsBKoQgIttBN4Ak-gDRqImBTYq4klTjrvX7Zt4DI&downloadName=%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2024-03-14+14.10.42.png)

<br>

## GitHub 가입

~~이미 GitHub에 가입되어 있으므로 생략~~

<br>

## Git 설정

### git 사용자 이름, 이메일 설정

```php
git config --global user.name "사용자 이름"
git config --global user.email "이메일"
```

<br>

### 사용자 이름, 이메일 확인

```php
git config user.name
git config user.email
```

![스크린샷 2024-03-14 14.20.11.png](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/9c6e3920-5c4b-4a63-b520-4c64d322413e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-14_14.20.11.png?id=9b30cbf1-224b-4158-88d6-7b450edc135c&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=4PJwfq3G4gy6pUZSlXfDUA0VIlO2BxspIPL7PzEPJq8&downloadName=%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2024-03-14+14.20.11.png)

<br>

### 줄바꿈 표시 문제 해결

```php
git config —global core.autocrlf true  // Windows
git config —global core.autocrlf input // MacOS
```

<br>

## 프로젝트 폴더 생성 및 초기화

![스크린샷 2024-03-14 15.26.04.png](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/69f65ca8-0745-41e8-8418-8634824070be/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-14_15.26.04.png?id=abcd4d2f-a77d-4dbc-988d-0c68933e3ea5&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=tjpn2Be5ap2d1KvGNk8m0FtlIdaF00y5Nllpf-Eep20&downloadName=%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2024-03-14+15.26.04.png)

- `mkdir` : 디렉토리 생성
- `cd` 디렉토리 이동
- `pwd` : 현재 디렉토리 절대 경로 출력

![스크린샷 2024-03-14 15.27.28.png](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/b7a9636b-4ef3-49ad-8739-180f3eab23da/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-14_15.27.28.png?id=3e1ad429-ae8d-4fbe-a1b0-8c405586f0e9&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=fxIpapGipkYZ7UhmUF4qgd3eMyYCbjLzOjkYjg4fWVc&downloadName=%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2024-03-14+15.27.28.png)

- `git init` : 현재 폴더를 Git저장소로 생성하며, `.git` 폴더가 자동으로 생성된다.
- `ls` : 현재 디렉토리의 파일 목록을 조회하며, `-la`옵션을 통해 히든파일까지 볼 수 있다.

<br>

## git init 후 생성되는 파일 구조

- `.git` : Git 저장소의 모든 설정, 로그, 객체 데이터베이스 등을 포함하는 디렉토리이며, 이 디렉토리가 프로젝트의 Git 저장소임을 나타낸다.
- `HEAD` : 현재 체크아웃된 커밋, 브랜치, 태그의 참조를 가리키며, 주로 현재 작업중인 브랜치를 가리키는 데 사용된다.
- `config` : 이 Git 저장소의 설정을 포함하며, 사용자 이름, 이메일, 원격 저장소 주소 등 저장소 수준의 설정이 이 파일에 저장된다.
- `description` **:** GitWeb과 같은 일부 서비스에서 사용하는 저장소의 설명을 포함하며, 기본적으로는 큰 영향을 미치지 않는다.
- `hooks` : Git 훅스를 위한 스크립트 파일들이 저장되는 디렉토리이며, 특정 이벤트(예: 커밋, 푸시)가 발생할 때 자동으로 실행되도록 설정할 수 있는 스크립트들이 포함되어 있다.
- `info` : exclude 파일을 포함하여, Git에서 무시해야 할 파일 패턴을 지정한다. 이는 `.gitignore` 파일과 유사하지만, **프로젝트 전체가 아닌 로컬 저장소에만 적용**됩니다.
- `objects` : Git의 모든 데이터(커밋, 트리, 블롭 등)를 저장하는 객체 데이터베이스이며, `info`와 `pack` 디렉토리를 포함한다.
    - `info` : 객체 데이터베이스에 대한 추가 정보를 포함
    - `pack` : Git 객체를 압축하여 저장하는 팩 파일들이 위치하며, 대규모 저장소의 효율성을 높이는 데 사용된다.
- `refs` : 참조들을 저장하는 디렉토리이며, 주로 브랜치(heads), 태그(tags) 등의 참조를 포함한다.
    - `heads` : 로컬 브랜치의 최신 커밋을 가리키는 파일들이 저장된다.
    - `tags` : 태그를 통해 특정 커밋을 참조하는 파일들이 저장된다.

<br>

## 프로젝트에 파일 작성하기

![스크린샷 2024-03-14 15.46.58.png](https://file.notion.so/f/f/c69962b0-3951-485b-b10a-5bb29576bba8/e40258c8-57fb-4772-a5af-98b7fe7c1a65/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2024-03-14_15.46.58.png?id=536a32f7-981d-4b4a-a78e-92d6b5a9260a&table=block&spaceId=c69962b0-3951-485b-b10a-5bb29576bba8&expirationTimestamp=1710504000000&signature=esX3hFHBxgGYHdhULUCnCdU6DLDg9gcMxXOWqZRPdwI&downloadName=%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA+2024-03-14+15.46.58.png)

- `resources` 디렉토리에 `application.properties` 파일 생성
- `src` 디렉토리에 `Hello.java` 파일 생성

> `git status` : 현재 작업 중인 Git 저장소의 상태를 보여준다. <br>
> `git add` : 크게 `버전 관리 준비`, `변경 사항 확정` 두 동작을 하며, Git에서 파일의 변경 사항을 추적하기 시작하게 하여, 이후에 이루어질 커밋에 포함시키기 위한 준비 단계이다.

<br>

## .gitignore 파일

> `.gitignore` : Git 버전 관리 시스템에서 추적하지 않을 파일이나 폴더를 지정하는 데 사용되며, 프로젝트의 루트 디렉토리에 위치하고, 텍스트 파일 형식으로 작성된다.

- `단일 파일 지정`: 특정 파일을 직접 명시하여 무시할 수 있다.
- `와일드카드 사용`: * 와 같은 와일드카드를 사용하여 특정 패턴에 매칭되는 파일을 무시할 수 있다.
- `디렉토리 지정` : 디렉토리 이름 뒤에 `/` 를 붙여서 특정 디렉토리와 그 안의 모든 파일을 무시할 수 있다.
- `부정패턴` : `!`를 사용하여 무시 규칙에서 예외를 지정할 수 있다.
- `디렉토리 내 특정 파일 무시` : 특정 디렉토리 아래의 파일만 무시하고 싶은 경우, 경로를 명시적으로 지정할 수 있다.

<br>

## 스테이징 영역

> `스테이징 영역` : 사용자가 Git 저장소에 파일의 변경사항을 최종적으로 커밋하기 전에, 변경사항을 모아 놓는 임시 저장 공간을 말한다.

### 역할

- `변경 분리` : 작업 디렉토리에서 변경된 모든 파일을 커밋하지 않고, 커밋할 변경사항을 세밀하게 선택할 수 있다.
- `커밋 준비` : 선택된 변경사항을 커밋하기 전에 검토하고, 여러 변경사항을 하나의 커밋으로 묶거나 여러 커밋으로 나누어 관리할 수 있다.
- `변경사항 확인` : 스테이징 영역에 추가된 파일의 변경사항을 `git diff --cached` 명령어를 통해 확인할 수 있으며, 커밋 전 마지막 검토 과정에서 유용하다.

<br>

### 사용 방법

1. `변경사항 스테이징` : `git add <파일명>` 명령어를 사용하여 작업 디렉토리의 변경사항을 스테이징 영역에 추가
2. `스테이징된 변경사항 확인` : `git status` 명령어를 사용하여 현재 스테이징 영역에 있는 변경사항과 작업 디렉토리의 변경사항을 확인
3. `변경사항 커밋`: `git commit` 명령어를 사용하여 스테이징 영역에 있는 변경사항을 저장소의 히스토리에 기록하며, 커밋 메시지를 함께 지정하여 변경사항에 대한 설명을 추가할 수 있다.
