# Pull Request
> `Pull Request` : 한 브랜치의 변경 사항을 다른 브랜치로 병합하기 위해 리뷰와 토론을 요청하는 메커니즘

<br>

## 과정
1. `포크(Fork)` : GitHub에서 대상 프로젝트를 자신의 계정으로 포크하여 개인 복사본을 만든다.
<img width="695" alt="스크린샷 2024-03-18 10 50 11" src="https://github.com/hamsangjin/TIL/assets/103736614/3ceec320-57a0-466b-9adb-b3ebc730d881">

<br>
<br>

2. `클론(Clone)` : 포크한 저장소를 로컬 시스템으로 클론합니다.
```php
git clone URL
```

<br>

3. `브랜치 생성` : 변경 사항을 작업하기 위한 새 브랜치를 생성하고, main에서 생성한 브랜치로 이동한다.
```php
// 생성 방법 1
git switch -c new_branch

// 생성 방법 2
git branch new_branch
git switch new_branch
```

<br>

4. `변경 사항 커밋` : 수정 사항을 커밋하고, 생성한 브랜치에 푸시한다.
```php
echo "Hello World" >> HelloWorld.txt

git add HelloWorld.txt
git commit -m "HelloWorld 생성"
git push -u origin new_branch
```

<br>

5. `Pull Request 생성` : GitHub에서 원본 프로젝트로 Pull Request를 생성하며 이때, 변경 사항을 설명하고, 리뷰어를 지정할 수 있다.
  - GitHub에서 fork한 리포지토리로 이동
  - `Pull requests` 탭을 클릭한 후, `New pull request` 버튼을 클릭
  - `base:` 드롭다운에서 `main 브랜치`를 선택하고, `compare:` 드롭다운에서 `new_branch 브랜치`를 선택
  - `Create pull request` 버튼을 클릭합니다.
  - Pull Request에 제목과 설명을 추가하고, `Create pull request` 버튼을 다시 클릭하여 PR을 생성

<br>

6. `리뷰 및 토론` : 프로젝트 메인테이너나 다른 기여자들이 코드를 검토하고, 필요한 경우 피드백을 제공한다.

<br>

7. `병합(Merge)` : 리뷰 과정을 거쳐 변경 사항이 승인되면, 프로젝트 메인테이너가 Pull Request를 병합하여 메인 브랜치에 변경 사항을 반영한다.

<br>

---

<br>

# Markdown

## 1. 제목
- `#`부터 `######`까지 사용하여 `h1`부터 `h6`까지의 제목을 만들 수 있음

### 사용법
```markdown
# 제목 1
## 제목 2
### 제목 3
#### 제목 4
##### 제목 5
###### 제목 6
```

### 실제 출력
# 제목 1
## 제목 2
### 제목 3
#### 제목 4
##### 제목 5
###### 제목 6

<br>

## 2. 강조
- 이탤릭체는 `*` 또는 `_`로, 볼드체는 `**` 또는 `__`로 감싸서 사용

### 사용법
```markdown
*이탤릭체*, _이탤릭체_, **볼드체**, __볼드체__
```

### 실제 출력
*이탤릭체*, _이탤릭체_, **볼드체**, __볼드체__

<br>

## 3. 목록
- `순서가 있는 목록`과 `순서가 없는 목록`이 존재

### 사용법
```markdown
- 순서 없는 목록
* 순서 없는 목록

1. 순서 있는 목록
2. 순서 있는 목록
```

### 실제 출력
- 순서 없는 목록
* 순서 없는 목록

1. 순서 있는 목록
2. 순서 있는 목록

<br>

## 4. 링크
- `[텍스트](URL)` 형식을 사용하여 링크 삽입

### 사용법
```markdown
[Google](https://www.google.com)
```

### 실제 출력
[Google](https://www.google.com)

<br>

## 5. 이미지
- `![대체 텍스트](이미지 URL)` 형식을 사용하여 이미지 삽입

### 사용법
```markdown
![pull shark](https://github.githubassets.com/assets/pull-shark-default-498c279a747d.png)
```

### 실제 출력
![pull shark](https://github.githubassets.com/assets/pull-shark-default-498c279a747d.png)

<br>

## 6. 코드와 코드 블록
- 인라인 코드는 `로 감싸고, 코드 블록은 ```로 감싸서 사용

### 사용법
```markdown
`인라인 코드`와

코드 블록(\는 제거)
\```
\```
```


### 실제 출력
`인라인 코드`와

코드 블록
```

```
<br>

## 7. 표
- `|`와 `-`를 사용하여 표 생성을 하며, 두 번째 줄에 따라서 정렬이 달라짐
  - `:-:` : 가운데 정렬
  - `:-` : 왼쪽 정렬
  - `-:` : 오른쪽 정렬

### 사용법
```markdown
| 헤더1 | 헤더2 | 헤더3 |
| :-- | :---: | --: |
| 왼쪽정렬 내용... | 가운데정렬 내용... | 오른쩌쪽정렬 내용... |
| 내용4 | 내용5 | 내용6 |
```

### 실제 출력
| 헤더1 | 헤더2 | 헤더3 |
| :-- | :---: | --: |
| 왼쪽정렬 내용... | 가운데정렬 내용... | 오른쪽정렬 내용... |
| 내용4 | 내용5 | 내용6 |

<br>

---

<br>

# TDD
> `TDD(테스트 주도 개발)` : 소프트웨어 개발 프로세스에서 테스트가 개발을 주도하는 방법론

## TDD 개발 준비

### 1. 프로젝트 설정
TDD를 위해 JUnit5와 같은 테스트 프레임워크를 포함시키려면, `build.gradle` 파일에 몇 가지 설정을 추가해야 한다.

- 기본적인 Gradle 설정 예시
```java
plugins {
    id 'java'
}
sourceCompatibility = '1.8'
targetCompatibility = '1.8'
compileJava.options.encoding = 'UTF-8'
compileTestJava.options.encoding = 'UTF-8'
repositories {
    mavenCentral()
}
dependencies {
    testImplementation('org.junit.jupiter:junit-jupiter:5.5.0')
}
test {
    useJUnitPlatform()
    testLogging {
        events "passed", "skipped", "failed"
    }
}
```

- `dependencies` 부분에서 JUnit5를 테스트 의존성으로 추가함으로써, TDD를 위한 테스트 환경을 구성할 수 있다.

<br>

### 2. 테스트 케이스 작성
TDD의 핵심은 개발 시작 전에 테스트 케이스를 먼저 작성하는 것이다.
```java
package chap01;

import static org.junit.jupiter.api.Assertions.*;
import org.junit.jupiter.api.Test;

public class StringTest {
@Test
    void substring() {
        String str = "abcde";
        assertEquals("cd", str.substring(2, 4));
    }
}
```
- JUnit5를 사용한 간단한 테스트 케이스로, `String`의 `substring` 메소드를 테스트하는 예시다.
- `assertEquals` : 기대하는 결과값 `cd`와 실제 `substring` 메소드의 결과를 비교한다.

<br>

### 3. TDD 사이클
TDD는 일반적으로 아래 세 단계로 진행된다.
1. `실패하는 테스트 작성` : 아직 구현되지 않은 기능에 대한 테스트를 먼저 작성하며, 이 테스트는 무조건 실패함을 확인할 수 있는 조건을 포함해야한다.
2. `테스트를 통과하기 위한 코드 작성` : 테스트를 통과하기 위한 최소한의 코드를 작성해야하며, 작성 후, 테스트 성공이 됐다면 다음 단계로 넘어간다.
3. `리팩토링` : 코드를 개선하고 중복을 제거하며, 이 과정에서도 모든 테스트가 통과돼야 한다.

> 이러한 과정을 반복해 추가 기능을 구현하거나 기존 기능을 변경할 때마다 테스트를 작성하고 코드를 개선한다.

<br>

---

<br>

## TDD 시작
### 1. 실패하는 테스트 작성
먼저 `Calculator` 클래스의 `plus` 메소드의 테스트 케이스를 작성하는데, 이때 `Calculator` 클래스가 아직 구현되지 않았거나, 메소드가 올바르게 작동하지 않게 설정해놓는다.

- `Calculator`
```java
public class Calculator {
    public static int plus(int a1, int a2) {
        return 0;
    }
}
```

- `CalculatorTest`
```
import static org.junit.jupiter.api.Assertions.assertEquals;
import org.junit.jupiter.api.Test;

public class CalculatorTest {
@Test
    public void testPlus() {
        assertEquals(5, Calculator.plus(2, 3));
    }
}
```


### 2. 테스트 실행과 실패 확인
작성한 테스트를 실행해, 테스트가 실패한 걸 확인한다.
<img width="768" alt="스크린샷 2024-03-18 14 14 20" src="https://github.com/hamsangjin/TIL/assets/103736614/9b4b5e6f-67e2-4052-b1b9-1a3d32588650">
- `5`를 예상했지만 결과는 `0`이 나왔다고 친절하게 알려준다.

<br>

### 3. 테스트를 통과하기 위한 최소한의 코드 작성
- `Calculator`
```java
public class Calculator {
    public static int plus(int a1, int a2) {
        return a1 + a2;
    }
}
```

<br>

### 4. 테스트 재실행과 통과 확인
수정한 코드로 테스트를 다시 실행해, 테스트가 성공한 걸 확인한다.
<img width="608" alt="스크린샷 2024-03-18 14 16 57" src="https://github.com/hamsangjin/TIL/assets/103736614/a59417b3-086c-42fa-800f-d1571637c645">

<br>

### 5. 리팩토링
이제 필요한 경우에는 코드를 개선하고, 중복을 제거하며, 가독성을 높이는 등의 리팩토링을 수행할 수 있다.

<br>

---

<br>

## JUnit의 테스트 관련 어노테이션

- `@Before` : 각각의 테스트 메서드가 실행되기 전에 실행되는 메서드를 정의
  - `@BeforrAll` : 테스트 클래스가 실행될 때 한 번 실행되는 메소드를 정의
  - `@BeforeEach` : 각 테스트 메소드가 실행되기 전에 실행되는 메소드를 정의
  - `@BeforeClass` : 모든 테스트 메서드가 실행되기 전에 한 번 실행되는 메소드를 정의

- `@After` : 각각의 테스트 메서드가 실행된 후에 실행되는 메소드를 정의
  - `@AfterAll` : 모든 테스트가 종료된 후 한 번 실행되는 메소드를 정의
  - `@AfterEach` : 각 테스트 메소드가 실행된 후 실행되는 메소드를 정의
  - `@Afterclass` : 모든 테스트 메서드가 실행된 후에 한 번 실행되는 메소드를 정의

- `@Test` : 단위 테스트를 수행하는 메소드를 정의
  - `@Test(expected = SomeException.class)` : 해당 테스트가 특정 예외를 발생시켜야함을 나타냄
  - `@Test(timeout = 1000)` : 해당 테스트가 특정 시간 안에 실행되어야 함을 나타냄
  - `@TestConfiguration` : 테스트 설정을 지정하는 클래스임을 나타냄

- `@lgnore` : 실행하지 않을 테스트 메소드를 정의

<br>

---

<br>

## DateCalculator 예시로 TDD 작성 순서 이해하기


### 1. 테스트 케이스 작성
먼저, `DateCalculatorTest` 클래스를 생성해 테스트 케이스를 작성하며, 이때 `DateCalculator` 클래스는 존재하지 않는다.
```java
package com.exam;

import static org.junit.jupiter.api.Assertions.assertEquals;
import java.time.LocalDate;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class DateCalculatorTest {
    private DateCalculator dateCalculator;

    @BeforeEach
    void setUp() {
        dateCalculator = new DateCalculator();
    }

    @Test
    void testCalculateAge() {
        LocalDate birthDate = LocalDate.of(1990, 5, 15);
        LocalDate currentDate = LocalDate.of(2022, 2, 28);
        int expectedAge = 32;
        int actualAge = dateCalculator.calculateAge(birthDate, currentDate);
        assertEquals(expectedAge, actualAge);
    }

    @Test
    void testCalculateDaysBetween() {
        LocalDate startDate = LocalDate.of(2022, 2, 15);
        LocalDate endDate = LocalDate.of(2022, 3, 1);
        int expectedDays = 14;
        int actualDays = dateCalculator.calculateDaysBetween(startDate, endDate);
        assertEquals(expectedDays, actualDays);
    }

    @Test
    void testIsLeapYear() {
        int leapYear = 2024;
        boolean isLeap = dateCalculator.isLeapYear(leapYear);
        assertEquals(true, isLeap);
    }
}
```

<br>

### 2. 실패하는 테스트 실행
<img width="1586" alt="스크린샷 2024-03-18 15 20 17" src="https://github.com/hamsangjin/TIL/assets/103736614/e9a3a89c-0f05-42d6-a310-6609100adbe1">

`DateCalculator` 클래스가 존재하지 않기 때문에 실패한 걸 확인할 수 있다.

<br>

### 3. 구현 코드 작성
이제 `DateCalculator` 클래스를 테스트를 통과하도록 작성해보자.
```java
import java.time.LocalDate;
public class DateCalculator {
    public int calculateAge(LocalDate birthDate, LocalDate currentDate) {
        return currentDate.getYear() - birthDate.getYear();
    }
    public int calculateDaysBetween(LocalDate startDate, LocalDate endDate) {
        return (int) startDate.until(endDate).getDays();
    }
    public boolean isLeapYear(int year) {
        return LocalDate.ofYearDay(year, 1).isLeapYear();
    }
}
```

<br>

### 4. 테스트 실행 및 성공 확인
<img width="675" alt="스크린샷 2024-03-18 15 23 32" src="https://github.com/hamsangjin/TIL/assets/103736614/b6d2788d-8c16-42d8-8ab4-7c7f4d34e7d3">

모든 테스트에 성공한 걸 확인할 수 있으므로 다음 단계로 넘어가자.

<br>

### 5. 코드 리팩토링
현재 코드에서 리팩토링할 부분이 없어 생략했지만, 필요한 경우 코드를 리팩토링해서 가독성을 높이고 중복을 제거하자.

<br>

### 6. 테스트 반복
새로운 기능을 추가하거나 기존 기능을 변경할 때마다 위 단계들을 반복하여, 소프트웨어를 안정적으로 유지하고 코드의 품질을 높여보자.

<br>

---

<br>

## 한글로 테스트 메소드 만들기

### 장점
1. `명확성` : 한글로 작성된 테스트 이름은 한국어가 모국어인 개발자에게 더 명확하게 의도를 전달할 수 있다.
2. `접근성 향상` : 한국어가 편한 개발자들에게 코드의 가독성을 높이고, 프로그래밍에 대한 접근성을 향상시킨다.
3. `의사소통 개선` : 팀 내에서 한국어를 사용하는 경우, 테스트 케이스의 의도를 더 쉽게 공유하고 논의할 수 있다.

### 단점
1. `호환성 문제` : 한글은 유니코드를 사용하기 때문에, 일부 환경이나 도구에서 인코딩 문제가 발생할 수 있다.
2. `이식성 제한` : 한글로 작성된 코드는 다국어를 사용하는 글로벌 개발 팀에서의 이식성이 제한될 수 있으며, 다른 국가의 개발자들이 코드를 이해하고 유지보수하는데 어렵다.
3. `검색성 감소` : 영어 기반의 검색 엔진과 도구를 사용할 때, 한글로 작성된 메소드나 주석은 검색하기 어려울 수 있으며, 이는 문서화 및 문제 해결 과정에서 불편할 수 있다.
