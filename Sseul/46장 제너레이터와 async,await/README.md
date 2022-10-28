# 46. 제너레이터와 async/await

## 46.1 제네레이터란?

**ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수**

- 제네레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
- 함수 호출자와 함수의 상태를 주고받을 수 있다.
- 함수를 호출하면 제너레이터 객체를 반환한다.
  <br/><br/>

## 46.2 제너레이터 함수의 정의

제너레이터 함수는 `function \*` 키워드로 선언한다. 그리고 하나 이상의 yield 표현식을 포함한다.

```js
//  제너레이터 함수 선언문
function* genDecFunc() {
  yield 1;
}

// 제너레이터 함수 표현식
const genExpFunc = function* () {
  yield 1;
};

// 제너레이터 메서드
const obj = {
  *genObjMethod() {
    yield 1;
  },
};

// 제너레이터 클래스 메서드
class MyClass {
  *genClsMethd() {
    yield 1;
  }
}
```

일관성을 유지하기 위해 function 키워드 바로 뒤에 붙이는 것을 권장한다.
단, 제너레이터 함수는 화살표 함수로 정의할 수 없으며, new 연산자와 함께 생성자 함수로 호출할 수 없다.
<br/><br/>

## 46.3 제너레이터 객체

제너레이터 함수를 호출하면 함수를 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 이때 제너레이터 객체는 이터러블(iterable)이면서 이터레이터(iterator)다. 다시 말해서 Symbol.iterator 메서드를 상속받는 이터러블이면서 value와 done 프로퍼티를 갖는 객체를 반환하는 next 메서드를 소유하는 이터레이터다.
<br/>

### next 메서드

next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드블록을 실행하고 yield된 값을 value 프로퍼티의 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
<br/>

### return 메서드

return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
<br/>

### throw 메서드

throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
<br/><br/>

## 46.4 제너레이터의 일시 중지와 재개

```js
// 제너레이터 함수 정의
function* counter() {
  console.log("Point 1");
  yield 1; // 첫번째 next 메소드 호출 시 여기까지 실행된다.
  console.log("Point 2");
  yield 2; // 두번째 next 메소드 호출 시 여기까지 실행된다.
  console.log("Point 3");
  yield 3; // 세번째 next 메소드 호출 시 여기까지 실행된다.
  console.log("Point 4"); // 네번째 next 메소드 호출 시 여기까지 실행된다.
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 제너레이터 객체는 이터러블이며 동시에 이터레이터이다.
// 따라서 Symbol.iterator 메소드로 이터레이터를 별도 생성할 필요가 없다
const generatorObj = counter();

// 첫번째 next 메소드 호출: 첫번째 yield 문까지 실행되고 일시 중단된다.
console.log(generatorObj.next());
// Point 1
// {value: 1, done: false}

// 두번째 next 메소드 호출: 두번째 yield 문까지 실행되고 일시 중단된다.
console.log(generatorObj.next());
// Point 2
// {value: 2, done: false}

// 세번째 next 메소드 호출: 세번째 yield 문까지 실행되고 일시 중단된다.
console.log(generatorObj.next());
// Point 3
// {value: 3, done: false}

// 네번째 next 메소드 호출: 제너레이터 함수 내의 모든 yield 문이 실행되면 done 프로퍼티 값은 true가 된다.
console.log(generatorObj.next());
// Point 4
// {value: undefined, done: true}
```

next 메소드는 이터레이터 리절트 객체와 같이 value, done 프로퍼티를 갖는 객체를 반환한다. value 프로퍼티는 yield 문이 반환한 값이고 done 프로퍼티는 제너레이터 함수 내의 모든 yield 문이 실행되었는지를 나타내는 boolean 타입의 값이다. 마지막 yield 문까지 실행된 상태에서 next 메소드를 호출하면 done 프로퍼티 값은 true가 된다.

<br/><br/>

## 46.5 제너레이터의 활용

제너레이터를 사용하면 then/catch/finally 메서드 없이 처리 결과를 반환할 수 있다. 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있는 것이다.

```js
axios.get("https://jsonplaceholder.typicode.com/todos/1").then((res) => {
  renderData(res.data);
});

// { userId: 1, id: 1, title: "delectus aut autem", completed: false }
```

위 코드를 제너레이터를 사용하여 다시 작성하면!!

```js
function* genFunc() {
  const res = yield axios.get("https://jsonplaceholder.typicode.com/todos/1");
  renderData(res.data);
}

const iter = genFunc();
const req = iter.next().value;

req.then((res) => {
  iter.next(res);
});
```

1. iter.next().value 를 실행하면 yield 표현식까지 실행되면서 axios.get 함수의 프로미스를 리턴하고, 그 값은 req 변수에 할당된다.
2. 프로미스가 resolve되면 iter에 res를 넘겨주면서 iter가 다시 시작하게 되고, iter 내부에서 renderData(res.data)를 통해 가져온 데이터를 렌더링하게 된다.
   <br/><br/>

## 46.6 async/await

async/awiat를 사용하면 프로미스의 then/catch/finally 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요 없이 마치 동기처리처럼 프로미스를 사용할 수 있다.
<br/>

```js
const fetch = require("node-fetch");

// Promise를 반환하는 함수 정의
function getUser(username) {
  return fetch(`https://api.github.com/users/${username}`)
    .then((res) => res.json())
    .then((user) => user.name);
}

async function getUserAll() {
  let user;
  user = await getUser("jeresig");
  console.log(user);

  user = await getUser("ahejlsberg");
  console.log(user);

  user = await getUser("ungmo2");
  console.log(user);
}

getUserAll();
```

- async 함수는 async키워드를 사용해 정의하며 언제나 프로미스를 반환한다
- await 키워드는 프로미스가 settled 상태가 될 때까지 일시 중지하다가
  settled 상태가 되면 프로미스가 resolve한 처리 결과를 재개한다.
- 반드시 async 함수 내부에서 프로미스 앞에 사용해야 한다.
- async/await 에서는 try...catch 문을 사용할 수 있다.
- async 함수 내에서 catch 문은 HTTP 통신에서 발생한 네트워크 에러뿐 아니라
  try 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치할 수 있다.
- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면
  async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.
