<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter45.%20%ED%94%84%EB%A1%9C%EB%AF%B8%EC%8A%A4&fontSize=60" width="100%" alt="">

ES6에서 비동기 처리를 위한 또 다른 패턴 프로미스는 전통적인 콜백 함수 패턴이 가진 단점을 보완하여 비동기 처리 시점을 명확하게 표현할 수 있다는 장점이 있다.

# **45.1 비동기 처리를 위한 콜백 패턴의 단점**

<br>

# **45.1.1 콜백 헬**

```js
//GET 요청을 위한 비동기 함수
const get = url =>{
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () =>{
        if(xhr.status === 200){
            console.log(JSON.parse(xhr.response));
        }else{
            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    }
}

get('https://jsonplaceholder.typicode.com/posts/1');
/*
{
    "userid":1,
    "id":1,
    "title":"sunt aut facere ...",
    "body": "quia et suscipit ..."
}
*/
```

>**비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.**(처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작 하지 않는다.)

```js
let g = 0;

//비동기 함수인 setTimeout 함수는 콜백 함수의 처리 결과를 외부로 반환하거나 상위스코프의 변수에 할당 하지 못한다.
setTimeout(() => {g = 0}, 0);
console.log(g); // 0
```

**get함수는 onload 이벤트 핸들러가 비동기로 동작하기 때문에 비동기 함수다.**(onload 이벤트 핸들러는 get함수가 종료된 이후에 실행된다.)

* GET 요청을 전송
* onload 이벤트 핸들러 등록
* undefined 반환 후 즉시 종료

## get함수가 서버에 응답 결과를 반환
```js
let gil;
//GET 요청을 위한 비동기 함수
const get = url =>{
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send()

    xhr.onload = () =>{
        if(xhr.status === 200){
            //상위스코프 변수에 할당
            gil = JSON.parse(xhr.response);
            //서버의 응답을 반환
            return JSON.parse(xhr.response);
        } else{
            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    };
};

// id가 1인 post 취득
get('https://jsonplaceholder.typicode .../1');
console.log(gil); //undefined
```

위의 코드처럼 상위스코프를 할당하고 return키워드를 사용했지만 console에서 응답결과 값이 할당 되지 않았다.

이처럼 xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러는 언제나 console.log가 종료한 ㅇ후에 호출된다. 따라서 아직 값이 할당 되기 전이다.
(서버응답의 처리 순서가 보장 되지 않는다.)