# 배열 실습 1

## 문제 1. **기본형 배열 선언과 초기화**

- `int` 타입의 배열 `numbers`를 선언하고, 크기가 10인 배열로 초기화하세요.
- 배열의 모든 요소를 0부터 9까지의 숫자로 초기화하는 코드를 작성하세요.

```java
public class Practice1 {
    public static void main(String[] args) {
        int[] numbers = new int[10];
        for (int i = 0; i < numbers.length; i++) {
            numbers[i] = i;
        }
    }
}
```

<br>

## **문제 2. 배열의 값 출력**

- `double` 타입의 배열 `doubles`가 다음과 같이 초기화되어 있습니다.

```java
double[] doubles = {1.0, 2.5, 3.7, 4.4};
```

- 배열의 모든 요소를 출력하는 코드를 for 루프를 사용하여 작성하세요.

```java
public class Practice2 {
    public static void main(String[] args) {
        double[] doubles = {1.0, 2.5, 3.7, 4.4};

        for(double n : doubles){
            System.out.println(n);
        }
    }
}
```

<br>

## **문제 3. 배열의 길이 구하기**

- `String` 타입의 배열 `words`가 다음과 같이 초기화되어 있습니다.

```java
String[] words = {"Hello", "World", "Java", "Programming"};
```

- 배열 `words`의 길이를 출력하는 코드를 작성하세요.

```java
public class Practice3 {
    public static void main(String[] args) {
        String[] words = {"Hello", "World", "Java", "Programming"};
        System.out.println(words.length);
    }
}
```

<br>

## **문제 4. for-each 문을 사용한 배열 요소 출력**

- `int` 타입의 배열 `numbers`가 다음과 같이 초기화되어 있습니다.

```java
int[] numbers = {5, 10, 15, 20, 25};
```

- for-each 문을 사용하여 배열 `numbers`의 모든 요소를 출력하는 코드를 작성하세요.

```java
public class Practice4 {
    public static void main(String[] args) {
        int[] numbers = {5, 10, 15, 20, 25};
        for(int num : numbers){
            System.out.println(num);
        }
    }
}
```

<br>

## **문제 5: 이차원 배열의 선언, 초기화 및 출력**

- `int` 타입의 이차원 배열 `matrix`를 선언하고, 다음과 같은 형태로 초기화하세요.

```java
1 2 3
4 5 6
7 8 9
```

- 이차원 배열 `matrix`의 모든 요소를 for 루프를 사용하여 출력하는 코드를 작성하세요. 출력 형태는 위와 같아야 합니다.

```java
public class Practice5 {
    public static void main(String[] args) {
        int[][] matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};

        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                System.out.print(matrix[i][j] + " ");
            }
            System.out.println();
        }
    }
}
```

<br>

---

<br>

# 배열 실습 2

## **문제 1. 배열 역순 출력**

- `int` 타입의 배열 `numbers`가 주어졌을 때, 배열의 요소를 역순으로 출력하는 코드를 작성하세요.
    
```java
int[] numbers = {3, 6, 9, 12, 15};
```

```java
public class Practice6 {
    public static void main(String[] args) {
        int[] numbers = {3, 6, 9, 12, 15};
        for(int i = numbers.length-1; i >= 0; i--){
            System.out.println(numbers[i]);
        }
    }
}
```

<br>

## **문제 2. 최대값과 최소값 찾기**
    
- `double` 타입의 배열 `doubles`에서 최대값과 최소값을 찾아 출력하는 코드를 작성하세요.
    
```java
double[] doubles = {1.5, 3.7, 2.4, 9.8, 7.6, 3.4};
```

```java
public class Practice7 {
    public static void main(String[] args) {
        double[] doubles = {1.5, 3.7, 2.4, 9.8, 7.6, 3.4};

        double min_num = Double.MAX_VALUE;
        double max_num = Double.MIN_VALUE;

        for(double n : doubles){
            if(min_num > n)     min_num = n;
            if(max_num < n)     max_num = n;
        }

        System.out.println("최대값 = " + max_num);
        System.out.println("최소값 = " + min_num);
    }
}
```

<br>

## **문제 3. 배열의 숫자 합계와 평균**

- `int` 타입의 배열 `scores`에 저장된 모든 숫자의 합계와 평균을 계산하는 코드를 작성하세요.

```java
int[] scores = {70, 85, 90, 45, 100};
```

- 평균은 소수점 두 자리까지 출력하세요.

```java
public class Practice8 {
    public static void main(String[] args) {
        int[] scores = {70, 85, 90, 45, 100};

        int total = 0;
        for(int s : scores){
            total += s;
        }

        System.out.printf("합계 = %d, 평균 = %.2f", total, (double)(total/scores.length));
    }
}
```

<br>

## **문제 4. 배열 요소의 이동**

- `String` 타입의 배열 `words`가 있을 때, 모든 요소를 한 칸씩 오른쪽으로 이동시키는 코드를 작성하세요.

```java
String[] words = {"Java", "Python", "C", "JavaScript"};
```

- 마지막 요소는 배열의 첫 번째 요소로 이동해야 합니다.
- 출력 예시: `{"JavaScript", "Java", "Python", "C"}`

```java
public class Practice9 {
    public static void main(String[] args) {
        String[] words = {"Java", "Python", "C", "JavaScript"};
        int n = words.length;
        
        // 마지막 요소 저장
        String temp = words[n-1];
        
        // 마지막 요소를 제외하고 나머지 요소 오른쪽으로 이동
        for(int i=n-1; i>=1; i--) {
            words[i] = words[i-1];
        }
        
        // 마지막 요소 첫 번째 요소로 변경
        words[0] = temp;

        // ["JavaScript", "Java", "Python", "C"]
        System.out.println(Arrays.toString(words));
    }
}
```

<br>

## **문제 5. 두 배열의 합집합 구하기**

- `int` 타입의 두 배열 `array1`과 `array2`가 주어졌을 때, 두 배열의 합집합을 구하여 새 배열에 저장하고, 결과 배열을 출력하는 코드를 작성하세요.

```java
int[] array1 = {1, 3, 5, 7, 9};
int[] array2 = {0, 2, 4, 6, 8, 10, 3, 5};
```

- 합집합 배열에는 중복된 요소가 없어야 합니다.

```java
import java.util.Arrays;

public class Practice10 {
    public static void main(String[] args) {
        int[] array1 = {1, 3, 5, 7, 9};
        int[] array2 = {0, 2, 4, 6, 8, 10, 3, 5};

        int[] temp = new int[array1.length + array2.length];
        int index = 0;

        // array1 복사
        for (int num : array1) {
            temp[index++] = num;
        }

        // array1와 중복된 요소는 포함하지 않고 array2 복사
        for (int num : array2) {
            boolean flag = false;
            for (int i = 0; i < index; i++) {
                if (temp[i] == num) {
                    flag = true;
                    break;
                }
            }
            if (!flag) {
                temp[index++] = num;
            }
        }

        // 중복된 요소가 제거된 크기의 배열 생성해 복사
        int[] result = new int[index];
        for(int i = 0; i < index; i++){
            result[i] = temp[i];
        }

        // 보기 좋게 정렬
        Arrays.sort(result);
        
        // 출력
        // [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
        System.out.println(Arrays.toString(result));

    }
}
```

<br>

## **문제 6. 이차원 배열에서의 대각선 요소 합계**

- `int` 타입의 이차원 배열 `matrix`가 주어졌을 때, 두 대각선의 요소 합계를 구하는 코드를 작성하세요.

```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

- 배열은 정사각형 배열이라고 가정합니다.
- 대각선 요소의 합계를 각각 구하고, 그 결과를 출력하세요.

```java
public class Practice11 {
    public static void main(String[] args) {
        int[][] matrix = {
                {1, 2, 3},
                {4, 5, 6},
                {7, 8, 9}
        };

        // 왼쪽 대각선 : i == j
        // 오른쪽 대각선 : i + y == matrix.length - 1
        int total = 0;
        for (int i = 0; i < matrix.length; i++) {
            for (int j = 0; j < matrix[i].length; j++) {
                if(i == j || i+j == matrix.length-1){
                    total += matrix[i][j];
                }
            }
        }
        
        System.out.println(total);
    }
}
```
