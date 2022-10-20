# 43. Ajax

## 43.1 Ajax란?
Ajax(Asynchronous JavaScript and XML)란 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식을 말한다.
<br/><br/>

## 43.2 JSON
JSON(JavaScript Object Notation)은 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷으로 대부분의 프로그래밍 언어에서 사용할 수 있다.

### 43.2.1 JSON 표기 방식
JSON은 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트다. JSON의 키는 반드시 큰따옴표로 묶어야 하며 값은 객체 리터럴과 같은 표기법을 그대로 사용할 수 있다. 하지만 문자열은 반드시 큰따옴표로 묶어야 한다.
<br/>

### 43.2.2 JSON.stringify
JSON.stringify 메서드는 객체를 JSON 포멧의 문자열로 반환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화해야 하는다. 이를 직렬화(serializing)라 한다.
<br/>

### 43.2.3 JSON.parse
JSON.parse 메서드는 JSON 포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화 해야 하는데 이를 역직렬화(deserializing)라 한다.
<br/><br/>

## 43.3 XMLHttpRequest
자바스크립트를 사용하여 HTTP 요청을 전송하려면 XMLHttpRequest 객체와 해당 객체에 포함되어 있는 프로퍼티와 메서드들을 사용한다.
<br/>

### 43.3.1 XMLHttpRequest 객체 생성
XMLHttpRequest 객체는 XMLHttpRequest 생성자 함수를 호출하여 생성한다. XMLHttpRequest 객체는 브라우저에서 제공하는 Web API 이므로 브라우저 환경에서만 정상적으로 실행된다.
<br/>

### 43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드
- readyState : HTTP 요청의 현재 상태를 나타내는 정수 
UNSENT: 0, OPENED: 1, HEADERS_RECEIVED: 2, LOADING: 3, DONE: 4
- status : HTTP 요청에 대한 응답 상태를 나나태는 정수
ex)200 (성공)
<br/><br/>

### 43.3.3 XMLHttpRequest 메서드
- open : HTTP 요청 초기화
- send : HTTP 요청 전송
- abort : 이미 전송된 HTTP 요청 중단
- setRequestHeader : 특정 HTTP 요청 헤더의 값을 설정
- getRequestHeader : 특정 HTTP 요청 헤더의 값을 문자열로 반환