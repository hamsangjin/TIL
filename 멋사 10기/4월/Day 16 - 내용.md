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
