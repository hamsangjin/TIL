# 실습 문제 1: 점수 입력받아 예외처리 후, 결과 출력하기

## 요구 사항

### 1. 입력 처리
- Scanner 클래스를 사용하여 사용자로부터 입력을 받습니다.
- 사용자가 0(더 이상입력하고 싶지 않을 때)을 입력할 때까지 반복하여 점수를 입력받습니다.
- 입력 받은 점수는 ArrayList<Integer>에 저장됩니다.

### 2. 점수 유효성 검사
- 입력 받은 점수가 0부터 100 사이가 아니면 예외를 발생시키고,
- 에러 메시지로 "0-100사이의 숫자만 입력이 가능합니다."와 해당 점수를 출력합니다.
- 예외 발생 시, 에러 스택 트레이스를 출력합니다.

### 3. 점수 리스트 관리
- 입력된 0은 점수 리스트에서 제거합니다. (종료 신호로 사용됨)

### 4. 결과 출력
- 입력된 점수를 모두 출력합니다.
- 입력된 모든 점수의 합(총점)과 평균을 계산하여 출력합니다.
- 평균은 정수로 계산하여 출력합니다.

<br>

## 풀이 코드

### 1, 2. 입력 처리 및 점수 유효성 검사
```java
public static void readScores() throws NotIntegerException, IntegerRangeException{
    Scanner sc = new Scanner(System.in);

    int number = -1;

    System.out.println("Start !!");
    while (number != 0) {
        System.out.print("0 - 100까지의 숫자를 입력해주세요. : ");
        String temp = sc.next();
        try{
            number = Integer.parseInt(temp);
            if(number < 0 || number > 100){
                throw new IntegerRangeException(number + "는 범위에 벗어난 숫자입니다. 0부터 100까지의 숫자만 입력해주세요.");
            } else{
                list.add(number);
            }
        } catch (NumberFormatException e) {
            throw new NotIntegerException(temp + "는 입력할 수 없습니다. 숫자만 입력해주세요.");
        }
    }
    sc.close();
}
```
```java
class NotIntegerException extends Exception{
    public NotIntegerException(String message) {
        super(message);
    }
}
```
```java
class IntegerRangeException extends Exception{
    public IntegerRangeException(String message) {
        super(message);
    }
}
```

### 3. 점수 리스트 관리
```java
public static void removeZero(){
    list.remove(list.size()-1);
}
```

### 4. 결과 출력
```java
public static void printScores(){
    int sum = 0;
    for (int i = 0; i < list.size(); i++) {
        System.out.println(list.get(i));
        sum += list.get(i);
    }
    System.out.println("모든 점수의 합 : " + sum + ", 점수의 평균 : " + (int)(sum/list.size()));
}
```

### 5. 클래스 및 메인 메소드(예외 받기)
```java
public class CollectionExam {
    static List<Integer> list = new ArrayList<>();

    public static void main(String[] args) {
        try {
            readScores();
            removeZero();
            printScores();
        } catch (NotIntegerException e) {
            e.printStackTrace();
        } catch (IntegerRangeException e) {
            e.printStackTrace();
        }
    }
}
```


<br>

---

<br>

# 실습 문제: WordCollectionExam

## 요구사항
### 1. 클래스 및 메인 메소드 설정
- `WordCollectionExam` 이라는 이름의 클래스를 생성합니다.
- `main` 메소드 내에서 프로그램이 실행될 수 있도록 구성합니다.

### 2. 입력 처리
- `Scanner` 클래스를 사용하여 사용자로부터 입력을 받습니다.
- 사용자가 "quit"을 입력할 때까지 반복하여 단어를 입력받습니다.
- 입력 받은 단어는 `ArrayList<String>`에 저장됩니다.

### 3. 단어 리스트 관리
- 입력된 "quit"은 단어 리스트에서 제거합니다. (종료 신호로 사용됨)

### 4. 결과 출력
- 입력된 단어를 모두 출력합니다.
- 모든 단어의 개수(단어 수)와 글자 수의 합을 계산하여 출력합니다.
- 단어 중에서 가장 긴 단어와 그 길이를 출력합니다.

### 5. 프로그램 종료
- 모든 결과를 출력한 후 프로그램이 종료됩니다.

<br>

## 풀이 코드

### 1, 5. 클래스 및 메인 메소드 설정 및 프로그램 종료
```java
public class WordCollectionExam {
    static ArrayList<String> list = new ArrayList<>();
    public static void main(String[] args) {
        readWords();
        removeQuit();
        printScores();
    }
}
```

### 2. 입력 처리
```java
public static void readWords(){
    Scanner sc = new Scanner(System.in);

    String str = "";
    while(!str.equals("quit")){
        System.out.print("단어를 하나 입력해주세요. : ");
        str = sc.next();
        list.add(str);
    }
    sc.close();
}
```

### 3. 단어 리스트 관리
```java
public static void removeQuit(){
    list.remove(list.size()-1);
}
```

### 4. 결과 출력
```java
public static void printScores(){
    int charSum = 0;
    int charMax = 0;
    
    for (int i = 0; i < list.size(); i++) {
        String temp = list.get(i);
        System.out.println(temp);
    
        charSum += temp.length();
        if(list.get(charMax).length() < list.get(i).length())   charMax = i;
    }
    System.out.println("가장 긴 단어는 " + list.get(charMax) + "이며, 길이는 " + list.get(charMax).length() + "입니다.");
    System.out.println("단어의 개수: " + list.size() + ", 글자 수: " + charSum);
    }
```
