# 스택

> `스택` : 마지막에 추가된 요소가 가장 먼저 제거되는 후입선출(LIFO) 방식을 따른다.

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

## 주요 연산
- `Enqueue` : 큐의 뒤쪽에 새로운 요소를 추가
- `Dequeue` : 큐의 앞쪽에서 요소를 제거하고 그 값을 반환
- `Peek/Front` : 큐의 맨 앞에 있는 요소를 반환
- `isEmpty` : 큐가 비어 있는지 확인

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
