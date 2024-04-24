# 문제 1: 문자열 리스트 정렬하기
**요구 사항**: 주어진 `List<String>`을 문자열 길이에 따라 정렬하되, 람다식을 사용하여 `Collections.sort()` 메서드를 활용하라.

## 풀이 코드
```java
List<String> list1 = new ArrayList<>(Arrays.asList("abe", "dqd", "dqqfqg", "dqwdqwwqf", "dqwdwfqg", "fq"));
Collections.sort(list1, (a, b) -> a.length() - b.length());
System.out.println(list1);
```

<br>

# 문제 2: 최대값 찾기
**요구 사항**: 주어진 정수 배열에서 최대값을 찾아 출력하라. 람다식을 사용하여 자바의 `Comparator` 인터페이스와 함께 `Arrays.sort()`를 활용하라.

## 풀이 코드
```java
Integer[] arr1 = {1, 334, 13, 44, 1415, 12, 4};
Arrays.sort(arr1, (a, b) -> b - a);
System.out.println(arr1[0]);
```

<br>

# 문제 3: 리스트의 각 요소에 연산 적용하기
**요구 사항**: 주어진 `List<Integer>`의 각 요소에 10을 더한 결과를 새 리스트에 저장하고 출력하라. 람다식을 사용하여 `List`의 `forEach()` 메서드를 활용하라.

## 풀이 코드
```java
List<Integer> list2 = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
List<Integer> result1 = new ArrayList<>();
list2.forEach(n -> result1.add(n+10));
System.out.println(result1);
```

<br>

# 문제 4: 조건에 맞는 요소 찾기
**요구 사항**: 주어진 `List<String>`에서 글자 수가 5 이상인 첫 번째 단어를 찾아 출력하라. `for` 루프와 람다식을 활용하여 조건에 맞는 요소를 찾아보세요.

## 풀이 코드
```java
List<String> list3 = new ArrayList<>(Arrays.asList("1", "22", "333", "333", "4444", "55555", "55555", "666666"));
Predicate<String> p1 = s -> s.length() >= 5;
for(String s : list3){
    if(p1.test(s)){
        System.out.println(s);
        break;
    }
}
```

<br>

# 문제 5: 요소 변환하기
**요구 사항**: 주어진 `List<Integer>`의 각 요소를 제곱한 결과를 새 리스트에 저장하고 출력하라. `for` 루프와 람다식을 활용하여 각 요소를 변환하라.

## 풀이 코드
```java
List<Integer> list4 = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
List<Integer> result2 = new ArrayList<>();
list4.forEach(n -> result2.add(n*n));
System.out.println(result2);
```

<br>


# 문제 6: 모든 요소에 대해 조건 확인하기
**요구 사항**: 주어진 `List<Integer>`의 모든 요소가 짝수인지 확인하라. 람다식을 활용하여 조건 검사를 수행하고 결과를 출력하라.

## 풀이 코드
```java
List<Integer> list5 = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));
Predicate<Integer> p2 = n -> n % 2 == 0;
list5.forEach(n -> System.out.println(p2.test(n)));
```
