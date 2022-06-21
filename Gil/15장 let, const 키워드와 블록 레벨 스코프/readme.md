<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter15.%20let,%20const%20%ED%82%A4%EC%9B%8C%EB%93%9C%EC%99%80%20%EB%B8%94%EB%A1%9D%EB%A0%88%EB%B2%A8%20%EC%8A%A4%EC%BD%94%ED%94%84&fontSize=35">

# **15.1 var 키워드로 선언한 변수의 문제점**

<br>

## **15.1.1 변수 중복 선언 허용**
var 키워드는 같은 스코프에서 중복선언이 가능하다.

```js
var Gil = 10;
var Young = 30;

var Gil = 100; // Gil = 100; 과 동일 재할당
var Young; // 무시

console.log(Gil); // 100
console.log(Young); // 30
```
>변수선언과 할당값이 같이 있는 초기화문이 있으면 중복 선언됐을때 var 가 없이 재할당 한것 처럼 동작하고, 변수선언만 중복하게되면 무시된다.

<br>

## **15.1.2 함수 레벨 스코프**
var 키워드는 함수 외에는 전역 스코프로 생성된다. 따라서 전역변수의 생성이 많아져 중복 선언이 되어 값이 변경 될 수 있다.

```js
var i = 1;

if(true){
    var i = 10;
}
console.log(i); //10

for (var i =0; i < 3; i++){
    console.log(i); //0 1 2
}

console.log(i); // 3
```

<br>

## **15.1.3 변수 호이스팅**
var 키워드는 에러가 뜨진 않지만 변수 호이스팅 때문에 맞지 않은 흐름이 생성되고, 가독성을 떨어뜨려 계산함에 있어 어려움이 생긴다.

<br>

# **15.2 let 키워드**

## **15.2.1 변수 중복 선언 금지**
let 키워드에 변수를 중복 선언시 에러가 발생한다.

<br>

## **15.2.2 블록 레벨 스코프**
if문, while문 등 블록레벨스코프를 가진다. 이때 함수 레벨 스코프 안에 블록레벨 스코프는 중첩이 된다.

```js
function Gil(){
    let i = 1;
    for(let i =0; i < 3; i++){
        console.log(i); // 0, 1, 2
    }
    console.log(i); //1 (var 였다면 3이 콘솔에 찍힌다.)
}
```

<br>

## **15.2.3 변수 호이스팅**
let 키워드는 선언단계와 초기화 단계가 나눠져 진행되어서 변수 호이스팅이 발생하지 않는것 처럼 동작한다.

```js
console.log(Gil); //Reference Error
let Gil;
console.log(Gil); // undefined
```

<br>

## **15.2.4 전역 객체와 let**
var 키워드로 선언한 변수는 window 객체의 프로퍼티이지만 let키워드로 선언한 변수는 보이지 않는 개념적인 블록 안에 있어서 window 객체가 아니다.

<br>

# **15.3 const 키워드**

<br>

## **15.3.1 선언과 초기화**
const 키워드로 선언한 변수는 선언과 초기화를 동시에 해야한다. 선언만 했을때 에러가 발생한다.

```js
const Gil; //SyntaxError
const Gil = 1;
```

<br>

## **15.3.2 재할당 금지**
const 키워드로 선언한 변수는 재할당 시 타입에러가 발생한다.

<br>

## **15.3.3 상수**
const 키워드로 원시값을 할당할 경우 값의 재할당을 할 수 없으므로 상수를 만들 수 있다.

<br>

## **15.4** **var** vs. **let** vs. **const**
* ES6이상 부터는 var키워드를 사용하지 않는다.
* 재할당이 필요한 경우 let을 사용하고 스코프는 최대한 좁게 만든다.
* 변경이 필요 없는 원시값과 객체에는 const를 사용한다.