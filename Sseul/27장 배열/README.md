# 27. 배열

## 27.1 배열이란?
배열은 중요하다. 원시값, 객체, 함수 배열 등 모든 것은 배열의 요소가 될 수 있다. 배열과 일반객체의 가장 명확한 차이는 **값의 순서**와 **length 프로퍼티**다. 배열의 장점은 순차적으로 요소에 접근 할 수 있고, 특정 위치부터 순차적으로 접근 할 수도 있다는 것이다.
<br><br>

## 27.2 자바스크립트 배열은 배열이 아니다
자바스크립트의 배열은 자료구조에서 말하는 일반적인 의미의 배열과 다른 배열을 갖는다. 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수도 있다.<br>
정리하자면 일반적인 배열은 인덱스 요소에 빠르게 접근할 수 있지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않다.
자바스크립트 배열은 인덱스 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서는 느리지만, 특정 요소를 검색하거나 삽입 또는 삭제하는 경우에는 빠른 성능을 기대할 수 있다.
<br><br>

## 27.3 length 프로퍼티와 희소 배열
```js
const arr = [1, 2, 3, 4, 5];

// 현재 length 프로퍼티 값인 5보다 작은 숫자 값 3을 length 프로퍼티에 할당
arr.length = 3;

// 배열의 길이가 5에서 3으로 줄어든다.
console.log(arr); // [1, 2, 3]
```
배열의 길이를 바탕으로 결정되지만 임의의 숫자 값을 명시적으로 할당할 수도 있다.length 프로퍼티 값보다 작은 숫자 값을 할당하면 배열의 길이가 줄어든다.

```js
const arr = [1];

// 현재 length 프로퍼티 값인 1보다 큰 숫자 값 3을 length 프로퍼티에 할당
arr.length = 3;

// length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다.
console.log(arr.length); // 3
console.log(arr); // [1, empty × 2]
```
현재 length 프로퍼티 값보다 큰 숫자 값을 할당하는 경우 length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다. 
**배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 희소 배열이라 하는데, 자바스크립트는 희소 배열을 문법적으로 허용한다.**
--> 결론 : 희소 배열을 허용하지만 사용하지 않는 것이 좋다.
<br><br>

## 27.4 배열 생성
배열도 다양한 생성 방식이 있다. 
```js
const arr = [1, 2, 3];
```
**배열 리터럴**
<br>

```js
new Array(1, 2, 3); // -> [1, 2, 3]

new Array(10);
// -> [empty * 10] 희소배열, 전달된 인수가 1개이고 숫자인 경우

new Array(); // -> []

new Array({}); // -> [{}]
//전달된 인수가 1개지만 숫자가 아니면 인수를 요소로 갖는 배열을 생성한다.
```
Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작하므로 주의가 필요하다.
<br>

```js
Array.of(1, 2, 3); // -> [1, 2, 3]

Array.of(1); // -> [1]

Array.of('string'); // -> ['string'] 
```
ES6에서 도입된 **Array.of** 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다. Array.of는 Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
<br>

```js
// 유사 배열 객체를 변환하여 배열을 생성한다.
Array.from({ length: 2, 0: 'a', 1: 'b' }); // -> ['a', 'b']

// 이터러블을 변환하여 배열을 생성한다. 문자열은 이터러블이다.
Array.from('Hello'); // -> ['H', 'e', 'l', 'l', 'o']
```
ES6에서 도입된 **Array.from** 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.
<br><br>

## 27.5 배열 요소의 참조
```js
const arr = [1, 2];

console.log(arr[0]); // 1
console.log(arr[1]); // 2
console.log(arr[2]); // undefined
```
배열의 요소를 참조할 때에는 대괄호([]) 표기법을 사용한다. 대괄호 안에는 인덱스가 와야 한다. 
<br><br>

## 27.6 배열 요소의 추가와 갱신
```js
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr['1'] = 2;

// 프로퍼티 추가
arr['foo'] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```
인덱스는 요소의 위치를 나타내므로 반드시 0이상의 정수(또는 정수 형태의 문자열)를 사용해야 한다. 만약 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성되며, 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.
<br><br>

## 27.7 배열 요소의 삭제
delete로 배열의 요소를 삭제 할 수 있지만 **length 프로퍼티에 영향을 주지 않는 희소 배열**이 되기 때문에 delete 연산자는 사용하지 않는 것이 좋다. 완전히 삭제하려면 Array.prototype.splice 메서드를 사용한다.
<br><br>

## 27.8 배열 메서드
1. 원본 배열을 직접 변경하는 메서드
2. 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드

```js
const arr = [1];

arr.push(2); // 1번
console.log(arr); // [1, 2]

const result = arr.concat(3); //2번
console.log(arr);    // [1, 2]
console.log(result); // [1, 2, 3]
```
**가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.**

1. Array.isArray : 전달된 인수가 배열이면 true, 배열이 아니면 false를 반환한다. 
```js
Array.isArray([]); // true
Array.isArray(); // false
```
2. Array.prototype.indexOf : 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.
```js
const arr = [1, 2, 2, 3];
arr.indexOf(2); // 요소 2 검색 1 반환
arr.indexOf(4); // 요소 4가 없어서 -1 반환
```

3. Array.prototype.push : 인수로 전달받은 모든 값을 배열의 마지막 요소로 추가, 원본 배열을 직접 변경한다. 성능에 안좋다.
```js
const arr = [1, 2];
let result = arr.push(3, 4); 
console.log(result); // 4
console.log(arr); // [1, 2, 3, 4]
```
4. Array.prototype.pop : 마지막 요소를 제거하고 제거한 요소를 반환한다. 원본 배열에 빈 배열이면 undefined를 반환한다. 원본 배열을 직접 변경한다.
```js
const arr = [1, 2];
let result = arr.pop();
console.log(result); // 2
console.log(arr); // [1]
```

5. Array.prototype.unshift : 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다. 원본 배열을 직접 변경한다.
```js
const arr = [1, 2];
let result = arr.unshift(3, 4);
console.log(result); // 4
console.log(arr); // [3, 4, 1, 2]
```

6. Array.prototype.shift : 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다. 원본 배열이 빈 배열이면 undefined를 반환한다. 원본 배열을 직접 변경한다.
```js
const arr = [1, 2];
let result = arr.shift();
console.log(result); // 1
console.log(arr); // [2]
```

7. Array.prototype.concat : 인수로 전달된 값을(배열 또는 원시값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다. 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.concat 메서드 대신 ES6의 스프레드 문법을 권장한다.  
```js
const arr1 = [1, 2];
const arr2 = [3, 4];
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(3);
console.log(result); // [1, 2, 3]

// 배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환한다.
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않는다.
console.log(arr1); // [1, 2]
```

8. Array.prototype.splice : 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다. 3개의 매개변수가 있으며 원본 배열을 직접 변경한다.
```js
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
// splice 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]
```

9. Array.prototype.slice : 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다. 원본 배열은 변경되지 않는다. 두 개의 매개변수를 갖는다. 첫번째 인수가 음수인 경우 배열의 끝에서 요소를 복사하여 배열로 반환한다.
```js
const arr = [1, 2, 3];

// arr[0]부터 arr[1] 이전(arr[1] 미포함)까지 복사하여 반환한다.
arr.slice(0, 1); // -> [1]
arr.slice(1, 2); // -> [2]
// 원본은 변경되지 않는다.
console.log(arr); // [1, 2, 3]

const arr = [1, 2, 3];
// 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다.
const copy = arr.slice();
console.log(copy); // [1, 2, 3]
console.log(copy === arr); // false
// 이때 생성된 복사본은 얕은 복사를 통해 생성된다.
```

10. Array.prototype.join : 원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환한다. 구분자는 생략 가능하며 기본 구분자는 콤마(,)다.
```js
const arr = [1, 2, 3, 4];
arr.join(); // -> '1,2,3,4';
arr.join(''); // -> '1234'
arr.join(':'); // -> '1:2:3:4'
```

11. Array.prototype.reverse :  원본 배열의 순서를 반대로 뒤집는다. 원본 배열이 변경되며, 반환값은 변경된 배열이다.
```js
const arr = [1, 2, 3];
const result = arr.reverse();
console.log(arr); // [3, 2, 1]
// 반환값은 변경된 배열이다.
console.log(result); // [3, 2, 1]
```

12. Array.prototype.fill : 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다. 이때 원본 배열이 변경된다.두번째 인수로 요소 채우기를 시작할 인덱스를 전달할 수 있고 세 번째 인수로 요소 채우기를 멈출 인덱스를 전달할 수 있다.
```js
const arr = [1, 2, 3];
arr.fill(0);
console.log(arr); // [0, 0, 0]
```

13. Array.prototype.includes : 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환한다. 첫 번째 인수로 검색할 대상을 지정한다.
```js
const arr = [1, 2, 3];
arr.includes(2); // -> true
arr.includes(100); // -> false
```

14. Array.prototype.flat : 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화 한다.
```js
[1, [2, 3, 4, 5]].flat(); // -> [1, 2, 3, 4, 5]
```
<br>

## 27.9 배열 고차 함수
고차 함수는 함수를 인수로 전달받거나 함수를 반환하는 함수를 말한다. 

1. Array.prototype.sort : 원본 배열을 직접 변경하며 정렬된 배열을 반환한다. 기본적으로 오름차순으로 요소를 정렬한다. 내림차순으로 정렬하려면 sort 메서드 사용 후 reverse 메서드를 사용하여 순서를 뒤집으면 된다.
```js
const fruits = ['Banana', 'Orange', 'Apple'];
fruits.sort();
console.log(fruits); // ['Apple', 'Banana', 'Orange']

//숫자 정렬은 그냥 sort만 쓰면 안된다.
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열의 오름차순 정렬. 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬한다.
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 숫자 배열에서 최소/최대값 취득
console.log(points[0], points[points.length]); // 1

// 숫자 배열의 내림차순 정렬. 비교 함수의 반환값이 0보다 작으면 b를 우선하여 정렬한다.
points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]

// 숫자 배열에서 최대값 취득
console.log(points[0]); // 100
```

2. Array.prototype.forEach : for 문을 대체할 수 있는 고차 함수이다. 자신의 내부에서 반복문을 실행한다.
```js
const numbers = [1, 2, 3];
let pows = [];

// forEach 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
numbers.forEach(item => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]
```

3. Array.prototype.map : 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다. 원본 배열은 변경되지 않는다. **forEach 메서드와 map 메서드의 공통점은 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다는 것이다. 하지만 forEach 메서드는 언제나 undefined를 반환하고, map 메서드는 콜백 함수의 반환값들로 구성된 새로운 배열을 반환하는 차이가 있다.**
```js
const numbers = [1, 4, 9];
const roots = numbers.map(item => Math.sqrt(item));
console.log(roots);   // [ 1, 2, 3 ]
// map 메서드는 원본 배열을 변경하지 않는다
console.log(numbers); // [ 1, 4, 9 ]
```

4. Array.prototype.filter : 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다. 
```js
const words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// ["exuberant", "destruction", "present"]
```

5. Array.prototype.reduce : 자신을 호출한 배열을 모든 요소로 순회하며 인수로 전달받은 콜백 함수를 반복 호출한다. 원본 배열은 변경되지 않는다.
```js
// [1, 2, 3, 4]의 모든 요소의 누적을 구한다.
const sum = [1, 2, 3, 4].reduce((accumulator, currentValue, index, array) => accumulator + currentValue, 0);
console.log(sum); // 10
```

6. Array.prototype.some : 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다. 이때 some 메서드는 콜백 함수의 반환값이 단 한번이라도 참이면 true, 모두 거짓이면 false를 반환한다.
```js
[5, 10, 15].some(item => item > 10); // -> true
[5, 10, 15].some(item => item < 0); // -> false
['apple', 'banana', 'mango'].some(item => item === 'banana'); // -> true
// some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환한다.
[].some(item => item > 3); // -> false
```

7. Array.prototype.every : 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출한다. 이때 every 메서드는 콜백 함수의 반환값이 모두 참이면 true, 단 한번이라도 거짓이면 false를 반환한다.
```js
[5, 10, 15].every(item => item > 3); // -> true
[5, 10, 15].every(item => item > 10); // -> false
// every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환한다.
[].every(item => item > 3); // -> true
```

8. Array.prototype.find : ES6에서 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환한다. true인 요소가 존재하지 않는다면 undefined를 반환한다.
```js
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' }
];

// id가 2인 첫 번째 요소를 반환한다. find 메서드는 배열이 아니라 요소를 반환한다.
users.find(user => user.id === 2); // -> {id: 2, name: 'Kim'}
```

9. Array.prototype.findIndex : ES6에서 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환한다. true인 요소가 존재하지 않는다면 -1을 반환한다.
```js
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 2, name: 'Choi' },
  { id: 3, name: 'Park' }
];

// id가 2인 요소의 인덱스를 구한다.
users.findIndex(user => user.id === 2); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex(user => user.name === 'Park'); // -> 3

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex(predicate('name', 'Park')); // -> 3
```

10. Array.prototype.flatMap : ES10에서 도입된 flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화한다. 즉, map 메서드와 flast 메서드를 순차적으로 실행하는 효과가 있다. 단 flat 메서드처럼 인수를 전달하여 평탄화 깊이를 지정할 수는 없고 1단계만 평탄화한다.