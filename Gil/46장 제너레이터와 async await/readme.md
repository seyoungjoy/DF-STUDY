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

# **46.6 async/await**