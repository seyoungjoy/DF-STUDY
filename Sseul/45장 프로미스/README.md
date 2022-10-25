# 45. 프로미스
자바스크립트는 비동기 처리를 위해 콜백 패턴을 사용한다.   
하지만 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러의 처치가 곤란하며 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있기 때문에 프로미스가 등장하였다.
<br/><br/>

## 45.1 비동기 처리를 위한 콜백 패턴의 단점
### 45.1.1 콜백 헬
<img width="767" alt="스크린샷 2022-10-25 오전 10 40 23" src="https://user-images.githubusercontent.com/104816267/197662177-93a17a66-c71e-4572-ac0e-f9d908fe9470.png">
함수 내부에 비동기로 동작하는 코드를 포함한 함수는 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.
(비동기 그림을 보다시피 순서대로 동작하지 않지 않는다.)   
<br/><br/>

### 비동기로 처리하는 이유
동기방식은 순차적이기 때문에 코드를 읽기 편하지만 느리다. 브라우저와 웹서버가 리로드를 하지않고도 자바스크립트로 소통하는것을 ajax인데 만약 컴퓨터가 동기적으로 통신한다면 우리는 아무것도 할 수가 없다.
비동기방식은 동기보다 복잡하지만 결과가 주어지는데 시간이 걸리더라도 그 시간 동안 다른 작업을 할 수 있고 서버와의 통신들은 언제 끝날지 모르기때문에 자원을 효율적으로 사용할 수 있는 장점이 있다.
<br/>

### 비동기를 제어하는 방법
- 콜백 : 비동기 함수의 콜백 내부에서 다음 작업(비동기 함수를 호출 등)하는 것이다.
```js
function fn() {
    setTimeout(() => {
        console.log('하나');
        setTimeout(() => {
            console.log('둘');
            setTimeout(() => {
                console.log('셋');
            }, 0);
        }, 0);
    }, 0);
}
fn(); // 결과 순서 => '하나', '둘', '셋'
```
콜백 지옥(callback hell)이란 콜백 함수를 익명 함수로 전달하는 과정에서 또 다시 콜백 안에 함수 호출이 반복되어 코드의 들여쓰기 순준이 감당하기 힘들 정도로 깊어지는 현상을 말한다.   
주로 이벤트 처리나 서버 통신과 같은 비동기 작업을 제어하기 위해서 사용되는데 이러한 프로그래밍은 가독성이 떨어지고 코드 수정을 어렵게한다.
<br/><br/>

### 45.1.2 에러처리의 한계
```js
try {
  setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
  // 에러를 캐치하지 못한다
  console.error('캐치한 에러', e);
}
```
setTimeout이 호출되면 setTimeout 함수의 실행 컨텍스트가 생성되어 콜 스택에 푸시되어 실행되고 비동기이므로 즉시 종료되어 콜 스택에서 제거된다. 그래서 setTimeout의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태라서 catch에서 에러를 잡아내지 못한다.
<br/><br/>

## 45.2 프로미스의 생성
프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체이다.   
Promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스를 생성한다. 이 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 콜백 함수는 resolve와 reject 함수를 인수로 전달받는다.
<br/>

프로미스의 상태 : pending(처음), fulfilled(resolve 호출), rejected(reject 호출)
<br/>

- 비동기 처리 성공 : resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경
- 비동기 처리 실패 : reject 함수를 호출해 프로미스를 rejected 상태로 변경
- fullfilled 또는 rejected 상태를 settled 상태라고 한다.
- settled 상태가 되면 다른 상태로 변화할 수 없다.
- 비동기 처리 상태와 더불어 비동기 처리 결과도 상태로 갖는다.
<br/><br/>

## 45.3 프로미스의 후속 처리 메서드
1. then : 2개의 콜백 함수를 인수로 전달받는다.
프로미스가 fulfiled 상태(resolve 함수가 호출된 상태)가 되면 호출된다. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
프로미스가 rejected 상태(reject 함수가 호출한 상태)가 되면 호출한다. 이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다.
2. catch : 한 개의 콜백 함수를 인수로 전달받는다. rejected 상태인 경우에만 호출
3. finally : 한 개의 콜백 함수를 인수로 전달받는다. 성공, 실패와 상관없이 무조건 한번 호출

- 에러처리는 then 메서드에서 하지 말고 catch 메서드에서 하는 것을 권장한다.
<br/><br/>

## 45.4 프로미스의 에러처리
```js
const url = "잘못된 주소";

// 부적절한 URL이 지정되었기 때문에 에러가 발생한다.
promiseGet(url)
  .then((res) => console.log(res))
  .catch((err) => console.error(err));
  ```
catch 메서드를 호출하면 내부적으로 then(undefined, onRejected)을 호출한다.
단, then 메서드의 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고, 코드가 복잡해져서 가독성이 좋지 않다.
<br/><br/>

## 45.5 프로미스 체이닝
```js
const url = "https://jsonplaceholder.typicode.com";

// id가 1인 post의 userId를 획득
promiseGet(`${url}/posts/1`)
  // 취득한 post의 userId로 user 정보를 획득
  .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
  .then((userInfo) => console.log(userInfo))
  .catch((err) => console.error(err));
```
.then -> .then -> .catch 후속 처리 메서드는 항상 프로미스를 반환하므로 연속적으로 호출할 수 있는데 이것이 프로미스체이닝이다.

```js
const url = "https://jsonplaceholder.typicode.com";

(async () => {
  // id가 1인 post의 userId를 획득
  const { userId } = await promiseGet(`${url}/posts/1`);

  // 취득한 post의 userId로 user 정보를 획득
  const userInfo = await promiseGet(`${url}/users/${userId}`);

  console.log(userInfo);
})();
```
프로미스체이닝은 async/await 를 통해 해결 할 수 있다.
<br/><br/>

## 45.6 프로미스의 정적 메서드
Promise는 객체이므로 메서드를 가질 수 있다.
<br/>

### Promise.resolve / Promise.reject
이미 존재하는 값을 래핑하여 프로미스를 생성하기 위한 용도이다. 인수로 전달받은 값을 resolve/reject하는 프로미스를 생성한다.
<br/>

### Promise.all
여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.   
Promise.all 메서드는 인수로 전달받은 배열의 모든 프로미스가 모두 fulfilled 상태가 되면 종료한다.   
하나라도 rejected 상태가 되면 나머지 프로미스를 기다리지 않고 즉시 종료한다.
<br/>

### Promise.race
가장 먼저 fulfilled 상태가 된 프로미스의 처리 결과를 resolve하는 새로운 프로미스를 반환하는 메서드   
메서드에 전달된 프로미스가 하나라도 rejected 상태가 되면 에러를 reject하는 새로운 프로미스를 즉시 반환한다.
<br/>

### Promise.allSettled
전달받은 프로미스가 모두 settled(fulfilled or rejected) 상태가 되면 처리 결과를 배열로 반환한다.
<br/><br/>

## 45.7 마이크로태스크 큐
<img src="https://user-images.githubusercontent.com/104816267/197685818-1a496c6e-99e8-4561-9ba7-a7d6360727a4.gif">

- 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 **마이크태스크 큐 microtask queue/job queue에 저장**   
- 마이크로태스크 큐는 태스크 큐보다 우선순위가 높다   
- promise와 setTimeout이 같이 호출된다면 promise가 먼저 실행된다.
<br/><br/>

## 45.8 fetch
- fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API 다.
- HTTP 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달한다.
1. GET : 존재하는 자원을 요청
2. POST : 새로운 자원 생성 요청
3. PUT : 존재하는 자원 변경 요청
4. DELETE : 존재하는 자원 삭제 요청   

<a href="https://velog.io/@eunjin/JavaScript-fetch-%ED%95%A8%EC%88%98-%EC%93%B0%EB%8A%94-%EB%B2%95-fetch-%ED%95%A8%EC%88%98%EB%A1%9C-HTTP-%EC%9A%94%EC%B2%AD%ED%95%98%EB%8A%94-%EB%B2%95">참조</a>