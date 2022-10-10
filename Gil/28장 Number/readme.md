<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter28.%20Number&fontSize=60" width="100%" alt="">

# **28.1 Number 생성자 함수**
Number 객체는 생성자 함수이므로 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

Number 생성자 함수에 인수를 전달 하지 않고 new 연산자와 함께 호출하면 내부슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.
```js
const gil = new Number();
console.log(gil); // Number {[[PrimitiveValue]]: 0}


//Number 생성자 함수에 인수로 전달 후 new 연산자와 함께 호출하면 인수로 전달 받은 숫자를 할당한 Number 래퍼 객체를 생성한다.(NumberData)

//래퍼객체: 원시값을 마침표 표기법으로 접근하면 객체처럼 접근할 수 있게 해주는 래퍼객체를 생성하여 프로퍼티 메서드를 상속받아 사용할 수 있게 해준다.

const gil2 = new Number(10);
console.log(gil2); //Number {[[PrimitiveValue]]: 10]}
```

인수로 받은 값을 숫자로 변환할 수 없으면 NaN을 할당 받아 래퍼객체를 생성한다. 
또한 new 연산자를 사용하지 않으면 인스턴스가 아닌 숫자를 반환한다.

```js
const gil = new Number('hi~~!');
console.log(gil); // Number {[[PrimitiveValue]]: NaN}

//명시적 타입 변환
Number(true); // 1
Number(false); // 0
```

<br>

# **28.2 Number 프로퍼티**

<br>

## **28.2.1 Number.EPSILON**
Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```js
function isEqual(a,b){
    return Math.abs(a - b) < Number.EPSIOLON;
}
isEqual(0.1 + 0.2, 0.3); // true
```

<br>

## **28.2.2 Number.MAX_VALUE**
자바스크립트에서 표현할 수 있는 가장 큰 양수 값(더 큰 숫자는 Infinity다.)

<br>

## **28.2.3 Number.MIN_VALUE**
자바스크립트에서 표현할 수 있는 가장 작은 양수 값(더 작은 숫자는 0이다.)

<br>

## **28.2.4 Number.MAX_SAFE_INTEGER**
정수값을 정확하고 올바르게 비교할수 있는 값중 가장 큰 값이다.

<br>

## **28.2.5 Number.MIN_SAFE_INTEGER**
정수값을 정확하고 올바르게 비교할수 있는 값중 가장 작은 값이다.

<br>

## **28.2.6 Number.POSITIVE_INFINITY**
Infinity와 같다.(양의 무한대)

<br>

## **28.2.7 Number.NEGATIVE_INFINITY**
-Infinity와 같다.(음의 무한대)

<br>

## **28.2.8 Number.NaN**
숫자가 아님을 나타내는 숫자값(= window.NaN)

<br>

# **28.3 Number 메서드**

<br>

## **28.3.1 Number.isFinite**
인수의 값으로 무한수, NaN 이면 false를 반환하고 유한수 이면 true를 반환한다.

>빌트인 전역함수 isFinite는 전달받은 인수를 암묵적 타입변환을 하여 검사를 수행하지만 Number.isFinite는 그렇지 않으므로 숫자 외 다른 타입을 값으로 전달 받았을 때 항상 false를 반환한다.

```js
Number.isFinite(0); // true
Number.isFinite(Infinity); //false
Number.isFinite(NaN); // false

//isFinite와 차이 (true는 암묵적 타입 변환시 1에 해당한다.)
Number.isFinite(true); //false
isFinite(true); // true
```

<br>

## **28.3.2 Number.isInteger**
인수로 전달받은 값이 정수인지 아닌지 불리언값으로 반환한다.(암묵적 타입변환을 하지않는다.)

```js
Number.isInteger(0); // true
Number.isInteger('0'); // false
```

<br>

## **28.3.3 Number.isNaN**
인수로 전달 받은 값이 NaN인지 유무를 불리언값으로 반환한다.(암묵적 타입변환을 하지 않는다.)

<br>

## **28.3.4 Number.isSafeInteger**
인수로 전달된 값이 안전한 정수(정수값을 정확하고 올바르게 비교할 수 있는 값) 인지 불리언 값으로 반환한다.

<br>

## **28.3.5 Number.prototype.toExponential**
숫자를 지수 표기법으로 변환하여 문자열로 반환한다. 인수값으로 표현할 자릿수를 전달할 수 있다.

```js
(33.33333333333333333).toExponential(4); //'3.3333e+1'
```

>괄호()를 사용하지 않으면 부동소수점 숫자의 소수 구분이 모호해 Syntax 에러가 발생한다.

<br>

## **28.3.6 Number.prototype.toFixed**
숫자를 반올림하여 문자열로 반환한다. 인수값으로 0~20까지 할당할 수 있으며 반올림할 자리수를 지정한다.

```js
(3.3333333).toFixed(4); //'3.3333'
```

<br>

## **28.3.7 Number.prototype.toPrecision**
인수로 전달받은 전체 자리수까지 유효하도록 나머지 자릿수를 반올림 하여 반환한다.

```js
(1234.6789).toPrecision(); // '12345.6789'
(1234.6789).toPrecision(2); // '1.2e+4'
```

<br>

## **28.3.8 Number.prototype.toString**
숫자를 진법을 나타내는 숫자 문자열로 변환하여 반환한다. 인수값을 설정하지 않으면 10진법이 기본으로 지정 된다.

```js
(10).toString(); // "10"
(16).toString(2); // "10000"
```