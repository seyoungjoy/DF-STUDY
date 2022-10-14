# 30장 Date

---

## 30.1 new Date 사용법
- 브랜든 아이크가 JS를 처음 만들때 JAVA를 참고하여 만들게됨
- - 근데 JAVA에서 너무 단점이 많아서 이제는 사용하지 않는 레거시 코드가됨
```jS
Date date = new Date();
System.out.printIn(date);
```
```js
var date = new Date();
console.log(date)
// Fri Oct 14 2022 14:58:19 GMT+0900 (한국 표준시)
```

## 30.2 Date 프로퍼티
> #### 1. Date.EPSILON
> - 부동소수점을 사용하여 연산 할 시 생기는 오차를 해결하기 위해 사용 (IEEE 754)
    
> #### 2. Date.MAX_VALUE
> - JS에서 표현할 수 있는 가장 큰 양수 값 (단, `MAX_VALUE` < `Infinity`)

> #### 3. Date.MIN_VALUE
> - JS에서 표현할 수 있는 가장 작은 양수 값 (단, `MAX_VALUE` > `0`)

> #### 4. Date.MAX_SAFE_INTEGER
> - JS에서 안전하게 표현할 수 있는 가장 큰 정수값 (`9007199254740991`)

> #### 5. Date.MIN_SAFE_INTEGER
> - JS에서 안전하게 표현할 수 있는 가장 작은 정수값 (`-9007199254740991`)

> #### 6. Date.POSITIVE_INFINITY
> - 양의 무한대를 나타내는 숫자값 `Infinity`와 같다

> #### 7. Date.NEGATIVE_INFINITY
> - 음의 무한대를 나타내는 숫자값 `-Infinity`와 같다

> #### 8. Date.NaN
> - 숫자가 아님을 나타내는 숫자값이다 (`Date.NaN` == `window.NaN`)

## 30.3 Date 메서드
> #### 1. Date.isFinite
> - 인수로 전달된 숫자값이 정상적인 유한수, `Infinity` 혹은 `-Infinity`가 아닌지 검사하며 불리언 값으로 반환
> - 인수가 NaN이면 언제나 `false`를 반환
> - 빌트인 전역함수 `isFinite`과 다르게 `Date.isFinite`는 암묵적 타입 변환을 하지 않으므로 숫자가 아닌 인수를 넣었을때 변환값은 언제나 `false`다 

> #### 2. Date.isInteger
> - 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환
> - 암묵적 타입 변환 없음

> #### 3. Date.isNaN
> - 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환
> - 빌트인 전역함수 `isNaN`과 다르게 `Date.isNaN`는 암묵적 타입 변환을 하지 않으므로 숫자가 아닌 인수를 넣었을때 변환값은 언제나 `false`다

> #### 4. Date.isSafeInteger
> - 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환
> - 암묵적 타입 변환 없음

> #### 5. Date.prototype.toExponential
> - 숫자를 지수 표기법으로 변환하여 문자열로 반환 (exponent)
> - 숫자 리터럴과 함께 메서드를 사용할 경우 혼란을 방지하기위해 그룹연산자 사용 (`(77).toExponential();`)


> #### 6. Date.prototype.toFixed
> - 숫자를 반올림하여 문자열로 반환
> - 반올림하는 소수점 이하 자릴 수를 나타내는 0~20사이의 정수값을 인수로 전달할 수 있다

> #### 7. Date.prototype.toPrecision
> - 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림 하여 문자열로 반환한다
> - 인수로 전달받은 전체 자릿수로 표현할 수 없는경우 지수 표기법으로 결과를 반환한다
> - 0~21 사이의 정수값을 인수로 전달 할 수 있으며 기본값은 0이 지정된다

> #### 8. Date.prototype.toString
> - 숫자를 문자열로 변환하여 반환한다. 진법을 나타내는 2~36사이의 정수값을 인수로 전달 할 수있다.
> - 인수 생략시 기본값 10진법

---
# 끝