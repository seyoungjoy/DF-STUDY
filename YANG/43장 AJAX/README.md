# 43 Ajax
## 43.1 Ajax란?
- Asynchronous Javascript and XML
- 자바스크립트를 이용해서 비동기적으로 서버와 통신할 수 있는 기술.
- 장점
  - 비동기 방식으로 데이터를 요청하기 때문에 블로킹이 발생하지 않는다.
  - 필요한 부분만 서버로부터 요청하기 때문에 불필요한 통신이 발생하지 않는다.
  - 변경이 필요한 부분만 업데이트되기 때문에 화면 깜빡임이 없다.

## 43.2 JSON
- 클라이언트와 서버단의 통신을 위한 데이터 포맷.

### JSON.stringify
- 객체를 JSON 포맷의 문자열로 변환
- 직렬화

### JSON.parse
- JSON 포맷의 문자열을 객체로 반환.
- 역직렬화

## 43.3 XMLHttpRequest
### 43.3.1 XMLHttpRequest 객체 생성
```jsx
const xhr = new XMLHttpRequest();
```

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드
### 43.3.3 HTTP 요청 전송
1. XMLHttpRequest.prototype.open http 요청 초기화
2. XMLHttpRequest.prototype.setRequestHeader 헤더 값 설정 가능
3. XMLHttpRequest.prototype.send http 요청

```jsx
const xhr = new XMLHttpRequest();

// 요청 초기화
xhr.open('GET', '/users');

xhr.setRequestHeader('content-type', 'application/json')

// open으로 초기화된 http 요청을 서버에 전송
xhr.send();
// 페이로드가 객체인 경우에는 stringify로 직렬화를 한후 전달
xhr.send(JSON.stringify({id:1, content:"HTML", completed:false}));
```
