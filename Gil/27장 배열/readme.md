<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter27.%20%EB%B0%B0%EC%97%B4&fontSize=60" width="100%" alt="">

# **27.1 배열이란?**
배열은 여러개의 값을 순차적으로 나열한 자료구조의 객체(객체타입)이다.<br>
배열이 가지고 있는 값을 **요소**라고 하고, 자신을 나타내는 순서를 **인덱스**라고 한다.

```js
// 'a, b, c'는 요소
const arr = ['a','b','c'];

console.log(arr[0]); //a
```

<br>

# **27.2 자바스크립트 배열은 배열이 아니다**
자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다. <br>
배열의 요소는 프로퍼티값이므로 어떤 타입의 값이라도 배열의 요소가 될 수 있다.

```js
const Gil = [
    'string', //string
    10,       //number
    true,     //bollean
    null,     //null
    undefined,//undifined
    NaN,      //NaN
    Infinity, //Infinity
    [],       //Array
    {},       //객체
    function (){} //함수
];
```

> 자바스크립트의 배열의 메모리공간은 연속적으로 이어져 있지 않을수 있는데 이것을 **희소배열** 이라고 한다.

## **27.2.1 일반적인 배열과 자바스크립트 배열의 장단점**
 * 일반적인 배열은 요소 삽입, 삭제 경우 효율 X
 * 자바스크립트 배열은 일반적인 배열과 상대적으로 느릴수 있으나 요소 삽입, 삭제 효율 O

<br>

# **27.3 Length 프로퍼티와 희소 배열**
배열의 length는 명시적으로 할당하여 길이를 결정할 수 있다.
```js
const gil = [1,2,3,4,5];
gil.legth = 3;

console.log(gil); //[1,2,3]
```

<br>

배열의 요소개수 보다 큰 값을 할당 할수 있지만 숫자(length 숫자)는 변경 되지만 **실제 배열에서는 아무런 변함이 없고 비어있는 요소를 위해 메모리에서 공간을 확보도 안하고, 요소도 생성 되지 않는다.**
<br>

이처럼 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 것을 ```희소배열```이라고 한다.

>희소배열은 문법적으로 허용하지만 사용하지 말도록 하자! 배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다...!

<br>

# **27.4 배열 생성**
## **27.4.1 배열 리터럴**
```js
//일반적인 배열 리터럴
const arr = [1,2,3];

//length가 0인 빈 배열
const arr2 = [];
```

<br>

## **27.4.2 Array 생성자함수**

```js
//인수가 1개 이고 숫자인 경우 length의 값이 인수(10)인 희소배열을 생성한다.
const arr = new Array(10);

//빈배열 생성(배열 리터럴과 동일)
new Array();// []

//인수가 2개 이상이거나 숫자가 아닌경우 인수의 값을 요소로 갖는다.
new Array(1,2,3); // [1,2,3]
new Array({}); // [{}]

//new 연산자가 없더라도 동작한다.
Array(1,2,3);// [1,2,3]
```

<br>

## **27.4.3 Array.of**
생성자함수(new)와 다르게 인수가 1개이고 숫자더라도 인수를 배열 요소로 갖는다. 
```js
Array.of(1); // [1]
```

<br>

## **27.4.4 Array.from**
유사배열객체를 반환하여 생성하고, 이터러블을 반환하여 배열을 생성한다.
```js
//유사배열객체
Array.from({length:2, 0:'a', 1:'b'}); //['a','b']

//이터러블(문자열)
Array.from('Gil'); //['G','i','l']
```

<br>

두번째 인수로 전달한 콜백함수로 인덱스를 순차적으로 전달하면서 호출하여 반환값으로 구성된 배열을 반환한다.

```js
Array.from({length:3},(_,i) => i); //[0,1,2]
```

<br>

# **27.5 배열 요소의 참조**
배열을 참조할때 대괄호를 사용하며(arr[0]) 존재하지 않는 요소에 접근하거나 희소배열을 참조 하면 undefined가 반환된다.

<br>

# **27.6 배열 요소의 추가와 갱신**
인덱스는 요소의 위치를 나타내므로 반드시 **0 이상의 정수 또는 정수형태의 문자열** 을 사용하며, 정수이외의 값을 인덱스처럼 사용하면 프로퍼티가 생성되며 이 프로퍼티는 length에 영향을 주지 않는다.

```js
const gil = [];

//배열요소 추가
gil[0] = 1;
gil["1"] = 2;

//프로퍼티 추가
gil['hi'] = 3;
gil.age = 31;
gil[-1] = 6;

console.log(gil);// [1, 2, hi:3, age:31, '-1':6]
console.log(gil.length); //2
```

<br>

# **27.7 배열 요소의 삭제**
Array.prototype.splice 메서드를 이용해 배열을 삭제할 수 있다.

```js
const gil = [1,2,3];

gil.splice(1,1);//[1.3]
console.log(gil.length) //2
```

<br>

# **27.8 배열 메서드**
배열에는 원본배열을 직접 변경하는 메서드와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반황하는 메서드가 있다.

<br>

# **27.8.1 Array.isArray**
Array.isArray는 전달된 인수가 배열일 경우 boolean으로 반환하는 메서드이다.

<br>

## **27.8.2 Array.prototype.indexOf**
indexOf 메서드는 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.

* 중복되는 요소가 있으면 첫번째로 검색된 요소의 인덱스틑 반환
* 존재하지 않으면 -1를 반환

```js
const gil = ['apple','banana','banana','orange'];

gil.indexOf('apple'); //0
gil.indexOf('melon');//-1
gil.indexOf('banana',2);//2

//includes 메서드를 사용하면 가독성이 올라간다.
if(!gil.includes('melon')){
    gil.push('melon');
}
console.log(gil); //['apple','banana','banana','orange','melon']
```

<br>

## **27.8.3 Array.prototype.push**
push 메서드는 인수로 받은 모든 값을 원본 배열의 마지막 요소로 추가하고 length 값이 반환된다.<br>
원본배열을 직접 변경하므로 추가하는 요소가 1개라면 length를 이용하거나 스프레드 문밥을 이용 하는것이 좋다.

```js
const gil = [1,2];
const result = gil.push(3);
console.log(result);//3

console.log(gil) //[1,2,3]

//length를 이용하는 방법
gil[gil.length] = 4;
console.log(gil); //[1,2,3,4]

//스프레드 문법을 이용하는 방법(ES6)
const newArr = [...gil,5];
console.log(newArr);//[1,2,3,4,5]
```

<br>

## **27.8.4 Array.prototype.pop**
pop 메서드는 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.
push와 pop을 이용해서 스택을 구현할 수 있다.


>생성자함수를 이용해 스택 구현
```js
const Stack = (function(){
    function Stack(array = []){
        if(!Array.isArray(array)){
            throw new TypeError(`${array} is not an array.`);
        }
        this.array = array;
    }
    
    Stack.prototype = {
        constructor: Stack,
        
        push(value){
            return this.array.push(value);
        },
        pop(){
            return this.array.pop();
        },
        entries(){
            return [...this.array];
        }
    };
    return Stack;
}());
```
<br>

>클래스로 구현

```js
class Stack {
    #array;
    
    contuctor(array = []){
        if(!Array.isArray(array)){
            throw new TypeError(`${array} is not an array.`);
        }
        this.#array = array;
    }
    
    push(value){
        return this.#array.push(value);
    }
    
    pop(){
        return this.#array.pop();
    }
    
    entries(){
        return [...this.#array];
    }
}
```

<br>

## **27.8.5 Array.prototype.unshift**
push와 반대로 원본배열 선두에 요소를 추가할 수 있다.(length값을 반환한다.)

```js
const gil = [1,2];
let result = gil.unshift(3,4);
console.log(gil);//[3,4,1,2]

//스프레드 문법
const gil2=[3,4];
const newArr = [3, ...gil2];
console.log(newArr); //[3,3,4]
```

<br>

## **27.8.6 Array.prototype.shift**
첫번째 요소를 제거하고 제거한 요소를 반환한다. push,pop을 이용하면 스택을 구현할 수 있다면 shift,push를 이용하면 큐를 구현할 수 있다. <br>
큐는 마지막에 요소를 추가하고 가장먼저 밀어 넣은 데이터를 꺼내는 역할을 한다.

> **Stack:** 마지막에 밀어 넣은 최신 데이터를 취득 <br>
> **Queue:** 밀어넣은 순서대로 데이터 취득
 
<br>

## **27.8.7 Array.prototype.concat**
concat 메서드는 인수로 전달된 값을 원본배열의 마지막 요소로 추가한 새로운 배열을 반환한다.
push, unshift를 대체할 수 있다.

* concat 메서드는 원본배열을 변경 하지 않으며, 사용시 변수에 할당받아야 한다.
* 인수로 전달받은 배열을 해체하여 새로운 배열의 마지막 요소로 추가한다.

```js
const arr = [3,4];
let result = [1,2].concat(arr);

result = result.concat(5,6);
console.log(result); //[1,2,3,4,5,6]

//스프레드 문법
const arr2 = [3,4];
let result2;
result2 = [...[1,2],...arr2];
console.log(result2);//[1,2,3,4]
```

<br>

## **27.8.8 Array.prototype.splice**
배열에서 중간에 있는 요소를 추가하거나 제거할때 사용한다. (제거된 요소가 반환되고, 원본배열이 수정된다.)

```js
const arr =[1,2,3,4];

//start: 삭제 시작할 배열의 index
//deleteCount: start로 부터 삭제할 요소 개수
//items: 삭제후 삽입할 요소
const gil = arr.splice(start,deleteCount,items);

const result = arr.splice(1,1,30);
console.log(result);//[2]
console.log(arr);//[1,30,3,4]
```

<br>

## **27.8.9 Array.prototype.slice**
인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.

```js
const arr = [1,2];

arr.slice(0,1);//[1]
arr.slice(0);//[1,2]
arr.slice(-1);//[2]
```

slice는 얕은 복사를 한다.

```js
//slice 이용
function sum(){
    var arr = Array.prototype.slice.call(arguments);
    return arr.reduce(function(pre,cur){
        return pre + cur;
    },0);
}

console.log(sum(1,2,3));//6

//from 이용
function sum2(){
    const arr = Array.from(arguments);
    return arr.reduce((pre,cur)=> pre + cur,0);
}
console.log(sum(2,3,4));//9

//스프레드 문법
function sum3(){
    const arr = [...arguments];
    return arr.reduce((pre,cur)=> pre+cur, 1);
}
console.log(sum3(3,4));//8
```

<br>

## **27.8.10 Array.prototype.join**
원본배열을 문자열로 반환한 후, 인수로 받은 문자열(구분자)로 연결한 문자열을 반환한다.

```js
const gil = [1,2,3];
gil.join('+'); // '1+2+3'
```

<br>

## **27.8.11 Array.prototype.reverse**
원본배열의 순서를 반대로 뒤집으며 변경된 배열을 반환한다.

```js
const gil = [1,2,3];
gil.reverse(); //[3,2,1]
console.log(gil);
```

<br>

## **27.8.12 Array.prototype.fill**
fill은 인수로 전달 받은 값을 배열의 처음부터 끝까지 요소로 채운다. (원본 배열을 변경한다.)

```js
const arr = new Array(3);

const result = arr.fill(1);
console.log(arr); //[1,1,1]
console.log(result); //[1,1,1]

result.fill(0,1,2);

console.log(result); //[0,0,1]
```

fill 메서드로 요소를 채울 경우 모든 요소를 하나의 값만으로 채울 수밖에 없는 단점이 있다. Array.from 메서드를 사용하면 두번째 인수로 전달한 콜백 함수를 통해 요소값을 만들면서 배열을 채울수 있다.

```js
const sequences = (length = 0) => Array.from({length}, (_, i) => i);
console.log(sequences(3)); //[0,1,2]
```

<br>

## **27.8.13 Array.prototype.includes**
배열 내에 특정 요소가 포함되어 있는지 확인하여 ture 또는 false 값을 반환한다.

```js
const arr = [1,2,3];
arr.includes(2); //true
arr.includes(1, 1);//false
arr.includes(3, -1);//true
```

<br>

## **27.8.14 Array.prototype.flat**
flat 메서드는 인수로 전달한 깊이만큼 배열을 평탄화 한다.(배열 속 배열을 끄집어냄...) Infinity를 쓰면 모든 배열을 평탄화 한다.

```js
[1,[2,[3,[4]]]].flat(); //[1,2,[3,[4]]]
[1,[2,[3,[4]]]].flat().flat(); //[1,2,3,[4]]
[1,[2,[3,[4]]]].flat(Infinity); //[1,2,3,4]
```

<br>

# **27.9 배열 고차 함수** 

