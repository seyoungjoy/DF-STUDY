<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter26.%20ES6%20%ED%95%A8%EC%88%98%EC%9D%98%20%EC%B6%94%EA%B0%80%20%EA%B8%B0%EB%8A%A5&fontSize=45" width="100%" alt="">

# **26.1 함수의 구분**
ES6 이전의 함수는 일반함수 호출, 생성자 함수 호출 등 명확한 구분이 없었다.**(ES6 이전의 모든 함수는 callable, constructor 이다.)**

이렇게 불필요한 프로토타입 프로퍼티를 생성해서 ES6부터는 함수 목적에 따라 세가지로 구분된다.

| ES6 함수의 구분 | constructor | prototype | super | arguments |
|:----------:|:-----------:|:---------:|:-----:|:---------:|
|    일반함수    |      O      |     O     |   X   |     O     |
|    메서드     |      X      |     X     |   O   |     O     |
|   화살표 함수   |      X      |     X     |   X   |     X     |

<br>

# **26.2 메서드**
ES6 이전의 메서드는 명확한 정의가 없었지만, ES6부터는 축약 표현으로 정의된 함수만을 의미한다.

```js
const obj = {
    x:1,
    //메서드(축약표현) non-constructor
    foo(){return this.x;},
    
    //일반함수
    gil: function(){return this.x;}
}
```

위 코드에서 foo() 함수는 메서드이며, non-constructor여서 생성자 함수로 호출이 불가능 하고 프로토타입도 생성하지 않는다.

```js
new obj.foo();//TypeError
obj.foo.hasOwnProperty('prototype') //false
```
>표준 빌트인 객체가 제공하는 프로토타입 메서드, 정적메서드는 모두 non-constructor이다.

<br>

ES6 메서드는 자신에게 바인딩된 객체를 가리키는 내부객체 [[HomeObject]]를 가지고 있어 super 키워드를 통해서 참조할 수 있다.

```js
const base = {
    name: 'gil',
    sayHi(){
        return `Hi! ${this.name}`;
    }
};

const derived = {
    __proto__:base,
    sayHi(){
        return `${super.sayHi()}, nice meet you`
    }
};

console.log(derived.sayHi()); // Hi! gil, nice meet you
```

<br>

# **26.3 화살표 함수**
화살표 함수는 콜백 함수 내부에서 this가 전역객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

<br>

## **26.3.1 화살표 함수 정의**

<br>

### 함수정의
```js
const gil = (x,y) => x * y;
gil(1,2); // 2
```

<br>

### 매개변수 선언
```js
//매개변수가 2개 이상인 경우 '()' 사용
const gil1 = (x,y) =>{}

//매개변수가 1개인 경우 '()' 생략가능
const gil2 = x =>{}

//매개변수가 0개인 경우 '()' 생략불가능
const gil3 = () =>{}
```

<br>

### 함수 몸체 정의

* 함수 몸체 중괄호{}를 생략하고 내부문이 표현식이 아닌 문이라면 에러가 발생한다.(반환이 불가능 해서)
1. 표현식이 아닌 문이면 중괄호{} 생략 불가능
2. 객체 리터럴을 반환하는 경우 소괄호()로 감싸주어야 한다.
3. 함수몸체가 여러개의 문으로 구성되어있고 반환값이 있으면 명시적으로 반환해야한다.
4. 즉시실행 함수로 사용할 수 있다.
5. 화살표함수는 일급객체이므로 고차함수에 인수로 전달 가능하다.

```js
//1
const gil = () => const x =1;
const gil2 = () => {const x =1;}

//2
const gil3 = (x,y) => ({x,y})
gil3(1,2); // {x:1, y:2}

//3
const gil4 = (x,y) =>{
    const result = x+y;
    return result;
}

//4
const gil = (name => ({
    sayHi(){return `hi! ${name}`}
}))('gil');

//5
[1,2,3].map(function(v){
    return v*2;
});
[1,2,3].map(v => v*2); // [2,4,6]
```

<br>

## **26.3.2 화살표 함수와 일반함수의 차이**

<br>

### *1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.*
```js
//생성자함수로 호출 불가능(인스턴스 생성 불가)
const Foo = () =>{};
new Foo(); //TypeError

//프로토타입 생성X
Foo.hasOwnProperty('prototype'); //false
```

<br>

### *2. 중복된 매개변수 이름을 선언할 수 없다.*
```js
//SyntaxError
const arrow = (a,a) =>{a+a};
```

<br>

### *3. 화살표함수는 자체의 this, arguments, super, new.target 바인딩을 갖지않는다.*

따라서 this, arguments, super, new.target 참조시 스코프체인을 통해 상위 스코프를 참조한다.

<br>

## **26.3.3 this**
화살표 함수는 함수자체의 this 바인딩을 갖지 않아 화살표함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 **lexical this**라 한다.

```js
//화살표 함수는 스코프체인을 이용해 this를 참조 하므로 중첩함수라도 this가 참조된다.
(function(){
    const gil = () => {
        () => console.log(this);
    }
    gil()();
}).call({a:1})// {a:1}
```

>화살표함수는 스코프체인을 이용해 this를 참조하므로 메서드를 정의할때는 ES6 축약표현으로 하는 것이 좋다.

<br>

## **26.3.4 super**
화살표 함수는 this와 마찬가지로 super 키워드도 상위스코프의 super를 참조한다.

```js
class Gil{
    constructor(name){
        this.name = name;
    }
    sayHi(){
        return `Hi! ${this.name}`
    }
}

class Derived extends Gil{
    sayHi = () => `${super.sayHi()} nice meet you`;
}

const derived = new Derived('gil');
console.log(derived.sayHi()); //Hi! gil nice meet you
```

<br>

## **26.3.5 arguments**
화살표함수 내부에서 arguments를 참조하면 스코프체인을 통해 상위스코프의 arguments를 참조한다.
따라서 함수로 가변 인자 함수를 구현해야 할 때는 반드시 **Rest 파라미터**를 사용한다.

<br>

## **26.4 Rest 파라미터**

<br>

## **26.4.1 기본 문법**
Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.

```js
function gil(param1,param2,...rest){
    console.log(param1);//1
    console.log(param2);//2
    console.log(rest);//[3,4,5]
}
gil(1,2,3,4,5);
```

매개변수와 섞어 쓸 경우 매개변수의 값을 제외한 값을 배열로 할당받는다. 이때 **Rest파라미터는 반드시 마지막 파라미터이어야 한다.**

📌 Rest 파라미터는 단 1개만 사용할 수 있다. 그리고 매개변수 개수를 나타내는 length 프로퍼티에 영향을 주지 않는다.

<br>

## **26.4.2 Rest 파라미터와 arguments 객체**
Rest 파라미터를 사용하면 arguments를 이용하여 배열로 변환 하는 작업을 피할 수 있고, 화살표 함수는 arguments 객체를 갖지 않으므로 가변인자를 구할때는 Rest 파라미터를 사용해야한다..

<br>

# **매개변수 기본값**
ES5에서 매개변수의 기본값을 정해 주지 않으면 의도치 않은 결과가 나올수 있는데 ES6의 매개변수의 기본값을 이용하면 이를 피할수 있다. 이때 매개변수 기본값은 인수를 전달 받지 않거나 undefined가 전달 될경우 유효하다.(할당된다)

```js
function gil(x,y){
    return x+y;
}
console.log(gil(1)); //NaN (y매개변수가 undefined이 할당 됐기 때문)

function gil2(x=0,y=0){
    return x+y; // x = 1, y = 0
}
console.log(gil2(1)); //1

//Rest 파마리터에는 기본값을 지정할 수 없다.
function gil3(...rest = []){
    console.log(rest);//SyntaxError
}
```

>매개변수 기본값은 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다.
 