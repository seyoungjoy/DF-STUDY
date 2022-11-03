# 46 제너레이터와 async/await
## 46.1 제너레이터란?
- 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수
- 제너레이터와 일반 함수의 차이
  - 제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
  - 함수 호출자와 함수의 상태를 주고 받을 수 있다.
  - 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.

## 46.2 제너레이터 함수의 정의
```jsx
// 제너레이터 함수 선언문
function* genDecFunc(){
    yield 1;
}

// 제너레이터 함수 표현식
const getExpFunc = function* (){
    yield 1;
}

// 제너레이터 메서드
const obj = {
    *genObjMethod(){
        yield 1;
    }
}

// 제너레이터 클래스 메서드
class MyClass{
    * genClsMethod(){
        yield 1;
    }
}
```
- 화살표 함수와 생성자 함수로 호출할 수 없다.

## 46.3 제너레이터 객체
- 제너레이터 함수를 호출하면 함수가 실행되는 것이 아니라 제너레이터 객체를 생성한다.
- 근데 이 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
```jsx
function* getFunc(){
    yield 1;
    yield 2;
    yield 3;
}
// 제너레이터 
const generator = getFunc();

console.log(generator)
```

> 이터레이션 프로토콜
> - 순회 가능한 자료구조를 만들기 위해 약속한 규칙
> - 이터러블 : 이터레이터를 리턴하는 `[Symbol.interator]()` 메서드를 가진 객체
> - 이터레이터 : `{value:값, done:true/false}` 형태의 이터레이터 객체를 리턴하는 `next()` 메서드를 가진 객체


## 46.6 async/await
- 제너레이터를 이용해서 비동기를 동기처럼 동작하도록 구현하는것은 매우 코드가 길어짐.
- 그래서 이를 간단하게 표현하기 위해 async/await가 도입됨.
- promise의 then/catch/finally 후속처리 메서드 없이 동기처럼 구현 가능.
```jsx
const fetch = require('node-fetch');

async function fetchTodo(){
    const url = 'https://jsonplaceholder.typicode.com/posts/1';
    
    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
}
fetchTodo();
```
## 46.6.1 async 함수
- async 함수는 async 키워드를 사용해 언제나 프로미스를 반환한다.
```jsx
async function foo(n) {return n;}
foo(1).then(v=> console.log(v))
```

## 46.6.2 await 키워드
- 프로미스가 settled(resolve, reject) 가 될때까지 기다렸다가 await를 실행
- 
```jsx
const get = async id => {
    const res = await fetch(`https://api.github.com/users/${id}`);
    const {name} = await res.json();
    console.log(name);
}

get("setoung")
```
