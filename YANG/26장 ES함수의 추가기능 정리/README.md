# 26장 ES6 함수의 추가 기능

## 26.1 함수의 구분

- ES6 이전의 자바스크립트 함수는 동일한 함수라도 다양한 형태(일반함수, 생성자 함수, 메서드)로 호출 가능하다.
  - 언뜻 보면 편리한 것 같지만 실수를 유발시킬 수 있고 성능 면에서도 손해다.

```javascript
var foo = function () {
  return 1;
};
foo(); // -> 1
new foo(); // -> foo {}
var obj = { foo: foo };
obj.foo(); // -> 1
```

- 복습 📕
  - callable: 호출할 수 있는 함수 객체
  - constructor: 인스턴스를 생성할 수 있는 함수 객체
  - non-constructor: 인스턴스를 생성할 수 없는 함수 객체
- ES6 이전의 모든 함수는 callable & constructor
  - 즉, 객체에 바인딩된 함수와 콜백함수도 constructor -> prototype 프로퍼티를 가지며, 프로토타입 객체 생성 -> 성능상 문제
- 이를 위해, ES6에서는 함수를 사용 목적에 따라 3가지 종류로 구분한다.

**ES6 함수의 구분**

| ES6 함수의 구분 | constructor | prototype | super | arguments |
| --------------- | ----------- | --------- | ----- | --------- |
| 일반 함수       | O           | O         | X     | O         |
| 메서드          | X           | X         | O     | O         |
| 화살표 함수     | X           | X         | X     | X         |

---

## 26.2 메서드

- ES6 이전 사양에는 메서드에 대한 명확한 정의가 없다. 일반적으로 메서드는 객체에 바인딩된 함수를 일컫는 의미로 사용되었다.
- ES6 사양에서는 메서드(ES6메서드)는 **"메서드 축약 표현으로 정의된 함수"** 만을 의미한다.

```javascript
const obj = {
  x: 1,
  foo() {
    return this.x;
  }, // ES6 메서드
  bar: function () {
    return this.x;
  }, // 일반함수
};
```

- non-constructor → 생성자 함수로 호출 X → 호출시 TypeError
- prototype 프로퍼티 X → 프로토타입 생성 X
- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 `[[HomeObject]]` 가짐 → `super` 참조는 내부 슬롯 `[[HomeObject]]`를 사용하여 수퍼클래스의 메서드를 참조 가능
- ES6 메서드가 아닌 함수는 내부 슬롯 `[[HomeObject]]`를 갖지 않기 때문에 `super` 키워드를 사용할 수 없다 -> 사용시 SyntaxError

```javascript
const base = {
  name: 'Lee',
  sayHi() {
    return `Hi! ${this.name}`;
  },
};
const derived = {
  __proto__: base,
  sayHi() {
    return `${super.sayHi()}. How are you doing?`;
  },
};
console.log(derived.sayHi()); // Hi! Lee. How are you doing?
```

- 메서드를 정의할 대 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전 방식은 권장하지 않는다.

---

## 26.3 화살표 함수

- `function` 키워드 대신 **화살표(`=>`)** 를 통해 간략하게 함수 정의 가능
- 화살표 함수는 콜백 함수 내부에서 `this`가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용

### 26.3.1 화살표 함수 정의

**함수 정의**

- 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의

```javascript
const multiply = (x, y) => x * y;
```

**매개변수 선언**

```javascript
// 매개변수가 여러 개인 경우, 소괄호 ()안에 매개 변수 선언
const arrow1 = (x,y) => { ... }
// 매개변수가 한 개인 경우, 소괄호() 생략 가능
const arrow2= x => { ... }
// 매개변수가 없는 경우, 소괄호() 생략 불가능
const arrow3 = () => { ... }
```

**함수몸체 정의**

- 함수 몸체가 한 줄의 문인 경우 → 중괄호 {} 생략 가능 → 함수 몸체 내부의 문이 표현식인 문이라면 암묵적으로 반환

```javascript
// 함수 몸체가 한 줄의 문 → 중괄호 {} 생략 가능 → 문을 암묵적으로 반환
const power = x => x ** 2;
// const power = x => { return x **2; }와 동일
// 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러 발생
const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'
const now = () => Date.now();
// const now = function() { return Date.now(); };
const identity = (value) => value;
// var identity = function(value) { return value; };
```

- 객체 리터럴을 반환하는 경우 → 객체 리터럴을 소괄호 ()로 감싸주어야함

```javascript
const arrow = () => ({ a: 1 });
// const arrow = () => {return { a:1 }; }
const create = (id, content) => ({ id, content });
// const create = function(id, content) {
//   return {id, content};
// };
```

- 함수 몸체가 여러 줄의 문인 경우 → 중괄호 {} 생략 불가능 → 명시적으로 `return`

```javascript
const sum = (a, b) => {
  const result = a + b;
  return result;
};
```

- 즉시 실행 함수로 사용 가능

```javascript
const person = ((name) => ({
  sayHi() {
    return `Hi! My name is ${name}`;
  },
}))('Joo');
// const person = (name => {
//   return {
//     sayHi() { return `Hi! My name is ${name}`; }
//   }
// })('Joo');
```

- 화살표 함수도 일급 객체이므로 고차함수의 인수로 전달 가능 -> 표현이 간결하고 가독성 향상

```javascript
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});
// ES6
[1, 2, 3].map((v) => v * 2);
```

### 26.3.2 화살표 함수와 일반 함수의 차이

1. **화살표 함수는 non-constructor → 인스턴스 생성 X**

- `new` 연산자를 통해 생성자 함수로 호출 불가능
- prototype 프로퍼티 X → 프로토타입 생성 X

2. **중복된 매개 변수 이름 X**

- 일반 함수는 중복된 매개 변수 이름을 선언해도 에러 발생하지 않음

```javascript
const sum = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```

3. **화살표 함수 자체의 `this`, `arguments`, `super`, `new.target` 바인딩을 갖지 않는다**

- 화살표 함수가 중첩 함수인 경우 상위 스코프에 존재하는 가장 가까운 함수 중에서 화살표 함수가 아닌 함수의 `this`, `arguments`, `super`, `new.target`을 참조한다

### 26.3.3 `this`

- 콜백 함수가 일반 함수로 호출되기 때문에 `this`가 엉키는 현상 발생

```javascript
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  prefixArray(arr) {
    // 인수로 전달된 배열 arr을 순회하며 배열의 모든 요소에 prefix를 추가
    return arr.map(function (item) {
      return this.prefix + ' ' + item;
      // TypeError: Cannot read property 'prefix' of undefined
      // 일반적으로 콜백함수는 일반함수로 호출되어서 this는 전역객체를 가리킴
      // 여기서는 class 내부에서 콜백함수가 일반함수로 호출됨
      // 클래스는 암묵적으로 'strict mode'
      // 'strict mode'에서 일반함수를 호출하면 this에 undefined바인딩
      // 따라서 undefined.prefix가 되기때문에 TypeError
    });
  }
}
```

- 이를 해결하기 위해 ES6 이전에는 3가지 방법 사용

1. `that`을 통하여 자신이 가리키고 싶은 `this`를 기억(지역변수 사용)
2. `Array.prototype.map`의 2번째 매개변수에 `this` 전달
3. `Function.prototype.bind`를 통해 `this` 바인딩

```javascript
// 1. that
prefixArray(arr) {
  // prefixArray 몸체 내부의 this를 that에 할당
  const that = this;
  return arr.map(function (item) {
    return that.prefix + ' ' + item;
  });
}
// 2. Array.prototype.map의 2번째 매개변수
prefixArray(arr) {
  // prefixArray 메서드 몸체 내부에 있는 this를 매개변수로 전달
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }, this);
}
// 3. Function.prototype.bind
prefixArray(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }.bind(this)); // prefixArray 메서드 몸체 내부의 this를 map함수 안에 있는 this로 교체
}
```

- **화살표 함수 사용!**
  - 화살표 함수 자체의 `this` 바인딩 없음 → 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 `this`를 참조
  - Lexical this: 렉시컬 스코프와 같이 화살표 함수가 정의된 위치에 의해 `this`가 결정됨

```javascript
prefixArray(arr) {
  return arr.map(item => `${this.prefix} ${item}`);
}
// 인수로 넘겨질때 화살표 함수 정의가 평가되고 객체가 생성됨
// 즉, prefixArray몸체 내부에서 정의되고 인수로 넘겨진것과 같은 동작원리
// 그때 화살표 함수의 this는 prefixArray의 this를 가리킴
```

- 화살표 함수가 중첩함수인 경우, 상위 스코프에 가장 가까운 함수 중에서 화살표 함수가 아닌 부모 함수의 `this`를 참조

```javascript
// 전역 함수 foo의 상위 컨텍스트는 전역이다.
const foo = () => console.log(this);
foo(); // window
// 중첩 함수 foo의 상위 컨텍스트는 즉시 실행 함수이다.
// 화살표 함수 foo의 this는 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => console.log(this);
  foo();
}.call({ a: 1 })); // { a: 1 }
// 함수 foo는 화살표 함수를 반환한다.
// 반환된 화살표 함수의 this는 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => () => console.log(this);
  foo()();
}.call({ a: 1 })); // { a: 1 }
// increase 프로퍼티에 할당한 화살표 함수의 상위 컨텍스트는 전역이다.
// increase 프로퍼티에 할당한 화살표 함수의 this는 전역 객체를 가리킨다.
const counter = {
  num: 1,
  increase: () => ++this.num,
};
console.log(counter.increase()); // NaN
```

- 화살표 함수 내부의 `this`는 결정된 이후 변경 불가능
  - `Function.prototype.call/apply/bind` 메서드를 통해 `this` 변경 불가능(단, 함수 사용은 가능. `call`,`apply`통해서 함수 호출 가능)

```javascript
window.x = 1;
const normal = function () {
  return this.x;
};
const arrow = () => this.x;
// 화살표 함수의 this는 변경되지 않는다.
console.log(normal.call({ x: 10 })); // 10
console.log(arrow.call({ x: 10 })); // 1
// 사용은 가능하다.
const add = (a, b) => a + b;
console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
console.log(add.bind(null, 1, 2)()); // 3
```

- 메서드나 프로토타입 객체에 화살표 함수를 할당하지 말자 → 둘 다 일반함수로 호출되기 때문에 `this`는 전역객체를 가리킴
  - 메서드 → ES6 메서드 정의를 사용!
  - 프로토타입 객체 → 일반 함수 할당

```javascript
// 메서드
const person = {
  name: 'Joo',
  sayHi: () => console.log(`Hi ${this.name}`)
  // this는 전역객체를 가리킴
  sayHello() {
    console.log(`Hello ${this.name}`);
  },
};
// 프로토타입 메서드
// this는 전역객체를 가리킴
Person.prototype.sayHi = () => console.log(`Hi ${this.name}`);
Person.prototype.sayHello = function () {
  console.log(`Hello ${this.name}`);
};
```

- 클래스 필드에 화살표 함수 할당 가능 → `this`는 constructor의 `this`와 같음 → 클래스가 생성할 인스턴스를 가리킴
- 메서드를 정의할 때는 ES6 메서드를 사용 권장

```javascript
// BAD
class Person {
  // 클래스 필드 정의 제안
  name = 'Joo';
  sayHi = () => console.log(`Hi ${this.name}`);
}
```

```javascript
class Person {
  constructor() {
    this.name = 'Lee';
    // 클래스가 생성한 인스턴스(this)의 sayHi 프로퍼티에 화살표 함수를 할당한다.
    // sayHi 프로퍼티는 인스턴스 프로퍼티이다.
    this.sayHi = () => console.log(`Hi ${this.name}`);
  }
}
```

```javascript
// Good
class Person {
  // 클래스 필드 정의
  name = 'Lee';
  // ES6 메서드 -> 프로토타입 메서드
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
}
const person = new Person();
person.sayHi(); // Hi Lee
```

### 26.3.4 `super`

- 화살표 함수는 함수 자체의 `super` 바인딩이 없다.
- 화살표 함수 내부에서 `super`를 참조하면 상위 스코프의 `super`를 참조

```javascript
class Base {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    return `Hi! ${this.name}`;
  }
}
class Derived extends Base {
  // super 키워드는 ES6 메서드 내에서만 사용 가능하다.
  // 화살표 함수 foo의 상위 스코프는 constructor이다.
  sayHi = () => `${super.sayHi()} how are you doing?`;
  // 위에 클래스 필드 정의는 사실상 constructor안에 인스턴스 메서드를 정의하는것과 같다
  // 따라서 화살표 함수의super는 constructor의 super를 가리킴
  // constructor {
  //   this.sayHi = () => `{super.sayHi()} how are you doing?`;
  // }
}
const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

### 26.3.5 `arguments`

- 화살표 함수는 함수 자체의 `arguments` 바인딩이 없다.
- 상위 스코프의 `arguments`를 참조 → 하지만 화살표 함수 자신에게 전달된 인수 목록을 확인할 수 없음
- 화살표 함수로 가변 인자 함수를 구현할 때는 반드시 rest 파라미터 사용

```javascript
(function () {
  // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
  foo(3, 4);
})(1, 2);
// 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError: arguments is not defined
```

---

## 26.4 Rest 파라미터

### 26.4.1 기본 문법

- Rest 파라미터(나머지 매개변수)는 매개변수 이름 앞에 3개의 점(...)을 붙여서 정의한 매개변수를 의미한다.
- Rest 파라미터는 전달된 인수들의 목록을 **배열**로 전달받는다.

```javascript
function foo(...rest) {
  console.log(rest); // [1, 2, 3]
}
foo(1, 2, 3);
```

- 반드시 마지막 파라미터이어야 한다.
- Rest 파라미터는 단 하나만 선언 가능하다.

```javascript
function foo1(param1, ...rest) {
  console.log(param1); // 1
  console.log(rest); // [2, 3]
}
function foo2(...rest, param1, param2) { }
function foo3(...rest1, ...rest2) { }
foo1(1, 2, 3);
foo2(1, 2, 3); // SyntaxError: Rest parameter must be last formal parameter
foo3(1, 2, 3); // SyntaxError: Rest parameter must be last formal parameter
```

- 함수 객체의 length(매개변수 개수) 프로퍼티에 영향 X

```javascript
function foo(...rest) {}
console.log(foo.length); // 0
function bar(x, y, ...rest) {}
console.log(bar.length); // 2
```

### 26.4.2 Rest 파라미터와 arguments 객체

- ES5에서 `arguments` 객체는 유사배열객체이기 때문에 배열 메서드를 사용할 수 없음 → 배열 메서드를 사용하려면 유사배열객체를 배열로 변환
- ES6부터는 rest 파라미터를 통해 가변 인자 목록을 배열로 직접 전달 받음

```javascript
// ES5
function convertToArray() {
  var array = Array.prototype.slice.call(arguments);
  return array;
}
console.log(convertToArray(1, 2, 3)); // [1, 2, 3]
// ES6
function sum(...args) {
  // rest 파라미터 args에는 배열 [1, 2, 3]이 할당
  return args.reduce((pre, cur) => pre + cur);
}
console.log(sum(1, 2, 3));
```

---

## 26.5 매개변수 기본값

- 자바스크립트 엔진은 매개변수의 개수와 인수의 개수를 체크하지 않는다.
- 인수가 전달되지 않은 매개변수의 값은 `undefined`다.
- 때문에 아래와 같이 인수를 전달하지 않을 경우 의도치 않은 결과가 나올 수 있다.

```javascript
function sum(x, y) {
  return x + y;
}
console.log(sum(1)); // NaN (1 + undefined)
```

- 따라서 인수가 전달되지 않은 경우 매개변수에 기본값을 할당할 필요가 있다.
- 즉, 방어 코드가 필요하다. 아래는 함수 내에서 단축평가를 사용해 매개변수 기본값을 할당하는 방어 코드이다.

```javascript
function sum(x, y) {
  x = x || 0;
  y = y || 0;
  return x + y;
}
```

- ES6에서 도입된 매개변수 기본값을 사용하면 인수 체크 및 초기화를 간소화할 수 있다.
- 단, 인수가 없을때와 `undefined`인 경우만 유효. `null`은 포함되지 않는다.

```javascript
// 초기화
function sum(x = 0, y = 0) {
  return x + y;
}
console.log(sum(1, 2)); // 3
console.log(sum(1)); // 1
// 인수가 없을때와 undefined일 경우만 유효
function logName(name = 'Joo') {
  console.log(name);
}
console.log(logName()); // Joo
console.log(logName(undefined)); // Joo
console.log(logName(null)); // null
```

- rest 파라미터에는 기본값을 지정 불가능

```javascript
function foo(...rest = []) {
  console.log(rest);
}
// SyntaxError: Rest parameter may not have a default initializer
```

- 매개변수 기본값은 length 프로퍼티(매개변수 개수)와 `arguments` 객체에 영향 X

```javascript
function sum(x, y = 0) {
  console.log(arguments);
}
console.log(sum.length); // 1
sum(1); // Arguments { '0': 1};
sum(1, 2); // Arguments { '0': 1, '1': 2};
