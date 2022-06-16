# 12장 함수
## 12.1 함수란?
- 일련의 과정을 문으로 구현하고 코드 블록으로 감싸서 하나의 실행 단위로 정의한 것.

## 12.2 함수를 사용하는 이유
- 코드의 재사용성 측면에서 유용.
- 유지보수의 편의성을 높이고 실수를 줄여 코드의 신뢰성을 높이는 효과

## 12.3 함수 리터럴
```jsx
var f = function add(x,y){
    return x+y;
}
```
- 함수는 객체다. 일반객체는 호출 X, 함수는 호출이 가능.

## 12.4 함수 정의
- 함수를 정의하는 방법에는 함수 선언문, 함수 표현식, Function 생성자 함수, 화살표 함수 네가지가 있다.

### 12.4.1 함수 선언문
```jsx
function add(x,y){
    return x+y;
}

console.log(add(2,4)); //6
```
- 함수 이름 생략 불가능
- 함수 선언문인 표현식이 아닌 문이다.
```jsx
var add = function add(x,y){
    return x+y;
}
```
- 근데 함수 선언문이 변수에 할당되는거처럼 보인다. 
- 이는 함수 선언문이 함수 리터럴 형태와 비슷하기 때문에 기명함수 리터럴은 함수 선언문 또는 함수 리터럴 표현식으로 해석될 가능성이 있다.

```jsx
//기명 함수 리터럴을 단독으로 사용하면 함수 선언문으로 해석된다.
// 함수 선언문에서는 함수 이름을 생략할 수 없다.
function foo(){ console.log('foo')};

foo();

// 함수 리터럴을 피연산자로 사용하면 함수 선언문이 아니라 함수 리터럴 표현식으로 해석된다.
// 함수 리터럴에서는 함수 이름을 생략할 수 있다.
(function bar() {console.log('bar')});
bar();

```
- 자바스크립트 엔진은 생성된 함수를 호출하기 위해 함수 이름과 동일한 이름의 식별자를 암묵적으로 생성하고, 거기에 함수 객체를 할당한다.
- 함수는 함수 이름으로 호출하는 것이 아니라, 함수 객체를 가리키는 식별자로 호출한다.
- 결론적으로 자바스크립트 엔진은 함수 선언문을 함수 표현식으로 변환해 함수 객체를 생성한다고 할 수 있다.

### 12.4.2 함수 표현식
- 일급객체 : 값의 성징을 갖는 객체, 자바스크립트 함수는 일급 객체!

```jsx
var add = function (x,y) {
    return x+y;
}

console.log(add(2,5));
```

* 함수 선언문은 표현식이 아닌 문이고, 함수 표현식은 표현식인 문이다... 미묘한 차이...

### 12.4.3 함수 생성 시점과 함수 호이스팅
```jsx
console.dir(add); //(1) f add(x,y)
console.dir(sub); //(2) undefined

console.log(add(2,5)); // (3) 7
console.log(sub(2,5)); // (4) TypeError: sub is not a function

//함수 선언문
function add(x,y){
    return x+y;
}

//함수 표현식
var sub = function(x,y){
    return x-y;
}

```
- 함수 선언문으로 정의한 함수와 함수 표현식으로 정의한 함수의 생성 시점이 다르다.
- 함수 선언문은 함수 호이스팅이 일어나 코드 선두로 끌어올려진거처럼 동작한다. 그래서 (1)번같은 경우 함수 참조가 가능, (3)에서 함수 호출도 가능. 
- 함수 표현식으로 함수를 정의하면 변수 호이스팅이 일어나 (2)에서는 변수의 초기화된 값 undefined가 출력되고, (4)에서는 호출할 함수를 찾을수 없기 때문에 typeerror가 뜬다.
- 함수 호이스팅은 함수를 호출하기전에 함수를 선언해야한다는 당연한 규칙을 무시하기 때문에 더글라스 크락포드라는 유명한 분은 함수 표현식 사용을 권장하고 있다.

### 12.4.4 Function 생성자 함수

### 12.4.5 화살표 함수

## 12.5 함수 호출
### 12.5.1 매개변수와 인수

### 12.5.2 인수 확인
- 자바스크립트에서는 매개변수의 타입을 사전에 지정할 수 없기 때문 이를 체크할 필요가 있다.

### 12.5.3 매개변수 최대 개수
- 이상적인 함수는 한 가지 일만 해야하며 가급적 작게 만들어야한다.
- 매개변수는 최대 3개 이상을 넘지 않는 것을 권장하고 있다.
### 12.5.4 반환문
- return 키워드를 지정하지 않으면 undefined 반환
- return과 키워드 사이에 줄바꿈이 있으면 의도치 않은 결과 발생할 수 있음.

## 12.6 참조에 의한 전달과 외부 상태의 변경

## 12.7 다양한 함수의 형태
### 12.7.1 즉시 실행 함수
- 함수 정의와 동시에 즉시 호출되는 함수
- 익명함수로사용
```jsx
(function(){
    console.log(1);
}());
```

### 12.7.2 재귀 함수
- 함수가 자기 자신을 호출하는 것
```jsx
function countdown(n){
    for(var i = n; i >= 0; i--) console.log(i);
}

countdown(10);
//잘동작 근데 이걸 재귀함수로 호출 가능

function countdown(n){
    if(n < 0) return;
    console.log(n);
    countdown(n-1); //재귀 호출
}

countdown(10);

```
### 12.7.3 중첩 함수
- 함수 내부에 정의된 함수를 중첩 함수
```jsx
function outer(){
    var x = 1;
    function inner(){
        var y = 2;
        console.log(x+y);
    }
    
    inner();
}

outer();
```
### 12.7.4 콜백 함수
```jsx
function repeat(n){
    for(var i =0; i<n; i++) console.log(i);
}
repeat(5); //0 1 2 3 4

function repeat1(n){
    for(var i=0; i<n; i++) console.log(i);
}

repeat1(5); //0 1 2 3 4

//n만큼 어떤 일을 반복한다.
function repeat2(n){
    for(var i=0; i<n; i++){
        if(i % 2) console.log(i);
    }
}

repeat2(5); //1 3
```

```jsx
function repeat(n,f){
    for(var i=0; i<n; i++){
        f(i);
    }
}

var logAll = function(i){
    console.log(i);
}

repeat(5, logAll);

var logOdds = function(i){
    if(i%2) console.log(i);
}

repeat(5, logOdds); //
```
```jsx
//익명 함수 리터럴을 콜백 함수로 고차 함수에 전달한다
//익면 함수 리터럴은 repeat 함수를 호출할 때마다 평가되어 함수 객체를 생성한다.

repeat(5, function(i){
    if(i %2) console.log(i);
})
// 다른 조건의 콜백함수가 필요할때도 있기때문에 외부에서 함수를 정의한 후 매개변수로 전달하는 편이 효율적
```

```jsx
var logOdds = function(i){
    if(i%2) console.log(i);
}

//고차 함수에 함수 참조를 전달한다.
repeat(5, logOdds);
```

- 콜백함수는 비동기 처리에서 활용되는 중요한 패턴
```jsx
// 콜백 함수를 사용한 이벤트 처리
// myButton 버튼을 클릭하면 콜백 함수를 실행한다.
document.getElementById('myButton').addEventListener('click', function(){
    console.log('button clicked')
})

setTimeout(function(){
    console.log('1초 경과')
}, 1000)
```

### 12.7.5 순수 함수와 비순수 함수


