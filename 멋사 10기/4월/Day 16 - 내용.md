# JavaScript

## 함수
`함수`는 특정 코드를 하나의 명령으로 실행할 수 있게 해주는 기능

- 예제 코드 1
```javascript
let a = 10;
let b = "test";

// 자바 스크립트의 함수는 일급객체로 취급되며, 함수도 타입이 될 수 있다.
function test(){
    console.log("test");
}

// 함수의 매개변수로 함수를 받을 수 있다.
function c(abc){
    return abc;
}

// 함수 내용이 전달되고, 그 내용을 출력
let ttt = c(test);
console.log(ttt);

// 호출한 c 함수에서 리턴받은 test 함수를 실행
c(test)();
```

- 예제 코드 2
```javascript
hello();
// func1();

// 함수는 호이스팅이 됨
function hello(){
    console.log("hello 1111");
}

// 중복 정의가능하고, 중복정의 되었을 때는 가장 마지막에 정의된 함수가 실행
function hello(){
    console.log("hello 2222");
}

// 상수 func1에 함수 정의(일급 객체라 가능)
// 하지만 상수라 호이스팅은 되지 않음
const func1 = function(){
    console.log("hello 3333");
};
func1();

// 화살표 함수
// 위 func1보다 간결하게 작성 가능
const func2 = () => console.log("hello 4444");
func2();


function hi(a){
    console.log("hello " + a);
}

const hi2 = (a) => console.log("hello " + a);

hi(10);
hi("aaa");
hi2("bbb");

function sum1(a, b){
    return a + b;
}
const sum2 = (a, b) => a + b;

const sum3 = function(a, b){
    return a + b;
}
```

<br>

## ES6
> `ES6`: ECMAScript6를 의미하며, 자바스크립트의 버전을 말한다. <br>
> 2015년에 도입이 되었으며, 계속해서 새로운 자바스크립트 버전이 나오고있다. <br>

<br>

## 객체
`객체`는 우리가 변수나 상수를 사용하게 될 때 하나의 이름에 여러 종류의 값을 넣을 수 있게 해준다.

- 예제 코드 1
```javascript
const obj = {};
console.log(typeof obj);

// 만약 key값이 공백으로 구분된 두 단어 이상이면 ''로 묶어준다.
const dog = {
    name: "멍멍",
    age: 2,
}

// 객체 자체를 출력
console.log(dog);

// 객체의 key에 해당하는 value 출력
console.log(dog.name);
console.log(dog["name"]);
```

- 예제 코드 2
```javascript
const ironMan = {
    name: '토니 스타크',
    actor: '로버트 다우니 주니어', 
    alias: '아이언맨'
};

const captainAmerica = { 
    name: '스티븐 로저스', 
    actor: '크리스 에반스', 
    alias: '캡틴 아메리카'
};

// 함수에서 객체를 파라미터로 받기
function print1(hero) {
    const text = `${hero.alias}(${hero.name}) 역할을 맡은 배우는 ${
    hero.actor} 입니다.`; 
    
    console.log(text);
}

// 객체 비구조화 할당(객체 구조 분해)
function print2(hero) {
    const { alias, name, actor } = hero;
    const text = `${alias}(${name}) 역할을 맡은 배우는 ${actor} 입니다.`; 
    
    console.log(text);
}

print1(ironMan);
print1(captainAmerica);

print2(ironMan);
print2(captainAmerica);
```

- 예제 코드 3
```javascript
const dog = {
    name: '멍멍이',
    sound: '멍멍!',
    say: function say() {
        console.log(this.sound);
        // 함수가 객체 안에 들어가면 자신이 속해있는 객체를 가리킴
        console.log("객체 안 :::" + this);
    }
};

// 위처럼 function 으로 선언한 함수는 this 가 제대로 자신이 속한 객체를 가리키게 되는데, 화살표 함수는 그렇지 않다.
const arrow = () => {
    console.log("객체 밖 :::" + this);
};

arrow();
dog.say();
```

- 예제 코드 4
```javascript
const numbers = {
    _a: 1,
    _b: 2,

    // getter, setter 함수
    get a(){
        return this._a;
    },
    set a(value){
        this._a = value;
    },
    get b(){
        return this._b;
    },
    set b(value){
        this._b = value;
    },

    // 덧셈 함수
    get sum(){
        return this.a + this.b;
    },
};

numbers.a = 20;
console.log(numbers.a);
console.log(numbers.b);
console.log(numbers.sum);
```

<br>

## 배열
`배열`은 여러 개의 항목들이 들어있는 리스트와 같다.

- 예제 코드
```javascript
const array = [1, 2, 3, 4, 5];
console.log(array);
console.log(array[0]);

// 다른 타입이어도 정의 가능
array[2] = "test";
console.log(array);

// 배열의 크기 정의
const array2 = new Array(10);
console.log(array2);

// 배열의 요소 초기값 정의
const array3 = new Array(10, 2);
console.log(array3);

// 배열 요소에 객체 정의 가능
array[0] = {name: "ham"};
console.log(array);

// 배열에 값 삽입
array.push(10);
console.log(array);

// 배열에 함수 삽입
array.push(() => console.log("hello array"));
console.log(array);
array[6]();

// 배열 비구조 할당
const[a, b, c, d, e, f, g, h] = array;
console.log(a);
```

<br>

## 반복문
`반복문`은 특정 작업을 반복자적으로 할 때 사용할 수 있는 구문이다.

- 예제 코드
```javascript
/*
for (초기 구문; 조건 구문; 변화 구문;) { 코드
}
*/

// for문을 이용한 기본적인 0 ~ 9까지의 출력문
for (let i = 0; i < 10; i++) {
    console.log(i);
}

// for문을 이용한 배열 출력문
const names = ['멍멍이', '야옹이', '멍뭉이'];
for (let i = 0; i < names.length; i++) {
    console.log(names[i]);
}

// while문
let i = 0;
while (i < 10) {
    console.log(i);
    i++;
}

// for-of
let numbers = [10, 20, 30, 40, 50];
for (let number of numbers) {
    console.log(number);
}


// for-in
const doggy = { name: '멍멍이', sound: '멍멍', age: 2};
for (let key in doggy) {
    console.log(`${key}: ${doggy[key]}`);
}
```

<br>

## 배열 내장함수

### forEach
`forEach`는 for문을 대체시킬 수 있는 내장함수이다.

- 예제 코드
```javascript
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지'];

console.log(superheroes);

const hi = function(name) {
    console.log("I am " + name);
};

// 아래 세가지는 다 똑같은 동작을 함
// 매개변수로 함수 호출
superheroes.forEach(hi);

// 매개변수에 함수 정의
superheroes.forEach(function(name){
    console.log("I am " + name);
});
// 매개변수에 익명함수 정의
superheroes.forEach((name) => console.log("I am " + name));
```

### map
`map`은 배열 안의 각 원소를 변환할 때 사용되며, 그 과정에서 새로운 배열이 만들어진다.

- 예제 코드
```javascript
const array = [1, 2, 3, 4, 5, 6, 7, 8];
        
// 기본 for문
const squared1 = [];
for (let i = 0; i < array.length; i++) {
    squared1.push(array[i] * array[i]);
}
console.log(squared1);

// forEach 함수 사용
const squared2 = [];
array.forEach(n => squared2.push(n * n));
console.log(squared2);

// map 사용
const squared3 = array.map(n => n * n);
console.log(squared3);
```

<br>

### indexOf, findIndex, find, filter
- `indexOf`, `findIndex`: 두 함수는 원하는 항목이 몇 번째 원소인지 찾아주는 함수
- `indexOf`: 배열 안에 있는 값이 숫자, 문자열, 또는 불리언일 때 사용
- `findIndex`: 배열 안에 있는 값이 객체이거나 배열일 때 사용한다.
- `find`는 `findIndex`와 비슷하지만, 찾아낸 값이 몇 번째인지 알아내는 것이 아니라 찾아낸 값을 반환
- `filter`: 배열에서 특정 조건을 만족하는 값들만 따로 추출하여 새로운 배열을 만듦

- 예제 코드
```javascript
const superheroes = ['아이언맨', '캡틴 아메리카', '토르', '닥터 스트레인지']; const index1 = superheroes.indexOf('토르');
console.log(index1);

const todos = [
    {
        id: 1,
        text: '자바스크립트 입문',
        done: true
    },
    {
        id: 2,
        text: '함수 배우기',
        done: true
    },
    {
        id: 3,
        text: '객체와 배열 배우기',
        done: true
    },
    {
        id: 4,
        text: '배열 내장함수 배우기',
        done: false
    }
];

const index2 = todos.findIndex(todo => todo.id === 3);
console.log(index2);

const todoFind = todos.find(todo => todo.id === 3);
console.log(todoFind);

const tasksNotDone = todos.filter(todo => todo.done === false);
// todos.filter(todo => !todo.done);
console.log(tasksNotDone);
```

<br>

## splice, slice
- `splice`: 배열에서 특정 항목을 제거할 때 사용
- `slice`: 기존의 배열을 건드리지 않고 새로운 배열로 생성

- 예제 코드
```javascript
const numbers1 = [10, 20, 30, 40];
const index = numbers1.indexOf(30);
numbers1.splice(index, 1);
console.log(numbers1);

const numbers2 = [10, 20, 30, 40];
const sliced = numbers2.slice(0, 2); // 0부터 시작해서 2전까지
console.log(sliced);
console.log(numbers2);
```

<br>

## shift, pop
- `shitf`: 첫 번째 원소를 배열에서 추출하고, 그 원소는 배열에서 삭제된다.
- `unshift`: 맨 앞에 원소를 추가한다.
- `pop`: 마지막 원소를 배열에서 추출하고, 그 원소는 배열에서 삭제된다.
- `push`: 맨 뒤에 원소를 추가한다.

- 예제 코드
```javascript
const numbers1 = [10, 20, 30, 40];
const value1 = numbers1.shift();
console.log(value1);
console.log(numbers1);

numbers1.unshift(10);
console.log(numbers1);

const numbers2 = [10, 20, 30, 40];
const value2 = numbers2.pop();
console.log(value2);
console.log(numbers2);

numbers2.push(40);
console.log(numbers2);
```

<br>

## concat
- `concat`: 여러 개의 배열을 하나의 배열로 합쳐줌

- 예제 코드
```javascript
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const concated = arr1.concat(arr2);
console.log(concated);
```

<br>

## join
- `join`: 배열 안의 값들을 문자열 형태로 합쳐준다.

- 예제 코드
```javascript
const array = [1, 2, 3, 4, 5];
console.log(array.join());       // 1,2,3,4,5
console.log(array.join(' '));    // 1 2 3 4 5
console.log(array.join(', '));   // 1, 2, 3, 4, 5
```

<br>

## reduce
- `reduce`: 배열의 각 요소를 순회하며 callback함수의 실행 값을 누적하여 하나의 결과값을 반환

- 예제 코드
```javascript

const numbers = [1, 2, 3, 4, 5];
let sum1 = 0;
numbers.forEach(n => sum1 += n);
console.log(sum1);

let sum2 = numbers.reduce((accumulator, current) => accumulator + current, 0);
console.log(sum2);
```
