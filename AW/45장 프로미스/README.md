# 45장 프로미스

---

## 45.1 비동기 처리를 위한 콜백 패턴의 단점
- 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.
- 비동기 함수의 처리 결과에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다.
- 일반적인 구조 : 비동기 처리가 성공/실패하면 콜백 함수 호출
- 위 의 콜백 함수에서 또 후속으로 비동기 처리를 할 때 콜백 체인이 발생함
## 45.2 프로미스의 생성
```js
const promise = new Promise((resolve, reject) =>{
	if (/*성공*/){
		resolve('result');
    } else {
		reject('failure reason');
    }
})
```
- pending : 비동기 처리가 **아직** 수행되지 않은 상태 (프로미스가 생성된 직후 기본 상태)
- fulfilled : 비동기 처리가 **성공적으로 수행된 상태** (resolve 함수 호출)
- rejected :  비동기 처리가 **실패적으로 수행된 상태** (reject 함수 호출)

프로미스의 상태는 resolve 혹은 reject 함수를 호출하는 것으로 결정됨

## 45.3 프로미스의 후속 처리 메서드
1. Promise.prototype.then 
 - 첫번째 콜백 함수는 프로미스가 fulfilled 상태가 되면 호출
 - 두번째 콜백 함수는 프로미스가 rejected 상태가 되면 호출
 - 언제나 프로미스 반환
2. Promise.prototype.catch
- 프로미스가 rejected 상태일때만 호출
- 언제나 프로미스 반환
3. Promise.prototype.finally
- 콜백함수는 프로미스의 성공 또는 실패와 상관없이 무조건 한번 호출
- 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을때 유용하다.
- 언제나 프로미스 반환

## 45.4 프로미스의 에러 처리
- then 메서드의 두번째 콜백함수로 처리
- 비동기 처리에서 발생한 에러는 프로미스의 후속처리 메서드 catch를 사용해 처리 할 수도있음

## 45.5 프로미스 체이닝
```js
const url = 'https://naver.com'

promiseGet(`${url}/posts/1`)
.then(({userId}) => promiseGet(`${url}/users/${userId}`))
.then(userInfo => console.log(userInfo))
catch(err -> console.error(err))
```

## 45.6 프로미스의 정적 메서드
1. Promise.resolve / Promise.reject
- 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용
2. Promise.all
- 여러개의 비동기 처리를 모두 병렬 처리할 떄 사용
3. Promise.race
- 가장 먼저 fulfilled 상태가 된 프로미스의 처리결과를 resolve 하는 새로운 프로미스를 반환
- reject 되면 all과 동일하게 처리
4. Promise.allSetteled
- 전달받은 프로미스가 모두 settled 상태가 되면 처리결과를 배열로 반환한다.

## 45.7 마이크로태스크 큐
- 프로미스의 후속 처리 메서드의 콜백 함수는 마이크로태스크 큐에 저장
- 마이크로 테스트 큐는 태스크 큐보다 우선순위가 높음

## 45.8 fetch
- http 응답을 나타내는 Response 객체를 래핑한 promise 객체를 반환한다
- fetch 함수가 반환하는 프로미스는 기본적으로 404 not found 나 500 internal server error 와 같은 HTTP에러가 발생해도 에러를 Reject하지 않고 불리언 타입의 ok 상태를 false로 설정한 response 객체를 resolve 한다.
- 오프라인 등의 네트워크 장애나 CORS에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject 한다.

---
# 끝