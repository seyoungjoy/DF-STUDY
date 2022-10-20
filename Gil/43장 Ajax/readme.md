<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter43.%20Ajax&fontSize=60" width="100%" alt="">

# **43.1 Ajax란?**
자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식이다.

>web API인 XMLHttpRequest 객체를 기반으로 동작한다.

이전에는 완전한 HTMl을 서버로부터 전송받아 웹페이지 전체를 처음부터 다시 렌더링하는 방식으로 동작했다.

📌 이전방식의 단점

* 불필요한 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받아 불필요한 데이터 통신이 발생한다.
* 변경할 필요가 없는 부분까지 다시 렌더링하여 화면 전환이 일어나면 화면이 순간적으로 깜박이는 현상이 발생한다.
* 클라어인트와 서버와의 통신이 동기 방식으로 동작하기 떄문에 서버로부터 응답이 있을 떄까지 다음 처리는 블로킹된다.

<br>

📌 Ajax 통신의 장점(비동기 데이터 전송)
* 변경할 부분을 갱신하는 데 필요한 데이터만 서버로부터 전송받기 떄문에 불필요한 데이터 통신이 발생하지 않는다.
* 변경할 필요가 없는 부분은 다시 렌더링하지 않는다.(화면이 깜박이는 현상이 발생하지 않는다.)
* 클아이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹이 발생하지 않는다.

<br>

# **43.2 JSON**
클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷이다. 자바스크립트에 종속되지 않는 언어 독립형 데이터 포맷이며 대부분의 프로그래밍 언어에서 사용할 수 있다.

<br>

## **43.2.1 JSON 표기 방식**
JSON의 키는 반드시 큰따옴표(작은따옴표 사용 불가)로 묶어서 사용한다. 객체 리터럴과 같은 표기법을 그대로 사용할 수있다. 문자열은 반드시 큰따옴표로 묶어야 한다.

```json
{
  "name":"Lee",
  "age": 20 ,
  "alive": true,
  "hobby": ["traveling","tennis"]
}
```

<br>

## **43.2.2 JSON.stringify**
객체를 JSON 포맷의 문자열로 변환한다. 클라이언트가 서버로 객체를 전송하려면 객체를 문자열화 해야하는데 이를 **직렬화**라 한다.

```js
const obj = {
  name:'Lee',
  age: 20 ,
  alive: true,
  hobby: ["traveling","tennis"]
}

//객체를 JSON 포맷의 문자열로 변환한다.
const json = JSON.stringify(obj);
//string {"name":"Lee", "age":20, "alive":true, "hobby":["traveling","tennis"]}

//객체를 JSON 포맷의 문자열로 변환하면서 들여쓰기 한다.
const prettyJosn = JSON.stringify(obj, null, 2);
/*
string{
    "name":"Lee",
    "age":20,
    "alive":true,
    "hobby":[
        "traveling",
        "tennis"
    ]
}
*/

//replacer 함수, 값의 타입이 Number이면 필터링되어 변환되지 않는다.
function filter(key, value){
    //undefined: 변환하지 않음
    return typeof value === 'number'? undefined : value;
}
const strFilteredObject = JSON.stringify(obj, filter, 2);
/*
string{
    "name":"Lee",
    "alive":true,
    "hobby":[
        "traveling",
        "tennis"
    ]
}
*/
```
> 배열도 JSON 포맷의 문자열로 변환된다.
```js
const todos = [
    {id:1, content:'HTML', completed: false},
    {id:2, content:'CSS', completed: true}
]

const json = JSON.stringify(todos, null, 2);
/*
 string[
    {
        "id":1,
        "content":"HTML",
        "completed":false
    },
    {
        "id":2,
        "content":"CSS",
        "completed":true
    }
 ]
*/
```

<br>

## **43.2.3 JSON.parse**
JSON포맷의 문자열을 객체로 변환한다. 서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이다. 이 문자열을 객체로서 사용하려면 JSON 포맷의 문자열을 객체화해야 하는데 이를 역직렬화라 한다.

마찬가지 JSON 포맷의 문자열이 배열인경우 배열객체로 변환한다.

<br>

# **43.3 XMLHttpRequest**
자바스크립트를 사용해서 HTTP 요청을 전송하려면 XMLHttpRequest 객체를 사용한다.

<br>

## **43.3.1 XMLHttpRequest 객체생성**
```js
//생성자 함수를 호출하여 생성한다.(브라우저 환경에서만 정상적으로 실행)
const xhr = new XMLHttpRequest();
```

<br>

## **43.3.2 XMLHttpRequest 객체의 프로퍼티와 메서드**

```XMLHttpRequest 객체의 프로토타입 프로퍼티``` <br>

|    프로토타입 프로퍼티    | 설명|
|:----------------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------|
|  **readyState**  | HTTP 요청의 현재 상태를 나타내는 정수, 다음과 같은 XMLHttpRequest의 정적 프로퍼티를 값으로 갖는다.<br/> * UNSENT:0<br/>* OPENED:1<br/>* HEADERS_RECEIVED:2 <br/>* LOADING:3<br/>* DONE:4 |
|    **status**    | HTTP 요청에 대한 응답 상태(HTTP상태코드)를 나타내는 정수<br/> ex) 200                                                                                                       |
| **statuesText**  | HTTP요청에 대한 응답 메세지를 나타내는 문자열<br/>ex) "OK"                                                                                                                |
| **responseType** | HTTP 응답 타입<br/>ex) docment, json, txt, blob, arraybuffer                                                                                                |
|   **response**   | HTTP요청에 대한 응답 몸체 responseType에 따라 타입이 다르다.                                                                                                              |
| responseText | 서버가 전송한 HTTP 요청에 대한 응답 문자열|

<br>

```XMLHttpRequest 객체의 이벤트 핸들러 프로퍼티``` <br>

|      이벤트 핸드러 프로퍼티      | 설명                                       |
|:----------------------:|:-----------------------------------------|
| **onreadystatechange** | readystate 프로퍼티 값이 변경된 경우                |
|      onloadstart       | HTTP요청에 대한 응답을 받기 시작한 경우                 |
|       onprogress       | HTTP 요청에 대한 응답을 받는 도중 주기적으로 발생           |
|        onabort         | abort 메서드에 의해 HTTP 요청이 중단된 경우            |
|      **onerror**       | HTTP 요청에 에러가 발생한 경우                      |
|       **onload**       | HTTP 요청이 성공적으로 완료한 경우                    |
|       ontimeout        | HTTP 요청 시간이 초과한 경우                       |
|       ontimeend        | HTTP 요청이 완료한 경우, HTTP 요청이 성공 또는 실패 하면 발생 |

<br>

```XMLHttpRequest 객체의 메서드``` <br>

|         메서드          | 설명                                       |
|:--------------------:|:-----------------------------------------|
|       **open**       | HTTP 요청 초기화                              |
|       **send**       | HTTP 요청 전송                               |
|      **abort**       | 이미 전송된 HTTP 요청 중단                        |
| **setRequestHeader** | 특정 HTTP 요청 헤더의 값을 설정                     |
|  getResponseHeader   | 특정 HTTP 요청 헤더의 값을 문자열로 반환                |

<br>

```XMLHttpRequest 객체의 정적 프로퍼티``` <br>

|     정적 프로퍼티      |  값  | 설명                     |
|:----------------:|:---:|:-----------------------|
|      UNSENT      |  0  | open 메서드 호출 이전         |
|      OPENED      |  1  | open 메서드 호출 이후         |
| HEADERS_RECEIVED |  2  | send 메서드 호출 이후         |
|     LOADING      |  3  | 서버 응답 중(응답 데이터 미완성 상태) |
|     **DONE**     |  4  | 서버 응답 완료               |

<br>

## **43.3.3 HTTP 요청 전송**
1. XMLHTTPRequest.prototype.open 메서드로 HTTP요청을 초기화한다.
2. 필요에 따라 XMLRequest.prototype.setRequestHeader 메서드로 특정 HTTP 요청의 헤더 값을 설정한다.
3. XMLHttpRequest.prototype.send 메서드로 HTTP 요청을 전송한다.

```js```