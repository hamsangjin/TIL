# nvm & node 설치(MAC)

- 개발 환경
    - node version: 20.11.1
    - npm version: 10.2.4

```php
// brew 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew install node
brew install nvm

mkdir ~/.nvm
cd ~/

// .zshrc 열기
vi ~/.zshrc
// 해당 내용 추가
export NVM_DIR="$HOME/.nvm"
  [ -s "/opt/homebrew/opt/nvm/nvm.sh" ] && . "/opt/homebrew/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm" ] && . "/opt/homebrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion

// 터미널 재시작

nvm install 15.12.0
nvm use 15.12.0

// 15.12.0
node -v

npm install -g npm@7.6.3
```

<br>

---

<br>

# VS code - Live Server
먼저 VS Code의 확장에서 다운로드 받아준다.
<img width="678" alt="스크린샷 2024-03-27 11 15 39" src="https://github.com/hamsangjin/TIL/assets/103736614/3843902c-a8a1-4a5a-bd11-2df66d3fe509">

<br>

그 후 실행시킬 파일에서 마우스 우클릭 후, Open With Live Server를 눌러 실행시켜준다.
<img width="629" alt="스크린샷 2024-03-27 11 16 12" src="https://github.com/hamsangjin/TIL/assets/103736614/587625b6-2dc2-45bf-a2ab-2f061f152abd">

<br>

그러면 아래와 같이 실행되며, 수정해도 동적으로 바로 변경된다.
<img width="534" alt="스크린샷 2024-03-27 11 17 09" src="https://github.com/hamsangjin/TIL/assets/103736614/efbfb99e-dfe0-4e2d-b112-7c2466495c8e">

<br>

---

<br>

# HTML5 

## 시작태그, 끝태그, 내용
```html
<h1> Hello </h1>
```

- 시작태그: `<h1>`
- 끝태그: `</h1>`
- 내용: `Hello`
- 요소: `시작태그` + `내용` + `끝태그`

<br>

## 속성
```html
<a href="http://www.naver.com">naver</a>
```
- 속성이름: `href`
- 속성값: `http://www.naver.com`
- 속성블록: `href="http://www.naver.com"`

<br>

## 내용이 없을 경우
```html
<br>

<br/>
```
- `<br>` or `<br/>` : 줄 바꿈

<br>

## HTML5 - 기본구조
```html
<!DOCTYPE html>
<html>
    <head>
        <title> 문서의 제목 </title>
    </head>    
    <body>
        문서의 내용
    </body>
</html>
```
- HTML5 문서는 첫번째 줄에 `<!DOCTYPE html>`가 있어야 한다.

<br>

## HTML5 - head
- 웹브라우저에 문서 정보를 알려주는 태그 `<head>`, `</head>`
- 화면에 보이지 않으며, `<meta>`, `<title>` 태그들을 사용하고, css파일도 연결해준다.

<br>

## meta 태그
문자 세트를 비롯해 문서 정보가 들어간다.

1. 문자셋 정의: 문서의 문자 인코딩 지정
```html
<meta charset="UTF-8">
```

2. 뷰포트 설정: 반응형 웹 디자인에 필수적인 속성으로, 모바일 브라우저에서 페이지가 어떻게 보여질지 제어
```html
 <meta name="viewport" content="width=device-width, initial-scale=1.0">
```

3. 키워드: 페이지 내용과 관련된 키워드를 제공하며, 검색엔진 최적화에 도움됨
```html
<meta name="keywords" content=" HTML, CSS, JavaScript">
```

4. 웹페이지 설명: 웹페이지의 간단한 설명을 제공
```html
<meta name="description" content="Html 구조에 대해 설명합니다.">
```

5. 저자: 웹페이지 작성자 정보를 제공
```html
<meta name="author" content="carami">
```

6. 리프레시: 페이지를 자동으로 새로고침하거나, 지정된 시간 후 다른 페이지로 리다이렉션
```html
<meta http-equiv="refresh" content="30;url=https://example.com">
```

7. 캐시 제어: 브라우저가 페이지를 얼마나 자주 캐싱할지 제어
```html
<meta http-equiv="Cache-Control" content="no-cache, no-store, must-revalidate">
```

8. 오픈 그래프 메타태그: 소셜 미디어 플랫폼에서 페이지를 공유했을 때 어떻게 보여질지 제어
```html
<meta property="og:title" content="제목">
<meta property="og:description" content="설명">
<meta property="og:image" content="이미지_URL">
```

<br>

## HTML5 - 제목
- `h1` ~ `h6`: h1은 가장 큰 제목, h6은 가장 작은 제목

```html
<h1> 가장 큰 제목 </h1>
<h6> 가장 작은 제목 </h6>
```

<br>

## HTML5 - 개행과 가로줄
- `br`: 줄바꿈
- `hr`: 가로줄이 긋기 
```html
<br>
<br/>

<hr>
<hr/>
```

<br>

## HTML5 - 앵커
- 하이퍼 링크 태그를 의미하며, 내용을 클릭하면 주소로 이동한다.
```html
<a href="주소">내용</a>
```

<br>

## HTML5 - 글자의 모양
- `<b>`, `<i>`, `<small>`, `<sub>`, `<sup>`, `<ins>`, `<del>`
- 위와 같은 태그가 있지만, 태그를 이용하지 않고 CSS를 이용해 설정하도록 한다.

<br>

## HTML5 - 목록태그 / 기본목록
- `<ul>`: 순서가 없는 목록 표현
- `<ol>`: 순서가 있는 목록 표현
- `<li>`: 목록의 요소

```html
<ul>
    <li>사과</li>
    <li>배</li>
    <li>수박</li>
</ul>
```

<br>

## HTML5 - 목록태그 / 정의목록
- `<dl>`: 정의 목록 태그
- `<dt>`: 정의 용어 태그
- `<dd>`: 정의 설명 태그

<br>

## HTML5 - 테이블 태그
- `table` : 표
- `tr` : 표 내부의 행
- `th` : 제목 셀 태그
- `td` : 일반 셀 태그
- `border` : 표의 테두리 두께 (CSS를 사용하자)
- `rowspan` : 세로로 셀을 합친다.
- `colspan` : 가로로 셀을 합친다.

<br>

## HTML5 - 이미지 태그
- `img` : 이미지
- `src` : 이미지 경로
- `alt` : 이미지 설명
- `width` : 이미지 너비
- `height` : 이미지 높이

<br>

## HTML5 - 입력양식 태그(form)
- `input` : 이미지
- `textarea` : 이미지 경로
- `select` : 이미지 설명
- `fieldset`, `legend` : 입력 양식 설명

<br>

## HTML5 - 공간 분할 태그(div, span)
- `div`: `블록형식`으로 공간을 분할
- `span`: `인라인형식`으로 공간을 분할

<br>

## HTML5 - 시멘틱 구조 태그
- `header`: 헤더
- `nav`: 네비게이션
- `aside`: 사이드에 위치하는 공간
- `section`: 여러 중심 내용을 감싸는 공간
- `article`: 내용
- `footer`: 푸터
- `address`: 주소
