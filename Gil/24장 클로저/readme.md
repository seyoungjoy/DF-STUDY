<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter24.%20%ED%81%B4%EB%A1%9C%EC%A0%80&fontSize=50">

# **24.1 렉시컬 스코프**
렉시컬 환경의 상위 스코프에 대한 참조는 **함수 정의가 평가되는 시점**에 함수가 정의된 환경(위치)에 의대 결정된다.

<br>

# **24.2 함수 객체의 내부 슬롯[[Environment]]**
함수는 자신의 내부 슬롯 [[Environment]]에 상위 스코프 참조를 저장한다.

<br>
이때 [[Environment]]슬롯에 저장되는 상위스코프는 함수가 정의 될때를 기준으로 한다. 따라서 아래 예시와 같이 결과값이 나온다.

<br>


```js
const x = 1;

function gil(){
    const x = 10;

    //호출 위치와 상관없이 young함수가 정의될때 기준 상위스코프가 결정된다.
    young(); //1
}

//young함수가 정의 되어 전역 스코프를 [[Environment]]에 저장, 기억한다.
function young(){
    console.log(x);
}

gil(); //1
young(); //1
```

<br>

# **24.3 클로저와 렉시컬 환경**

```js
const x = 1;
function outer(){
    const x = 10;
    const inner = function(){console.log(x);};
    return inner;
}

const innerFunc = outer();
innerFunc(); //10
```

위의 예제에서 outer함수의 호출 되면 함수가 종료 되어 실행컨텍스트에서 변수 x가 저장 되어있던 outer함수 실행컨텍스트가 삭제되어서 더이상 변수x를 참조 할 수 없지만 이후에 innerFunc으로 호출을 해보면 outer함수 **실행컨텍스트가 살아 있는것 처럼 변수 x가 참조되어 결과값으로 반환**된다.

<br>

이처럼 외부 함수보다 내부함수가 더 오래 유지되는 경우 생명주기가 끝난 외부함수의 변수를 참조 할수있다. 이를 ```클로저```라고 한다.

<br>

이는 outer함수의 실행컨텍스트는 삭제 됐지만 **렉시컬 환경은 삭제되지 않은것을 의미**한다.

<br>

위의 예제에서 outer 내부에 있는 x변수를 자유변수라고 한다.
> 외부 함수 보다 내부함수가 생명주기가 길고, 외부 함수의 실행컨텍스트가 삭제됐지만 클로저 함수(내부함수)에 의해 참조 되고 있는 변수를 뜻한다.

<br>

# **24.4 클로저의 활용**
클로저는 상태가 의도치 않게 변경 되지 않도록 안전하게 은닉
하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.

```js
const Counter = (function(){
    let num = 0;

    function Counter(){}

    Counter.prototype.increase = function(){
        return ++num;
    };

    Counter.prototype.decrease = fucntion(){
        retrun num > 0 ? --num : 0;
    };

    return Counter;
}());

const counter = new Counter();

console.log(counter.increase()); //1
console.log(counter.increase()); //2
console.log(counter.decrease()); //1
console.log(counter.decrease()); //0
```