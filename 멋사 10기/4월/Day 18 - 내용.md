# JavaScript

## async/await

`async/await` 문법은 ES8에 해당하는 문법으로서, Promise를 더욱 쉽게 사용할 수 있게 해준다.
- `async`: function 앞에 async를 붙이면 해당 함수는 항상 프라미스를 반환하며, 프라미스가 아닌 값을 반환하더라도 resolved promise로 값을 감싸 반환한다.
- `await`: 자바스크립트는 await 키워드를 만나면 프라미스가 처리될 때까지 기다린다.

- 예제 코드
```javascript
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

const getDog = async () => {
    // await 아래 있는 건 콜백 함수로 묶어서 처리해준다.
    // 즉, sleep이 끝나야 return '멍멍이' 살행
    await sleep(1000);
    return '멍멍이';
};

const getRabbit = async () => {
    await sleep(500);
    return '토끼';
};

const getTurtle = async () => {
    await sleep(3000);
    return '거북이';
};

async function process() {
    // 여러 개의 프로미스를 등록해서 실행했을 때 가장 빨리 끝난것 하나만의 결과값을 가져온다.
    console.time(); // 측정 시작
    const first = await Promise.race([
        getDog(),
        getRabbit(),
        getTurtle()
    ]);
    console.timeEnd(); // 측정 종료
    console.log(first);

    // 여러 개의 프로미스를 동시에 실행시킨다.
    console.time(); // 측정 시작
    const [dog, rabbit, turtle] = await Promise.all([getDog(), getRabbit(), getTurtle()]);
    console.timeEnd(); // 측정 종료

    console.log(dog);
    console.log(rabbit);
    console.log(turtle);
}

process();
```

- `Promise.all`: 여러 개의 프로미스를 동시에 작업을 시작하게 만들어 줌
- `Promise.race`: 여러 개의 프로미스를 등록해서 실행했을 때 가장 빨리 끝난것 하나만의 결과값을 가져온다.

<br>

---

<br>

# HTML과 JavaScript 연동하기

HTML을 사용하면 브라우저에서 우리가 보여주고 싶은 UI를 보여줄 수 있다. <br>
만약에 사용자의 인터랙션에 따라 동적으로 UI를 업데이트하고 싶다면, JavaScript를 연동해야 한다. <br>

보통 인터랙션이 많은 경우에는 `Vanilla JavaScript`를 사용해서 하기에는 코드의 양도 많아지고 관리도 어려운 편이라, `React`, `Vue`, `Angular` 등의 도구를 사용한다.

<br>

## DOM
`DOM(Document Object Model)`은 XML이나 HTML 문서에 접근하기 위한 일종의 인터페이스로, 문서 내의 모든 요소를 정의하고, 각각의 요소에 접근하는 방법을 제공한다. <br>
또한, W3C의 표준 객체 모델이며, 아래와 같이 계층 구조로 표현된다. <br>

<img width="656" alt="스크린샷 2024-04-04 10 46 18" src="https://github.com/hamsangjin/TIL/assets/103736614/d1558810-4722-4218-89e4-ca094f76458f">

<br>
<br>

## 카운터
버튼을 클릭하면 숫자가 올라가거나 내려가는 카운터를 만들어보자. <br>

- event 작성 방법: `소스(대상).이벤트 리스너(ex: clck)(이벤트이름, 이벤트핸들러(함수), [ToCapture(캡처링 여부)])`
  - ex) `document.addEventListener('click', hi)`

<br>

- 예제 코드
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Parcel Sandbox</title>
        <meta charset="UTF-8" />
    </head>
    <body>
        
        <h2 id="number">0</h2>
        <div>
            <button id="increase">+1</button>
            <button id="decrease">-1</button>
        </div>
        <script>
            const number = document.getElementById("number");
            const increase = document.getElementById("increase");
            const decrease = document.getElementById("decrease");

            increase.onclick = () => {
                const current = parseInt(number.innerText, 10);
                number.innerText = current + 1;
            };

            decrease.onclick = () => {
                const current = parseInt(number.innerText, 10);
                number.innerText = current - 1;
            };
        </script>
    </body>
</html>
```

- jquery라이브러리 사용한 예제 코드
```html
<!DOCTYPE html>
<html>
    <head>
        <title>Parcel Sandbox</title>
        <meta charset="UTF-8" />
    </head>
    <body>
        
        <h2 id='number'>0</h2>
        <div>
            <button id='increase'>+1</button>
            <button id='decrease'>-1</button>
        </div>
        <script src="https://code.jquery.com/jquery-3.7.1.min.js" integrity="sha256-/JqT3SQfawRcv/BIHPThkBvs0OEvtFFmqPF/lYI/Cxo=" crossorigin="anonymous"></script>
        <script>
            $('#increase').click(() => {
                $('#number').text(parseInt($('#number').text()) + 1);
            });
            $('#decrease').click(() => {
                $('#number').text(parseInt($('#number').text()) - 1);
            });
        </script>
    </body>
</html>
```

<br>

## 연습 예제
- 1번 예제: 요소(버튼)를 불러와 이벤트 적용시키기
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
    </head>
    <body>
        <button> 클릭1 </button>
        <button> 클릭2 </button>
        <script>
            function changeColor(event){
                const color = prompt("색을 입력하세요.");
                document.body.style.backgroundColor = color;
            }

            function changeColor(event) {
                // PointerEvent: 발생한 이벤트의 추상화된 정보를 알 수 있음
                console.log(event);
                console.log('event target : ' + this);
                console.log(this);
                console.log(event.target);
                const color = prompt("색을 입력하세요.");
                document.body.style.backgroundColor = color;
            }

            // 모든 button을 NodeList로 받음
            let buttonList1 = document.querySelectorAll("button");
            console.log(buttonList1);
        
            for(let i = 0; i < buttonList1.length; i++){
                buttonList1[i].addEventListener("click", changeColor);
            }

            // 모든 button을 HTMLCollection으로 받음
            let buttonList2 = document.getElementsByTagName("button");
            console.log(buttonList2);
        
            for(let i = 0; i < buttonList2.length; i++){
                buttonList2[i].addEventListener("click", changeColor);
            }

            // button 하나만
            let button = document.querySelector("button")
            button.addEventListener("click", changeColor);
        </script>
    </body>
</html>
```

<br>

- 2번 예제: 이벤트 버블링/캡쳐링
  - `이벤트 버블링`: 한 요소에 이벤트가 발생하면 이 요소에 할당된 핸들러가 동작하고, 이어서 부모 요소의 핸들러가 동작하고 최상단의 부모 요소를 만날 때까지 반복되면서 핸들러가 동작하는 현상
  - `이벤트 캡쳐링`: 캡처링은 버블링과는 반대로 최상위 태그에서 해당 태그를 찾아 내려간다.
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>Document</title>
    <style>
      .pink {
        position: relative;
        background-color: pink;
        height: 200px;
        width: 200px;
        display: flex;
        justify-content: center;
        align-items: center;
      }
      .red {
        position: absolute;
        top: 0;
        left: 0;
        background-color: red;
        width: 100px;
        height: 100px;
      }
    </style>
  </head>
  <body>
    <div class="pink">
      <div class="red"></div>
    </div>

    <script>
      let pinkbox = document.querySelector(".pink");
      let redbox = document.querySelector(".red");
      let body = document.querySelector("body");
      let html = document.querySelector("html");

      pinkbox.addEventListener(
        "click",
        function (e) {
            e.stopPropagation();
            alert("pink box!!");
        },
        false
      );
      redbox.addEventListener(
        "click",
        function (e) {
            e.stopPropagation();
            alert("red box");
        },
        false
      );

      body.addEventListener(
        "click",
        function (e) {
            e.stopPropagation();
            alert("body box");
        },
        false
      );

      html.addEventListener(
        "click",
        function (e) {
            e.stopPropagation();
            alert("html box");
        },
        false
      );
    </script>
  </body>
</html>
```

<br>

- 3번 예제: 회원가입 유효성 검사
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <form action="05.html" method="get" id="joinForm">
        아이디 : <input type="text" name="id" id="id"> <br>
        비밀번호 : <input type="password" name="password" id="password"> <br>
        비밀번호확인  : <input type="password" name="checkPassword" id="checkPassword"> <br>
        <input type="submit" value="등록">
    </form>
    <script>
        // 서버에 보내기 전에 미리 유효성검사를 해주는 역할을 자바스크립트가 해준다.
        const join = document.getElementById('joinForm');

        join.addEventListener('submit', (e) =>{
            const [id, pw, chpw] = document.querySelectorAll("input");

            // 기존에 존재하는 id
            db_id = ['aaa', 'bbb', 'ccc'];
            
            if(id.value == ""){
                alert("아이디를 입력해주세요");
                // .focus(): 해당 요소로 이동
                id.focus();
            } else if(pw.value == ""){
                alert("패스워드를 입력해주세요");
                pw.focus();
            } else if(chpw.value == ""){
                alert("비밀번호 확인을 해주세요.");
                chpw.focus();
            } else{
                if(db_id.indexOf(id.value) != -1){
                    alert("중복된 ID입니다.");
                    id.focus();
                } else if(pw.value != chpw.value){
                    alert("비밀번호가 같지 않습니다.");
                    pw.focus();
                } else{
                    alert("회원가입 성공");
                    // action으로 이동하지 마라
                    return;
                }
            }
            // 
            e.preventDefault();
            return;
        })
    </script>
</body>
</html>
``` 
- `preventDefault`: 브라우저가 적용하는 기본 동작을 방지하는 역할을 한다. 기본 동작은 이벤트의 종류에 따라 다르며, submit의 기본동작은 form 데이터를 서버에 전송하고, 페이지를 새로고침하는 부분이다.

<br>

- 4번 예제: TodoList를 추가하고, 클릭하면 해당 todo가 출력되게 해주는 예제
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <ul>
        <li>할일 1</li>
        <li>할일 2</li>
        <li>할일 3</li>
        <li>할일 4</li>
        <li>할일 5</li>
    </ul>

    <label for="todo"> 오늘의 할일 : </label>
    <input type="text" id="todo-value">
    <button id="todoBtn"> 추가 </button>
    <script>
        const todoBtn = document.getElementById('todoBtn');
        const ul = document.querySelector('ul');

        todoBtn.addEventListener('click', (e) => {
            const todo = document.getElementById('todo-value');
            
            const li = document.createElement("li");
            const text = document.createTextNode(todo.value);
            li.appendChild(text);
            ul.appendChild(li);

            // 새로 입력
            todo.value = "";
        });

        ul.addEventListener('click', (e)=>{
            alert(e.target.innerText);
        })
    </script>
</body>
</html>
```
