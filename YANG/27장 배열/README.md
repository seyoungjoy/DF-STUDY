# 27장 배열
## 27.1 배열이란?
- 여러 개의 값을 순차적으로 나열한 자료구조
```jsx
const arr = ['apple', 'banana', 'orange'];
arr[0]; //apple
arr.length; //3
```

- 배열은 object지만 일반 객체와는 구별되는 독특한 특징이 있다.

|구분|객체| 배열  |
|---|---|-----|
|구조|프로퍼티 키, 값|인덱스, 요소|
|값의 참조|프로퍼티 키|인덱스|
|값의 순서|X|O|
|length 프로퍼티|X|O|

## 27.3 length 프로퍼티와 희소 배열
- 현재 length 프로퍼티 값보다 작은 값을 할당하면 배열의 길이가 줄어든다.
```jsx
const arr = [1,2,3,4,5];

arr.length = 3;
console.log(arr); //[1,2,3]
```

- 현재 length 보다 큰 값을 할당하면 length 프로퍼티 값은 변경되는데 배열의 실제 길이가 늘어나진 않는다.
```jsx
const arr = [1,2,3];
arr.length = 5;
console.log(arr.length);//5
console.log(arr);//(5) [1, 2, 3, empty × 2]
```
- 이렇게 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 `희소배열`이라고 함.
- 자바스크립트에서 문법적으로 허용하나 사용하지 않는 것을 추천.

## 27.4 배열 생성
### 27.4.1 배열 리터럴
```jsx
const arr = [1,2,3,4,5];
```

### 27.4.2 Array 생성자 함수
```jsx
const arr = new Array(1,2,3,4,5);

const arr = new Array(10);
console.log(arr.length);//10
console.log(arr);//(10) [empty × 10]
```
### 27.4.3 Array.of
- ES6에서 도입된 메서드
```jsx
Array.of(1); //[1]
Array.of(1,2,3); //[1,2,3]
```

### 27.4.4 Array.from
- ES6
- 유사 배열 객체, 이터러블 객체를 인수로 전달 받아 배열로 변환
#### 유사 배열 객체란? : 배열처럼 보이는데 프로퍼티 키를 index로 가지고, length 값을 가지는 객체
```html
<p>하나</p>
<p>둘</p>
<p>셋</p>
```
```jsx
const arr = document.querySelectorAll('p');

arr.forEach(item => item.style.color="red");

const highlight = arr.filter((item) => item.style.color="red");
//Uncaught TypeError: array.filter is not a function

// 유사배열객체에 배열의 메서드를 사용하고 싶을 때 Array.from을 통해 배열로 변환시켜줄 수 있다.
const highlight = Array.from(arr).filter((item) => item.style.color="red");
console.log(highlight); //(3) [p,p,p]

```
![img.png](img.png)

#### 이터러블 객체란? : for of 문으로 순회할 수 있고, 스프레드 문법, 배열 비구조화 할당의 대상이 되는 객체
- Array, String, Map, Set, DOM컬렉션(NodeList, HTMLCollection),arguments
```jsx
Array.from('Hello'); //(5) ['H', 'e', 'l', 'l', 'o']
```

- 두번째 인수에 콜백함수를 넣어서 배열을 만들수도 있따.
```jsx
Array.from({length:10}, (_,i) => i);
//(10) [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

Array.from({length:10}, () => false);
//(10) [false, false, false, false, false, false, false, false, false, false]
```

## 27.5 배열 요소의 참조
```jsx
const arr = [1,2];
console.log(arr[0]); //1
```

## 27.6 배열 요소의 추가와 갱신
```jsx
const arr = [0];
// 요소 추가
arr[1] = 1;
console.log(arr); //[0,1]

//요소 갱신
arr[0] = 1;
console.log(arr);//[1,1]
```

## 27.7 배열 요소의 삭제
- delete 연산자는 희소 배열을 생성하기 때문에 splice 메서드를 사용한다.
```jsx
const arr = [1,2,3];

arr.splice(1,1);
console.log(arr);//[1,3]
```

## 27.8 배열 메서드

- 배열 객체의 프로토타입인 Array.prototype은 프로토타입 메서드를 제공한다.
- 결과물을 반환할 때 두가지 패턴을 가진다.
  - 원본 배열을 직접 변경하는 메서드
  - 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드
- 원본 배열을 직접 변경하는 메서드는 상태를 직접 변경하는 부수 효과를 가지기 때문에 주의해야 한다.
- **가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.**

### 27.8.1 `Array.isArray`

- 전달된 인수가 배열이면 true, 배열이 아니면 false

```jsx
console.log(Array.isArray([])); //true
console.log(Array.isArray([1, 2])); //true
console.log(Array.isArray({})); //false
```

### 27.8.2 `Array.prototype.indexOf`

- 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환
  - 인수로 전달된 요소가 여러 개면 첫 번째로 검색된 요소의 인덱스를 반환
  - 인수로 전달된 요소가 없으면 `-1`을 반환
- 배열에 특정 요소가 존재하는지 확인활 때 유용
- (ES7) Array.prototype.includes 메서드가 가독성이 더 좋다.

```javascript
const arr = [1, 2, 2, 3];
console.log(arr.indexOf(2));
1;
console.log(arr.indexOf(2, 2));
2;
//배열에 특정 요소가 존재하는지 확인할 때 유용하다.
const foods = ['apple', 'banana', 'orange'];
if (foods.indexOf('orange') === -1) {
  console.log('오렌지 X');
} else {
  console.log('오렌지 O');
}
//Array.prototype.includes
const fooods = ['apple', 'banana', 'orange'];
if (!foods.includes('orange')) {
  console.log('오렌지 X');
} else {
  console.log('오렌지 O');
}
```

### 27.8.3 `Array.prototype.push`

- 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가
- 변경된 length 프로퍼티 값 반환
- 원본 배열 직접 변경

```jsx
const arr = [1, 2];
let result = arr.push(3, 4);
console.log(result); //4
console.log(arr); //[1,2,3,4]
```

- push 메서드 사용을 지양하는 이유
  1. 성능 면에서 좋지 않다. 추가할 요소가 하나라면 length 프로퍼티로 직접 추가하는게 더 빠르다.
  2. 배열을 직접 변경하는 부수 효과가 있기 때문에 스프레드 문법을 사용하는 편이 좋다.

```jsx
//length 프로퍼티로 배열 추가
const arr = [1, 2];
arr[arr.length] = 3;
console.log(arr); //[1,2,3]
//스프레드 문법으로 추가
const arr = [1, 2];
const newArr = [...arrr, 3];
console.log(newArr); //[1,2,3]
```

### 27.8.4 `Array.prototype.pop`

- 마지막 요소를 제거하고 제거한 요소를 반환
- 원본 배열이 빈 배열이면 undefined 반환
- 원본 배열 직접 변경

```jsx
const arr = [1, 2];
let result = arr.pop();
console.log(result); //2
console.log(arr); //[1]
```

### 27.8.5 `Array.prototype.unshift`

- 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가
- 변경된 length 프로퍼티 값 반환
- 원본 배열 직접 변경
- 부수 효과가 있기 때문에 스프레드 문법을 추천

```jsx
const arr = [1, 2];
let result = arr.unshift(0);
console.log(result); //3
console.log(arr); //[0,1,2]
//스프레드 문법으로 선두에 요소 추가
const arr = [1, 2];
const newArr = [0, ...arr];
console.log(newArr); //[0,1,2]
```

### 27.8.6 `Array.prototype.shift`

- 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환
- 원본 배열이 빈 배열이면 undefined 반환
- 원본 배열 직접 변경

```jsx
const arr = [1, 2];
const result = arr.shift();
console.log(result); //1
console.log(arr); //[2]
```

### 27.8.7 `Array.prototype.concat`

- 인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환.
- 원본 배열 변경X, 새로운 배열 반환
- 스프레드 문법으로 대체 가능.

```jsx
const arr1 = [1, 2];
const arr2 = [3, 4];
let result = arr1.concat(arr2);
console.log(result); //[1,2,3,4]
//스프레드 문법
let result2 = [...arr1, ...arr2];
console.log(result2); //[1,2,3,4]
```

> 📌 결론적으로 push/unshift, concat 메서드 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 권장한다.
### 27.8.8 `Array.prototype.splice`

- 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거할 때 사용.
- 원본 배열 직접 변경

```jsx
//제거 후 새로운 요소 삽입
const arr = [1, 2, 3, 4];
const result = arr.splice(1, 2, 20, 30);
console.log(arr); //[1,20,30,4]
//새로운 요소 삽입
const arr = [1, 2, 3, 4];
const result = arr.splice(1, 0, 100);
console.log(arr); //[1,100,2,3,4]
//요소 제거
const arr = [1, 2, 3, 4];
const result = arr.splice(1, 2);
console.log(arr); //[1,4]
```

### 27.8.9 `Array.prototype.slice`

- 인수로 전달된 범위의 요소들을 복사하여 배열로 반환.
- 원본 배열 변경X

```jsx
const arr = [1, 2, 3];
console.log(arr.slice(0, 1)); //[1]
console.log(arr.slice(1, 2)); //[2]
```

### 27.8.10 `Array.prototype.join`

- 원본 배열의 모든 요소를 문자열로 변환 후, 인수로 전달받은 문자열(구분자)로 연결한 문자열을 반환.
- 구분자는 생략가능
- 기본 구분자는 콤마(,)

```jsx
const arr = [1, 2, 3];
//기본 구분자 콤마
//원본 배열 arr의 모든 요소를 문자열로 변환 후 기본 구분자로 연결한 문자열 반환
console.log(arr.join()); //1,2,3
//빈 문자열로 연결
//원본 배열 arr의 모든 요소를 문자열로 변환 후, 빈 문자열로 연결한 문자열을 반환
console.log(arr.join('')); //123
//:로 연결
console.log(arr.join(':')); //1:2:3
```

### 27.8.11 `Array.prototype.reverse`

- 원본 배열의 순서를 반대로 뒤집는다
- 원본 배열 변경

```jsx
const arr = [1, 2, 3];
const result = arr.reverse();
console.log(arr); //[3,2,1]
console.log(result); //[3,2,1]
```

### 27.8.12 `Array.prototype.fill`

- ES6
- 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.
- 원본 배열 변경

```jsx
const arr = [1, 2, 3];
arr.fill(0);
console.log(arr); //[0,0,0];
```

### 27.8.13 `Array.prototype.includes`

- ES7
- 배열 내에 특정 요소가 포함되어 있으면 true, 없으면 false를 반환

```jsx
const arr = [1, 2, 3];
console.log(arr.includes(3)); //true
console.log(arr.includes(4)); //false
```

### 27.8.14 `Array.prototype.flat`

- ES10
- 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

```jsx
const result = [1, [2, 3, 4], 5].flat();
console.log(result); //[1,2,3,4,5]
```

## 27.9 배열 고차 함수
- 고차 함수: 함수를 인수로 전달받는 함수

### 27.9.1 Array.prototype.sort
- 순수함수 X
- 문자열 요소 오름차순/내림차순 정렬
```jsx
const fruits = ['Banana', 'Orange', 'Apple'];

// 오름차순 정렬
fruits.sort();
console.log(fruits); //(3) ['Apple', 'Banana', 'Orange']

// 내림차순 정렬
fruits.reverse();
console.log(fruits); //(3) ['Orange', 'Banana', 'Apple']
```

- 기본 정렬 순서가 유니코드 코드 포인트
  - 배열의 요소가 숫자 타입이라도 문자열 타입으로 변경하고 유니코드 코드 포인트에 따라 정렬을 한다.
```jsx
['2','1'].sort(); // ['1','2']
[2,1].sort(); // [1,2]
[2,10].sort(); // [10,2]
```

- 숫자 요소를 정렬할 때는 sort 메서드에 비교 함수를 인수로 전달한다.
  - `< 0` : 첫번째 인수 우선 정렬
  - `= 0` : 그대로
  - `> 0` : 두번째 인수를 앞으로 정렬
```jsx
let points = [40,100,1,5,2,25,10];

// 오름차순
points.sort((a,b) => a-b); //(7) [1, 2, 5, 10, 25, 40, 100]

// 배열에서 최소, 최대값 취득
console.log(points[0], points[points.length-1]); //1, 100

// 내림차순
points.sort((a,b) => b-a);//(7) [100, 40, 25, 10, 5, 2, 1]
```

### 27.9.2 Array.prototype.forEach
- for 문을 대체할 수 있는 고차 함수.
- 내부 반복문을 통해 자신을 호출한 배열을 순회하면서 콜백함수로 전달받아 반복 호출.
- 원본 배열 변경 X
```jsx
[1,2,3].forEach((item, index, arr) => {
    console.log(`요소값 : ${item}, 인덱스 : ${index}, this:${JSON.stringify(arr)}`)
})
/*
요소값 : 1, 인덱스 : 0, this:[1,2,3]
요소값 : 2, 인덱스 : 1, this:[1,2,3]
요소값 : 3, 인덱스 : 2, this:[1,2,3] 
*/

const result = [1,2,3].forEach((item, index, arr) => {
  console.log(`요소값 : ${item}, 인덱스 : ${index}, this:${JSON.stringify(arr)}`)
});
console.log(result); //undefined

```
### 27.9.3 Array.prototype.map
- 콜백 함수의 반환값들로 구성된 새로울 배열을 반환.
- 원본 배열 변경 X
```jsx
const numbers = [1,4,9];

const roots = numbers.map(item => Math.sqrt(item));

console.log(numbers); //[1,4,9];
console.log(roots); //[1,2,3]
```

### 27.9.4 Array.prototype.filter
- 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환.
- 원본 배열 변경 X
- 배열에서 필터링 조건을 만족하는 특정 요소만 추출하여 새로운 배열을 만들때 유용.
```jsx
const numbers = [1,2,3,4,5];

const odds = numbers.filter(item => item%2);
console.log(odds); //[1,3,5]
```

### 27.9.5 Array.prototype.reduce
- 배열의 각 요소를 순회하며 callback 함수의 실행값을 누적하여 하나의 결과값을 반환.
- 모든 요소를 순회하며 하나의 결과값을 구해야 하는 경우 사용.
```jsx
const sum = [1,2,3,4].reduce((previousValue,currentValue) => previousValue+currentValue,0);

console.log(sum); //10
```

### 27.9.6 Array.prototype.some
- 배열의 요소 중 콜백함수를 만족하는 요소가 1개 이상 존재하는지 확인하여 boolean 값으로 반환.
```jsx
const result = [5,10,15].some(item => item > 10);
console.log(result) //true

const result = [5,10,15].some(item => item < 0);
console.log(result) //false

```
### 27.9.7 Array.prototype.every
- 배열의 요소를 순회하며 콜백함수의 조건을 모두 만족하는지 확인하여 boolean 값으로 반환.
```jsx
const result = [5,10,15].every(item => item > 10);
console.log(result);//false

const result = [5,10,15].every(item => item > 0);
console.log(result);//true
```

### 27.9.8 Array.prototype.find
- 배열의 요소를 순회하면서 콜백 함수의 반환값이 true인 첫 번째 요소를 반환
```jsx
const users = [
    {id:1, name:'Lee'},
    {id:2, name:'Kim'},
    {id:3, name:'Choi'},
    {id:4, name:'Park'},
]
const result = users.find(user => user.id === 2);
console.log(result.name); // Kim
```

### 27.9.9 Array.prototype.findIndex
- 배열의 요소를 순회하면서 콜백 함수의 반환값이 true인 첫 번째 요소의 인덱스를 반환
```jsx
const users = [
    {id:1, name:'Lee'},
    {id:2, name:'Kim'},
    {id:3, name:'Choi'},
    {id:4, name:'Park'},
]
const result = users.findIndex(user => user.id === 2);
console.log(result); //1
```
### 27.9.10 Array.prototype.flatMap
- map 메서드를 통해 생성된 새로운 배열을 평화화한다.
- map과 flat 메서드를 순차적으로 실행하는 효과.
```jsx
const arr = ['hello', 'world'];

let result = arr.map(x => x.split(''));
// [['h', 'e', 'l', 'l', 'o'],['w', 'o', 'r', 'l', 'd']]
let result = arr.map(x => x.split('')).flat();
// ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

arr.flatMap(x => x.split(''));
//['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
```


