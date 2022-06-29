# 15장 let, const 키워드와 블록 레벨 스코프
## 15.1 var 키워드로 선언한 변수의 문제점

### 15.1.1 변수 중복 허용
- 의도치 않게 중복 선언하여 오류 발생 가능성 up

### 15.1.2 함수 레벨 스코프
- 함수 레벨 스코프 때문에 if, for 문 사용시 의도치 않게 전역변수를 생성할 수 있음.
```jsx
var x = 1;
if(true){
	 var x =10;
}
console.log(x); //10
```

### 15.1.3 변수 호이스팅
- 변수 호이스팅이 에러를 발생시키진 않지만 프로그램의 흐름상 맞지 않고 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남긴다.

## 15.2 let 키워드
### 15.2.1 변수 중복 선언 금지
- 변수 중복 선언시 `SyntaxError`

### 15.2.2 블록 레벨 스코프
- 함수, if문, for문, while문, try/catch 문 등 모든 코드 블록을 지역 스코프로 인정한다.
```jsx
let foo = 1;
{
	 let foo=2;
	 let bar=3;
}

console.log(foo); //1
console.log(bar); //ReferenceError
```
### 15.2.3 변수 호이스팅
- 변수 호이스팅이 발생하지 않는것처럼 동작.
- var 키워드는 런타임 이전에 변수 선언과 초기화 단계가 동시에 이루어져 있어서, 선언 전 참조시 undefined를 반환.
- let 키워드는 런타임 이전에 변수 선언만 이루어지고 초기화 단계는 런타임에 변수 선언문에 도달했을 때 실행된다.
- 그래서 변수 선언전에 접근하려고 하면 Reference에러가 발생.
- 이렇게 변수를 참조할 수 없는 구간을 일시적 사각지대 (TDZ)라고 부름.
```jsx
let foo =1;
{
	 console.log(foo); //ReferenceError
	 let foo = 2;
}
```

### 15.2.4 전역 객체와 let
- var 변수로 선언했을 때는 자동으로 전역 객체의 프로퍼티가 되지만 let 은 X
```jsx
let x = 1;

console.log(window.x); //undefined
```

## 15.3 const 키워드
- 상수 선언시 사용

### 15.3.1 선언과 초기화
- const 키워드로 변수를 선언할 때 선언과 동시에 초기화해야한다.
```jsx
const foo = 1;
const foo; //SyntaxError
```
- 블록 레벨 스코프
- 변수 호이스팅이 발생하지 않는것처럼 동작.

### 15.3.2 재할당 금지
```jsx
const foo = 1;
foo = 2; //TypeError
```

### 15.3.3 상수
- const는 재할당이 금지되기때문에 상태 유지와 가독성, 유지보수의 편의를 위해 적극적으로 사용!
```jsx
const TAX_RATE = 0.1;

let preTaxPrice = 100;

let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice)
```

### 15.3.4 const 키워드와 객체
- const 키워드로 선언된 변수에 객체를 할당할 경우 값 변경 가능.
- 원시값은 변경하려면 재할당을 해야하니 const 특성상 불가능.
- 객체는 재할당 없이 변경이 가능하기 떄문.

## 15.4 var, let, const
- 변수 선언에는 기본적으로 const
- 재할당이 필요한 경우 let
- ES6 var 키워드는 사용 x

