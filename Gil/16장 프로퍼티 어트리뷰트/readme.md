<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter16.%20%EB%82%B4%EB%B6%80%20%EC%8A%AC%EB%A1%AF%EA%B3%BC%20%EB%82%B4%EB%B6%80%20%EB%A9%94%EC%84%9C%EB%93%9C&fontSize=40">

# **16.1 내부 슬롯과 내부 메서드**
내부슬롯과 내부메서드는 자바스크립트 엔진에서의 의사 프로퍼티와 의사 메서드이다.
<br>즉 객체의 구현 사양이며(값, 변경가능여부, 재할당 가능여부 등 스펙을 나타내는 것) 이 값을 외부로 노출 시키진 않지만 간접적으로 개발자가 수정할 수 있다.

```js
const gil = {
    name: 'gil'
};

console.log(gil.[[prototype]]); //syntax Error 직접접근 X
console.log(gil.__proto__);
//{constructor: ƒ, __defineGetter__: ƒ, __defineSetter__: ƒ, hasOwnProperty: ƒ, __lookupGetter__: ƒ, …}
```
<br>

# **16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체**
프로퍼티를 생성할때 자바스크립트 엔진은 프로퍼티 어트리뷰트(프로퍼티 상태: 값 / 갱신 가능 여부 / 열거 가능여부(배열에 넣어 나열 가능여부) / 재정의 가능 여부)를 정의한다.

```js
const gil = {
    age: 31
};
console.log(Object.getOwnPropertyDescriptor(gil, 'age'));
// {value: 31, writable: true, enumerable: true, configurable: true}
```
>Object.getOwnPropertyDescriptor 메서드를 이용해 한개 프로퍼티의 상태를 간접으로 확인할 수 있다.

<br>

```js
console.log(Object.getOwnPropertyDescriptor(gil, 'age'));
```

* 여기서 첫번째 매개변수에 들어간 인수값 gil은 객체의 참조(객체이름)를 전달하고, 두번째 매개변수인 인수값 age는 프로퍼티키를 문자열(string)로 전달한다.

```js
const gil = {
    name: 'yg'
};
gil.age = 31;

console.log(Object.getOwnPropertyDescriptors(gil));
/*
age: {value: 31, writable: true, enumerable: true, configurable: true}
name: {value: 'yg', writable: true, enumerable: true, configurable: true}
*/
```

<br>

# **16.3 데이터 프로퍼티와 접근자 프로퍼티**

>데이터프로퍼티: 프로퍼티키, 프로퍼티값으로 구성된 일반적인 프로퍼티
<br>

>접근자프로퍼티: 데이터프로퍼티의 값을 읽고, 저장할때 호출되는 접근자함수로 구성된 프로퍼티

<br>

# **16.3.1 데이터 프로퍼티**

>**[[Value]]** (어트리뷰트) / **value** (디스크립터 객체 프로퍼티)
>>* 반환되는 값
>>* 값을 변경하면 [[Value]]에 값을 재할당한다. 값이 없으면 생성 후 값을 저장한다.

<br>

>**[[Writable]]** (어트리뷰트) / **writable** (디스크립터 객체 프로퍼티)
>>* 값의 변경 가능 여부(불리언)
>>* false이면 읽기 전용 프로퍼티가 된다.

>**[[Enumerable]]** (어트리뷰트) / **enumerable** (디스크립터 객체 프로퍼티)
>>* 열거 가능 여부(for...in, Object.keys 등을 이용해서 콘솔로 찍었을때 배열에 담겨서 나오는가?)
>>* false이면 열거할 수 없다.

<br>

>**[[Configurable]]** (어트리뷰트) / **configurable** (디스크립터 객체 프로퍼티)
>>* 프로퍼티의 재정의 가능 여부(불리언)
>>* false이면 프로퍼티의 삭제, 프로퍼티 어트리뷰트의 값변경이 불가능하다.
>>* [[Configurable]]가 false 일때 [[Writable]]가 true이라면 [[Value]]의 값과, [[Writable]]을 false로 변경은 가능하다.

<br>

```js
const gil = {
    name:'gil'
};
console.log(Object.getOwnPropertyDescriptor(gil, 'name'));
//{value: 'gil', writable: true, enumerable: true, configurable: true}
```

위 코드를 보면 프로퍼티가 생성되면 [[Value]]의 값은 **프로퍼티값**으로 초기화 되고, [[Writable]], [[Enumerable]], [[Configurable]]은 **true**로 초기화가 된다. <br>

*📌동적으로 추가된 프로퍼티도 마찬가지!*

<br>

# **16.3.2 접근자 프로퍼티**

자체적으로 값(프로퍼티 어튜리뷰트 [[Value]])을 가지지 않으며 데이터 프로퍼티의 값을 읽거나(참조), 저장하는 역할을 한다.

>**[[Get]]** (어트리뷰트) / **get** (디스크립터 객체 프로퍼티)
>>접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값, getter함수가 호출되고 그 결과가 프로퍼티 값으로 **반환**된다.

<br>

>**[[Set]]** (어트리뷰트) / **set** (디스크립터 객체 프로퍼티)
>>접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, setter함수가 호출되고 그 결과가 프로퍼티 값으로 **저장**된다.

<br>

```js
const gil = {
    AA: 'YG',
    BB: 'JYP',

    get team(){//getter함수
        return `${this.AA} ${this.BB}`;
    },

    set team(who){//setter함수
        [this.AA, this.BB] = who.split(' ');
    }
};

gil.team = 'Hive Cube'; // 값 저장, 접근자프로퍼티 setter 호출
console.log(gil.team); // 값 참조, 접근자 프로퍼티 getter 호출

let descriptor = Object.getOwnPropertyDescriptor(gil, 'AA');
console.log(descriptor);
// {value: 'Hive', writable: true, enumerable: true, configurable: true}

descriptor = Object.getOwnPropertyDescriptor(gil, 'team');
console.log(descriptor);
// {enumerable: true, configurable: true, get: ƒ, set: ƒ}
```

>### **접근자 프로퍼티로 프로퍼티값에 접근해서 내부적으로 [[Get]] 내부메서드 호출시 동작**
>* 프로퍼티 키가 유효한지 확인(문자열, 심벌 이어야 한다.)
>* 프로토타입 체인에서 프로퍼티를 검색
>* 접근자프로퍼티인지 데이터프로퍼티인지 확인
>* 접근자프로퍼티의 어트리뷰트[[Get]]의 값을 반환(getter함수를 호출하여 그 결과를 반환) // [[Get]]의 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 디스크립터 객체의 get의 프로퍼티 값과 같다.

<br>

## **프로토타입**
객체의 상위(부모)역할을 하는 객체로 프로토타입은 하위(자식) 객체에게 자신의 프로퍼티와 메서드를 상속(준다)한다. 상속받은 하위객체는 자신의 프로퍼티, 메서드인 것 처럼 자유롭게 사용할 수 있다.

```js
function Gil(x, y){
    this.x = x;
    this.y = y;
}

const gil2 = new Gil(1, 2); //생성자함수에 의해 생성된 하위객체

console.log(gil2);
```

프로토타입 체인이란 하위의 객체에서 프로퍼티나 메서드에 접근할때 값이 없으면 상위 객체로 검색을 이어 가는 것을 말한다.

<br>

# **16.4 프로퍼티 정의**
새로운 프로퍼티를 추가하면서 어트리뷰트를 명시적으로 정의하거나 재정의 하는것을 말한다.

```js
const gil = {};

Object.defineProperty(gil, 'age', {
    value: 'none',
    writable: true,
    enumerable: true,
    configurable: true
});
Object.defineProperty(gil, 'say', {
    value: 'hi'
    //writable: false,
    //enumerable: false,
    //configurable: false
});

// gil.say는 enumerable: false 이므로 ['age'] 만 찍힌다.
console.log(Object.keys(gil)); 

// gil.say는 writable: false 이므로 값은 그대로 hi이다.
gil.say = 'hellow';
console.log(gil.say);

// gil.say는 configurable: false 이므로 프로퍼티는 삭제 되지 않는다.
delete gil.say;
console.log(gil.say); // hi
```
<br>

**defineProperty로 정의시 생략했을때 기본값**
* value: undefined
* get: undefined
* set: undefined
* writable: false
* enumerable: false
* configurable: false

📌 Object.defineProperties 메서드를 이용하면 한번에 여러개의 프로퍼티를 정의할 수 있다.

<br>

# **16.5 객체 변경 방지**

<br>

## **16.5.1 객체 확장 금지**
Object.preventExtensions 메서드로 프로퍼티 추가가 안 되도록할 수 있다.
```js
const gil = {name: 'gil'};
Object.preventExtensions(gil); //확장금지

gil.where = 'seoul'; //확장X
Object.defineProperty(gil, 'age', {//TypeError
    value: 'none'
});
```
📌 Object.isExtensible 메서드로 확장 여부를 확인 가능하다.(불리언)

<br>

## **16.5.2 객체 밀봉**
Object.seal 메서드로 읽기(참조,열거)와 쓰기(값 변경)만 가능하다.
```js
const gil = {name: 'gil'};
Object.seal(gil); //읽기, 쓰기만 가능

gil.where = 'seoul'; //확장X
delete gil.name; // 삭제X
Object.defineProperty(gil, 'name', {configurable});//TypeError 재정의 X
```
📌 Object.isSealed 메서드로 밀봉 객체 여부를 확인 가능하다.(불리언)

<br>

## **16.5.3 객체 동결**
Object.freeze 메서드로 읽기(참조,열거)만 가능하다.
```js
const gil = {name: 'gil'};
Object.freeze(gil); //읽기만 가능

gil.where = 'seoul'; //확장x
gil.name = 'young'; //값수정x
delete gil.name; // 삭제X
Object.defineProperty(gil, 'name', {configurable});//TypeError 재정의 X
```
📌 Object.isFrozen 메서드로 동결 객체 여부를 확인 가능하다.(불리언)

<br>

## **16.5.4 불변 객체**
Object.freeze 메서드를 사용하더라도 중첩객체의 동결은 할 수 없다.

```js
const gil ={
    name: {gil: 'young'} //중첩객체
};
```

그래서 재귀적(반복 호출하여)으로 Object.freeze 메서드를 사용해야 한다.

```js
function deepFreeze(target){
    if(target && typeof target === 'object' && !Object.isFrozen(target)){
        Object.freeze(target);

        Object.keys(target).forEach(key => deepFreeze(target[key]));

        return target;
    }
}

const gil ={
    name: {gil: 'young'},
    where: 'seoul'
};

deepFreeze(gil);

gil.name.gil = 'hi'
console.log(gil); // gil.name.gil = young 변경되지 않았다.
```