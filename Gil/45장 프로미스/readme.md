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

```js
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET','/users');

//HTTP 요청 헤더 설정
//클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json

//HTTP 요청 전송
xhr.send();
```

<br>

```XMLHttpRequest.prototype.open```
open 메서드는 서버에 전송할 HTTP 요청을 초기화한다.

```js
//대괄호 부분은 있어도 되고, 없어도 되는 선택사항을 의미
xhr.open(method, url[, async]);
xhr.open(method, url, async);
xhr.open(method, url);
```

|  매개변수  | 설명                                          |
|:------:|:--------------------------------------------|
| method | HTTP 요청 메서드("GET","POST","PUT","DELETE" 등)  |
|  url   | HTTP 요청을 전송할 URL                            |
| async  | 비동기 요청 여부, 옵션으로 기본값은 true이며, 비동기 방식으로 동작한다. |

HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법이다.

| HTTP 요청 메서드 |       종류       |           목적            | 페이로드<br>(전송되는 데이터) |
|:-----------:|:--------------:|:-----------------------:|:------------------:|
|     GET     | index/retrieve |      모든/특정 리소스 취득       |         X          |
|    POST     |     create     |         리소스 생성          |         O          |
|     PUT     |    replace     |       리소스의 전체 교체        |         O          |
|    PATCH    |     modify     |       리소스의 일부 수정        |         O          |
|   DELETE    |     delete     |      모든/특정 리소스 삭제       |         X          |

<br>

📌 페이로드(payload) <br>
실제 데이터에서의 payload는 아래의 json에서 “data”입니다. 그 이외의 데이터들은 전부 통신을 하는데 있어 용이하게 해주는 부차적인 정보들입니다.
```json
{
  "status":"",
  "from": "localhost",
  "to": "http://melon ...",
  "method": "GET",
  "data": {"message": "There is a cuty dog!"}
}
```

<br>

```XMLHttpRequest.prototype.send```
send 메서드는 open 메서드로 초기화된 HTTP 요청을 서버에 전송한다.
* GET 요청 메서드의 경우 데이터를 URL의 일부분인 쿼리 문자열로 서버에 전송된다.
* POST 요청 메서드의 경우 데이터(페이로드)를 요청 몸체에 담아 전송한다.

```js
xhr.send(JSON.stringify({id:1, content:'HTML', completed:false}));
```
**HTTP 요청 메서드가 GET인 경우 send 메서드에 페이로드로 전달한 인수는 무시되고 요청 몸체는 null로 설정된다.**

<br>

```XMLHttpRequest.prototype.setRequestHeader```
setRequestHeader 메서드는 특정 HTTP 요청의 헤더 값을 설정한다.
> 반드시 open 메서드를 호출한 이후에 호출해야 한다.


##**content-type**
|   MIME 타입   | 서브타입                                               |
|:-----------:|:---------------------------------------------------|
|    text     | text/plain, text/html, text/css, text/javascript   |
| application | application/json, application/x-www-form-urlencode |
|  multipart  | multipart/formed-data                              |


```js
//요청 몸체에 담아 서버로 전송할 페이로드의 MIME 타입을 지정하는 예다.

//XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

xhr.open('POST','/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정:json
xhr.setRequestHeader('content-type','appliction/json');

//HTTP 요청 전송
xhr.send(JSON.stringify({id:1, content:'HTML', completed:false}));

//서버가 응답할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('accept','application/json');
```

> accept 헤더를 설정하지 않으면 send 메서드가 호출될 때 Accept 헤더가 */*으로 전송된다.

<br>

##**43.3.4 HTTP 응답처리**
서버가 전송한 응답을 처리하려면 XMLHttpRequest 객체가 발생시키는 이벤트를 캐치 해야하는데 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티 값이 변경된 경우 발생하는 readystatechange 이벤트를 캐치하여 HTTP 응답을 처리할 수 있다.(XMLHttpRequest 객체는 브라우저에서 제공하는 WebAPI이므로 브라우저 환경에서 실행햐야 한다.)

```js
const xhr = new XMLHttpRequest();

//HTTp 요청 초기화
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

//HTTP 요청 전송
xhr.send();

//readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는
//readyState 프로퍼티가 변경될때 마다 발생한다.
xhr.onreadystatechange = () =>{
    
    //readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
    //readyState 프로퍼티 값이 4(XMLHttpRequet.DONE)가 아니면 서버 응답이 완료되지 않은 상태이다.
    if(xhr.readyState !== XMLHttpRequet.DONE) return;
    
    //status 프로퍼티는 응답 상태 코드를 나타낸다.
    //status 프로퍼티 값이 200이면 정상적으로 응답된 상태
    //status 프로퍼티 값이 200이 아니면 에러가 발생한 상태
    //정상적으로 응답됐으면 response 프로퍼티에 서버의 응답 결과가 담겨있다.
    
    if(xhr.status === 200){
        console.log(JSON.parse(xhr.response));
        // {userId: 1, id:1, title: "delectus aut autem", completed:false}
    }else{
        console.error('Error', xhr.status, xhr.statusText);
    }
}
```

readystatechange 이벤트 대신 load 이벤트를 캐치해도 된다. load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
따라서 **xhr.readyState 가 XMLHttpRequest.DONE 인지 확인할 필요가 없다.**

```js
const xhr = new XMLHttpRequest();
xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

//HTTP 요청 전송
xhr.send();

//load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
xhr.onload = () => {
    //status 프로퍼티는 응답 상태 코드를 나타낸다.
    //status 프로퍼티 값이 200이면 정상적으로 응답된 상태
    //status 프로퍼티 값이 200이 아니면 에러가 발생한 상태
    
    if(xhr.status === 200){
        conosole.log(JSON.parse(xhr.response));
        // {userId: 1, id:1, title: "delectus aut autem", completed:false}
    }else{
        console.error('Error', xhr.status, xhr.statusText);
    }
}
```