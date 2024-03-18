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
| 왼쪽정렬 내용... | 가운데정렬 내용... | 오른쩌쪽정렬 내용... |
| 내용4 | 내용5 | 내용6 |

<br>

---

<br>

