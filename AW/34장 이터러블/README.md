# 34장 이터러블

---

### 34.1 이터레이션 프로토콜
> ES6에서 도입된 이터레이션 프로토콜(iteration protocol)은 데이터 컬렉션을 순회하기 위한 프로토콜(미리 약속된 규칙)이다. 이터레이션 프로토콜을 준수한 객체는 for…of 문으로 순회할 수 있고 Spread 문법의 피연산자가 될 수 있다.
이터레이션 프로토콜에는 이터러블 프로토콜(iterable protocol)과 이터레이터 프로토콜(iterator protocol)이 있다.
1. 이터러블
- 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.
- `Symbol.iterator`를 자신의 프로퍼티로 가지고 있는(prototype도 포함) 객체가 이터러블 객체이다. `Symbol.iterator`는 object를 반환하고, arguments를 받지 않으며 이터레이터 프로토콜을 따르는 함수이다.
- 배열은 Symbol.iterator 메소드를 소유한다. 따라서 배열은 이터러블 프로토콜을 준수한 이터러블이다.

2. 이터레이터
> iterator protocol 은 value( finite 또는 infinite) 들의 sequence 를 만드는 표준 방법을 정의합니다.
객체가 next() 메소드를 가지고 있고, 아래의 규칙에 따라 구현되었다면 그 객체는 iterator이다.
- 이터레이터 프로토콜은 어떠한 value들의 배열을 만드는 표준 방법에 대한 규약
- 객체가 next() 메소드를 가졌다면 그 객체를 이터레이터라고 한다.
> **`next()` 메소드 규칙**
> 1. object를 반환하며 arguments를 받지 않는다.
> 2. object는 value와 done으로 구성되어 있다.
> 3. value는 Iterator가 반환하는 모든 JS 값이며 done이 true라면 생략될 수 있다.
> 4. done은 Iterator가 마지막 반복 작업을 마쳤을 경우 true. 만약 iterator에 return 값이 있다면 value의 값으로 지정된다. Iterator의 작업이 남아있을 경우 false. Iterator에 done 프로퍼티 자체를 특정짓지 않은 것과 동일하다.

### i.e.
- String, Array, TypedArray, Map, Set등은 내장 이터러블이다. 이들은 처음부터 `Symbol.iterator` 프로퍼티를 가진다.
```js
const array = [1, 2, 3];
const arrayIterator = array[Symbol.iterator]();

arrayIterator.next();
arrayIterator.next();
arrayIterator.next();
arrayIterator.next();
```
![img1.png](https://velog.velcdn.com/images%2Fcode-bebop%2Fpost%2F04253efc-a027-4782-951b-d2b504c64cc8%2F%EC%9D%B4%ED%84%B0%201.PNG)
- 배열을 생성해서 array 변수에 담았다. 그리고 array.Symbol.iterator 메소드를 사용해서 array의 iterator를 반환받아 arrayIterator 변수에 담았다.
그리고 arrayIterator.next()를 실행하니 value와 done으로 구성된 object를 반환하고 있다. value와 done은 위에 설명한 규칙을 잘 따르고 있다. 그러므로 arrayIterator는 이터레이터 프로토콜을 잘 따르고 있고, 이터레이터이다.

### 34.2 빌트인 이터러블
- 34.5 에서 설명
### 34.3 for...of 문
- for..of문은 이터레이터 프로토콜을 준수하는 객체의 next 메서드를 호출하고, 호출로부터 반환된 value 값을 for..of 문의 변수에 할당한다. 이터레이터 리절트 값의 done 값이 false면 계속해서 순회하고, true면 순회를 중단한다.
```js
const array = [1, 2, 3];

for (let item of array) {
  console.log(item);
} // 1 2 3

const string = "hello";

for (let letter of string) {
  console.log(letter);
} // 'h' 'e' 'l' 'l' 'o'

const map = new Map([
  ["a", "1"],
  ["b", "2"],
  ["c", "3"],
]);

for (let [key, value] of map) {
  console.log(key, value);
} // a, 1 b, 2 c, 3
```
이 코드는 아래와 같이 내부적으로 동작한다
```js
const array = [1, 2, 3];

const iterator = array[Symbol.iterator]();

for (;;) {
  const res = iterator.next();

  if (res.done) break;

  console.log(res);
}
```
### 34.4 이터러블과 유사 배열 객체
- 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다. length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고, 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다. Symbol.iterator 메서드가 없기 때문에 for ... of 문으로 순회할 수 없다.

```js
const arrayLike = {
  0:1,
  1:2,
  2:3,
  length:3
};

for(let i=0;i<arrayLike.length;i++){
  console.log(arrayLike[i])
}
//1 2 3

for(let x of arrayLike){
  console.log(x)
}
// TypeError: arrayLike is not iterable
```
물론 배열 메서드들도 사용할 수 없기 때문에 만약 사용하려면 배열로 먼저 만들어주면 된다.
````js
let sum = arrayLike.reduce((arr,cur) => arr+cur,0)
//TypeError: arrayLike.reduce is not a function

// 유사 배열 객체이지만, 이터러블이 아니기 때문에 스프레드 문법을 사용해 배열로 만드는건 불가하다.
let sum = Array.from(arrayLike).reduce((arr,cur) => arr+cur,0)
console.log(sum)
// 6
````
- 단, arguments, NodeList, HTMLColection은 유사 배열 객체이면서 이터러블이다. (모든 유사 배열 객체가 이터러블인 것은 아니다.)
- 이터러블이기 때문에 for ... of 로 순회하는 것은 가능하지만 여전히 배열 메서드들은 사용할 수 없고, 배열로 만들어줘야 사용 가능하다.



### 34.5 이터레이션 프로토콜의 필요성
- 이터레이블 객체들은 for...of, spread연산자, 비구조화 할당, Map, Set, Promise.all 등의 API를 사용 할 수 있다고 설명하였다. 바꿔 말하면 이 많은 API들은 이터레이블 객체에만 호환된다. 이것을 '데이터 소비자'라고 이야기 할 수 있다.
- JS에는 내장 이터러블(Built-in Iterable)이 존재
> Array, String, Map, Set, TypedArray(Int8Array, Uint8Array, Uint8ClampedArray, Int16Array, Uint16Array, Int32Array, Uint32Array, Float32Array, Float64Array), DOM data structure(NodeList, HTMLCollection), Arguments
- 다양한 데이터 소스들이 모두 자신만의 순회 방식을 갖게 되면 위에 설명한 데이터 소비자들은 각각의 순회 방식을 모두 지원해야 한다. 하지만 이 다양한 데이터 소스들이 이터레이션 프로토콜을 따른다면 데이터 소비자들은 이터레이터를 반환 받아 순회하며 작업을 처리하면 된다.
- 즉 데이터 소비자들은 이터레이션 프로토콜만을 지원하면 되는 것
![img2.png](https://velog.velcdn.com/images%2Fcode-bebop%2Fpost%2F8f18da66-96cc-4304-8765-d9f2aaecc689%2F%EC%9D%B4%ED%84%B0%202.png)
### 34.6 사용자 정의 이터러블
- 내가 Object에 for...of나 Spread연산자 등을 사용하고 싶다면 Object를 이터러블 객체로 바꿀 수 있다.
````js
const obj = {};

console.log(Symbol.iterator in obj);

[...obj];
````
![img3.png](https://velog.velcdn.com/images%2Fcode-bebop%2Fpost%2F8fe75a65-a9c5-48ef-b02a-9f077a70c461%2F%EC%9D%B4%ED%84%B0%203.PNG)
-  obj는 `Symbol.iterator`를 프로퍼티로 갖고 있지 않다. 그래서 Spread연산자를 사용하고 싶어도 not iterable error를 받을 뿐이다.
   그럼 obj의 프로퍼티에 `Symbol.iterator`를 추가해보자.
````js
const obj = {
  [Symbol.iterator]() {
    let a = 1;
    return {
      next() {
        return a < 5 ? {value: a++, done: false} : {value: undefined, done: true};
      }
    }
  }
};

console.log(Symbol.iterator in obj);

const iterator = obj[Symbol.iterator]();
iterator.next();
iterator.next();
iterator.next();
iterator.next();
iterator.next();
````
![img4.png](https://velog.velcdn.com/images%2Fcode-bebop%2Fpost%2Fb2728c8f-617d-475f-b815-4a4d36a1ea9b%2F%EC%9D%B4%ED%84%B0%204.PNG)
- obj가 객체임에도 불구하고 이터러블 객체와 유사한 동작을 보여주고 있다. 그렇다면 과연 obj는 for...of문이나 Spread연산자도 사용할 수 있을까?

````js
const obj = {
  [Symbol.iterator]() {
    let a = 1;
    return {
      next() {
        return a < 5 ? {value: a++, done: false} : {value: undefined, done: true};
      }
    }
  }
};

const iterator = obj[Symbol.iterator]();

for(item of obj) {
  console.log(item);
};
[...obj];
````
![img5.png](https://velog.velcdn.com/images%2Fcode-bebop%2Fpost%2Facfd7d0d-0af0-4937-9f0e-80981513f396%2F%EC%9D%B4%ED%84%B0%205.PNG)
- 이로써 obj는 커스텀 이터러블 객체가 되었다.
- 
1. 사용자 정의 이터러블 구현
2. 이터러블을 생성하는 함수
3. 이터러블이면서 이터레이터인 객체를 생성하는 함수
4. 무한 이터러블과 지연 평가


---
# 끝