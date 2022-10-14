# 28장 Number

---

## 28.1 Number 생성자 함수
- Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한 후, `[[NumberData]]` 내부 슬롯에 변환된 숫자를 할당한 래퍼 객체 생성. 변환할 수 없을땐 NaN을 할당.
- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환 => 명시적 타입 변환 가능

## 28.2 Number 프로퍼티
> #### 1. Number.EPSILON
> - 부동소수점을 사용하여 연산 할 시 생기는 오차를 해결하기 위해 사용 (IEEE 754)
    
> #### 2. Number.MAX_VALUE
> - JS에서 표현할 수 있는 가장 큰 양수 값 (단, `MAX_VALUE` < `Infinity`)

> #### 3. Number.MIN_VALUE
> - JS에서 표현할 수 있는 가장 작은 양수 값 (단, `MAX_VALUE` > `0`)

> #### 4. Number.MAX_SAFE_INTEGER
> - JS에서 안전하게 표현할 수 있는 가장 큰 정수값 (`9007199254740991`)

> #### 5. Number.MIN_SAFE_INTEGER
> - JS에서 안전하게 표현할 수 있는 가장 작은 정수값 (`-9007199254740991`)

> #### 6. Number.POSITIVE_INFINITY
> - 양의 무한대를 나타내는 숫자값 `Infinity`와 같다

> #### 7. Number.NEGATIVE_INFINITY
> - 음의 무한대를 나타내는 숫자값 `-Infinity`와 같다

> #### 8. Number.NaN
> - 숫자가 아님을 나타내는 숫자값이다 (`Number.NaN` == `window.NaN`)

## 28.3 Number 메서드
> #### 1. Number.isFinite
> - 인수로 전달된 숫자값이 정상적인 유한수, `Infinity` 혹은 `-Infinity`가 아닌지 검사하며 불리언 값으로 반환
> - 인수가 NaN이면 언제나 `false`를 반환
> - 빌트인 전역함수 `isFinite`과 다르게 `Number.isFinite`는 암묵적 타입 변환을 하지 않으므로 숫자가 아닌 인수를 넣었을때 변환값은 언제나 `false`다 

> #### 2. Number.isInteger
> - 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환
> - 암묵적 타입 변환 없음

> #### 3. Number.isNaN
> - 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환
> - 빌트인 전역함수 `isNaN`과 다르게 `Number.isNaN`는 암묵적 타입 변환을 하지 않으므로 숫자가 아닌 인수를 넣었을때 변환값은 언제나 `false`다

> #### 4. Number.isSafeInteger
> - 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환
> - 암묵적 타입 변환 없음

> #### 5. Number.prototype.toExponential
> - 숫자를 지수 표기법으로 변환하여 문자열로 반환 (exponent)
> - 숫자 리터럴과 함께 메서드를 사용할 경우 혼란을 방지하기위해 그룹연산자 사용 (`(77).toExponential();`)


> #### 6. Number.prototype.toFixed
> - 숫자를 반올림하여 문자열로 반환
> - 반올림하는 소수점 이하 자릴 수를 나타내는 0~20사이의 정수값을 인수로 전달할 수 있다

> #### 7. Number.prototype.toPrecision
> - 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림 하여 문자열로 반환한다
> - 인수로 전달받은 전체 자릿수로 표현할 수 없는경우 지수 표기법으로 결과를 반환한다
> - 0~21 사이의 정수값을 인수로 전달 할 수 있으며 기본값은 0이 지정된다

> #### 8. Number.prototype.toString
> - 숫자를 문자열로 변환하여 반환한다. 진법을 나타내는 2~36사이의 정수값을 인수로 전달 할 수있다.
> - 인수 생략시 기본값 10진법

---
# 끝