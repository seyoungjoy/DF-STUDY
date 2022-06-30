<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter18.%20%ED%95%A8%EC%88%98%EC%99%80%20%EC%9D%BC%EA%B8%89%20%EA%B0%9D%EC%B2%B4&fontSize=45">

# **18.1 일급 객체**

일급객체의 조건

* 무명의 리터럴로 생성할 수 있다.(런타임에 생성가능)
* 변수나 자료구조(객체, 배열)에 저장 가능하다.
* 함수의 매개변수에 전달 가능하다.
* 함수의 반환값으로 사용 가능하다.

> 함수는 4가지에 모두 해당한다.

<br>

# **18.2 함수 객체의 프로퍼티**

<br>

## **18.2.1 arguments 프로퍼티**

arguments 프로퍼티는 함수 호출 시 전달된 인수의 저오를 담고 있는 순회 가능한 유사 배열객체의 정보를 가지고 있다.

```js
function gil(x,y){
    console.log(arguments);
    return x + y;
}
console.log(gil('gi','hi')); // 0: 'gi', 1: 'hi'
```

* **collee:** 함수 호출됐을때 호출된 함수 자신의 정보를 가지고 있다.
* **length:** 인수의 개수를 나타냄

<br>

* **arguments 객체의 Symbol(Symbol.itertator) 프로퍼티**

>arguments 객체를 순화 가능한 itertator(이터러블)로 만들기위한 프로퍼티이다.

<br>

arguments 객체는 매개변수 개수를 정해 놓지 않고 가변인자함수를 만들때 유용하다.

```js
function gil(){
    let res = 0;

    for(let i=0; i < arguments.length; i++){
        res += arguments[i];
    }

    return res;
}

console.log(gil(1,2,3));// 6

```

📌 arguments는 유사배열객체인데 length프로퍼티를 가진객체로 for문으로 순회할 수 있는 객체를 말한다.

<br>

## ~~**18.2.2 caller 프로퍼티**~~

<br>

## **18.2.3 length 프로퍼티**
함수를 정의할때 매개변수의 개수 정보를 가지고 있다.

```js
function gil(x,y,z){
    console.log(arguments.length); // 인자개수 0
}

console.log(gil.length); // 매개변수 개수 3

gil();
```

<br>

## **18.2.4 name 프로퍼티**
name 프로퍼티는 기명 함수 객체에서는 함수 이름을 가리키고, 익명 함수에서는 함수객체를 가리키는 변수 이름을 가리킨다.

```js
const gil1 = function hi(){}
const young = function (){}

console.log(gil1.name); // hi
console.log(young.name); // young
```

<br>

## **18.2.5 _ _ proto _ _ 접근자 프로퍼티**
객체의 [[Prototype]] 내부슬롯을 간접적으로 접근 할수있는 프로퍼티이다.

<br>

## **18.2.6 prototype 프로퍼티**
생성자 함수로 호출할 수 있는 함수객체(constructor)만 소유한 프로퍼티 이다. 즉, 일반객체와 생성자 함수로 호출 불가능한 non-constructor는 가지고 있지 않다.

📌 생성자함수로 호출될때 인스턴스 프로토타입 객체를 가리킨다.