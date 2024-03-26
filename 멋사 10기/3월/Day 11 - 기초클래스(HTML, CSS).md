# 나만의 이력서 만들기

html과 css를 공부하며, 나만의 이력서 양식을 적어보자 !!

<br>

## 1. HTML과 인사하기
`코딩을 한단 생각`보단 `문서를 만든다는 생각`으로 접근해보자.

### p 태그
```html
<p> Hello </p>
```

### h1 태그
```html
<h1> Hello </h1>
```

<br>

---

<br>

## 2. 문서의 골격

### html 문서에서 쓰이는 태그 비중
<img width="634" alt="스크린샷 2024-03-26 15 55 40" src="https://github.com/hamsangjin/TIL/assets/103736614/738cfbca-e0bd-4296-b1d8-9dc59bb05c47">

그림과 같이 가장 많이 쓰이는 태그들을 중점적으로 배우며, 간결하게 작성해보자.

<br>

### html 태그 
- 어떤 문서든 들어가야 할 구조
```html
<!DOCTYPE html>
<html>
    내용
</html>
```

### head 태그
```java
<head></head>
```

### body 태그
```java
<body></body>
```

### 전체적인 구조
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
    </head>
    <body>
        <h1> 함상진 </h1>
        <p> HTML/CSS </p>
    </body>
</html>
```

<br>

---

<br>

## 3. CSS와 인사하기

### footer 태그 추가 및 link 태그를 통한 css 파일 연결
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="example.css">
    </head>
    <body>
        <h1> 함상진 </h1>
        <p> HTML/CSS </p>
        <footer> copyright CODE LION. All rights reserved </footer>
    </body>
</html>
```


### footer 태그 설정
- `example.css`
```css
footer {
    text-align: center;
    color: white;
    background-color: black;
}
```
- `text-align: center`: 텍스트 중앙 정렬
- `color: white`: 글씨 색을 흰색으로 변경
- `background-color: black`: 글씨 배경 색을 검은색으로 변경


<br>

---

<br>


## 4. 가독성 챙기기

### p 태그에 class 속성 사용해보기
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <link rel="stylesheet" href="example.css">
    </head>
    <body>
        <p class="big-font"> 보통 글씨 출력 </p>
        <p class="big-font"> 큰 글씨 출력 </p>
        <p class="small-font"> 작은 글씨 출력 </p>
    </body>
</html>
```

### p 태그 설정 및 big-font, small-font 클래스 설정
- `example.css`
```css
p {
    font-size: 30px;
}

.big-font {
    font-size: 40px;
}

.small-font {
    font-size: 20px;
}
```

- `font-size: 30px`: 글씨 크기 30px로 설정
- `.클래스명{}`: 해당 클래스 스타일 설정

<br>

---

<br>

## 5. 중앙에 배치하기

### div 태그로 h1과 p 태그 묶기
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="example.css">
    </head>
    <body>
        <div class="mainbox">
            <h1> 함상진 </h1>
            <p> HTML/CSS </p>
        </div>

        <footer> copyright CODE LION. All rights reserved </footer>
    </body>
</html>
```
- `div 태그`: 특별한 기능이 있는 태그는 아니고, 가상의 레이아웃을 설계하는데 쓰인다.


### footer 태그 설정 및 mainbox 클래스 설정
- `example.css`
```css
footer {
    text-align: center;
    color: #919191;
    background-color: #1e1e1e;
    font-size: 12px;
}

.mainbox{
    border: 1px solid #ebebeb;
    width: 610px;
    text-align: center;
    margin-left: auto;
    margin-right: auto;
}
```

- `border: width(테두리 두께) style(테두리 스타일) color(테두리 색)` : 태그의 테두리를 설정하는 속성으로 `background` 속성과 비슷하게 사용 가능하다.
- `margin-left: auto`과 `margin-right: auto` : 가로 여백을 균등하게 분배하여 배치하라는 뜻으로, `margin: 0 auto`와 같다.

<br>

---

<br>

## 6. 박스 쪼개기

### 박스가 나뉘는 단위
<img width="853" alt="스크린샷 2024-03-26 16 47 03" src="https://github.com/hamsangjin/TIL/assets/103736614/3abaf850-3fa3-4185-9182-447550528c0c">
이 단위를 잘 보고, `margin`과 `border`, 그리고 `padding` 값을 잘 설정해보자

### div로 박스 클래스 2개 생성
- `box_model.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="box_model.css">
    </head>
    <body>
        <div class="box1"> 박스 1 </div>
        <div class="box2"> 박스 2 </div>

        <footer> copyright CODE LION. All rights reserved </footer>
    </body>
</html>
```

### 박스 클래스 설정
- `box_model.css`
```css
.box1{
    background-color: skyblue;
    width: 60px;
    height: 60px;
    border: 5px solid black;
    padding: 20px;
    margin: 20px;
}

.box2{
    background-color: violet;
    width: 100px;
    height: 100px;
    border: 5px solid purple;
}
```

- `padding`: content(내용)와 border(선)사이의 공간
- `margin`: 테두리(border)와 이웃하는 요소 사이의 간격

<br>

---

<br>

## 7. 그림자 표현하기

### 그림자 표현할 div 설정
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="resume.css">
    </head>
    <body>
        <div class="mainbox">
            <h1> 함상진 </h1>
            <p class="name-text"> HTML/CSS 개발자 </p>
        </div>

        <footer>
            <p> copyright CODE LION. All rights reserved  </p>
        </footer>
    </body>
</html>
```

### mainbox 및 name-text 클래스 설정
- `resume.css`
```css
.name-text{
    font-size: 17px;
    color: #7c7c7c;
    font-weight: bold;
}

.mainbox{
    width: 610px;
    padding: 30px;
    margin: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid #ebebeb;
    box-shadow: 0 1px 20px 0 rgba(0,0,0,0.1);
}

footer{
    text-align: center;
    backgroud-color: #1e1e1e;
    padding: 20px;
    font-size: 12px;
    color: #919191;
}
```

- `font-weight`: 글씨체의 두께를 설정
- `box-shadow: 그림자x축 그림자y축 블러값 그림자크기 그림자색깔(R,G,B,투명도)`: div 박스에 그림자를 설정

<br>

## 8. 구글 웹 폰트 사용하기

### section 태그 추가
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="resume.css">
    </head>
    <body>
        <div class="mainbox">
            <h1> 함상진 </h1>
            <p class="name-text"> HTML/CSS 개발자 </p>

            <section>
                <h2> ABOUT ME </h2>
                <p class="about-me-text"> </p>
            </section>
        </div>

        <footer>
            <p> copyright CODE LION. All rights reserved  </p>
        </footer>
    </body>
</html>
```

- `section` : `div 태그`와 기능은 동일하며, 무분별한 `div 태그` 사용을 줄이기 위해 사용한다. 또 이와 같은 태그로 `article`이라는 태그가 있다.

### 전체 폰트 변경 및 body, h1, h2 태그 설정
- `resume.css`
```css
@import url('https://fonts.googleapis.com/css?family=Montserrat:100,200,300,400,500,600,700,800&display=swap');

*{
    font-family: 'Montserrat';
}

body, h1, h2{
    margin: 0px;
    padding: 0px;
}

body{
    min-width: fit-content;
}

.name-text{
    font-size: 17px;
    color: #7c7c7c;
    font-weight: bold;
}

.mainbox{
    width: 610px;
    padding: 30px;
    margin: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid #ebebeb;
    box-shadow: 0 1px 20px 0 rgba(0,0,0,0.1);
}

footer{ 
    text-align: center;
    background-color: #1e1e1e;
    padding: 20px;
    font-size: 12px;
    color: #919191;
}
```

- `*{}` : 문서 전체 적용할 설정
- `font-family: '폰트 이름'` : 폰트의 이름 설정

<br>

---

<br>

## 9. About Me 제작하기

### h1과 h2 태그 설정
- `resume.css`
```css
@import url('https://fonts.googleapis.com/css?family=Montserrat:100,200,300,400,500,600,700,800&display=swap');

*{
    font-family: 'Montserrat';
}

body, h1, h2{
    margin: 0px;
    padding: 0px;
}

body{
    min-width: fit-content;
}

h1{
    font-size: 36px;
    font-weight: bold;
    font-style: italic;
}

h2{
    font-size: 20px;
    font-weight: lighter;
    color: #282828;
    border-bottom: 1px solid #ebebeb;
    margin-bottom: 16px;
    padding-bottom: 5px;
}

.name-text{
    font-size: 17px;
    color: #7c7c7c;
    font-weight: bold;
}

.mainbox{
    width: 610px;
    padding: 30px;
    margin: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid #ebebeb;
    box-shadow: 0 1px 20px 0 rgba(0,0,0,0.1);
}

footer{
    text-align: center;
    background-color: #1e1e1e;
    padding: 20px;
    font-size: 12px;
    color: #919191;
}
```

- `font-style: italic`: 글씨체 italic체로 변경

<br>

---

<br>

## 10. Experience 제작하기

### Experience section 추가
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="resume.css">
    </head>
    <body>
        <div class="mainbox">
            <h1> 함상진 </h1>
            <p class="name-text"> HTML/CSS 개발자 </p>

            <section>
                <h2> ABOUT ME </h2>
                <p class="about-me-text"> </p>
            </section>

            <section>
                <h2> EXPERIENCE </h2>
                <p class="title-text"> Awesome Programming Company </p>
                <p class="year-text"> 2020 - Now </p>
            </section>

        </div>

        <footer>
            <p> copyright CODE LION. All rights reserved  </p>
        </footer>
    </body>
</html>
```

### about-me-text, title-text, year-text 클래스 설정 및 section 태그 설정
- `resume.css`
```css
@import url('https://fonts.googleapis.com/css?family=Montserrat:100,200,300,400,500,600,700,800&display=swap');

*{
    font-family: 'Montserrat';
}

body, h1, h2{
    margin: 0px;
    padding: 0px;
}

body{
    min-width: fit-content;
}

h1{
    font-size: 36px;
    font-weight: bold;
    font-style: italic;
}

h2{
    font-size: 20px;
    font-weight: lighter;
    color: #282828;
    border-bottom: 1px solid #ebebeb;
    margin-bottom: 16px;
    padding-bottom: 5px;
}

.name-text{
    font-size: 17px;
    color: #7c7c7c;
    font-weight: bold;
}

.mainbox{
    width: 610px;
    padding: 30px;
    margin: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid #ebebeb;
    box-shadow: 0 1px 20px 0 rgba(0,0,0,0.1);
}

footer{
    text-align: center;
    background-color: #1e1e1e;
    padding: 20px;
    font-size: 12px;
    color: #919191;
}

section{
    margin-bottom: 24px;
}

.about-me-text{
    font-size: 10px;
    line-height: 16px;
}

.title-text{
    font-size: 11px;
    font-weight: bold;
    color: #282828;
    float: left;
}

.year-text{
    font-size: 11px;
    font-weight: bold;
    color: #282828;
    float: right;
}
```

- `line-height`: 줄 간 간격 설정
- `float`: 한 줄에 정렬을 해주지만, html 문서 안에서 다른 글씨와 겹칠 수 있으므로 가두리를 만들어줘야한다.

<br>

---

<br>

## 11. 뗏목 띄우기

### Experience p태그들 묶어주기, Experience 목록 추가
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="resume.css">
    </head>
    <body>
        <div class="mainbox">
            <h1> 함상진 </h1>
            <p class="name-text"> HTML/CSS 개발자 </p>

            <section>
                <h2> ABOUT ME </h2>
                <p class="about-me-text"> </p>
            </section>

            <section>
                <h2> EXPERIENCE </h2>
                <div class="float-wrap">
                    <p class="title-text"> Awesome Programming Company </p>
                    <p class="year-text"> 2020 - Now </p>
                </div>
                <p class="desc-text"> Front-End Web Developer </p>
                <p class="desc-subtext"> HTML/CSS, JS, React, .. </p>

                <div class="float-wrap">
                    <p class="title-text"> Ministry of Health </p>
                    <p class="year-text"> 2015 - 2018 </p>
                </div>
                <p class="desc-text"> UI/UX Designer </p>
                <p class="desc-subtext"> Web design </p>

                <div class="float-wrap">
                    <p class="title-text"> Preelance Work </p>
                    <p class="year-text"> 2011 - 2015 </p>
                </div>
                <p class="desc-text"> Graphic Designer </p>
                <p class="desc-subtext"> Graphic Design, Editorial Design </p>
            </section>

        </div>

        <footer>
            <p> copyright CODE LION. All rights reserved  </p>
        </footer>
    </body>
</html>
```

### float-wrap, desc-text, desc-subtext 클래스 설정
- `resume.css`
```css
@import url('https://fonts.googleapis.com/css?family=Montserrat:100,200,300,400,500,600,700,800&display=swap');

*{
    font-family: 'Montserrat';
}

body, h1, h2{
    margin: 0px;
    padding: 0px;
}

body{
    min-width: fit-content;
}

h1{
    font-size: 36px;
    font-weight: bold;
    font-style: italic;
}

h2{
    font-size: 20px;
    font-weight: lighter;
    color: #282828;
    border-bottom: 1px solid #ebebeb;
    margin-bottom: 16px;
    padding-bottom: 5px;
}

.name-text{
    font-size: 17px;
    color: #7c7c7c;
    font-weight: bold;
}

.mainbox{
    width: 610px;
    padding: 30px;
    margin: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid #ebebeb;
    box-shadow: 0 1px 20px 0 rgba(0,0,0,0.1);
}

footer{
    text-align: center;
    background-color: #1e1e1e;
    padding: 20px;
    font-size: 12px;
    color: #919191;
}

section{
    margin-bottom: 24px;
}

.name-text{
    font-size: 16px;
    color: #7c7c7c;
    font-style: bold
}

.about-me-text{
    font-size: 10px;
    line-height: 16px;
}

.title-text{
    font-size: 11px;
    font-weight: bold;
    color: #282828;
    float: left;
}

.year-text{
    font-size: 11px;
    font-weight: bold;
    color: #282828;
    float: right;
}

.float-wrap{
    overflow: hidden;
}

.desc-text{
    font-size: 9px;
    color: #282828;
    font-weight: bold;
    float: left;
}

.desc-subtext{
    font-size: 9px;
    color: #282828;
    padding-left: 16px;
    font-weight: bold;
    float: left;
}
```

- `overflow: hidden`: float으로 띄워져있는 요소들을 다 묶어주고, 그 다음에 오는 html 요소들이 겹치지 않게 해준다.

<br>

---

<br>

## 12 이력서 완성하기

### h1, p 태그 div로 묶어주기, sns 이미지 및 링크 추가
- `index.html`
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진의 이력서 </title>
        <link rel="stylesheet" href="resume.css">
    </head>
    <body>
        <div class="mainbox">
            <div class="title-box">
                <h1> 함상진 </h1>
                <p class="name-text"> HTML/CSS 개발자 </p>
            </div>

            <section>
                <h2> ABOUT ME </h2>
                <p class="about-me-text"> </p>
            </section>

            <section>
                <h2> EXPERIENCE </h2>
                <div class="float-wrap">
                    <p class="title-text"> Awesome Programming Company </p>
                    <p class="year-text"> 2020 - Now </p>
                </div>
                <p class="desc-text"> Front-End Web Developer </p>
                <p class="desc-subtext"> HTML/CSS, JS, React, .. </p>

                <div class="float-wrap">
                    <p class="title-text"> Ministry of Health </p>
                    <p class="year-text"> 2015 - 2018 </p>
                </div>
                <p class="desc-text"> UI/UX Designer </p>
                <p class="desc-subtext"> Web design </p>

                <div class="float-wrap">
                    <p class="title-text"> Preelance Work </p>
                    <p class="year-text"> 2011 - 2015 </p>
                </div>
                <p class="desc-text"> Graphic Designer </p>
                <p class="desc-subtext"> Graphic Design, Editorial Design </p>
            </section>

            <div class="sns-wrap">
                <a href="http://facebook.com"> <img class="sns-img" src="images/facebook.png"> </a>
                <a href="http://instagram.com"> <img class="sns-img" src="images/inta.png"> </a>
            </div>

        </div>

        <footer>
            <p> copyright CODE LION. All rights reserved  </p>
        </footer>
    </body>
</html>
```

### sns-img, sns-wrap 클래스 설정
- `resume.css`
```css
@import url('https://fonts.googleapis.com/css?family=Montserrat:100,200,300,400,500,600,700,800&display=swap');

*{
    font-family: 'Montserrat';
}

body, h1, h2{
    margin: 0px;
    padding: 0px;
}

body{
    min-width: fit-content;
}

h1{
    font-size: 36px;
    font-weight: bold;
    font-style: italic;
}

h2{
    font-size: 20px;
    font-weight: lighter;
    color: #282828;
    border-bottom: 1px solid #ebebeb;
    margin-bottom: 16px;
    padding-bottom: 5px;
}

.name-text{
    font-size: 17px;
    color: #7c7c7c;
    font-weight: bold;
}

.mainbox{
    width: 610px;
    padding: 30px;
    margin: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid #ebebeb;
    box-shadow: 0 1px 20px 0 rgba(0,0,0,0.1);
}

footer{
    text-align: center;
    background-color: #1e1e1e;
    padding: 20px;
    font-size: 12px;
    color: #919191;
}

section{
    margin-bottom: 24px;
}

.about-me-text{
    font-size: 10px;
    line-height: 16px;
}

.title-text{
    font-size: 11px;
    font-weight: bold;
    color: #282828;
    float: left;
}

.year-text{
    font-size: 11px;
    font-weight: bold;
    color: #282828;
    float: right;
}

.float-wrap{
    overflow: hidden;
}

.desc-text{
    font-size: 9px;
    color: #282828;
    font-weight: bold;
    float: left;
}

.desc-subtext{
    font-size: 9px;
    color: #282828;
    padding-left: 16px;
    font-weight: bold;
    float: left;
}

.title-box{
    text-align: right;
}

.sns-img{
    width: 12px;
    height: 12px;
    
}

.sns-wrap{
    text-align: right;
}
```

<br>

---

<br>

## 13. 나만의 이력서 정리하기

예제 내용이 아닌 나의 내용으로 정리해보았다.

경력에 적을게 없는 게 너무 슬프다 ...

### index.html
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title> 함상진 이력서 </title>
        <link rel="stylesheet" href="resume.css">
    </head>
    <body>
        <div class="mainbox">
            <div class="title-box">
                <h1> 함상진 </h1>
                <p class="name-text"> Back-end 취준생 </p>
            </div>

            <section>
                <h2> ABOUT ME </h2>
                <p class="about-me-text"> 함상진에 관한 내용 .. </p>
            </section>

            <section>
                <h2> EXPERIENCE </h2>

                <div class="float-wrap">
                    <p class="title-text"> HanKyong National University </p>
                    <p class="year-text"> 2018 - 2024 </p>
                </div>
                <p class="desc-text"> Computer Engineering </p>
                <p class="desc-subtext"> 3.98/4.5 </p>

                <div class="float-wrap">
                    <p class="title-text"> 경력 제목 </p>
                    <p class="year-text"> 경력 기간 </p>
                </div>
                <p class="desc-text"> 경력 직업 </p>
                <p class="desc-subtext"> 경력 내용 </p>
            </section>

            <div class="sns-wrap">
                <a href="https://github.com/hamsangjin"> <img class="sns-img" src="images/github.png"> </a>
                <a href="https://velog.io/@hamsangjin"> <img class="sns-img" src="images/velog.png"> </a>
                <a href="https://www.instagram.com/ham___sj"> <img class="sns-img" src="images/instagram.png"> </a>
            </div>
        </div>

        <footer>
            <p> copyright HamSangJin. All rights reserved  </p>
        </footer>
    </body>
</html>
```

### resume.css
```css
@import url('https://fonts.googleapis.com/css?family=Montserrat:100,200,300,400,500,600,700,800&display=swap');

* {
    font-family: 'Montserrat';
}

body,h1,h2 {
    margin:0px;
    padding:0px;
}

body {
    min-width: fit-content;
}

h1 {
    font-size:36px;
    font-weight: bold;
    font-style: italic;
}

h2 {
    font-size:20px;
    color:#282828;
    font-weight: lighter;
    margin-bottom: 16px;
    border-bottom: 1px solid #ebebeb;
    padding-bottom: 5px;
}

.name-text {
    font-size:16px;
    color:#7c7c7c;
    font-weight: bold;
}

.about-me-text {
    font-size:10px;
    line-height: 16px;
}

.mainbox {
    width: 610px;
    padding: 30px;
    margin: 30px;
    margin-right: auto;
    margin-left: auto;
    border: 1px solid #ebebeb;
    box-shadow: 0 1px 20px 0 rgba(0, 0, 0, 0.1);
}

.title-box {
    text-align: right;
}

section {
    margin-bottom:24px;
}

.float-wrap {
    overflow: hidden;
}

.title-text {
    font-size:11px;
    font-weight: bold;
    color: #282828;
    float: left;
}

.year-text{
    font-size:11px;
    font-weight: bold;
    color: #282828;
    float: right;
}

.desc-text {
    font-size: 9px;
}

.desc-subtext {
    font-size: 9px;
    color:#282828;
    padding-left:16px;
}

.sns-img {
    width:12px;
    height:12px;
}

.sns-wrap {
    text-align:right;
}

footer {
    text-align: center;
    background-color: #1e1e1e;
    padding: 20px;
    font-size: 12px;
    color: #919191;
}
```

### 구현 결과
<img width="689" alt="스크린샷 2024-03-26 21 07 29" src="https://github.com/hamsangjin/TIL/assets/103736614/369e087d-79cf-4d29-9aaa-8336d446b6d1">

