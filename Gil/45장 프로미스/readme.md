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

이처럼 xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러는 언제나 console.log가 종료한 이후에 호출된다. 따라서 아직 값이 할당 되기 전이다.
(서버응답의 처리 순서가 보장 되지 않는다.)

<br>

## 📌 이유

xhr.onload 이벤트 핸들러는 load 이벤트가 발생하면 태스크 큐에 저장이 되어 대기 하다가 콜스택이 비면 이벤트 루프에 의해 콜스택으로 푸시되어 실행된다. <br>

1. 이벤트 핸들러 평가(함수이기 때문에)
2. 이벤트 핸들러의 실행 컨텍스트 생성
3. 콜 스택에 푸시
4. 이벤트 핸들러 실행

<br>

> 비동기 함수의 처리결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다. <br><br> 이때 비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다. <br><br> 필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 실패하면 호출될 콜백 함수를 전달할 수 있다.

```js
//GET 요청을 위한 비동기 함수
const get = (url, successCallback, failureCallback) => {
    const xhr = new xhr.XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () =>{
        if(xhr.status === 200){
            //서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리를 한다.
            successCallback(JSON.parse(xhr.response));
        } else {
            //에러 정보를 콜백 함수에 인수로 전달하면서 호출하여 에러 처리를 한다.
            failureCallback(xhr.status);
        }
    };
};

//id가 1인 post 취득
//서버의 응답에 대한 후속 처리를 위한 콜백 함수를 비동기 함수인 get에 전달한다.
get('https://jsonplace.../1', console.log, console.error);
/*
{
    "userid":1,
    "id":1,
    "title":"sunt aut ...",
    "body":"quia et...."
}
*/
```

이처럼 콜백함수를 통해 비동기 처리 결과에 대한 후속처리를 수행하는 비동기 함수가 비동기 처리 결과를 가지고 또 다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩 되어 복잡도가 높아지는 현상이 발생하는데, 이를 **콜백 헬**이라 한다.

<br>

```js
const get = (url, callback) =>{
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () => {
        if(xhr.status === 200){
            //서버의 응답을 콜백 함수에 전달되면서 호출하여 응답에 대한 후속 처리를 한다.
            callback(JSON.parse(xhr.response));
        } else{
            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    };
};

const url = "https://json....com";

//id가 1인 post의 userId 취득
get(`${url}/post/1`, ({userId}) =>{
    console.log(uesrId);//1

    //post의 userId를 사용하여 user 정보 취득
    get(`${url}/users/${userId}`, userInfo => {
        console.log(userInfo);//{id:1, name: "Leanne Graham", username:"Bret",....}
    });
});
```

위 예제는 GET요청을 통해 서버로부터 응답을 취득하고, 이 데이터로 다시 GET을 요청한다. 콜백헬은 가독성을 나쁘게 하며 실수를 유발하는 원인이 된다.

```js
//콜백 헬이 발생하는 전형적인 사례
get('/step1', a =>{
    get(`/step2/${a}`,b =>{
        get(`/step3/${b}`, c=>{
            get(`/step4/${c}`, d=>{
                console.log(d);
            });
        });
    });
});
```

<br>

## **45.1.2 에러 처리의 한계**
```js
try{
    setTimeout(()=>{throw new Error('Error!');}, 1000);
} catch(e){
    //에러를 캐치하지 못한다
    console.error('캐치한 에러', e);
}
```

1. setTimeout(비동기함수)이 호출, setTimeout 함수의 실행 컨텍스트 생성 및 콜스택 푸시 되어 실행
2. 콜백 함수 호출을 기다리지 않고 즉시 종료 및 콜스택에서 제거
3. 타이머 만료 후 setTimeout의 콜백 함수가 태스크 큐로 푸시
4. 콜스택이 비어진 후 이벤트 루프에 의해 콜 스택으로 푸시되어 실행

<br>

### **에러는 호출자 방향으로 전파된다.**

콜스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.


>try..catch...finally 문은 에러 처리를 구현하는 방법이다.<br><br>try 코드블록이 실행되고, try코드블록에 포함된 문 중에서 에러가 발생하면 catch문의 err변수에 전달되고 catch 코드블록이 실행된다. finally 코드블록은 에러 발생과 상관없이 반드시 한번 실행된다.

<br>

# **45.2 프로미스의 생성**
Promise 생성자 함수를 new 연산자와 함꼐 호출 하면 프로미스(Oromise 객체)를 생성한다. (호스트 객체가 아닌 ECMAScript 사양에 정의된 표준 빌트인 객체다.)

Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수(executor함수)를 **resolve와 reject 함수**로 인수를 통해 전달 받는다.

```js
//프로미스 생성
const promise = new Promise((resolve, reject) => {
    //Promise 함수의 내부에서 비동기 처리를 수행한다.
    if(/*비동기 처리 성공*/){
        resolve('result');
    }else{
        reject('failure reason');
    }
});
```

<br>

### **📌 get을 프로미스로 사용하여 재구현**
```js
const promiseGet = url =>{
    return new Promise((resolve, reject) =>{
        const xhr = new XMLHttpRequest();
        xhr.open('GET',url);
        xhr.send();

        xhr.onload = () =>{
            if(xhr.status === 200){
                //성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
                resolve(JSON.parse(xhr.response));
            } else {
                //에러 처리를 위해 reject 함수를 호출한다.
                reject(new Error(xhr.status));
            }
        };
    });
};

promiseGet('https://jsonplace..../posts/1');
```

|프로미스의 상태정보| 의미| 상태변경 조건|
|:-------:|:-------:|:-------:|
|pending|비동기 처리가 아직 수행되지 않은 상태|프로미스가 생성된 직후 기본 상태|
|fulfilled|비동기 처리가 수행된 상태(성공)|resolve 함수 호출|
|rejected|비동기 처리가 수행된 상태(실패)|reject 함수 호출|

생성된 직후 프로미스는 pending(수행되지않은)상태다. 이후 과정은 다음과 같이 변경된다.**(resolve, reject함수를 호출 하는것으로 결정)**
* 비동기 처리 성공 : resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경
* 비동기 처리 실패 : reject 함수를 호출해 프로미스를 rejected 상태로 변경

fulfilled(성공), reject(실패)상태를 settled 상태라고 한다. (비동기 처리가 수행된 상태)
settled 상태가 되면 더는 다른 상태로 변화할 수 없다.

```js
//프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체이다.
const resolve = new Promise(resolve => resolve(1));//성공
const reject = new Promise((_, reject) => reject(new Error('error occured')));//실패
```

<br>

# **프로미스의 후속 처리 메서드**
프로미스의 처리결과를 위해 후속메서드 then, catch, finally를 제공한다.
**프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백함수가 선택적으로 호출된다.**

<br>

## **45.3.1 Promise.prototype.then(두개의 인수를 받는다)**
* 첫 번째 콜백 함수는 fulfilled 상태(성공)가 되면 호출된다.(프로미스의 비동기 처리 결과를 인수로 전달받는다.)
* 두 번째 콜백 함수는 프로미스가 reject 상태(실패)가 되면 호출된다.(프로미스의 비동기 처리 결과를 인수로 전달받는다.)

```js
new Promise(resolve => resolve('fulfilled')).then(v => console.log(v), e=> console.error(e)); //fulfilled(성공)
//실패를 두번째 인수로 받아야 하기때문에 _(언더스코어)를 첫번째 인수로 두는것같다.
new Promise((_,reject) => reject(new Error('rejected'))).then(v => console.log(v), e=> console.error(e)); //Error: rejected(실패)
```
> 콜백함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 resolve 또는 reject하여 프로미스를 생성해 반환한다.
 
<br>

## **45.3.2 Promise.prototype.catch(한개의 인수를 받는다)**
rejected(실패)상태인 경우만 호출된다.

```js
//Error: rejected (두개 메서드가 동일하게 동작한다.)
new Promise((_,reject) => reject(new Error('rejected'))).catch(e => console.log(e));
new Promise((_,reject) => reject(new Error('rejected'))).then(undefined, e => console.log(e));
```

<br>

## **45.3.3 Promise.prototype.finally(한개의 인수를 전달 받는다.)**
성공,실패와 상관없이 무조건 한 번 호출된다.(상태와 상관없이 공통적으로 수행해야 할때 사용한다.)

```js
//get 비동기 함수를 사용해 후속처리 구현
const promiseGet = url =>{
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();
    
    xhr.onload = () =>{
        if(xhr.status === 200){
            resolve(JSON.parse(xhr.response));
        }else{
            reject(new Error(xhr.status));
        }
    }
}

promiseGet('https://json.../posts/1')
    .then(res => console.log(res))
    .catch(err => console.error(err))
    .finally(()=> console.log('finally!'));
```

<br>

# **45.4 프로미스의 에러 처리**
프로미스의 비동기 처리에서 발생한 에러는 then메서드의 두번째 콜백함수와 catch로 처리할 수 있다.
```js
const wrongUrl = 'https://json.../XXX/1';
const promiseGet = url =>{
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () =>{
        if(xhr.status === 200){
            resolve(JSON.parse(xhr.response));
        }else{
            reject(new Error(xhr.status));
        }
    }
}

//then Error:404
promiseGet(wrongUrl).then(
    res => console.log(res),
    err => console.error(err)
);

//가독성이 떨어지며 첫번째 콜백함수에서 발생한 에러를 캐치못한다.(catch 메서드 권장)
promiseGet(wrongUrl)
    .then(res => console.log(res))
    .then(undefined, err => console.error(err));

//catch Error:404
promiseGet(wrongUrl)
    .then(res => console.log(res))
    .catch(err => console.error(err));
```

<br>

# **45.5 프로미스 체이닝**
후속 처리 메서드를 이용하여 콜백 헬을 처리해보자.

```js
const url = 'https://json....com';
const promiseGet = url =>{
    const xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.send();

    xhr.onload = () =>{
        if(xhr.status === 200){
            resolve(JSON.parse(xhr.response));
        }else{
            reject(new Error(xhr.status));
        }
    }
}

//id가 1인 post의 userId를 취득
promiseGet(`${url}/posts/1`)
//취득한 post의 useId로 user 정보를 취득
.then(({ useId }) => promiseGet(`${url}/user/${`userId`}`))
.then(userInfo => console.log(userIndo))
.catch(err => console.error(err));
```

> then, catch, finally 후속처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다. 이를 프로미스 체이닝이라 한다.
 
<br>

# **45.6 프로미스의 정적 메서드**
Promise는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있다.

<br>

## **45.6.1 Promise.resolve / Promise.reject**
이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용한다.

resolve 메서드는 인수로 전달받은 값을 resolve 하는 프로미스를 생성한다.

<br>

### **resolve**
```js
//배열을 resolve 하는 프로미스를 생성
const resolvedPromise = Promise.resolve([1,2,3]);
resolvedPromise.then(console.log); //[1,2,3]

//위와 동일
const resolvedPromise2 = new Promise(resolve => resolve([1,2,3]));
resolvedPromise2.then(console.log); //[1,2,3]
```

### **reject**
```js
//에러 객체를 reject하는 프로미스를 생성
const rejectPromise = Promise.reject(new Error("error!"));
rejectPromise.catch(console.log); // Error: error!

//위와 동일
const rejectPromise2 = new Promise((_, reject) => reject(new Error('error!')));
rejectPromise2.catch(console.log); // Error: error!
```

<br>

## **45.6.2 Promise.all**
여러 개의 비동기 처리를 모두 병렬 처리할 때 사용한다.
요소로 갖는 배열 등의 이터러블을 인수로 전달 받는다. 전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환한다.

```js
const requestData1 = () => new Promise(resolve => setTimeout(() => resolve(1),3000));
const requestData2 = () => new Promise(resolve => setTimeout(() => resolve(2),2000));
const requestData3 = () => new Promise(resolve => setTimeout(() => resolve(3),1000));

Promise.all([requestData1(),requestData2(),requestData3()]).then(console.log).catch(console.error);
```

모든 프로미스가 fulfilled 상태가 되면 resolve된 처리 결과(위 예제의 경우 1,2,3)를 모두 배열에 저장해 새로운 프로미스를 반환한다.
> 첫번째 프로미스가 가장 나중에 fulfilled 상태가 되어도 차례대로 배열에 저장해 처리 순서가 보장된다.

<br>

## **45.6.3 Promise.race**
프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달받는다.
all과 다르게 먼저 fulfilled 상태가 된 프로미스의 처리결과를 resolve하는 새로운 프로미스를 반환한다.

>메서드에 전달된 프로미스가 하나라도 rejected 상태가 되면 에러를 reject하는 새로운 프로미스를 즉시 반환한다.
 
<br>

## **45.6.4 Promise.allSettled**
전달받은 프로미스가 모두 settled 상태(비동기 처리가 수행된 상태, fulfilled rejected 상태)가 되면 처리 결과를 배열로 반환한다.