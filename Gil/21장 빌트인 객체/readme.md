<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter21.%20%EB%B9%8C%ED%8A%B8%EC%9D%B8%20%EA%B0%9D%EC%B2%B4&fontSize=50">

# **21.1 자바스크립트 객체의 분류**
**```표준 빌트인 객체```** <br>
* ECMAScript 사양에 정의 O
* 자바스크립트 실행 환경에 상관없이 사용 가능
* 전역 객체의 프로퍼티로 제공되며, 별도 선언 없이 전역 변수 처럼 사용 가능

<br>

**```호스트 객체```**
* ECMAScript 사양에 정의 X
* 자바스크립트 실행 환경(브라우저, node.js 환경)에서 추가로 제공하는 객체
* 브라우저환경: DOM, BOM, Canvas, XHTMLHttpRequest, fetch, requestAnimationFrame, SVG, WebStorage, WebComponent, Web Worker와 같은 클라이언드 사이드 Web API를 호스트객체로 제공
* Nodejs 환경: 고유 API를 호스트 객체로 제공

<br>

**```사용자 정의 객체```**
사용자가 직접 정의한 객체

<br>

# **21.2 표준 빌트인 객체**
표준 빌트인 객체는 생성자함수로 호출 가능 하며 인스턴스 프로토타입은 Number.protoype이다.

<br>

**```표준 빌트인 객체```** 는 인스턴스 없이도 호출가능한 정적 메서드도 제공한다.

```js
const gilObj = new Number(1.5); //Number {1.5}

console.log(Object.getPrototypeOf(gilObj) === Number.prototype); //true

console.log(gilObj.toFixed()); // 2 -> toFixed()는 소수점에서 반올림하는 포로토타입 메서드이다.
console.log(Number.isInterger(0.5));//false -> isInterger() 정수인지 확인하는 정적 메서드
```

<br>

# **21.3 원시값과 래퍼 객체**
원시값을 마침표 표기법으로 접근하면 객체처럼 접근할 수 있게 해주는 ```래퍼객체```를 생성하여 프로퍼티 메서드를 상속받아 사용할 수 있게 해준다. <br><br>
그리고 호출 후 다시 원시값으로 되돌리고, 마지막으로 **가비지컬렉션** 대상이 되어 메모리에서 삭제 된다.

```js
const gil = 2.2;

console.log(gil.toFixed());//2, -> 래퍼객체를 생성 하여 프로퍼티 메서드에 접근
console.log(typeof gil, gil); //number, 2.2 -> 호출 후 다시 원시값으로 되돌아 가며 가비지컬렉션 대상이 된다.
```

<br>

# **21.4 전역 객체**
코드가 실행되기 이전단계(소스코드 평가)에 어떤 객체보다도 먼저 생성되고, 어떤 객체에도 속하지 않은 최상위 객체이다.

📌 globalThis
> ES11에서 도입된 것으로 브라우저 환경에서는 **window**, node.js환경에서는 **global**이라고 알고 있으면 된다.

```전역객체```는 모든 빌트인 객체의 최상위객체이다.(프로토타입 체인에서의 최상위 X)

### **전역객체의 특징**
* 전역객체를 생성자 함수는 제공되지않아 의도적으로 생성 불가능
* 전역객체의 프로퍼티를 참조할때 window(global)를 생략 가능
* 모든 표준 빌트인 객체를 프로퍼티로 가지고 있다.
* 자바스크립트 실행 환경(브라우저 or Node.js)에 따라 추가적으로 프로퍼티와 메서드를 갖는다.(호스트객체)
* var 키워드로 선언한 전역변수, 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역함수는 전역객체의 프로퍼티가 된다.

    ```js
    var gil = 1;
    gil2 = 2;
    functon gil3(){ return 3 };

    console.log(window.gil); // 1
    console.log(window.gil2); // 2
    console.log(window.gil3()); // 3
    ```
    📌 let, const로 선언한 전역변수는 보이지않는 개념적인 블록 내 존재해서 전역객체의 프로퍼티가 아니다.

* 브라우저 환경의 모든 자바스키립트 코드는 하나의 전역 객체 window를 공유한다.

<br>

## **21.4.1 빌트인 전역 프로퍼티(전역객체의 프로퍼티)**

<br>

```Infinity``` <br>
Infinity 프로퍼티는 무한대를 나타내는 숫자값이다.

<br>

```NaN``` <br>
숫자가 아님을 나타내는 숫자값이다. (= Number.NaN)

<br>

```undefined``` <br>
원시 타입 undefined를 값으로 갖는다.

<br>

## **21.4.2 빌트인 전역 함수(전역객체의 메서드)**

<br>

~~```eval``` <br>~~

<br>

```isFinite``` <br>
전달받은 인수가 유한수이면 true, 무한수이면 false를 반환한다. (숫자타입이 아니라면 숫자타입으로 변환하여 검사를 수행한다.)

<br>

```isNaN``` <br>
전달받은 인수가 NaN인지 유무를 불리언 타입으로 반환한다.

<br>

```parseFloat``` <br>
전달받은 인수를 부동 소수점 숫자(실수)로 해석하여 반환한다.

<br>

```parseInt``` <br>
전달받은 인수를 부동 소수점 숫자(실수)로 해석하여 반환한다.

<br>

>계산을 하다보면 숫자 밑에 소수점이 엄청난 자리숫자로 막 뜨는경우가 있는데 이 숫자로 계산할 경우 원하는 값이 제대로 안나올때가 있습니다. 이걸 방지하려고 parseFloat, parseInt를 썼던 적 있습니다.

```encodeURI / decodeURI``` <br>
우리가 알고있는 사이트 주소(링크 주소)를 이스케이프 처리 하여 아스키 문자셋 바꿔 어떤 시스템에서도 읽을 수 있도록 한다.

<br>

```encodeURIComponent / decodeURIComponent``` <br>
encodeURI / decodeURI 와 같은 역할을 하나 쿼리 스트링 구분자로 사용 되는 =, ?, &까지 이스케이프 처리해준다.

<br>

## **21.4.3 암묵적 전역**
선언 하지 않은 식별자를 사용하여 값을 할당 할 경우 window의 프로퍼티가 된다. 따라서 호이스팅이 일어 나지않고(프로퍼티이기 때문) delete를 이용하여 값을 지울수도 있다.

>전역에 선언한 식별자는 프로퍼티이지만 삭제할 수 있고, 호이스팅도 일어난다.