# 문제 1: 배열의 모든 요소 출력하기

주어진 숫자 배열의 모든 요소를 콘솔에 출력하는 코드를 작성하세요.

```javascript
const numbers = [1, 2, 3, 4, 5];
        
for(let i = 0; i < numbers.length; i++){
    console.log(numbers[i]);
}
```

<br>

# 문제 2: 객체의 모든 프로퍼티와 값 출력하기

주어진 객체의 모든 프로퍼티와 그 값을 콘솔에 출력하는 코드를 작성하세요.

```javascript
const person = {
    name: '홍길동',
    age: 25,
    email: 'hong@example.com'
};

for (let key in person) {
    console.log(`${key}: ${person[key]}`);
}
```

<br>

# 문제 3: 특정 값의 개수 찾기

숫자 배열이 주어졌을 때, 배열 안에 특정 숫자가 몇 개 있는지 세는 코드를 작성하세요.

```javascript
const numbers = [1, 3, 7, 8, 3, 3, 4, 3];
const targetNumber = 3;
const findNum = (arr, target) => {
    let answer = 0;
    for (let n of arr) {
        if(n === target){
            answer ++;
        }
    }
    return answer;
}

console.log(findNum(numbers, targetNumber));
```

<br>

# 문제 4: 최대값 찾기

숫자 배열에서 가장 큰 숫자를 찾는 코드를 작성하세요.

```javascript
const numbers = [10, 36, 54, 89, 12];
const max = (arr) => {
    let maxVal = 0;
    for (let n of arr) {
        maxVal = Math.max(maxVal, n);
    }
    return maxVal;
}

console.log(max(numbers));
```

<br>

# 문제 5: 객체 배열에서 특정 조건의 객체 찾기

사용자 정보가 담긴 객체 배열에서 나이가 30 이상인 사람들만 찾아 새 배열을 만드는 코드를 작성하세요.

```javascript
const users = [
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 30 },
    { name: 'Charlie', age: 28 },
    { name: 'Dave', age: 35 }
];

const newUsers = (arr) => {
    newUsersArr = [];
    for(let person of arr){
        if(person.age >= 30){
            newUsersArr.push(person);
        }
    }
    return newUsersArr;
}

console.log(newUsers(users));
```

<br>

---

<br>

# 문제 6: 모든 요소의 제곱 구하기

주어진 숫자 배열의 각 요소에 대해 제곱을 한 새로운 배열을 생성하세요.

```javascript
const numbers = [1, 2, 3, 4, 5];

let powArr = numbers.map(n => n * n);
console.log(powArr);
```

<br>

# 문제 7: 짝수만 필터링하기

주어진 숫자 배열에서 짝수만 필터링하여 새로운 배열을 생성하세요.

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

let evenNums = numbers.filter(n => n%2 == 0);
console.log(evenNums);
```

<br>

# 문제 8: 총합 구하기

주어진 숫자 배열의 모든 요소의 합을 구하세요.

```javascript
const numbers = [1, 2, 3, 4, 5];

let sum = numbers.reduce((x, y) => x + y, 0);
console.log(sum);
```

<br>

# 문제 9: 특정 조건의 요소 찾기

주어진 객체 배열에서 나이가 30 이상인 첫 번째 사람을 찾으세요.

```javascript
const people = [
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 30 },
    { name: 'Charlie', age: 28 }
];

let targetPeople = people.find(p => p.age >= 30);
console.log(targetPeople);
```

<br>

# 문제 10: 모든 요소가 조건을 만족하는지 확인하기

주어진 숫자 배열의 모든 요소가 10 이상인지 확인하세요.

```javascript
const numbers = [10, 20, 30, 40, 50];

const overTen = numbers.every(i => i >= 10);
console.log(overTen);
```

<br>

# 문제 11: 조건을 만족하는 요소가 하나라도 있는지 확인하기

주어진 숫자 배열에 홀수가 하나라도 있는지 확인하세요.

```javascript
const n = [2, 4, 6, 8, 10, 11];

const result = n.some(n => n%2!==0);
console.log(result);
```
