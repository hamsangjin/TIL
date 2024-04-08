# 시험 문제: 간단한 투두 리스트 애플리케이션 구현하기

## 목표
본 시험의 목적은 JavaScript, HTML, CSS를 사용하여 간단한 투두 리스트 애플리케이션을 구현하는 것입니다.<br>
이 애플리케이션은 사용자가 할 일을 추가하고, 할 일의 상태를 업데이트하며, 할 일을 삭제할 수 있어야 합니다.

<br>

## 요구사항

### 1. 초기 데이터 설정
애플리케이션은 다음과 같은 형태의 초기 데이터를 가지고 시작해야 합니다.

```javascript
let basicDatas = [
    { id: 1, title: "Todo 1", done: false },
    { id: 2, title: "Todo 2", done: true },
];
```

### 2. 할 일 목록 표시
페이지가 로드되면, 초기 데이터에 있는 할 일 목록을 화면에 표시해야 합니다.

### 3. 할 일 추가 기능
사용자가 텍스트 입력 필드에 할 일을 입력하고 "추가" 버튼을 클릭하면, 할 일 목록에 새 항목이 추가되어야 합니다. <br>
새로운 할 일 항목은 `{ id, title, done }` 형식의 객체로 `basicDatas` 배열에 추가되어야 합니다. <br>
`id`는 현재 할 일 목록에서 가장 큰 `id`에 1을 더한 값으로 설정해야 합니다. <br>
입력 상자에서 엔터 키 입력으로 할 일 추가되도록 구현합니다. <br>

> `Hint`: key 가 눌릴때 일어나는 이벤트 이름은 `keydown` 입니다. <br>
> 엔터키가 눌린 것을 확인 하는 방법은 `event.key === "Enter"` 또는 `event.keyCode === 13` 으로 확인 할 수 있습니다.

### 4. 할 일 상태 업데이트 기능
각 할 일 항목을 클릭하면, 해당 항목의 `done` 상태가 토글되어야 합니다. <br>
`done` 상태에 따라 할 일 텍스트에 취소선이 표시되어야 합니다. 

### 5. 할 일 삭제 기능
각 할 일 항목 옆에는 `삭제 버튼( X )`이 있어야 합니다. <br>
삭제 버튼을 클릭하면, 해당 항목이 목록에서 제거되어야 합니다.

<br>

## 구현 방법
HTML, CSS, JavaScript를 사용하여 위 요구사항을 만족하는 애플리케이션을 구현하세요. <br>
HTML은 할 일을 입력하는 텍스트 필드, 할 일을 추가하는 버튼, 할 일 목록을 표시할 요소를 포함해야 합니다. <br>
CSS를 사용하여 애플리케이션의 스타일을 구성하세요. 할 일 목록의 외관, 할 일 항목의 스타일(완료 상태에 따라 다름), 삭제 버튼의 스타일 등을 정의해야 합니다. <br>
JavaScript에서는 `basicDatas` 배열을 관리하고, 사용자 인터랙션(할 일 추가, 할 일 상태 업데이트, 할 일 삭제)에 따라 DOM을 업데이트하는 로직을 작성해야 합니다.

<br>

## 제출 코드

### HTML
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>To-Do List</title>
    <link rel="stylesheet" href="todo.css">
  </head>
  
  <body>
    <div class="container">
      <h1>To-Do List</h1>
      <input type="text" id="todoInput" placeholder="할일을 입력해주세요.">
      <button id="addButton">추가</button>
      <hr>
      <ul id="todoList"></ul>
    </div>

    <script src="todo.js"></script>
  </body>
</html>
```

<br>

### CSS
```css
body {
    display: flex;
    justify-content: center;
    background-color: #f0f0f0;
    margin: 0;
    font-family: Arial, sans-serif;
}

.container {
    width: 300px;
    margin-top: 50px;
    padding: 20px;
    border: 1px solid #ccc;
    border-radius: 5px;
    background-color: white;
}

h1 {
    color: white;
    background-color: black;
    padding: 10px 0;
    text-align: center;
}

input {
    width: 200px;
    padding: 8px;
    margin-bottom: 10px;
}

button {
    width: 60px;
    padding: 8px;
    margin-left: 10px;
}

ul {
    list-style-type: none;
    padding: 0;
}

li {
    margin-bottom: 5px;
    position: relative;
    background-color: white;
    padding: 8px 10px;
    word-wrap: break-word;
    position: relative;
    overflow: hidden;
}

.done {
    text-decoration: line-through;
    color: gainsboro;
}

.delete-button {
    display: none;
    margin-left: 10px;
    top: 50%;
    right: 5px;
    transform: translateY(-50%);
    background-color: transparent;
    border: none;
    cursor: pointer;
    position: absolute;
}

li:hover .delete-button {
    display: inline-block;
}

li:hover {
    background-color: #f0f0f0;
}
```

<br>

### Javascript
```javascript
let basicDatas = [
    { id: 1, title: "Todo 1", done: false },
    { id: 2, title: "Todo 2", done: true }
];
  
const todoList = document.getElementById('todoList');
const todoInput = document.getElementById('todoInput');
const addButton = document.getElementById('addButton');
  
// 할 일의 상태를 토글하는 함수
function toggleTodoStatus(id) {
    for (let i = 0; i < basicDatas.length; i++) {
        if (basicDatas[i].id === id) {
            basicDatas[i].done = !basicDatas[i].done;
            renderTodos();
            break;
        }
    }
}

// 할 일을 삭제하는 함수
function deleteTodo(id) {
    const newTodos = [];
    for (let i = 0; i < basicDatas.length; i++) {
            if (basicDatas[i].id !== id) {
                newTodos.push(basicDatas[i]);
            }
    }
    basicDatas = newTodos;
    renderTodos();
}
  
// 새로운 할 일을 추가하는 함수
function addTodo() {
    const newTodoTitle = todoInput.value;

    // 할 일 제목이 비어 있지 않은 경우에만 처리하기
    if (newTodoTitle !== '') {
    
        // id 설정
        let newId;
        if (basicDatas.length > 0) {
            let maxId = 0;
            for (let i = 0; i < basicDatas.length; i++) {
                if (basicDatas[i].id > maxId) {
                    maxId = basicDatas[i].id;
                }
            }
            newId = maxId + 1;
        } else {
            newId = 1;
        }

    const newTodo = { id: newId, title: newTodoTitle, done: false };
    basicDatas.push(newTodo);
    renderTodos();
    todoInput.value = '';
    }
}

addButton.addEventListener('click', addTodo);

todoInput.addEventListener('keydown', (event) => {
    if (event.key === 'Enter') {
        addTodo();
    }
});

// 할 일 목록을 UI에 렌더링하는 함수
function renderTodos() {
    todoList.innerHTML = '';
  
    basicDatas.forEach(todo => {
        // li 생성해서 내용 반영해주기
        const listItem = document.createElement('li');
        listItem.textContent = todo.title;
        if (todo.done) {
            listItem.classList.add('done');
        }
        listItem.addEventListener('click', () => toggleTodoStatus(todo.id));
  
        // 삭제 버튼 추가
        const deleteButton = document.createElement('button');
        deleteButton.classList.add('delete-button');
        deleteButton.textContent = '❌';
        deleteButton.addEventListener('click', () => {
            deleteTodo(todo.id);
        });
        
        // 전체 리스트에 추가해주기
        listItem.appendChild(deleteButton);
        todoList.appendChild(listItem);
    });
}

renderTodos();
```

<br>

---

<br>

# 동물로 보는 MBTI 테스트 만들기

## HTML
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- 검색엔진 최적화 -->
    <title> 동물로 보는 MBTI 테스트 </title>
    <meta name="description" content="나와 성격이 닮은 동물을 MBTI를 기반으로 찾아주는 테스트입니다.">
    <meta property="og:type" content="website"> 
    <meta property="og:title" content="동물로 보는 MBTI 테스트">
    <meta property="og:description" content="나와 성격이 닮은 동물을 MBTI를 기반으로 찾아주는 테스트입니다.">
    <meta property="og:image" content="https://sangjin-mbti-test.netlify.app/lion.png">
    <meta property="og:url" content="https://sangjin-mbti-test.netlify.app/">

    <!-- bootstrap을 이용하기 위한 설정 -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous">
    <link rel="stylesheet" href="style.css">
</head>

<body class="container">
    <!-- 시작화면 -->
    <article class="start mx-auto">
        <h1 class="mt-5 text-center font-weight-bold">동물로 보는 MBTI 테스트</h1>
        <h2 class="text-center mt-3 font-weight-lighter"> 나는 어떤 동물일까 ? </h2>
        <img src="img/lion.png" id="start-img" alt="">
        <button type="button" class="start-btn btn btn-primary mt-5" onclick='start();'> 테스트 시작</button>
    </article>

    <!-- 문제화면 -->
    <article class="question">
        <div class="progress mt-5">
            <div class="progress-bar" role="progressbar" style="width: calc(100/12*1%)" aria-valuenow="25"></div>
        </div>
        <h2 id="title" class="text-center mt-5"> 문제 </h2>
        <input id="type" type="hidden" value="EI">
        <button id="A" type="button" class="btn btn-primary mt-5">Primary</button>
        <button id="B" type="button" class="btn btn-primary mt-5">Primary</button>
    </article>

    <!-- 결과 화면 -->
    <article class="result">
        <img class="rounded-circle mt-5" id="img" src="img/lion.png" alt="animal">
        <h2 id="animal" class="text-center mt-5 font-weight-bold">동물 이름</h2>
        <h3 id="explain" class="text-center mt-5 mb-5 font-weight-lighter">설명</h3>

        <!-- SNS 공유하기 -->
        <div class="share a2a_kit a2a_kit_size_32 a2a_default_style">
            <a class="a2a_button_facebook"></a>
            <a class="a2a_button_twitter"></a>
            <a class="a2a_button_whatsapp"></a>
            <a class="a2a_dd"></a>
        </div>
    </article>

    <!-- 카카오 애드핏 설정 -->
    <article class="kakao_ad mt-5">
        <ins class="kakao_ad_area" style="display:none;"
            data-ad-unit = "DAN-IA73aVp3qNvIew13"
            data-ad-width = "320"
            data-ad-height = "100"></ins>
        <script type="text/javascript" src="//t1.daumcdn.net/kas/static/ba.min.js" async></script>
    </article>

    <!-- 코드라이언 배너 추가 -->
    <a class="mt-5 banner" href="https://www.codelion.net/catalog?utm_source=animal_test&ytm_medium=web_lecture&utm_campaign=hamsangjin">
        <img class="banner-img" src="img/banner.png" alt="배너">
    </a>
    <!--  -->

    <!-- 카카오 애드핏 연동 -->
    <script type="text/javascript" src="//t1.daumcdn.net/kas/static/ba.min.js" async></script>

    <!-- mbti 결과 저장 -->
    <input type="hidden" id="EI" value="0">
    <input type="hidden" id="SN" value="0">
    <input type="hidden" id="TF" value="0">
    <input type="hidden" id="JP" value="0">

    <!-- bootstrap을 이용하기 위한 설정 -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js" integrity="sha384-DfXdz2htPH0lsSSs5nCTpuj/zy4C+OGpamoFVy38MVBnE+IbbVYUew+OrCXaRkfj" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js" integrity="sha384-ho+j7jyWK8fNQe+A12Hb8AhRq26LrZ/JpcUGGOn+Y7RsweNrtN/tE3MoK7ZeZDyx" crossorigin="anonymous"></script>

    <script>
        let num = 1;

        // 문제 정보
        let q = {
            1: {"title": "친구: 오늘 내 친구가 같이 놀자는데 괜찮아 ?", "type":"EI", "A":"오히려 좋아", "B":"안돼. 절대 싫어"},
            2: {"title": "친구: 야 나 오늘 너무 아파서 못 나갈 거 같아 다음에 보자 ㅜㅜ", "type":"EI", "A":"아 오늘은 그럼 다른 애들 만나야겠다", "B":"밖에 가기 싫었는데 오히려 좋아"},
            3: {"title": "친구: 야 기분 전환할 겸 놀러가자", "type":"EI", "A":"어디로 갈까 ??", "B":"무슨 소리야 힘들어 집에서 쉴래"},
            4: {"title": "사과하면 떠오르는 것은 ?", "type":"SN", "A":"빨갛고 맛있는 과일", "B":"백설공주 ? 아이폰 ?"},
            5: {"title": "풍선을 보면 떠오르는 생각은 ?", "type":"SN", "A":"저 풍선 진짜 빵빵하네 불기 힘들었겠다", "B":"헬륨 마시고 목소리 변하는거 궁금하다"},
            6: {"title": "시험이 다가오는데 공부가 손에 안잡힌다면 ?", "type":"SN", "A":"지난 시험 문제는 어떻게 나왔더라 ..", "B":"시험이 없는 세상은 어떨까?"},
            7: {"title": "친구: 하.. 나 오늘 헤어졌어.. 위로 좀 해주라", "type":"TF", "A":"사람은 사람으로 잊는거래 힘내", "B":"괜찮아? 너무 힘들겠다 기다려봐 내가 전화할게 !!"},
            8: {"title": "친구: 나 이빨 10개 뽑히는 꿈 꿨어", "type":"TF", "A":"그래? 이빨 뽑히는 꿈은 어떤 뜻이지 ?", "B":"헐 무서웠겠다.. 나였으면 바로 기절했을듯 ㅜ"},
            9: {"title": "친구: 야 나 늦을거 같아ㅜㅜ 눈 앞에서 버스를 놓쳐서 택시 탔는데 차가 막히네 ㅜㅜ", "type":"TF", "A":"알겠어 기다리고 있을게(이유가 타당하네)", "B":"알겠어 기다리고 있을게(사과는 ?)"},
            10: {"title": "친구: 뭐하냐 ~ 심심한데 나와", "type":"JP", "A":"갑자기 ?", "B":"좋지 ~ 바로 나갈게"},
            11: {"title": "친구: 오늘 뭐 먹을까 ?", "type":"JP", "A":"여기는 이 식당이 맛있대 거기로 가자 ", "B":"저기 맛있겠다 !! 저기로 가자"},
            12: {"title": "약속 시간 1시간 전 나의 행동은?", "type":"JP", "A":"이미 준비를 마치고 일찍 나갈 준비를 하는중", "B":"이제 막 일어나서 준비하는중"},
        }

        // 결과에 따라 반환할 정보
        let result = {
            "ISTJ": {"animal": "느리지만 의지가 확고한 거북이", "explain": "ISTJ 유형은 가장 다수의 사람이 속하는 성격 유형으로 인구의 대략 13%를 차지합니다.<br><br>청렴결백하면서도 실용적인 논리력과 헌신적으로 임무를 수행하는 성격으로 묘사되기도 하는 이들은, 가정 내에서뿐 아니라 법률 회사나 법 규제 기관 혹은 군대와 같이 전통이나 질서를 중시하는 조직에서 핵심 구성원 역할을 합니다.<br><br>이 유형의 사람은 자신이 맡은 바 책임을 다하며 그들이 하는 일에 큰 자부심을 가지고 있습니다.<br><br>또한, 목표를 달성하기 위해 시간과 에너지를 허투루 쓰지 않으며, 이에 필요한 업무를 정확하고 신중하게 처리합니다.", "img": "turtle.jpg"},
            "ISFJ": {"animal": "자신의 영역을 지키고 방어적인 코뿔소", "explain": "ISFJ 특징 유형 성격의 소유자는 조용하고 차분하며 따뜻하고 친근합니다.<br><br>책임감과 인내력 또한 매우 강하며, 본인의 친한 친구나 가족에게 애정이 가득합니다.<br><br> 이들은 언제나 진솔하려 노력하고 가볍지 않기 때문에 관계를 맺기에 가장 믿음직스러운 유형입니다.<br><br>수호자형 사람은 무엇을 받으면 몇 배로 베푸는 진정한 이타주의자로 열정과 자애로움으로 일단 믿는 이들이라면 타인과도 잘 어울려 일에 정진합니다. ", "img": "Rhinoceros.jpg"},
            "INFJ": {"animal": "희귀하면서 매혹적인 팬더", "explain": "INFJ 특징은 인내심이 많고 통찰력과 직관력이 뛰어나며 화합을 추구합니다.<br><br> 창의력이 좋으며, 성숙한 경우엔 강한 직관력으로 타인에게 말없이 영향력을 끼칩니다.<br><br>독창성과 내적 독립심이 강하며, 확고한 신념과 열정으로 자신의 영감을 구현시켜 나가는 정신적 지도자들이 많습니다.<br><br>나무보다 숲을 보는 편입니다.<br><br>한곳에 몰두하는 경향으로 목적 달성에 필요한 주변적인 조건들을 경시하기 쉽고, 자기 내부의 갈등이 많고 복잡합니다.<br><br>이들은 풍부하고 감성적인 내적인 성격을 갖고 있습니다.", "img": "panda.jpg"},
            "INTJ": {"animal": "홀로있는 사냥꾼 호랑이", "explain": "INTJ의 지식을 향한 갈증은 어릴 적부터 두드러지게 나타나는데, 때문에 건축가형 사람은 어릴 때 ‘책벌레’라는 소리를 자주 듣습니다.<br><br>대개 친구들 사이에서는 놀림의 표현임에도 불구하고 전혀 개의치 않아 하며, 오히려 깊고 넓은 지식을 가지고 있는 그들 자신에게 남다른 자부심을 느낍니다.<br><br>이들은 또한 관심 있는 특정 분야에 대한 그들의 방대한 지식을 다른 이들과 공유하고 싶어 하기도 합니다.<br><br>반면, 일명 가십거리와 같이 별 볼 일 없는 주제에 대한 잡담거리보다는 그들 나름의 분야에서 용의주도하게 전략을 세우거나 이를 실행해 옮기는 일을 선호합니다.", "img": "lion.png"},
            "ISTP": {"animal": "조용하고 예측 불가능한 뱀", "explain": "타인을 잘 도우며 그들의 경험을 다른 이들과 공유하는 것을 좋아하기도 하는 ISTP는 특히나 그들이 아끼는 사람일수록 더욱 그러합니다.<br><br>이들이 인구의 고작 5%만이 차지합니다.<br><br>더욱이 여성의 경우는 더욱 흔치 않으며, 이 유향의 여성은 사회가 일반적으로 요구하는 이상적인 여성상에 들어맞지 않는 경우가 많으며, 이들은 자라면서 말괄량이 소리를 듣기도 합니다.<br><br>냉철한 이성주의적 성향과 왕성한 호기심을 가진 만능재주꾼형 사람은 직접 손으로 만지고 눈으로 보면서 주변 세상을 탐색하는 것을 좋아합니다.<br><br>무엇을 만드는 데 타고난 재능을 가진 이들은 하나가 완성되면 또 다른 과제로 옮겨 다니는 등 실생활에 유용하면서도 자질구레한 것들을 취미 삼아 만드는 것을 좋아하는데, 그러면서 새로운 기술을 하나하나 터득해 나갑니다.<br><br>종종 기술자나 엔지니어이기도 한 이들에게 있어 소매를 걷어붙이고 작업에 뛰어들어 직접 분해하고 조립할 때보다 세상에 즐거운 일이 또 없을 것입니다.", "img": "snake.jpg"},
            "ISFP": {"animal": "악의없고 섬세한 고양이", "explain": "말없이 다정하고 온화하며 사람들에게 친절하고 상대방을 잘 알게 될 때까지 내면의 모습이 잘 보이지 않습니다.<br><br>의견 충돌을 피하고, 인화를 중시합니다.<br><br>인간과 관계되는 일을 할 때 자신의 감정과 타인의 감정에 지나치게 민감한 경향이 있습니다.<br><br>이들은 결정력과 추진력을 기를 필요가 있습니다.<br><br>3차 기능인 Ni(내향 직관)으로 눈치가 빠르며, 조용히 자기 일만 하고 있는 것처럼 보이지만 사실 주변 상황파악도 다 하고 있습니다.<br><br>경험을 통해서 부기능 Se(외향 감각)과 함께 주기능 Fi(내향 감정)인 자신의 내면을 보호하는데에 잘 사용할 수 있습니다.", "img": "cat.jpg"},
            "INFP": {"animal": "어린이같지만 남을 배려하는 물개", "explain": "INFP 유형의 사람은 최악의 상황이나 악한 사람에게서도 좋은 면만을 바라보며 긍정적이고 더 나은 상황을 만들고자 노력하는 진정한 이상주의자입니다.<br><br>간혹 침착하고 내성적이며 심지어는 수줍음이 많은 사람처럼 비추어지기도 하지만, 이들 안에는 불만 지피면 활활 타오를 수 있는 열정의 불꽃이 숨어있습니다.<br><br>인구의 대략 4%를 차지하는 이들은 간혹 사람들의 오해를 사기도 하지만, 일단 마음이 맞는 사람을 만나면 이들 안에 내재한 충만한 즐거움과 넘치는 영감을 경험할 수 있을 것입니다.", "img": "seal.jpg"},
            "INTP": {"animal": "현명하고 침착한 부엉이", "explain": "INTP 특징은 조용하고 과묵하며 논리와 분석으로 문제를 해결하기를 좋아합니다.<br><br>먼저 대화를 시작하지 않는 편이나 관심이 있는 분야에 대해서는 말을 많이 하는 편입니다.<br><br>이해가 빠르고 직관력으로 통찰하는 능력이 있으며 지적 호기심이 많아, 분석적이고 논리적입니다.<br><br> MBTI 16가지 성격 유형 중 창의적 지능과 논리면에서 가장 뛰어나, 비과학적이거나 논리적이지 못한 일들에 대단히 거부반응을 보일 경향이 높습니다.<br><br>아이디어와 원리, 인과관계에 관심이 많으며 실체보다는 실체가 안고 있는 가능성에 관심이 많습니다.", "img": "owl.jpg"},
            "ESTP": {"animal": "교활하고 기회주의적인 하이에나", "explain": "ESTP 특징은 주변에 지대한 영향을 주는 사업가형 사람은 여러 사람이 모인 행사에서 이 자리 저 자리 휙휙 옮겨 다니는 무리 중에서 어렵지 않게 찾아볼 수 있습니다.<br><br>직설적이면서도 친근한 농담으로 주변 사람을 웃게 만드는 이들은 주변의 이목을 끄는 것을 좋아합니다.<br><br>만일 관객 중 무대에 올라올 사람을 호명하는 경우, 이들은 제일 먼저 자발적으로 손을 들거나 아니면 쑥스러워하는 친구를 대신하여 망설임 없이 무대에 올라서기도 합니다.", "img": "hyena.jpg"},
            "ESFP": {"animal": "분위기 메이커 돌고래", "explain": "ESFP 유형이면서, 갑자기 흥얼거리며 즉흥적으로 춤을 추기 시작하는 누군가가 있다면 이는 연예인형의 사람일 가능성이 큽니다.<br><br>이들은 순간의 흥분되는 감정이나 상황에 쉽게 빠져들며, 주위 사람들 역시 그런 느낌을 만끽하기를 원합니다.<br><br>다른 이들을 위로하고 용기를 북돋아 주는 데 이들보다 더 많은 시간과 에너지를 소비하는 사람 없을 겁니다.<br><br>더욱이나 다른 유형의 사람과는 비교도 안 될 만큼 거부할 수 없는 매력으로 말이죠.", "img": "dolphin.jpg"},
            "ENFP": {"animal": "즉흥적이고 창의적인 오랑우탄", "explain": "ENFP 유형의 사람은 자유로운 사고의 소유자입니다.<br><br>종종 분위기 메이커 역할을 하기도 하는 이들은 단순한 인생의 즐거움이나 그때그때 상황에서 주는 일시적인 만족이 아닌 타인과 사회적, 정서적으로 깊은 유대 관계를 맺음으로써 행복을 느낍니다.<br><br>매력적이며 독립적인 성격으로 활발하면서도 인정이 많은 이들은 인구의 대략 7%에 속하며, 어느 모임을 가든 어렵지 않게 만날 수 있습니다.", "img": "orangutan.png"},
            "ENTP": {"animal": "매력적이고 영리한 앵무새", "explain": "ENTP 유형의 사람은 타인이 믿는 이념이나 논쟁에 반향을 일으킴으로써 군중을 선동하는 일명 선의의 비판자입니다.<br><br>결단력 있는 성격 유형이 논쟁 안에 깊이 내재한 숨은 의미나 상대의 전략적 목표를 꼬집기 위해 논쟁을 벌인다고 한다면, 변론가형 사람은 단순히 재미를 이유로 비판을 일삼습니다.<br><br>아마도 이들보다 논쟁이나 정신적 고문을 즐기는 성격 유형은 없을 것입니다.<br><br>이는 천부적으로 재치 있는 입담과 풍부한 지식을 통해 논쟁의 중심에 있는 사안과 관련한 그들의 이념을 증명해 보일 수 있기 때문입니다.<br><br>ENTP 특징은 본인이 구상하는 바를 실현시키고 싶어 하는 기질이 강하며, 여기에 특유의 아웃사이더적인 성격까지 겹쳐 말 그대로 혁명가의 기질을 띠고 있습니다.", "img": "parrot.jpg"},
            "ESTJ": {"animal": "엄격하면서 공격적인 늑대", "explain": "ESTJ 유형은 그들 생각에 반추하여 무엇이 옳고 그른지를 따져 사회나 가족을 하나로 단결시키기 위해 사회적으로 받아들여지는 통념이나 전통 등 필요한 질서를 정립하는 데 이바지하는 대표적인 유형입니다.<br><br>정직하고 헌신적이며 위풍당당한 이들은 비록 험난한 가시밭길이라도 조언을 통하여 그들이 옳다고 생각하는 길로 사람들을 인도합니다.<br><br>군중을 단결시키는 데에 일가견이 있기도 한 이들은 종종 사회에서 지역사회조직가와 같은 임무를 수행하며, 지역 사회 발전을 위한 축제나 행사에서부터 가족이나 사회를 하나로 결집하기 위한 사회 운동을 펼치는 데 사람들을 모으는 역할을 하기도 합니다.", "img": "wolf.jpg"},
            "ESFJ": {"animal": "온화하면서 남을 보살피는 코끼리", "explain": "ESFJ 유형인 사교형 사람을 한마디로 정의 내리기는 어렵지만, 간단히 표현하자면 이들은 ‘인기쟁이’입니다.<br><br>인구의 대략 12%를 차지하는 꽤 보편적인 성격 유형으로, 이를 미루어 보면 왜 이 유형의 사람이 인기가 많은지 이해가 갑니다.<br><br>이들은 분위기를 좌지우지하며 여러 사람의 스포트라이트를 받거나 학교에 승리와 명예를 불러오도록 팀을 이끄는 역할을 하기도 합니다.<br><br>이들은 또한 훗날 다양한 사교 모임이나 어울림을 통해 주위 사람들에게 끊임없는 관심과 애정을 보임으로써 다른 이들을 행복하고 즐겁게 해주고자 노력합니다.", "img": "elephant.jpg"},
            "ENFJ": {"animal": "충성심있고 애정이 많은 개", "explain": "ENFJ 특징은 따뜻하고 적극적이며 책임감이 강하고 사교성이 풍부하고 동정심이 많습니다.<br><br>상당히 이타적이고 민첩하고 인화를 중요시하며 참을성이 많으며, 다른 사람들의 생각이나 의견에 진지한 관심을 가지고 공동선을 위하여 다른 사람의 의견에 대체로 동의합니다.<br><br>미래의 가능성을 추구하며 편안하고 능란하게 계획을 제시하고 집단을 이끌어가는 능력이 있습니다.<br><br>때로 다른 사람들의 좋은 점을 지나치게 이상화하는 경향이 있으며 다른 사람들에 대해서도 자기와 같을 것이라고 생각하는 경향이 있습니다.", "img": "dog.jpg"},
            "ENTJ": {"animal": "동물의 왕 사자", "explain": "ENTJ 유형 특징은 천성적으로 타고난 리더입니다.<br><br>이 유형에 속하는 사람은 넘치는 카리스마와 자신감으로 공통의 목표 실현을 위해 다른 이들을 이끌고 진두지휘합니다.<br><br>예민한 성격의 사회운동가형 사람과 달리 이들은 진취적인 생각과 결정력, 그리고 냉철한 판단력으로 그들이 세운 목표 달성을 위해 가끔은 무모하리만치 이성적 사고를 하는 것이 특징입니다.<br><br>ENTJ는 인구의 단 3%에 해당하는 유형입니다.", "img": "lion.png"},
        }

        // 시작 버튼 이벤트
        function start(){
            $(".start").hide();
            $(".question").show();
            next();
        }

        // 문제 버튼에 따른 클릭 이벤트
        $("#A").click(function() {
            let type = $("#type").val();
            let preValue = parseInt($("#" + type).val());
            $("#" + type).val(preValue + 1);
            next();
        });
        $("#B").click(function() {
            next();
        });


        // 다음 문제로 넘어가는 함수
        function next(){
            // 마지막 문제에서 선택이 끝난경우
            if(num == 13){
                $('.progress-bar').attr('style', 'width: calc(100/12*'+num+'%)');

                $(".question").hide();
                $(".result").show();

                // mbti 결정 로직
                let mbti = "";
                ($("#EI").val() < 2) ? mbti += "I" : mbti += "E";
                ($("#SN").val() < 2) ? mbti += "N" : mbti += "S";
                ($("#TF").val() < 2) ? mbti += "F" : mbti += "T";
                ($("#JP").val() < 2) ? mbti += "P" : mbti += "J";
                alert("당신의 mbti는 " + mbti + "입니다.");

                // 해당 mbti의 이미지와 설명 불러오기
                $("#img").attr("src", "img/" + result[mbti]["img"]);
                $("#animal").html(result[mbti]["animal"]);
                $("#explain").html(result[mbti]["explain"]);

            // 마지막 문제가 아닌 경우
            } else{
                $('.progress-bar').attr('style', 'width: calc(100/12*'+num+'%)');
                $("#title").html(q[num]["title"]);
                $("#type").val(q[num]["type"]);
                $("#A").html(q[num]["A"]);
                $("#B").html(q[num]["B"]);
                num++;
            }
        }
    </script>
    <!-- SNS 공유하기 기능 연동 -->
    <script async src="https://static.addtoany.com/menu/page.js"></script>
</body>
</html>
```

<br>

## CSS
```css
article {
    display: flex;
    flex-direction: column;
}

h1{
    width: 500px;
    margin: 0 auto;
}

.question{
    display: none;
}

.result{
    display: none;
}

#img{
    width: 300px;
    height: 300px;
    margin: 0 auto;
}

#start-img{
    width: 300px;
    height: 300px;
    margin: 0 auto;
}

.start-btn{
    width: 300px;
    margin: 0 auto;
    
}

.share{
    margin: 0 auto;
}

.kakao_ad{
    width: 320px;
    margin: 0 auto;
}

.banner{
    display: flex;
    justify-content: center;
    width: 300px;
    margin: 0 auto;
}

.banner-img{
    width: 300px;
}
```

- SNS 공유하기 기능: `AddToAny`
- 광고 배너: `카카오 adfit`, `codeLion`
- 검색 엔진 최적화: `Naver Search Advisor`

> 웹사이트 링크: [https://sangjin-mbti-test.netlify.app/](https://sangjin-mbti-test.netlify.app/)
