# 테두리 속성

## border-width, border-style 속성
- `border-width`: 테두리의 너비를 지정하는 스타일 속성으로, 크기 단위의 키워드 사용
- `border-style`: 테두리의 형태를 지정하는 속성

```html
<!DOCTYPE html>
<html>
  <head>
    <title>CSS3 Property Basic</title>
    <style>
        .box {
            border-width: thick;
            border-style: dashed;
            border-color: black;
        }
    </style>
  </head>
  <body>
    <div class="box">
      <h1>Lorem ipsum dolor amet</h1>
    </div>
  </body>
</html>
```

<br>

## border-radius 속성
- `border-radius`: 테두리가 둥근 사각형 또는 원을 만들 수 있다.
```html
<!DOCTYPE html>
<html>
  <head>
      <title>CSS3 Property Basic</title>
      <style>
        .box {
          border: thick dashed black;
          /* border-radius: 왼쪽위 오른쪽위 오른쪽아래 왼쪽아래 */ border-radius: 50px 40px 20px 10px;
        }
      </style>
  </head>
  <body>
      <div class="box">
          <h1>Lorem ipsum dolor amet</h1>
      </div>
  </body>
</html>
```

<br>

---

<br>

# 배경 속성

## background-image, background-size 속성
- `background-image`: 여러 개의 배경을 적용할 경우에는 왼쪽에 위치한 이미지가 앞으로 나오게 되며, 코드를 실행하면 2개의 이미지가 층을 이루어 출력됨
- `background-size`: 그림 크기를 조절할 때 사용하며, 너비와 높이를 설정해줄 수 있다.

```html
<!DOCTYPE html>
<html>
  <head>
      <title>CSS3 Property Basic</title>
      <style>
          body {
              background-image: url('BackgroundFront.png'), url('BackgroundBack.png');
              background-size: 100%;
          }
      </style>
  </head>
  <body>
  </body>
</html>
```

<br>

## background-repeat 속성
- `background-repeat`: 그림이 패턴을 이루어 여러 개 출력되게 하는 속성
  - `repeat-x` 키워드를 적용하면 X축 방향으로 이미지가 반복
  - `repeat-y` 키워드를 적용하면 Y축 방향으로 이미지가 반복
 
```html
<!DOCTYPE html>
<html>
  <head>
      <title>CSS3 Property Basic</title>
      <style>
          body {
              background-image: url('BackgroundFront.png'), url('BackgroundBack.png');
              background-size: 100%;
              background-repeat: no-repeat;
          }
      </style>
  </head>
  <body>
  </body>
</html>
```

<br>

## background-attachment, background-position 속성
- `background-attachment`: 배경 이미지를 어떠한 방식으로 화면에 붙일 것인지 를 지정하는 스타일 속성
  - `scroll 키워드`: 기본 키워드로, 화면 스크롤에 따라 배경 이미지가 함께 이동
  - `fixed 키워드`: 스크롤을 내려도 배경 이미지는 고정
- `background-position`: 2개의 값을 입력하면 각각 X축 위치와 Y축 위치를 적용
  - `background-position: X축크기`
  - `background-position: X축크기 Y축크기`

<br>

---

<br>

# 폰트 속성

## font-size 속성
- `font-size`: 글자의 크기를 지정하는 스타일 속성
  - h1 태그의 기본 크기: 32픽셀
  - p 태그의 기본 크기: 16픽셀
 
```html
<!DOCTYPE html>
<html>
    <head>
        <title>CSS3 Font Property</title>
        <style>
            .a { font-size: 32px; }
            .b { font-size: 2em; }
            .c { font-size: large; }
            .d { font-size: small; }
        </style>
    </head>
    <body>
        <h1>Lorem ipsum</h1>
        <p class="a">Lorem ipsum</p>
        <p class="b">Lorem ipsum</p>
        <p class="c">Lorem ipsum</p>
        <p class="d">Lorem ipsum</p>
    </body>
</html>
```

<br>

## font-family 속성
- `font-family`: 폰트를 지정하는 스타일 속성
  - 일반적으로 한 단어로 이루어진 폰트는 따옴표를 사용하지 않고, 그렇지 않다면 따옴표를 반드시 사용해야 한다.
  - 사용자에겐 해당 폰트가 설치되어있지 않을 수 있으므로, font-famaly 속성을 여러개 사용한다.

```html
<!DOCTYPE html>
<html>
    <head>
        <title>CSS3 Font Property</title>
        <style>
            .font_arial { font-family: '없는 폰트', Arial; }
            .font_roman { font-family: 'Times New Roman', Arial; }
        </style>
    </head>
    <body>
        <h1 class="font_arial">Lorem ipsum</h1>
        <p class="font_roman">Lorem ipsum</p>
    </body>
</html>
```

<br>

## font-style, font-weight 속성
- `font-style`, `font-weight`: 폰트의 기울기 또는 두께를 조정할 수 있다.
  - 일반 폰트의 두께: 400
  - 두꺼운 폰트의 두께: 700
 
```html
<!DOCTYPE html>
<html>
    <head>
        <title>CSS3 Font Property</title>
        <style>
            .font_big { font-size: 2em; }
            .font_italic { font-style: italic; }
            .font_bold { font-weight: bold; }
        </style>
    </head>
    <body>
        <p class="font_big font_italic font_bold">Lorem ipsum dolor amet</p>
    </body>
</html>
```

<br>

## line height 속성
- `line height`: 글자의 높이를 지정할 수 있다.
```html
<!DOCTYPE html>
<html>
    <head>
        <title>CSS3 Font Property</title>
        <style>
            .font_big { font-size: 2em; }
            .font_italic { font-style: italic; }
            .font_bold { font-weight: bold; }
            .font_center { text-align: center; }
            .button {
                width: 150px;
                height: 70px;
                background-color: #FF6A00;
                border: 10px solid #FFFFFF;
                border-radius: 30px;
                box-shadow: 5px 5px 5px #A9A9A9;
            }
            .button > a {
                display: block;
                line-height: 70px;
            }
        </style>
    </head>
    <body>
        <div class="button">
            <a href="#" class="font_big font_italic font_bold font_center">Click</a>
        </div>
    </body>
</html>
```

<br>

## text-align 속성
- `text-align`: 글자의 정렬과 관련된 속성

```html
<!DOCTYPE html>
<html>
    <head>
        <title>CSS3 Font Property</title>
        <style>
            .font_big { font-size: 2em; }
            .font_italic { font-style: italic; }
            .font_bold { font-weight: bold; }
            .font_center { text-align: center; }
            .font_right { text-align: right; }
        </style>
    </head>
    <body>
        <p class="font_big font_italic font_bold font_center">Lorem ipsum dolor amet</p>
        <p class="font_bold font_right">2019.02.14</p>
        <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse sed nisi velit. Phasellus suscipit pellentesque leo, vel efficitur mi placerat sed. Fusce vel condimentum leo, at iaculis ante. Suspendisse posuere, dolor non tempor ullamcorper, nisl elit facilisis erat, nec lacinia augue erat id lacus. Curabitur mollis, justo nec lobortis hendrerit, libero nunc aliquam lacus, ut tristique sem nunc eu metus. Quisque varius orci eu felis sollicitudin malesuada. Vivamus pretium ligula velit, eget facilisis enim imperdiet ac.</p>
    </body>
</html>
```

<br>

## text-decoration 속성
- `text-decoration`: a 태그에 href 속성을 사용하면 글자에 밑줄이 생긴다.
- 하지만 일반적인 웹 페이지에서 링크에 밑줄이 없는 이유는 text-decoration 속성으로 밑줄을 제거했기 때문이다.

```html
<!DOCTYPE html>
<html>
    <head>
        <title>CSS3 Font Property</title>
        <style>
            a { text-decoration: none; }
        </style>
    </head>
    <body>
        <h1>
            <a href="#">Lorem ipsum dolor amet</a>
        </h1>
    </body>
</html>
```

<br>

---

<br>

# 위치/Position 속성
- `절대 위치 좌표`: 요소의 X좌표와 Y좌표를 설정해 절대 위치를 지정
  - `absolute`: 절대 위치 좌표를 설정
  - `fixed`: 화면을 기준으로 절대 위치 좌표를 설정
- `상대 위치 좌표`: 요소를 입력한 순서를 통해 상대적으로 위치를 지정
- 상대 위치 좌표를 사용할 때는 position 속성에 `static 키워드` 또는 `relative 키워드`를 적용
  - `static 키워드`: 태그가 “위에서 아래로”와 “왼쪽에서 오른쪽으로” 순서에 맞게 배치
  - `relative 키워드`: static 키워드로 초기 위치가 지정된 상태에서 상하좌우로 이동 가능
 
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      div {
        width: 800px;
        height: 1000px;
        border: black solid 5px;
        margin: 30px;
        display: inline-block;
        color: white;
      }
      .scroll {
        overflow: visible;
        display: block;
      }
      .static {
        width: 150px;
        height: 150px;
        border: none;
        background: gray;
        text-align: center;
        line-height: 150px;
      }
      .sticky {
        width: 150px;
        height: 150px;
        border: none;
        background: blue;
        text-align: center;
        line-height: 150px;
        position: sticky;
        top: 50px;
      }
      .fixed {
        width: 150px;
        height: 150px;
        border: none;
        background: red;
        text-align: center;
        line-height: 150px;
        position: fixed;
        bottom: 150px;
      }
    </style>
  </head>
  <body>
    <div class="scroll">
      <div class="static">.static</div>
      <div class="sticky">.sticky</div>
      <div class="fixed">.fixed</div>
    </div>
    <div class="scroll">
      <div class="static">.static</div>
      <div class="sticky">.sticky</div>
    </div>
    <div class="scroll"></div>
  </body>
</html>
```

<br>

## z-index 속성
- `z-index`: 객체 순서를 변경하고 싶을 때 사용하며, z-index 속성에 숫자를 적용하며 숫자가 클 수록 앞에 위치하게 된다.

```html
<!DOCTYPE html>
<html>
  <head>
    <title>CSS3 Property Basic</title>
    <style>
        .box {
            width: 100px; height: 100px;
            position: absolute;
        }
        .box:nth-child(1) {
            background-color: red;
            left: 10px; top: 10px;
            z-index: 100;
        }
        .box:nth-child(2) {
            background-color: green;
            left: 50px; top: 50px;
            z-index: 10;
        }
        .box:nth-child(3) {
            background-color: blue;
            left: 90px; top: 90px;
            z-index: 1;
        }
    </style>
  </head>
  <body>
      <div class="box red"></div>
      <div class="box green"></div>
      <div class="box blue"></div>
  </body>
</html>
```

<br>

## float 속성
- `float`: 요소를 띄워주는 속성

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>CSS속성 - 배치</title>
    <style type="text/css">
      .box1 {
        width: 200px;
        height: 200px;
        background-color: #438eb9;
        float: left;
      }
      .box2 {
        width: 200px;
        height: 200px;
        background-color: yellow;
        float: left;
      }

      .box3 {
        width: 200px;
        height: 200px;
        background-color: #835eda;
        clear:both
      }
    </style>
  </head>
  <body>
    <div class="box1">box1</div>
    <div class="box2">box2</div>
    <div class="box3">box3</div>
  </body>
</html>
```

- `clear: both`: 이 요소는 float을 적용 안 하겠다라는 뜻

<br>

---

<br>

# 그림자/text-shadow 속성
- `text-shadow`, `box-shadow`: 박스와 텍스트에 그림자를 부여해주는 속성
  - CSS3 Generator 툴 : [http://css3generator.com](http://css3generator.com)
  - 툴을 사용하면 `box-shadow`와 `text-shadow` 속성을 쉽게 생성

```html
<!DOCTYPE html>
<html>
  <head>
      <title>CSS3 Property Basic</title>
      <style>
          h1 {
              border: 3px solid black;
              text-shadow: 5px 5px 5px black;
              box-shadow: 10px 10px 30px black
          }
      </style>
  </head>
  <body>
      <h1>Lorem ipsum dolor amet</h1>
  </body>
</html>
```
