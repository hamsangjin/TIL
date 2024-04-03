# JavaScript

## 객체 생성자
`객체 생성자`는 함수를 통해서 새로운 객체를 만들고 그 안에 넣고 싶은 값 혹은 함수들을 구현할 수 있게 해준다.

- 예제 코드
```javascript
function Animal(type, name, sound){
    this.type = type;
    this.name = name;
    this.sound = sound;
    this.say = function(){
        console.log(this.sound);
    };
}

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();

// 정의된 객체 생성자 값들이 아닌 또 다른 값을 넣을 수 있다.
cat.hi = function(){
    console.log('hihi cat');
}
cat.age = 10;

cat.hi();
console.log(cat.age);
```

<br>

## 프로토타입
`프로토타입`은 같은 객체 생성자 함수를 사용하는 경우, 객체를 생성할 때마다 새로 정의해줄 필요없이, 특정 함수 또는 값을 재사용 할 수 있게 해준다.

- 예제 코드
```javascript
function Animal(type, name, sound){
    this.type = type;
    this.name = name;
    this.sound = sound;
}

Animal.prototype.say = function(){
    console.log(this.sound);
};

Animal.prototype.sharedValue = 1;

const dog = new Animal('개', '멍멍이', '멍멍');
const cat = new Animal('고양이', '야옹이', '야옹');

dog.say();
cat.say();

console.log(dog.sharedValue);
console.log(cat.sharedValue);

cat.sharedValue = 2;
console.log(dog.sharedValue);
console.log(cat.sharedValue);

cat.say = function(){
    console.log('hihi');
}
```

<br>

## 객체 생성자 상속
`Cat`과 `Dog`라는 새로운 객체 생성자를 만든다고 할 때, 해당 객체 생성자들에서 `Animal`의 기능을 재사용한다고 가정해보자. <br>

- 여기서 첫번째 인자에는 `this`를 넣어주어야 하고, 그 이후에는 Animal 객체 생성자 함수에서 필요로 하는 `파라미터`를 넣어주어야 한다.

- 예제 코드
```javascript
function Animal(type, name, sound) {
    this.type = type;
    this.name = name;
    this.sound = sound;
}
Animal.prototype.say = function() {
    console.log(this.sound);
};
Animal.prototype.sharedValue = 1;

function Dog(name, sound) { 
    Animal.call(this, '개', name, sound);
}
Dog.prototype = Animal.prototype;

function Cat(name, sound) {
    Animal.call(this, '고양이', name, sound); 
}
Cat.prototype = Animal.prototype; 

const dog = new Dog('멍멍이', '멍멍');
const cat = new Cat('야옹이', '야옹');
dog.say();
cat.say();
```

<br>

## 클래스
ES6부터 `class`라는 문법이 추가되며, 객체 생성자로 구현했던 코드를 조금 더 명확하고, 깔끔하게 구현할 수 있게 해주고, 상속도 훨씬 쉽게 해준다.

- 예제 코드
```javascript
class Animal {
    constructor(type, name, sound) {
        this.type = type;
        this.name = name;
        this.sound = sound;
    }
    say() {
        console.log(this.sound);
    }
}

const dog1 = new Animal('개', '멍멍이', '멍멍');
const cat1 = new Animal('고양이', '야옹이', '야옹');
dog1.say();
cat1.say();

class Dog extends Animal {
    constructor(name, sound) {
        super('개', name, sound); 
    }
}
class Cat extends Animal {
    constructor(name, sound) {
        super('고양이', name, sound); 
    }
}

const dog2 = new Dog('멍멍이', '멍멍');
const cat2 = new Cat('야옹이', '야옹');
dog2.say();
cat2.say();
```

- 상속할 땐 자바와 같이 `extends` 키워드를 사용하며 `constructor`에서 사용하는 `super` 함수가 상속받은 클래스의 생성자를 가리킨다.

<br>

## 연산자
1. 논리 연산자를 사용한 단축 평가
  - `논리 AND(&&)`: 첫 번째 피연산자가 `true`면, 두 번째 피연산자의 값을 반환하고, 그렇지 않으면 첫 번째 피연산자의 값을 반환한다.
  - `논리 OR(||)`: 첫 번째 피연산자가 `false`면, 두 번째 피연산자의 값을 반환하고, 그렇지 않으면 첫 번째 피연산자의 값을 반환한다.
  - `논리 Nullish(??)`: 첫 번째 피연산자가 `null`이나 `undefined`가 아니면, 첫 번째 피연산자의 값을 반환하고, 그렇지 않으면 두 번째 피연산자의 값을 반환한다.
2. `조건부(삼항) 연산자`: 삼항 연산자(조건 ? 표현식1 : 표현식2) 조건이 `true`로 평가되면 `표현식1`을, 그렇지 않으면 `표현식2`를 실행한다.
3. `할당 연산자를 이용한 단축`: 할당과 동시에 연산하는 연산자(`+=`, `-=`, `*=`, `/=`, `%=`) 등을 사용하여 값을 업데이트하면서 할당할 수 있다.

- 예제 코드 1
```javascript
const food = { foodName: "피자" };

function getFoodName(food) {
    if (!food) return "아무거나";
    return food.foodName;
}
console.log(getFoodName(food));


function getFoodName(food){
    return food && food.foodName;
}
const food2 = null;
console.log(getFoodName(food2));

console.log("===== && 연산자 =====");

// && 연산자
// true면 마지막거 반환
console.log(true && "hello");
// false인 부분 반환
console.log(0 && "hello");

console.log("===== || 연산자 =====");

// || 연산자
// true면 가장 먼저 나오는 true 반환
console.log(true || "hello");
console.log(false || "hello");
// false면 마지막 false인 부분 반환
console.log(0 || "");

console.log("===== ?? 연산자 =====");
console.log("" ?? "hello");
console.log(0 ?? "hello");
console.log(null ?? "hello");
console.log(undefined ?? "hello");

console.log("a" ?? "hello");
console.log(1 ?? "hello");


console.log("===== false를 의미하는 것 =====");
console.log("" || "hello");
console.log(0 || "hello");
console.log(null || "hello");
console.log(undefined || "hello");

console.log("===== true를 의미하는 것 =====");
console.log("a" || "hello");
console.log(1 || "hello");

console.log("===== 예제 =====");
let a;
let b = null;
let c = undefined;
let d = 4;
let e = "test";

let f = a || b || c || d || e;
console.log(f);
```

- 예제 코드 2
```javascript
let oldName = "홍길동";
let newName = "아무개";

if(newName){
    oldName = newName;
}

// 위 if문과 동일한 동작
newName && (oldName = newName);

const person = {};

person.age ??= 21;
console.log(person);

person.name ||= "홍길동";
console.log(person);

function makeObj(obj) {
    obj.name ??= "guest";
    obj.age ??= 20;
    return obj;
}

console.log(makeObj({}));
console.log(makeObj({ name: "carami" }));
console.log(makeObj(person));
```

<br>

## 비동기처리
<img width="478" alt="스크린샷 2024-04-03 13 22 53" src="https://github.com/hamsangjin/TIL/assets/103736614/45bb778f-ed66-42d1-871b-90e94a2d7b53">

<br>

자바스크립트는 `싱글 스레드` 언어이기 때문에 한 번에 하나의 작업만 수행할 수 있다. <br>
즉, 이전 작업이 완료되어야 다음 작업을 수행할 수 있는 동기 처리 방식이다. <br>
그런데 `동기 처리`는 간단하고 직관적이지만, 작업이 오래 걸리거나 응답이 늦어지는 경우에는 전체적인 성능과 사용자 경험에 영향을 줄 수 있다. <br>
따라서, 자바스크립트로 여러 작업을 동시에 처리하기 위해 `비동기` 처리를 도입해, 특정 작업의 완료를 기다리지 않고 다른 작업을 동시에 수행할 수 있도록 하였다. <br>
> 출처 : [Inpa Dev](https://inpa.tistory.com/entry/%F0%9F%8C%90-js-async)

- 예제 코드
```javascript
// work가 실행중일 때 비동기로 실행시킬 수 있게 callback 추가
function work(callback) {
    setTimeout(() =>{
        const start = Date.now();
        for (let i = 0; i < 1000000000; i++) {}
        const end = Date.now();
        console.log('work: ' + (end - start) + 'ms');

        callback();
    }, 0);
}

console.log('work 시작');
// work 함수에 매개변수로 'work 끝'을 출력하는 익명함수를 넘겨줌
work(() => {
    console.log('work 끝');
});

function hi(){
    console.log("hi");
}

console.log("hi 시작");
setTimeout(hi, 1000);
console.log("hi 끝");
```
<br>

위 코드의 실행과정을 자세히 알고싶으면 자바스크립트의 `이벤트루프`를 알면 된다.
![eventloop](https://github.com/hamsangjin/TIL/assets/103736614/a2814d7d-06ea-451e-a311-6da7d5a6a065)

<br>

## promise
`프로미스`는 비동기 작업을 조금 더 편하게 처리할 수 있도록 ES6에 도입된 기능이다.
- Promise는 성공할 수도 있고, 실패할 수도 있으며, 성공할 때에는 `resolve`를 호출해주면 되고, 실패할 때에는 `reject`를 호출해주면 된다.
- 지금 당장은 실패하는 상황은 고려하지 않고, 1초 뒤에 성공시키는 상황에 대해서만 구현을 한다.
- `resolve`를 호출 할 때 특정 값을 파라미터로 넣어주면, 이 값을 작업이 끝나고 나서 사용 할 수 있다.
- 작업이 끝나고 나서 또 다른 작업을 해야 할 때에는 Promise 뒤에 `.then(...)` 을 붙여서 사용하면 된다.

- 예제 코드 1
```javascript
function increaseAndPrint(n, callback) {
    setTimeout(() => {
        const increased = n + 1;
        console.log(increased);
        if (callback) {
        callback(increased);
        }
    }, 1000);
}

increaseAndPrint(0, n => {
    increaseAndPrint(n, n => {
        increaseAndPrint(n, n => {
            increaseAndPrint(n, n => {
                increaseAndPrint(n, n => {
                    console.log('끝!');
                });
            });
        });
    });
});


const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve(1);
    }, 1000);
});

myPromise
    .then(n => {
        console.log(n);
    })
    .catch(error => {
        console.log(error);
    });
```

- 예제 코드 2
```javascript
function increaseAndPrint(n) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const value = n + 1;
            if (value === 5) {
                const error = new Error();
                error.name = 'ValueIsFiveError';
                reject(error);
                // return 하지 않으면 5가 출력됨
                return;
            }
            console.log(value);
            resolve(value);
        }, 1000);
    });
}

// 1
increaseAndPrint(0)
.then(n => {
    return increaseAndPrint(n);
})
.then(n => {
    return increaseAndPrint(n);
})
.then(n => {
    return increaseAndPrint(n);
})
.then(n => {
    return increaseAndPrint(n);
})
.then(n => {
    return increaseAndPrint(n);
})
.catch(e => {
    console.error(e);
});

// 2
increaseAndPrint(0)
.then(increaseAndPrint).then(increaseAndPrint).then(increaseAndPrint).then(increaseAndPrint).then(increaseAndPrint)
.catch(e => {
    console.error(e);
});
```
