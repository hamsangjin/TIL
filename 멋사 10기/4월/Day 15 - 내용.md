# 로또 번호 추첨기

## 1. javascript를 사용하는 방법

### 1. html

보통 html 파일을 다 불러오고 시작하며, `body태그`가 끝나는 지점에 작성해준다.

작성하는 방법은 `<script>` 태그를 이용한다.
```html
<body>

...

<script>

</script>
</body>

```

### 2. js 파일
`<script>` 태그 내에 src 속성을 이용해 파일을 연결해준다.
```javascript
<body>

...

<script scr='myScript.js'></script>
</body>
```

### document.write()
말그래도 괄호 안에 들어가있는 것을 페이지에 쓰는(출력하는) 함수다.
```javascript
document.write('안녕하세요')
```

<br>

## 2. 세미콜론과 주석

### 세미콜론
보통 해당 코드가 끝났다는 의미로 `;`를 붙여주지만 js는 유연한 코드이기 때문에 적지 않아도 알아서 인식해준다. <br>
하지만 두 명령어를 한 줄에 적을 경우에는 `;`로 구분해줘야한다.
```javascript
document.write('안녕하세요')
document.write('안녕하세요');document.write('안녕하세요')
```

### 주석
`//`나 `/* ... */`를 이용해 주석을 처리한다.
```javascript
// 주석 1
/*
  주석처리입니다
*/
```

<br>

## 3. 데이터 상자 만들기

### var
변수를 선언할 땐 variable의 앞 세글자를 따서 `var`로 선언해준다.

```javascript
var name = '엄준식';
document.write(name);
document.write(typeof name);

var intVal = 30;
document.write(intVal);
document.write(typeof intVal);

var floatVal = 4.5;
document.write(floatVal);
document.write(typeof floatVal);

var boolVal = true;
document.write(boolVal);
document.write(typeof boolVal);
```

최신 버전에서는 `let`과 `const`로도 선언할 수 있다.

### 문자열(String)
`''`나 `""`로 묶어주는 걸로 표현할 수 있다.

### 숫자(int, float)
정수형(int)과 실수형(float)으로 나뉜다.

### 불(bool)
`true`와 `false`로 나뉜다.

### typeof 데이터
자료형을 알아내는 함수

<br>

## 로또 번호 추첨기 1
1 ~ 45번 공 중 6개 뽑아주는 웹서비스이다 <br>

먼저 공 하나를 뽑는 로직을 짜보자
```javascript
var num = Math.random() * 45 + 1;
var ball1 = parseInt(num);
document.write(ball1);
```

<br>

## 로또 번호 추첨기 2

6개의 공을 배열을 사용해 뽑아보자.

- 배열 선언 방법
```
var lotto = [1, 2, 3, 4, 5, 6];  // 선언
lotto.push(7);                    // 배열에 값 추가
document.write(lotto[1]);
document.write(lotto);
```

- 6개 공 뽑기
```javascript
var lotto = [];

lotto.push(parseInt(Math.random() * 45 + 1));
lotto.push(parseInt(Math.random() * 45 + 1));
lotto.push(parseInt(Math.random() * 45 + 1));
lotto.push(parseInt(Math.random() * 45 + 1));
lotto.push(parseInt(Math.random() * 45 + 1));
lotto.push(parseInt(Math.random() * 45 + 1));

document.write(lotto);
```

<br>

## 로또 번호 추첨기 3

> `Don't Repeat Yourself`: 정보의 반복을 줄이는 것을 목표로 하는 소프트웨어 개발의 기본 원칙

### 반복문 - for
```javascript
for(시작; 끝; 증가){
  반복 코드
}

for(var i = 0; i < 6; i++){
  반복 코드
}
```

- 6개 공 뽑기(반복문)
```javascript
var lotto = [];
for (var i = 0; i < 6; i++){
    lotto.push(parseInt(Math.random() * 45 + 1));
}
document.write(lotto);
```

<br>

## 로또 번호 추첨기 4
현재는 중복된 값이 들어갈 수 있는 가능성이 있으므로 조건문을 이용해 공을 뽑아보자.

### 조건문 - if
```
if(조건){
  참일 경우
} else{
  거짓일 경우
}
```

```javascript
var lotto = [];
for (var i = 0; i < 6; i++){
    var num = parseInt(Math.random() * 45 + 1);
    if (lotto.indexOf(num) == -1) {
        lotto.push(num);
    }
}
document.write(lotto);
```

- `.indexOf(값)`: 값이 있으면 값의 위치, 없으면 -1 반환하는 함수

<br>

## 로또 번호 추첨기 5

만약 중복된 값이 있으면 공이 하나 안 들어가게 되니, `for문`이 아닌 `while문`으로 작성해보자.

### 반복문 - while
```javascript
while (조건) {
  반복 코드
}
```

```javascript
var lotto = [];
while (lotto.length < 6){
    var num = parseInt(Math.random() * 45 + 1);
    if (lotto.indexOf(num) == -1) {
        lotto.push(num);
    }
}
document.write(lotto);
```

- `.length`: 배열의 길이

<br>

## 로또 번호 추첨기 6

로또 번호를 오름차순으로 정렬하고, 로또 번호 공처럼 꾸며서 출력해보자.

- `.sort()`: 배열 값을 정렬해주는 함수로, 그냥 .sort()를 하면 사전순으로 정렬되며, 숫자순으로 정렬하려면 `.sort((a,b) => a-b)`라고 작성해주면 된다.

- html 파일
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>로또 번호 추첨기</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <h1>로또 번호 추첨기</h1>
    <script>
        var lotto = [];
        while (lotto.length < 6){
            var num = parseInt(Math.random() * 45 + 1);
            if (lotto.indexOf(num) == -1) {
                lotto.push(num);
            }
        }
        lotto.sort((a,b) => a-b);
        document.write("<div class='ball ball1'>" + lotto[0] + "</div>");
        document.write("<div class='ball ball2'>" + lotto[1] + "</div>");
        document.write("<div class='ball ball3'>" + lotto[2] + "</div>");
        document.write("<div class='ball ball4'>" + lotto[3] + "</div>");
        document.write("<div class='ball ball5'>" + lotto[4] + "</div>");
        document.write("<div class='ball ball6'>" + lotto[5] + "</div>");
    </script>
</body>
</html>
```

- css 파일
```css
.ball {
    float: left;
    width: 60px;
    height: 60px;
    line-height: 60px;
    font-size: 28px;
    border-radius: 100%;
    text-align: center;
    vertical-align: middle;
    color: #fff;
    font-weight: 500;
    text-shadow: 0px 0px 3px rgba(73, 57, 0, .8);
    margin-right: 6px;
}

.ball1 {
    background: #fbc400;
}
.ball2 {
    background: #69c8f2;
}
.ball3 {
    background: #ff7272;
}
.ball4 {
    background: #aaa;
}
.ball5 {
    background: #b0d840;
}
.ball6 {
    background: #c7c7c7;
}
```

<br>

---

<br>

# 자소서 글자수 계산기

## DOM
> `DOM(Document Object Model)`: 웹화면을 구성하는 html코드를 쉽게 접근할 수 있게 만든 모델

<br>

## 자소서 글자수 계산기 1

```javascript
<body class="container">
    <h1>자기소개</h1>
    <textarea class="form-control" rows="3" id="jasoseol">저는 인성 문제가 없습니다.</textarea>
    <script>
        var content = document.getElementById('jasoseol').value;
        console.log(content);
    </script>
</body>
```

- `document.getElementById()`: 주어진 문자열과 일치하는 `id` 속성을 가진 요소를 찾고, 이를 나타내는 Element 객체를 반환
  - `value` or `innerHTML`: 태그 안쪽에 있는 값만 따로 가져올 수 있음
- `console.log('값')` : 콘솔 화면에 값을 출력하는 방법

<br>

## 자소서 글자수 계산기 2

가져온 문자열의 길이를 확인해보자

```javascript
<body class="container">
    <h1>자기소개</h1>
    <textarea class="form-control" rows="3" id="jasoseol">저는 인성 문제가 없습니다.</textarea>
    <script>
        var content = document.getElementById('jasoseol').value;
        console.log(content.length);
    </script>
</body>
```

<br>

## 자소서 글자수 계산기 3

문자열의 길이를 웹 화면 상에도 원하는 양식으로 출력해주자.

```javascript
<body class="container">
    <h1>자기소개</h1>
    <textarea class="form-control" rows="3" id="jasoseol">저는 인성 문제가 없습니다.</textarea>
    <span id="count"> (0/200) </span>
    <script>
        var content = document.getElementById('jasoseol').value;
        document.getElementById('count').innerHTML = '(' + content.length + '/200)';
    </script>
</body>
```

<br>

## 자소서 글자수 계산기 4

출력하는 방식을 함수를 이용해 출력하자.

- 함수
```javascript
function 함수이름(){
  명령어1;
  명령어2;
}
```

```javascript
<body class="container">
    <h1>자기소개</h1>
    <textarea class="form-control" rows="3" id="jasoseol">저는 인성 문제가 없습니다.</textarea>
    <span id="count"> (0/200) </span>
    <script>
        function counter() {
            var content = document.getElementById('jasoseol').value;
            document.getElementById('count').innerHTML = '(' + content.length + '/200)';
        }
        counter();
    </script>
</body>
```

## 자소서 글자수 계산기 5

글자를 쓸 때마다 자동으로 글자 수를 세보도록 해보고싶다. <br>
이때, 이벤트를 이용하여 키보드를 누를 때마다 셀 수 있다.

```javascript
<textarea 이벤트=이벤트핸들링> </textarea>
```

```javascript
<body class="container">
    <h1>자기소개</h1>
    <textarea onkeydown='counter()' class="form-control" rows="3" id="jasoseol">저는 인성 문제가 없습니다.</textarea>
    <span id="count"> (0/200) </span>
    <script>
        function counter() {
            var content = document.getElementById('jasoseol').value;
            document.getElementById('count').innerHTML = '(' + content.length + '/200)';
        }
        counter();
    </script>
</body>
```

- `onkeydown`: 사용자가 키보드의 키를 눌렀을 때 발생

<br>

## 자소서 글자수 계산기 6
 
200자가 넘어지면 더이상 안 써지도록 해보자. <br>
200자가 넘었다는 건 조건문으로 확인하고, 200자 이후는 잘라버리는 식으로 해보면 될거 같다.

```javascript
<body class="container">
    <h1>자기소개</h1>
    <textarea onkeydown='counter()' class="form-control" rows="3" id="jasoseol">저는 인성 문제가 없습니다.</textarea>
    <span id="count"> (0/200) </span>
    <script>
        function counter() {
            var content = document.getElementById('jasoseol').value;
            if (content.length > 200){
                content = content.substring(0, 200);
                document.getElementById('jasoseol').value = content;
            }
            document.getElementById('count').innerHTML = '(' + content.length + '/200)';
        }
        counter();
    </script>
</body>
```

<br>

---

<br>

# 미니 스타크래프트

## JQuery 시작하기

- `JQuery`: 자바스크립트를 쉽게 사용할 수 있게 해주는 라이브러리로 장점은 아래와 같다.
  - 간결한 문법
  - 편리한 API
  - 크로스 브라우징

[code.jquery.com](code.jquery.com)에 들어가서 제일 최근 버전의 `minified` 코드를 복사하면 인터넷 상에 올라와있는 jquery라이브러리를 사용할 수 있다.

- 기본 문법: `$(선택자).행위;`

```html
<body>
    <h1>jQuery 기초</h1>
    <textarea id="content">jQuery를 배워보자</textarea>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        console.log($('#content').val());
    </script>
</body>
```

- 기존에 특정 id의 값을 가져올 땐 `document.getElementById('').value;`처럼 길게 사용했지만 jquery를 사용하면 이렇게 간결하게 표현가능하다.

<br>

## JQuery Event

기존에 사용했던 이벤트를 Jquery로 사용해보자

```html
<body>
    <h1>jQuery 이벤트</h1>
    <button id="click">클릭</button>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        function hello() {
            console.log('hello');
        }
        $('#click').click(hello);
    </script>
</body>
```

- 기존 js로 작성했을 경우: `<button id="click" onclick="hello();"> 클릭 </button>`

<br>

## 익명 함수

`익명 함수`는 이름이 없는 함수를 의미하며, 보통 일회성으로 사용하는 경우에 사용한다.

```html
<body>
    <h1>jQuery 이벤트</h1>
    <button id="click">클릭</button>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        $('#click').click(function() {
            console.log('hello');
        });
    </script>
</body>
```

<br>

## 미니 스타크래프트 1

우리가 만들 미니 스타크래프트는 드론을 클릭하면 침이 발사되고, 벙커를 파괴하는 식이다. <br>
먼저 드론을 클릭 이벤트를 만들어보자.

```html
<body>
    <div class='background'>
        <img id='drone' src="drone.png" alt="drone">
        <img id='spit' src="spit.png" alt="spit">
        <img id='bunker' src="bunker.png" alt="bunker">
    </div>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        $('#drone').click(function(){
            console.log('침 발사');
        });
    </script>
</body>
```

<br>

## 미니 스타크래프트 2

이제 드론을 클릭했을 때 침이 나타나게 해보자.

```html
<body>
    <div class='background'>
        <img id='drone' src="drone.png" alt="drone">
        <img id='spit' src="spit.png" alt="spit">
        <img id='bunker' src="bunker.png" alt="bunker">
    </div>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        $('#drone').click(function(){
            $('#spit').fadeIn();
        });
    </script>
</body>
```

<br>

## 미니 스타크래프트 3

이제 침이 발사되게 해보자.

```html
<body>
    <div class='background'>
        <img id='drone' src="drone.png" alt="drone">
        <img id='spit' src="spit.png" alt="spit">
        <img id='bunker' src="bunker.png" alt="bunker">
    </div>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        $('#drone').click(function(){
            $('#spit').fadeIn();
            $('#spit').animate({left: '+=250'});
        });
    </script>
</body>
```

- `.anumate`: 총 네가지 요소가 들어갈 수 있고, 변화할 css부분만 필수 사항이므로 이것만 작성해줬다.

<br>

## 미니 스타크래프트 4

이제 침이 발사되고 없애주는 코드를 작성해보자. <br>
침이 안 보이게 설정하고 날라간 위치에서 기존 위치로 변경해준다.

```html
<body>
    <div class='background'>
        <img id='drone' src="drone.png" alt="drone">
        <img id='spit' src="spit.png" alt="spit">
        <img id='bunker' src="bunker.png" alt="bunker">
    </div>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        $('#drone').click(function(){
            $('#spit').fadeIn();
            $('#spit').animate({left: '+=250'});
            $('#spit').fadeOut();
            $('#spit').css({left: '150px'});
        });
    </script>
</body>
```

- `.css`: 변경할 css 코드를 작성

<br>

## 미니 스타크래프트 5

이제 벙커에 hp를 추가해 침에 맞을 때마다 하나씩 깎이게 해주자.

```html
<body>
    <div class='background'>
        <img id='drone' src="drone.png" alt="drone">
        <img id='spit' src="spit.png" alt="spit">
        <img id='bunker' src="bunker.png" alt="bunker">
    </div>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        var hp = 3;
        $('#drone').click(function(){
            $('#spit').fadeIn();
            $('#spit').animate({left: '+=250'});
            $('#spit').fadeOut(function(){
                hp = hp - 1;
                $('#hp').text('HP :' + hp);
            });
            $('#spit').css({left: '150px'});
        });
    </script>
</body>
```

- `callback 함수`를 `익명함수`로 만들어서 사용해서 hp가 깎이는 시점을 잘 맞춰보자

<br>

## 미니 스타크래프트 6

벙커의 hp가 0이 되면 벙커가 사라지게 만들어보자. <br>
구현이 완료됐으니 전체 코드로 적어줬다.
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>스타크래프트</title>
    <style>
        .background {
            position: relative;
            background-image: url('background.png');
            background-size: 500px 330px;
            width: 500px;
            height: 330px;
        }
        #drone {
            position: absolute;
            width: 100px;
            height: 100px;
            top: 100px;
            left: 60px;
        }
        #bunker {
            position: absolute;
            width: 150px;
            height: 150px;
            top: 80px;
            right: 20px;
        }
        #spit {
            display: none;
            position: absolute;
            top: 140px;
            left: 150px;
            width: 50px;
            height: 30px;
            z-index: 2;
        }
    </style>
</head>
<body>
    <h1 id='hp'>HP: 3</h1>
    <div class='background'>
        <img id='drone' src="drone.png" alt="drone">
        <img id='spit' src="spit.png" alt="spit">
        <img id='bunker' src="bunker.png" alt="bunker">
    </div>
    <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
    <script>
        var hp = 3;
        $('#drone').click(function(){
            $('#spit').fadeIn();
            $('#spit').animate({left: '+=250'});
            $('#spit').fadeOut(function(){
                hp = hp - 1;
                $('#hp').text('HP: ' + hp);
                if(hp == 0) {
                    $('#bunker').fadeOut();
                }
            });
            $('#spit').css({left: '150px'});
        });
    </script>
</body>
</html>
```

<br>

---

<br>

# 기념일 계산기

## 객체 알아보기
객체엔 속성이 들어있고 속성은 key와 value로 나뉜다. <br>
이때, value엔 문자열이나 숫자가 들어갈 수도 있지만, 함수(메소드)나 또다른 객체가 들어갈 수도 있다.

- 객체 선언
```javascript
var person = {
            name: 'jocoding',
            sayHello: function() { console.log('hello'); }
}
```

- 객체 속성 호출
```javascript
console.log(person.name);
person.sayHello();
```

<br>

## Date 객체 알아보기
- Date 객체 생성 방법
```javascript
var now = new Date();
```

- 사용 예제
```javascript
var now = new Date();

console.log(now.getFullYear());    // 년 정보를 가져오는 메소드
console.log(now.getMonth()+1);     // 월 정보를 가져오는 메소드(+1 해줘야함)
console.log(now.getDate());        // 일 정보를 가져오는 메소드
console.log(now.getTime());        // 특정 시간을 기준으로 흐른 시간을 밀리초로 표시하는 메소드

var christmas = new Date('2020-12-25');      // 특정 일의 Date 객체 생성
console.log(christmas);

var ms = new Date(1000);                     // 특정 ms의 Date 객체 생성
console.log(ms);
```

<br>

## 기념일 계산기 1

- `만난 밀리초`: 오늘.getTime() - 사귄날.getTime()
- `만난 일`: 만난 밀리초를 일로 환산

```javascript
var now = new Date();
var start = new Date('2020-06-30');

var timeDiff = now.getTime() - start.getTime();
var day = Math.floor(timeDiff / (1000 * 60 * 60 * 24) + 1);
$('#love').text(day + '일째');
```

<br>

## 기념일 계산기 2

- `남은 밀리초`: 기념일.getTime() - 오늘.getTime()
- `만난 일`: 만난 밀리초를 일로 환산

```javascript
var now = new Date();
var start = new Date('2020-06-30');

var timeDiff = now.getTime() - start.getTime();
var day = Math.floor(timeDiff / (1000 * 60 * 60 * 24) + 1);
$('#love').text(day + '일째');

var valentine = new Date('2024-02-14');
var timeDiff2 = valentine.getTime() - now.getTime();
var day2 = Math.floor(timeDiff2 / (1000 * 60 * 60 * 24) + 1);
$('#valentine').text(day2 + '일 남음');
```

<br>

## 기념일 계산기 3

1000일은 언제인가 계산해주는 기능을 추가해보자

- `1000일의 밀리초`: 사귄날.getTime() + 999일의.getTime()
- `1000일`: new Data(1000일의 밀리초)

```javascript
var now = new Date();
var start = new Date('2020-06-30');

var timeDiff = now.getTime() - start.getTime();
var day = Math.floor(timeDiff / (1000 * 60 * 60 * 24) + 1);
$('#love').text(day + '일째');

var valentine = new Date('2024-02-14');
var timeDiff2 = valentine.getTime() - now.getTime();
var day2 = Math.floor(timeDiff2 / (1000 * 60 * 60 * 24) + 1);
$('#valentine').text(day2 + '일 남음');

var ms = start.getTime() + 999 * (1000 * 60 * 60 * 24);
var thousand = new Date(ms);
var thousandDate = thousand.getFullYear() + '.' + (thousand.getMonth()+1) + '.' + thousand.getDate();
$('#thousand-date).text(thousandDate);
```

<br>

## 기념일 계산기 4

오늘부터 1000일이 며칠 남았는지 계산해주는 기능을 추가해보자.<br>

마지막 기능 추가이므로 전체 코드를 작성해보자.

```javascript
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" integrity="sha384-JcKb8q3iqJ61gNV9KGb8thSsNjpSL0n8PARn9HuZOnIxN0hoP+VmmDGMN5t9UJ0Z" crossorigin="anonymous">
    <title>기념일 계산기</title>
    <style>
        * {
            color: #333333;
        }
        p {
            margin-bottom: 1px;
        }
        .photos {
            margin-top: 50px;
            display: flex;
            justify-content: center;
            align-items: center;
        }
        #duhee {
            width:150px;
            height:150px;
            object-fit:cover;
            border-radius:50%;
            margin-right: 30px;
        }
        #jisook {
            width:150px;
            height:150px;
            object-fit:cover;
            border-radius:50%;
            margin-left: 30px;
        }
        #heart {
            width:50px;
            height:50px;
        }
        .gray {
            color: #a0a0a0;
        }
        .special-day {
            display: flex;
            justify-content: space-between;
        }
        .title {
            display: flex;
            align-items: center;
        }
        .days-left {
            text-align: right;
        }
        .date {
            text-align: right;
            color: #a0a0a0;
        }
    </style>
</head>
<body class="container">
    <section class='photos'>
        <img id='duhee' src="duhee.jpeg" alt="duhee">
        <img id='heart' src="heart.png" alt="heart">
        <img id='jisook' src="jisook.jpeg" alt="jisook">
    </section>
    <div class='container d-flex flex-column justify-content-center align-items-center mt-3'>
        <h3>두희♥지숙</h3>
        <h3 id='love'>0일째</h3>
        <h4 class="date">2020.6.30</h4>
    </div>
    <hr/>
    <section class='special-day'>
        <h3 class='title'>발렌타인 데이</h3>
        <div class='date-box'>
            <p id='valentine' class='days-left'>0일 남음</p>
            <p class='date'>2021.2.14</p>
        </div>
    </section>
    <hr/>
    <section class='special-day'>
        <h3 class='title'>1000일</h3>
        <div class='date-box'>
            <p id='thousand' class='days-left'>0일 남음</p>
            <p id='thousand-date' class='date'>0000.00.00</p>
        </div>
    </section>
    <hr/>
    <script
  src="https://code.jquery.com/jquery-3.5.1.min.js"
  integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0="
  crossorigin="anonymous"></script>
    <script>
        var now = new Date();
        var start = new Date('2020-06-30');

        //우리 몇 일째?
        var timeDiff = now.getTime() - start.getTime();
        var day = Math.floor(timeDiff / (1000 * 60 * 60 * 24) + 1);
        $('#love').text(day + '일째');

        //기념일까지 남은 날짜는?
        var valentine = new Date('2021-02-14');
        var timeDiff2 = valentine.getTime() - now.getTime();
        var day2 = Math.floor(timeDiff2 / (1000 * 60 * 60 * 24) + 1);
        $('#valentine').text(day2 + '일 남음');

        //천일은 언제인가?
        var thousand = new Date(start.getTime() + 999 * (1000 * 60 * 60 * 24));
        var thousandDate = thousand.getFullYear() + '.' + (thousand.getMonth()+1) + '.' + thousand.getDate();
        $('#thousand-date').text(thousandDate);

        //기념일까지 남은 날짜는?
        var timeDiff3 = thousand.getTime() - now.getTime();
        var day3 = Math.floor(timeDiff3 / (1000 * 60 * 60 * 24) + 1);
        $('#thousand').text(day3 + '일 남음');
    </script>
</body>
</html>
```

<br>

---

<br>

# JavaScript

## 개발자 도구
자바스크립트는 브라우저에서 언제든지 사용 가능하다.

개발자 도구를 열어서 console 탭에 아래 코드를 입력해보면 출력되는 걸 확인할 수 있다.
```php
console.log('Hello JavaScript!');
```

<br>

## 변수와 상수

### 1. 변수 - let
변수는 바뀔 수 있는 값을 말하며 아래와 같이 선언한다.
```javascript
let value = 1;
console.log(value);
value = 2; 
console.log(value);
```

### 2. 상수 - const
상수는 한 번 선언하고 값이 바뀌지 않는 값을 말하며, 아래와 같이 선언한다.
```javascript
const a = 1;
// a = 2; 
```

### let(변수), const(상수) 키워드의 특징
- 중복 선언 불가능
- 호이스팅 불가능(함수는 가능)
- block 단위

### 3. 변수, 상수 - var
var를 이용하면 변수나 상수든 뭐든 사용하다
- 중복 선언 가능
- 호이스팅 가능
- 함수 단위

위와 같은 점때문에 ES6 이후엔 `let`, `const`의 사용을 권장하고 있다.

<br>

## 데이터 타입, 연산자, 문자열 붙이기, 조건문
자바와 사용법이 비슷하므로 생략
