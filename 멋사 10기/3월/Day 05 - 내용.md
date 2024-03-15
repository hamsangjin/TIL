# Git

## objects 폴더의 파일들
> `objects` : Git의 객체 데이터베이스를 저장하며, 커밋, 트리, 블롭, 그리고 태그 객체가 포함되는 핵심적인 부분이다.

### 객체 폴더와 파일 이름
objects 디렉토리 내의 서브디렉토리와 파일 이름은 객체의 `SHA-1 해시`를 기반으로 한다.

- `SHA-1 해시` : Git은 객체의 내용을 기반으로 SHA-1 해시를 생성해 각 객체를 고유하게 식별하며, 40자리의 16진수 문자열이다.
- `디렉토리 이름` : 해시의 처음 2자리는 서브디렉토리의 이름으로 사용되고, 예를 들어, `35e...` 해시를 가진 객체는 `35` 디렉토리에 저장된다.
- `파일 이름` : 해시의 나머지 38자리는 객체 파일의 이름으로 사용되고, 예를 들어, `...eee90a6631956ed31df7f8817f721a499c8f05` 는 실제 객체 데이터를 저장하는 파일의 이름이다.

<br>

### 객체 파일의 내용
객체 파일에는 Git 객체의 압축된 내용이 저장되며, 객체 유형에 따라 다양하다.

- `블롭(blob) 객체` : 파일의 내용을 나타낸다.
- `트리 객체` : 디렉토리의 구조를 나타낸다.
- `커밋 객체` : 특정 시점의 프로젝트 상태를 나타낸다.
- `태그 객체` : 특정 커밋을 참조하는 데 사용된다.

> Git의 데이터 무결성을 보장하고, 저장소의 크기를 최소화하며, 효율적인 데이터 접근을 가능하게 해줌

<br>

## 파일의 해시값(SHA-1) 구하기
- `sha1sum`
```php
> sha1sum <파일명>
> sha1sum Hello.java
// 2360a43bbdd5b6792a8fe92a5e874fc09b30fa54  Hello.java 출력
```

- `git ls-files -s`
```php
> git ls-files -s
// 100644 f180d4529eabbb1f13d3ed3a12b3272b4b873f33 0	.DS_Store
// 100644 09089f5519210d72eff01fa634f7b57f9668a583 0	.gitignore
// 100644 f9e91b41ae5b8bf97a889c79b6a68a22a00f71d7 0	resources/application.properties
// 100644 e8065c18882310031a06826470acae56563a3f59 0	src/Hello.java
```

<br>

## git commit

### git ls-tree HEAD
`commit`을 한 이후에 tree타입으로 폴더가 저장된 것을 볼 수 있다.
```php
> git ls-tree
(base) sangjin@mulyeo-Macbook-Pro my_project % git ls-tree HEAD
100644 blob f180d4529eabbb1f13d3ed3a12b3272b4b873f33	.DS_Store
100644 blob 09089f5519210d72eff01fa634f7b57f9668a583	.gitignore
040000 tree d7eaf7d57596a8cc7184c638eb4cbedaf248d607	resources
040000 tree 119d17e73120a38e52cff7e5950c00788c3b1367	src
```

<br>

### git log
```php
> git log
(base) sangjin@mulyeo-Macbook-Pro my_project % git log
commit 06775d18037edd452e4c8a889e440f6c5cb8edb5 (HEAD -> main)
Author: hamsangjin <rhkstlash@naver.com>
Date:   Fri Mar 15 10:22:31 2024 +0900

    커밋 시도

commit 8a6c834c4f586fdeb06f0b2cb50e5941023f06a5
Author: hamsangjin <rhkstlash@naver.com>
Date:   Thu Mar 14 17:04:38 2024 +0900

    첫 번째 커밋
```
- git의 log들을 확인할 수 있음.

<br>

### git cat-file -p 해시값
> `git cat-file -p 해시코드` :  Git 저장소에서 객체를 검사할 때 사용된다.

```php
> git cat-file -p 06775d18037edd452e4c8a889e440f6c5cb8edb5
tree 9615f010e51637cdca02e36ea7fe95b62520a841
parent 8a6c834c4f586fdeb06f0b2cb50e5941023f06a5
author hamsangjin <rhkstlash@naver.com> 1710465751 +0900
committer hamsangjin <rhkstlash@naver.com> 1710465751 +0900

커밋 시도
```

<br>

### git reset

> `git reset --옵션 [커밋 해시]` : 주로 커밋을 취소하거나 변경 사항을 재정렬하기 위해 사용된다.

- **옵션**
  - `--soft` : HEAD는 지정 된 커밋으로 이동하지만, 스테이징 영역(인덱스)와 작업 디렉토리는 변경되지 않는다.
  - `--mixed` : HEAD가 지정된 커밋으로 이동하고, 스테이징 영역은 리셋되지만, 작업 디렉토리는 변경되지 않는다.
  - `--hard` : HEAD, 스테이징 영역, 그리고 작업 디렉토리가 모두 지정된 커밋으로 리셋된다.

- `git reset --hard 커밋 해시` : 해시값에 원하는 커밋 해시값을 넣어주면 해당 커밋으로 돌아간다.
<p align=center>
<img width="711" alt="스크린샷 2024-03-15 11 46 04" src="https://github.com/hamsangjin/TIL/assets/103736614/7087f4d4-8773-4908-a7db-5eb8b64b525c">
</p>

<br>

## Git의 고수준 명령과 저수준 명령

### 고수준 명령어
> 일반적으로 사용자가 자주 사용하며, 사용자가 직관적으로 이해하고 사용할 수 있게 설계되었다.
- `git clone` : 다른 저장소를 복제
- `git init` : 새로운 Git 저장소를 초기화
- `git add` : 작업 디렉토리의 변경 사항을 스테이징 영역에 추가
- `git commit` : 스테이징 영역의 변경 사항을 저장소의 이력에 추가
- `git pull` : 원격 저장소의 변경 사항을 현재의 작업 브랜치와 병합
- `git push` : 로컬 브랜치의 변경 사항을 원격 저장소에 업로드
- `git branch` : 브랜치를 생성, 나열, 삭제하는 등의 작업을 수행
- `git merge` : 두 개의 브랜치를 병합
- `git checkout` : 다른 브랜치로 전환하거나 작업 디렉토리의 파일을 복원

<br>

### 저수준 명령어
> Git의 내부 구조와 데이터 모델을 직접 다루는데 사용되며, 일반적으로 스크립트나 고급 Git 사용자에 의해 사용된다.
- `git cat-file` : 객체의 내용을 보여주거나 객체 유형을 확인
- `git hash-object` : 파일의 내용으로부터 SHA-1 해시 값을 계산
- `git reflog` : 특정 참조의 업데이트 로그
- `git ls-tree` : 트리 객체의 내용 출력
- `git pack-objects` : 여러 객체를 하나의 팩 파일로 압축

<br>

## Git의 명령 옵션
- `단일 대시(-)`로 시작하는 짧은 옵션
- `이중 대시(--)`로 시작하는 긴 옵션
- 옵션 없이 사용되는 명령어 인자

### POSIX 스타일 CLI의 특징
> `POSIX 스타일 CLI` : 운영 체제와 사용자 간의 상호 작용을 위한 텍스트 기반의 인터페이스
> `POSIX` : 유닉스 운영체제를 기반으로 한 **다양한 OS에서 호환성과 이식성을 제공**하기 위해 IEEE에 의해 개발된 표준

- `표준화된 명령어 구문` : 명령어의 구분과 사용방법을 표준화해 다양한 시스템에서 일관성을 제공
- `옵션과 인자의 구분` : 대부분의 POSIX 호환 명령어는 `짧은 옵션(-)`, `긴 옵션(--)`, `옵션 없는 인자`를 구분해, 사용자가 명령어를 더 명확하고 유연하게 사용할 수 있게 해준다.
- `파이프라인과 리디렉션` : POSIX 쉘은 `파이프라인(|)`과 `리디렉션(<, >)`과 같은 기능을 지원해, 다양한 출력과 입력 작업을 가능하게 해준다.
- `백그라운드 실행` : 명령어 뒤에 `&`를 붙여 프로세스를 백그라운드에서 실행할 수 있게 해준다.
- `와일드카드와 패턴 매칭` : 파일명이나 다른 문자열을 다룰때, `*`, `?`, `[...]`와 같은 와일드카드를 사용하여 패턴 매칭을 사용할 수 있다.
- `변수와 환경 변수` : POSIX 쉘은 사용자 정의 변수와 시스템 환경 변수를 지원한다.

<br>

## 커밋 메시지 작성법
> `좋은 커밋 메시지`는 프로젝트의 유지보수를 용이하게 하고, 팀원들 간의 효율적인 커뮤니케이션을 가능케 한다.

1. `명령문 사용` : 커밋 메시지는 무엇을 했는지가 아닌, 무엇을 할 것인지를 설명해야 한다.
2. `첫 줄은 요약, 본문은 상세 설명` : 첫 줄에는 커밋의 목적을 적어주고, 필요한 경우 본문에 상세한 설명을 추가해준다.
3. `이슈 트래커 ID 포함` : 만약 프로젝트가 이슈 트래커를 사용한다면, 관련된 `이슈 번호`나 `ID`를 커밋 메시지에 포함시키는 것이 좋다.
4. `변경 내용의 범위를 명확히` : 여러 변경이 포함된 커밋의 경우, 변경의 내용의 범위를 명확히하여, 어떤 부분에 영향을 미치는지를 알 수 있도록 한다.
5. `일관성 유지` : 프로젝트 내에서 커밋 메시지의 형식을 일관되게 유지하는 것이 중요하다.

> `커밋 메시지 언어 선택` : 프로젝트에 외국인이 없다면 영어를 사용하는 좀 더 포괄적이겠지만, 팀원 모두가 한글을 사용하는 것이 더 익숙하다면 한글을 사용해도 좋다.

<br>

## git diff
> `git diff` : git 저장소에서 두 개의 커밋이나 브랜치 사이, 또는 작업 디렉토리와 인덱스 사이의 차이를 보여준다.

### 옵션
- `git diff` : 변경되지 않고 스테이징되지 않은 파일들 사이의 차이
- `git diff --cached` or `git diff --staged` : 스태이징 영역과 마지막 커밋 사이의 차이
- `git diff [branch1] [branch2]` : 두 브랜치 사이의 차이
- `git diff [commit1] [commit2]` : 두 커밋 사이의 차이

<br>

## git restore
> `git restore` : 변경사항을 되덜리거나, 스테이징된 변경 사항을 취소하는 등의 작업을 수행하는 명령어

### 옵션
- `-s` or `--source` : 복원할 소스를 지정하는 옵션으로, 복원할 데이터의 출처를 나타내며, 커밋 해시, 브랜치 이름, 태그 등으로 설정할 수 있다.
- `--staged` : 변경사항을 스테이징 영역에서 복원하는 옵션
- `-W` or `--worktree` : 작업 디렉터리에서 파일을 복원할 때 사용하는 옵션

<br>

### 사용 예제

```php
// 파일의 변경사항을 무시하고, 마지막 커밋 상태로 되돌리기
git restore <파일명>

// 파일을 스테이징 영역에서 워킹 디렉토리로 다시 이동
git restore --staged <파일명>

// 특정 커밋이나 브랜치에서 파일의 상태 복원
git restore --source=<커밋 해시 또는 브랜치> <파일명>

// 파일을 스테이징 영역과 작업 디렉터리 양쪽에서 복원
git restore --staged --worktree <파일명>
```

<br>

## git show
> `git show` : git에서 특정 객체(커밋, 태그 등)의 상세 정보를 보여주는 데 사용되며, 기본적으로 커밋의 메타데이터와 패치 정보를 출력한다.

### 옵션
- `커밋 해시 또는 태그이름` : `git show [커밋 해시 또는 태그 이름]` 을 사용하면 지정된 커밋 또는 태그의 상세 정보를 볼 수 있다.
- `-p` or `--patch` : 변경 사항을 패치 형식으로 보여준다.
- `--stat` : 커밋에 의해 변경된 파일들의 목록과 각 파일에서 추가되거나 삭제된 줄의 수를 요약해서 보여준다.
- `--name-only` : 변경된 파일의 이름만을 출력한다.
- `--name-status` : 변경된 파일의 이름과 해당 파일에 적용된 변경의 종류(추가, 수정, 삭제 등)를 출력합니다.
- `--pretty=oneline`, `--pretty=short`, `--pretty=full`, `--pretty=fuller`, `--pretty=format:"<format>"` : 이 옵션들은 커밋 메시지의 출력 형식을 조정한다.
- `--numstat` : 파일당 추가 또는 삭제된 줄의 수를 숫자로 보여준다.
- `--color`, `--no-color` : 출력에 색상을 사용할지 여부를 결정한다.

<br>

## 브랜치
<p align=center>
<img width="719" alt="스크린샷 2024-03-15 13 27 51" src="https://github.com/hamsangjin/TIL/assets/103736614/b0794777-de1f-46d4-a123-a67882338d2e">
</p>

> 개발 과정에서 브랜치를 사용하는 것은 코드 관리와 협업에 있어 필수적인 방법이며, 개발 프로세스를 더욱 유연하고 효율적으로 만들어준다.

1. 기능별 개발 : 각각의 새로운 기능이나 개선사항을 별도의 브랜치에서 개발할 수 있다.
2. 독립적인 작업 환경 : 개발자들은 자신의 작업 브랜치에서 독립적으로 작업함으로써, 개발자간의 충돌을 최소화한다.
3. 코드 리뷰 및 협업 용이 : 기능별 또는 작업별로 브랜치를 나누면, 코드 리뷰 과정이 훨씬 용이해진다.
4. 버전 관리 및 릴리스 관리 : 릴리스를 위한 코드 준비 과정을 메인 코드 베이스와 분리할 수 있다.
5. 실험 및 테스트 : 새로운 아이디어를 실험하거나 리팩토링을 수행할 수 있다.

<br>

> `브랜치 전략` : 소프트웨어 개발 프로젝트에서 코드베이스를 효과적으로 관리하고 협업을 용이하게 하는 방법론
1. `Feature Branch Workflow`
2. `Git Flow`
3. `GitHub Flow`
4. `GitLab Flow`

<br>

### 브랜치 생성과 목록
```php
// 브랜치 생성과 동시에 변경
git switch -c blog-create
// 브랜치 생성
git branch blog-update
// 브랜치 목록 확인
git branch
// 브랜치 변경
git switch blog-update

// blog-update 브랜치에 자바 파일 생성
vim src/BlogUpdate.java
```
<p align=center>
<img width="448" alt="스크린샷 2024-03-15 13 47 42" src="https://github.com/hamsangjin/TIL/assets/103736614/c7f2cba1-7379-4bc4-9dc7-9c96dfbebcf6">
</p>

`git branch`와 `git switch`의 많은 옵션들이 있으니 잘 활용해서 사용해보자.

<br>

## Merge
`git merge` 명령어를 통해 `blog-update` 브랜치에 생성한 자바 파일을 `main`에 합쳐보자.

```php
git switch main
ls -la src/

git merge blog-update
ls -la src/
```
<p align=center>
<img width="489" alt="스크린샷 2024-03-15 14 25 38" src="https://github.com/hamsangjin/TIL/assets/103736614/87864833-a4e7-4f46-9b73-cbe8e89bbc86">
</p>

이때, `merge`하기 전으로 돌아가고 싶다면  `git reset --hard <merge 전 커밋 해시>` 명령어를 이용해 돌아갈 수 있다.

<br>

---

<br>

# Github

## Github 레포지토리 생성 및 연결
~~알고있는 내용이므로 생략~~
