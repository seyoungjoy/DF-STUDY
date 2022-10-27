<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter46.%20%EC%A0%9C%EB%84%88%EB%A0%88%EC%9D%B4%ED%84%B0%EC%99%80%20ascync/await&fontSize=40" width="100%" alt="">


# **46.1 제너레이터란?**
코드블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수이다.

1. 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.
2. 함수 호출자와 함수의 상태를 주고받을 수 있다.
3. 함수를 호출하면 제너레이터 객체를 반환한다.

<br>

# **46.2 제너레이터 함수의 정의**
function* 키워드로 선언하고, 하나 이상의 yield 표현식을 포함한다. (function 키워드 바로 뒤에 *을 붙이는 것을 권장한다.)

또한 화살표 함수로 정의할 수 없고, 생성자 함수로 호출할 수 없다.

```js
function* gil(){
    console.log('hi!');
    yield 1;
}
```

<br>

# **46.3 제너레이터 객체**
일반 함수처럼 함수 코드블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.

* next 메서드를 호출하면 yield 표현식까지 코드 블록을 실행하고 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
* return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
* throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키로 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 이터레이터 리절트 객체를 반환한다.

```js
function* gil(){
    try {
        yield 1;
        yield 2;
        yield 3;
    } catch(e){
        console.error(e);
    }
}

const generator = gil();

console.log(generator.next()); //{value:1, done:false}
console.log(generator.return('hi!')); //{value:"hi!", done:true}
console.log(generator.throw('hi!')); //{value:undefined, done:true}
```

<br>

# **46.4 제너레이터의 일시 중지와 재개**
제너레이터 함수를 호출하면 코드를 일괄 실행을 하지 않고, yield 표현식까지만 실행한다.
yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.

```js
function* getFunc(){
    yield 1;
    yield 2;;
}

const generator = getFunc();

console.log(generator.next()); //{value:1, done:false}
console.log(generator.next()); //{value:2, done:false}
console.log(generator.next()); //{value:undefined, done:true}
```

제너레이터 객체 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지 한다. 함수의 제어권이 호출자로 양도되고,

제너레이터 객체의 next 메서드는 value,done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.

value에는 yield 표현식에서 뒤의 값이 할당되고 done 프로퍼티에는 함수가 끝까지 실행되었는지 불리언 값이 할당된다.

```js
function* genFunc(){
    const x = yield 1;
    const y = yield (x + 10);
    
    return x + y;
}

```

> 제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.

<br>

# **46.5 제너레이터의 활용**

<br>

## **46.5.1 이터러블의 구현**

제너레이터 함수를 이용한 무한 이터러블 생성

```js
const infiniteFibonacci = (function* (){
    let [pre, cur] = [0,1];
    
    while (true){
        [pre,cur] = [cur, pre + cur];
        yield cur;
    }
}());

for (const num of infiniteFibonacci){
    if(num > 10000) break;
    console.log(num);
    //1 2 3 5 8 ... 6765
}
```

<br>

## **46.5.2 비동기처리**

next 메서드와 yield 표현식이 상태를 주고받을 수 있는 특성을 활용하여 프로미스를 사용한 비동기처리를 동기 처럼 구현(후속 처리 메서드 없이 비동기 처리 결과 반환)<br> 📌 비권장

<br>

# **46.6 async/await**
프로미스의 후속 처리 메서드 없이 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현 가능하다.

```js
const fetch = require('node-fetch');
async function fetchTodo(){
    const url = 'https://jsonplaceholder.../todos/1';
    
    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
    //{userId:1, id:1, title:'...', completed: false}
}
```

<br>

## **46.6.1 async 함수**
async 키워드를 사용하며 언제나 프로미스를 반환한다. 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환한다. <br>
**class의 cunstructo메서드는 async 메서드가 될 수 없다.**

```js
async function foo(n){return n;}
foo(1).then(v => console.log(v));//1
```

<br>

## **46.6.2 await 키워드**
프로미스가 settled(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 프로미스가 resolve한 처리결과를 반환한다. <br>
**프로미스 앞에 사용해야하며, async 함수 안에서 사용 가능하다.**

```js
const fetch = require('node-fetch');

const getGithubUserName = async id => {
    const res = await fetch(`https://api.github.com/users/${id}`); //1
    const {name} = await res.json(); //2
    console.log(name); // gil
}

getGithubUserName('gil');
```

프로미스가 settled 상태가 될때까지 1번은 대기한다.


모든 프로미스에 await 키워드는 개별적 수행되는 비동기 처리이므로 아래와 같이 한꺼번에 처리하도록 한다. (처리결과를 순차적으로 처리 하고 싶다면 각각의 프로미스에 await 키워드를 쓰자!)

```js
async function foo(){
    const res = await Promise.all([
        new Promise(resolve => setTimeout(()=>resolve(1), 3000)),
        new Promise(resolve => setTimeout(()=>resolve(2), 2000)),
        new Promise(resolve => setTimeout(()=>resolve(3), 1000)),
    ]);
    
    console.log(res); // [1,2,3]
}

foo(); //3초 소요
```

<br>

## **46.6.3 에러처리**
async/await은 프로미스를 반환하는 비동기 함수이므로 명시적 호출이 가능하기 때문에 호출자가 명확하다. <br> **또한 async 함수 내에서 catch문을 사용하여 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject 하는 프로미스를 반환한다.**

```js
const fetch = require('node-fetch');

const foo = async () => {
    try{
        const wrongUrl = 'https://..';
        
        const response = await fetch(wrongUrl);
        const data = await response.josn();
        console.log(data);
    } catch (err){
        console.error(err);
    }
};

const foo2 = async () => {
    const wrongUrl = 'https://..';

    const response = await fetch(wrongUrl);
    const data = await response.josn();
    return data;
};

foo(); // TypeError: Failed to fetch
foo2().then(console.log).catch(console.error);// TypeError: Failed to fetch
```