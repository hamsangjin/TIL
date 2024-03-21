# 스택

> `스택` : 마지막에 추가된 요소가 가장 먼저 제거되는 후입선출(LIFO) 방식을 따른다.

<br>

## 주요 연산
- `push` : 스택의 가장 위에 새로운 요소 추가
- `pop` : 스택의 가장 위에 있는 요소를 제거하고 그 값을 반환
- `peek/top` : 스택의 가장 위에 있는 요소 반환
- `isEmpty` : 스택이 비어있는지 확인

<br>

## 직접 구현
```java
public class StackExam {

    private int[] stack;
    private int top;
    private int maxSize;

    public StackExam(int size){
        this.maxSize = size;
        this.stack = new int[this.maxSize];
        this.top = -1;
    }

    public void push(int obj){
        if(!isFull())  stack[++top] = obj;
        else System.out.println("스택이 가득 찼습니다.");
    }

    public int pop(){
        if(!isEmpty())  return stack[top--];
        else    return 0;
    }

    public int peek(){
        if(!isEmpty())    return stack[top];
        else return 0;
    }

    public boolean isEmpty(){
        return top == -1;
    }

    public boolean isFull(){
        return top == this.maxSize-1;
    }

    public static void main(String[] args) {
        StackExam stackExam = new StackExam(5);

        stackExam.push(1);
        stackExam.push(2);
        stackExam.push(3);

        System.out.println(stackExam.pop());      // 3
        System.out.println(stackExam.peek());     // 2
        System.out.println(stackExam.isEmpty());  // false
        System.out.println(stackExam.pop());      // 2
        System.out.println(stackExam.pop());      // 1
        System.out.println(stackExam.isEmpty());  // true
    }
}
```

- `생성자` : 스택 사이즈를 매개변수로 받아서 스택 배열을 생성하고, 현재 위치를 가리키는 `top` 값 설정
- `push` : 스택의 가장 위에 새로운 요소 추가하지만, 스택이 가득 차있다면 동작하지 않는다.
- `pop` : 스택의 가장 위에 있는 요소를 제거하고 그 값을 반환하지만, 스택이 비어있다면 동작하지 않는다.
- `peek` : 스택의 가장 위에 있는 요소 반환하지만, 비어있다면 동작하지 않는다.
- `isEmpty` : 스택이 비어있는지 확인한다.
- `isFull` : 스택이 가득 차있는지 확인한다.

<br>

---

<br>

# 큐

> `큐` : 처음에 추가된 요소가 가장 먼저 제거되는 선입선출(FIFO) 방식을 따른다.

<br>

## 주요 연산
- `Enqueue` : 큐의 뒤쪽에 새로운 요소를 추가
- `Dequeue` : 큐의 앞쪽에서 요소를 제거하고 그 값을 반환
- `Peek/Front` : 큐의 맨 앞에 있는 요소를 반환
- `isEmpty` : 큐가 비어 있는지 확인

<br>

## 직접 구현 - 원형큐
```java
public class QueueExam {
    private  int[] queueArray;    // 큐
    private int front;            // 큐의 맨 앞 값의 인덱스
    private int rear;             // 큐 맨 뒤 값의 다음 인덱스
    private int capacity;         // 큐 최대용량
    private int count;            // 큐에 저장된 데이터의 수

    public QueueExam(int capacity){
        this.capacity = capacity;
        queueArray = new int[capacity];
        front=0;
        rear=0;
        count=0;
    }

    // 큐에 추가하는 메소드
    public void enqueue(int item){
        if(count == capacity){
            System.out.println("큐가 가득찼어요.");
        }else {
            queueArray[rear] = item;
            rear = (rear+1) % capacity;  // 원형 큐 알고리즘
            count++;
        }
    }
    //큐에서 item을 반환하고, 삭제해주는 메서드
    public int dequeue(){
        if(count==0){
            System.out.println("큐가 비었어요.");
            return -1;
        }else{
            int item = queueArray[front];
            front = (front+1) % capacity;  // 원형 큐 알고리즘
            count--;
            return item;
        }
    }
    public int peek(){
        if(count==0){
            System.out.println("큐가 비었어요.");
            return -1;
        }else{

            return  queueArray[front];
        }
    }
    public boolean isEmpty(){
        return count == 0;
    }

    public static void main(String[] args) {
        QueueExam queue = new QueueExam(5);

        queue.enqueue(1);
        queue.enqueue(2);
        queue.enqueue(3);

        System.out.println(queue.dequeue());//1
        System.out.println(queue.peek()); //2
        System.out.println(queue.dequeue()); //2
        queue.enqueue(4);
        System.out.println(queue.dequeue()); //3
        queue.enqueue(5);
        queue.enqueue(6);
    }
}
```

- `생성자` : 스택 사이즈를 매개변수로 받아서 스택 배열을 생성하고, 요소를 가리키는 `front`, `rear`과 요소의 개수 `capacity` 값 설정
- `push` : 스택의 가장 위에 새로운 요소 추가하지만, 스택이 가득 차있다면 동작하지 않으며, 나머지 계산을 통해 원형 큐를 구현한다.
- `pop` : 스택의 가장 위에 있는 요소를 제거하고 그 값을 반환하지만, 스택이 비어있다면 동작하지 않으며, 나머지 계산을 통해 원형 큐를 구현한다.
- `peek` : 스택의 가장 위에 있는 요소 반환하지만, 비어있다면 동작하지 않는다.
- `isEmpty` : 스택이 비어있는지 확인한다.
- `isFull` : 스택이 가득 차있는지 확인한다

<br>

---

<br>

# 재귀 알고리즘
> `재귀` : 자신을 정의할 때 자기 자신을 호출하는 방식을 의미하며, 분할 정복과 밀접한 관련이 있다.

<br>

## 재귀 함수 구성
1. `기저 조건` : 재귀 호출을 멈추고 함수가 자기 자신을 더이상 호출하지 않는 조건으로, 무한루프에 빠지지 않도록 하는 역할을 한다.
2. `재귀적 단계` : 문제의 크기를 줄이고 자기 자신을 호출하는 부분이며, 전체 문제를 더 작은 문제로 나누어 접근하게 해준다.

<br>

## 팩토리얼 구하기
```java
import java.util.Scanner;

public class FactorialExam {
    public static int fac(int n){
        if(n <= 1)  return 1;
        else return n * fac(n-1);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();

        System.out.printf("%d! = %d", n, fac(3));
    }
}
```

<br>

## 유클리드 호제법

### 2가지 방법
1. 뺄셈을 이용한 방법 : 두 수가 같아질 때까지 두 수 중 큰 수에서 작은 수를 빼는 걸 반복하고, 최종적으로 남은 수가 두 수의 최대 공약수가 된다.
2. 나눗셈을 이용한 방법
- 두 수 `a`와 `b`가 있을 때, `a > b`라고 가정
- `a`로 `b`로 나눈 `나머지` 구하기
- 그 후 나머지가 0이 될 때까지 **b를 그 나머지로 계속 나눠준다.**
- 나머지가 0이 되었을 때 b가 두 수의 최대공약수가 된다.

<br>

### 구현코드
```java
public class GCDExam {
    public static int gcd(int a, int b){
        if(b == 0) return a;

        return gcd(b, a % b);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("두 수의 최대 공약수를 계산합니다.");

        System.out.print("첫 번째 수 : ");
        int a = sc.nextInt();
        System.out.print("두 번째 수 : ");
        int b = sc.nextInt();

        System.out.printf("최대 공약수는 %d입니다.", gcd(Math.max(a, b), Math.min(a, b)));
    }
}
```

<br>

---

<br>

# 정렬
> `정렬` : 데이터를 특정 기준에 따라 순서대로 배열하는 과정

<br>

## 버블 정렬
> `버블 정렬` : 인접한 두 요소를 비교하고, 필요한 경우에 위치를 교환하는 동작을 요소가 정렬될 때까지 반복하는 방법이다. 구현이 간단하지만, 효율성이 낮아 큰 데이터셋에는 비효율적이다.

### 구현 코드
```java
public class BubbleSortExam {
    public static void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void bubbleSort(int[] arr){
        int n = arr.length;
        for(int i = 0; i < n-1; i++){
            for (int j = 0; j < n-i-1; j++) {
                if(arr[j] > arr[j+1]){
                    swap(arr, j, j+1);
                }
            }
        }
    }
}
```
- 한 번 반복문을 돌 때마다 큰 값이 뒤쪽에 정렬된다.
- `swap()` : 배열과 두 인덱스를 받아 배열의 두 인덱스의 값을 교환해주는 메소드
- `bubbleSort()` : 인덱스를 하나씩 줄여가면서 버블 정렬 알고리즘을 반복해주는 메소드

<br>

## 선택 정렬
> `선택 정렬` : 전체 요소 중에서 가장 작은 요소를 찾아 첫 번째 위치에 놓고 나머지 요소에 대해 같은 동작을 반복하는 방법이다. 버블 정렬과 같이 구현이 쉽지만, 큰 데이터셋에는 비효율적이다.

### 구현 코드
```java
public class SelectionSortExam {
    public static void swap(int[] arr, int i, int j){
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    public static void selectionSort(int[] arr){
        int n = arr.length;
        for (int i = 0; i < n-1; i++) {
            int min = i;
            for (int j = i+1; j < n; j++) {
                if(arr[min] > arr[j])    min = j;
            }
            swap(arr, i, min);
        }
    }
}
```

- `swap()` : 버블 정렬에 정의된 메소드와 동일한 메소드
- `selectionSort()` : 정렬되지 않은 범위에서 **가장 작은 값을 찾아** 현재 위치 `i`로 변경시켜주는 메소드
