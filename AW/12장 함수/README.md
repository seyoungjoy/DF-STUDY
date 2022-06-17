# 12장 함수

## 12.1 함수란?
***
- **함수 호출**: Input 을 받아서 코드 블록에 담긴 문들이 실행되고 값을 반환하는 프로세스

## 12.2 함수를 사용하는 이유
***
- 함수는 ***객체 타입의 값*** 이다!!
- 일반 객체와는 다르게 호출 할 수 있으며, 고유한 프로퍼티를 가짐
> 함수 객체는 함수로서 동작하기 위해 함수 객체만을 위한 `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯과 `[[Call]]`, `[[Construct]]` 같은 내부 메서드를 추가로 갖고 있다.
>
> **=>** 여기서 자바스크립트에서 typeof를 할 때, 해당 객체 안에 `[[Call]]`이란 내부 메서드가 있으면 function으로 반환되는 것
```js
typeof [] // output: 'object'
typeof {} // output: 'object'
typeof new Date() // output: 'object'
typeof function() {} // output: 'function'
const arrFunc = () => {}
typeof arrFunc // output: 'function'
```
## 12.4 함수 정의
***
- 함수는 정의 / 변수는 선언
> 함수 선언문
> ```js
> function add(x,y) {
>   return x + y;
> };
> ``` 
> 함수 표현식
> ```js
> var add = function add(x,y) {
>   return x + y;
> };
> ``` 
> Function 생성자 함수
> ```js
> var add = new Function('x','y','return x + y');
> ```
> 화살표 함수
> ```js
> var add = (x,y) => x + y;
> ```
### 12.4.1 함수 선언문
- 앞에 var 안붙이는거
- 함수 이름 생략 불가
- 식별자 없는 것 처럼 보이지만 사실 js엔진이 몰래 함수명과 똑같은걸로 붙이고 있는 중 ㅋㅋ
```js
function (x,y) {
    return x + y;
    // SyntaxError
}
```
### 12.4.2 함수 표현식
- 앞에 var 붙이는거
- 함수는 함수 이름으로 호출하는 것이 아니라 함수 객체를 가리키는 식별자로 호출한다
- 호이스팅 이슈땜에 함수 선언문 말고 함수 표현식이 권장됨
```js
var add = function add(x,y) {
    return x + y;
};
console.log(add(2,5));
// var add = add(2,5) 의 add 가 식별자로 호출되는 것이다
```
### 12.4.5 화살표 함수
- 일반 function과 다르게 this, prototype, arguments 등이 다른데 이건 나중에 알아보자
## 12.5 함수 호출
***
- parameter : 는 argument 가 들어갈 포탈 경로
- arguments 가 초과되어도 argument 객체로 보관됨 (`console.log(arguments)`로 찍을수있음)
- parameter 는 가급적 3개까지만

## 12.7 다양한 함수의 형태
***
### 12.7.1 즉시 실행 함수
- 그룹 연산자 `(...)`로 감싸면 바로 호출됨
```js
(function (){
    var a = 3;
    var b = 5;
    return a * b;
}());
```
### 12.7.2 재귀 함수
- 자기 몸안에서도 똑같은 함수 실행하는것
### 12.7.3 중첩 함수
- 내부에 있는 함수가 외부 변수를 참조 하는것
### 12.7.4 콜백 함수
- parameter를 통해서 다른 함수의 내부로 전달되는 함수
```js
  var logOdds = function repeat(n, f) {
    for (var i = 0 ; i < n; i++ ){
		f(i);
    } 
};
```
### 12.7.4 순수 함수와 비순수 함수
- 순수 : 함수 돌려도 집어 넣은 원본 변수가 안바뀜
- 비순수 : 돌리면 원본 변수를 바꿀수있음
***
# 끝