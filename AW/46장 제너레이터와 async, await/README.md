# 46장 프로미스

---

## 46.1 제너레이터란?
> 코드블록의 실행을 일시 중지 했다가 필요한 시점에 재개할 수 있는 특수한 함수
1. 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
    - 제너레이터는 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다. 이는 함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있다는 것을 의미한다.
2. 제너레이터 함수는 함수 호출자와 함수의 상태를 주고 받을 수 있다.
   - 제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.
3. 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
   - 제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.
## 46.2 제너레이터 함수의 정의
- 제너레이터 함수는 `function*` 키워드로 선언
- 제너레이터 함수는 화살표 함수로 정의할 수 없다.
- 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없다.
- 하나이상의 yield 표현식을 포함

## 46.3 제너레이터 객체
- 제너레이터 함수를 호출하면 일반 함수 처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다. 제너레이터 함수가 반환한 제너레이터 객체는 이터러블 이면서 동시에 이터레이터다.
- 다시 말해, 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다.
```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();

// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log("next" in generator); // true
```
- 제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에는 없는 ① return ② throw 메서드를 갖는다. 제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.
   - next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 이터레이터 리절트 객체를 반환한다. { value : ***, done : boolean}
   - return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다. { value: '인수', done: true }
  -  throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다. { value: undefined, done : true}
```js
function* genFunc() {
	try {
		yield 1;
		yield 2;
		yield 3;
	} catch (e) {
		console.error(e);
	}
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.return("End!")); // {value: "End!", done: true}
```
```js
function* genFunc() {
  try {
    yield 1;
    yield 2;
    yield 3;
  } catch (e) {
    console.error(e);
  }
}

const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}
console.log(generator.throw("Error!")); // {value: undefined, done: true}
```
   
## 46.4 제너레이터의 일시 중지와 재개
- yield 키워드와 next 메서드를 통해 실행을 중지햇다가 다시 재게할수있음
- yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다
- 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지 된다. 이때 함수의 제어권이 호출자로 양도 된다.
- 이때 제너레이터 객체의 next 메서드는 valuse, done 프로피티를 갖는 이터레이터 리절트 객체를 반환함
```js
// 제너레이터 함수
function* genFunc() {
  yield 1;
  yield 2;
  yield 3;
}

// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc();

console.log(generator.next()); // {value: 1, done: false}

console.log(generator.next()); // {value: 2, done: false}

console.log(generator.next()); // {value: 3, done: false}

console.log(generator.next()); // {value: undefined, done: true}
```
- 제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지된다. 이때 함수의 제어권이 다시 호출자로 양도(yield)된다. 이후 필요한 시점에 홏출자가 또다시 next 메서드를 호출하면 일시 중지된 코드부터 실행을 재개하기 시작하여 다음 yield 표현식까지 실행되고 또 다시 일시 중지된다.

리절트 객체 { value : , done : }를 반환하는데, yield 표현식이 끝까지 실행될 경우 value 에는 undefined를, done 은 true인 리절트 객체를 반환하고 종료된다.
## 46.6 async / await
- 제너레이터를 사용해서 비동기 처리를 동기처리 처럼 동작하도록 구현했지만 코드가 무척 장황해지고 가독성이 안좋음
- async/await는 프로미스를 기반으로 동작한다.
- 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

1. async 함수
- await 키워드는 반드시 async 함수 내부에서 사용해야 한다. async 함수는 async 키워드를 사용해 정의하며 언제나 프로미스를 반환한다.
- 암묵적으로 반환값을 resolve 하는 프로미스를 반환
- 클래스의 constructor(생성자 함수) 메서드는 async 메서드가 될 수 없다. 클래스의 constructor는 인스턴스를 반환해야 하지만 async 함수는 언제나 프로미스를 반환해야 한다.


```js
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) { return n; };
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
  async foo(n) { return n; }
};
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
  async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5
```
2. await 키워드
- 프로미스가 settled 상태가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve 한 처리 결과를 반환한다. await 키워드는 반드시 프로미스 앞에서 사용해야 한다.
- 모든 프로미스에 await 을 사용하는것은 주의해야함 (개별적으로 수행되는 비동기 처리는 하나의 await 안에서 처리)
```js
<body>
  <pre></pre>
  <script>
    const getGithubUserName = async (id) => {
      const res = await fetch(`https://api.github.com/users/${id}`); // ①
      const { name } = await res.json(); // ②
      name
        ? (document.querySelector(
            "pre"
          ).innerHTML = `찾은 이름은 ${name}입니다 😁`)
        : (document.querySelector("pre").innerHTML =
            "존재하지 않는 사용자입니다 🥲");
    };

    const result = window.prompt("찾고 싶은 사용자의 id를 입력하세요");
    if (result) getGithubUserName(result);
  </script>
</body>
```
- 프로미스에 await 키워드를 사용하는 것은 주의해야 한다.
```js
async function foo() {
  const a = await new Promise((resolve) => setTimeout(() => resolve(1), 3000));
  const b = await new Promise((resolve) => setTimeout(() => resolve(2), 2000));
  const c = await new Promise((resolve) => setTimeout(() => resolve(3), 1000));

  console.log([a, b, c]); // [1, 2, 3]
}

foo(); // 약 6초 소요된다.
```
```js
async function foo() {
  const res = await Promise.all([
    new Promise((resolve) => setTimeout(() => resolve(1), 3000)),
    new Promise((resolve) => setTimeout(() => resolve(2), 2000)),
    new Promise((resolve) => setTimeout(() => resolve(3), 1000)),
  ]);

  console.log(res); // [1, 2, 3]
}

foo(); // 약 3초 소요된다.
```
3. 에러처리
- 비동기 처리를 위한 콜백 패턴의 단점 중 가장 심각한 것은 에러 처리가 곤란하다는 것이다. 에러는 호출자(caller) 방향으로 전파된다. 즉, 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다. 하지만 비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니기 때문에 try ... catch 문을 사용해 에러를 캐치할 수 없다.
- async/await에서 에러 처리는 try ... catch 문을 사용할 수 있다. 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.
- async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.
```js
try {
  setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다
  console.error('캐치한 에러', e);
}
```
```js
 const getGithubUserName = async (id) => {
      try {
        const res = await fetch(`https://api.github.com/users/${id}`); // ①
        const { name } = await res.json();
        name ? document.querySelector('pre').innerHTML= `찾은 이름은 ${name}입니다 😁` : document.querySelector('pre').innerHTML = '존재하지 않는 사용자입니다 🥲'};
      } catch(err){
        console.error(err)
      }

    const result = window.prompt('찾고 싶은 사용자의 id를 입력하세요');
    if(result)getGithubUserName(result);
```
---
# 끝