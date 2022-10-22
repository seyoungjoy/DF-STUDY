# 43장 Ajax

---

## 43.1 요청과 응답
> Ajax란 JS를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식이다.
- Web API인 XMLHttpRequest 객체를 기반으로 동작
- Ajax를 기반으로 동작하는 구글 맵스 때문에 재평가됨
- Ajax를 통해 변경할 필요가 없는 부분은 다시 렌더링하지 않고, 변경할 필요가 있는 부분만 한정적으로 랜더링하는 방식이 가능해졌따!!
  - 서버로 부터 불필요한 통신 할 필요 없음
  - 변경이 필요없는 부분은 렌더링 하지 않기 떄문에 순간적으로 깜박이는 현상 없음
  - 비동기 방식이므로 블로킹이 발생하지 않는다

## 43.2 JSON
> JavaScript Object Notation은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다.
- JS에 종속되지 않는 독립형 데이터 포맷이므로, 대부분의 프로그래밍 언어에서 사용할 수 있다.

#### 1. JSON 표기방식
```js
{
   "name" : "Lee",
   "age" : 20,
   "alive" : true,
   "hobby" : ["traveling","tennis"]
}
```
- JSON의 키는 ***반드시 큰따옴표***로 묶어야 한다

#### 2. JSON.stringify
- JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 우리가 서버로 객체를 전송하려면 객체를 문자열화 해야하는데 이를 Serializing(직렬화)라 한다.
- 이 메서드는 객체뿐만 아니라 배열도 JSON포맷의 문자열로 변환한다.

#### 3. JSON.parse
- JSON.stringify 메서드는 객체를 JSON 포맷의 문자열로 변환한다. 우리가 서버로 객체를 전송하려면 객체를 문자열화 해야하는데 이를 Serializing(직렬화)라 한다.
- 이 메서드는 객체뿐만 아니라 배열도 JSON포맷의 문자열로 변환한다.

#### 4. XMLHttpRequest
브라우저는 주소창이나 HTML의 form 태그 또는 a 태그를 통해 HTTP 요청 전송 기능을 기본 제공한다. 자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다. Web API인 XMLHttpRequest 객체는 HTTP 요청 전송과 HTTP 응답 수신을 위한 다양한 메서드와 프로퍼티를 제공한다.

- HTTP 요청 전송
HTTP 요청을 전송하는 경우 다음 순서를 따른다.
1. XMLHttpRequest.prototype.open 메서드로 HTTP 요청을 초기화한다.
2. 필요에 따라 XMLHttpRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

##### XMLHttpRequest.prototype.open
  -  open 메서드는 서버에 전송할 HTTP 요청을 초기화한다. open 메서드를 호출하는 방법은 다음과 같다.
   > xhr.open(method, url[, async])

 - method: HTTP 요청 메서드
 - async: 비동기 요청 여부, 옵션으로 기본값은 true이며 비동기 방식으로 동작한다.

 > HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종료와 목적(리소스에 대한 행위)을 알리는 방법이다.
   주로 5가지 요청 메서드 (GET, POST, PUT, PATCH, DELETE 등)를 사용하여 CURD를 구현한다.

##### XMLHttpRequest.prototype.send
   send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다. 기본적으로 서버로 전송하는 데이터는 GET, POST 요청 메서드에 따라 전송 방식에 차이가 있다.

##### XMLHttpRequest.prototype.setRequestHeader
   setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다. setRequestHeader 메서드는 반드시 open 메서드를 호출한 이후에 호출해야 한다. 자주 사용하는 HTTP 요청 헤더인 Content-type 과 Accept에 대해 살펴보자
Content-type은 요청 몸체에 담아 전송할 데이터의 MIME 타입의 정보를 표현한다.

- HTTP 응답 처리
서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치해야 한다. XMLHttpRequest 객체는 onreadystatechange, onload, onerror 같은 이벤트 핸들러 프로퍼티를 갖는다. 이 이벤트 핸들러 프로퍼티 중에서 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 HTTP응답을 처리할 수 있다.

---
# 끝