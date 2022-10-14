# 28. Number

## 28.1 Number 생성자 함수
Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한다.
```js
const numObj = new Number(); //빈값
console.log(numObj); // 0을 할당한 래퍼객체

const numObj = new Number(10); //숫자
console.log(numObj); // 10 할당

const numObj = new Number('10'); //문자열
console.log(numObj); // 10 할당

const numObj = new Number('Hello'); //문자열(숫자로 변환X)
console.log(numObj); // NaN
```
<br>

## 28.2 Number 프로퍼티

### Number.EPSILON
자바스크립트의 숫자는 십진 부동 소수점 숫자로 접근하는데 반해 그 내부 동작 원리는 이진 부동 소수점 숫자이기 때문에 오차가 발생한다.
```js
console.log(.1 + .2); // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3); // false

//부동소수점의 오차를 해결하기 위해 사용.
function isEqual(a, b){
    return Math.abs(a-b) < Number.EPSILON;
}
isEqual(0.1 + 0.2 , 0.3); //true
```
<br>

### Number.MAX_VALUE
- 자바스크립트에서 표현하는 가장 큰 양수 값
<br>

### Number.MIN_VALUE
- 자바스크립트에서 표현하는 가장 작은 양수 값
<br>

### Number.MAX_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현하는 가장 큰 정수값
<br>

### Number.MIN_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현하는 가장 작은 정수값
<br>

### Number.POSITIVE_INFINITY
- 양의 무한대를 나타내는 숫자값 infinity와 같다.
<br>

### Number.NEGATIVE_INFINITY
- 음의 무한대를 나타내는 숫자값 -infinity와 같다.
<br> 

### Number.NaN
- 숫자가 아님을 나타태는 숫자값이다.
<br><br>

## 28.3 Number 메서드

### Number.isFinite
- 인수로 전달된 숫자값이 정상적인 유한수면 true 유한수가 아니면 false를 반환한다. (NaN은 false를 반환.)
<br>

### Number.isInteger
- 인수로 전달된 숫자값이 정수면 true 정수가 아니면 false를 반환한다.
<br>

### Number.isNaN
- 인수로 전달된 숫자값이 NaN이면 true를 반환한다.
<br>

### Number.isSafeInteger
- 인수로 전달된 숫자값이 안전한 정수면 true 아니면 false를 반환한다.
<br>

### Number.prototype.toExponential
- 숫자값을 지수표기법으로 변환하여 문자열로 반환한다. 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다. 
<br>

### Number.prototype.toFixed
- 숫자값을 반올림하여 문자열로 반환한다. 인수로 소수점 아래 자리수를 나타내는 수를 전달할 수 있다. 기본값은 0이다. 
```js
(12345.6789).toFixed(1); /// "12345.7"
```
<br>

### Number.prototype.toPrecision
- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다. 
<br>

### Number.prototype.toString
- 숫자값을 문자열로 변환하여 반환한다.
```js
(10).toString()  // '10'
(16).toString(2)  // 10000'
(16).toString(8)  // '20'
```