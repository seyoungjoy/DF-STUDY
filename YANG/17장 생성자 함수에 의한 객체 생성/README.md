# 17장 생성자 함수에 의한 객체 생성
## 17.1 Object 생성자 함수
```jsx
// 빈 객체의 생성
const person = new Object();

// 프로퍼티 추가
person.name = 'Lee';
person.sayHello = function(){
		console.log('Hi! My Name is ' + this.name);
}

console.log(person);
person.sayHello();
```

### 생성자 함수란?
- new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
- String, Number, Boolean, Function, Array, Date, RegExp, Promise 등의 빌트인 생성자 함수를 제공한다.


## 17.2 생성자 함수
### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점
- 직관적이긴 하나, 동일한 프로퍼티를 갖는 객체를 여러개 생성해야할 때 매번 같은 프로퍼티를 기술해야 하기 때문에 비효율적.

### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점
- 객체(인스턴스)를 생성하기 위한 템플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성할 수 있다.
```jsx
// 생성자 함수
function Circle(radius){
		this.radius = radius;
		this.getDiameter = function(){
				return 2 * this.radius;
        };
}

// 인스턴스의 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);

console.log(circle1);
console.log(circle2.getDiameter())
```

### 17.2.3 생성자 함수의 인스턴스 생성 과정
```jsx
function Circle(radius){
		// 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
		console.log(this); //Circle{}
    
    // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
    this.radius = radius;
		this.getDiameter = function(){
				return 2*this.raduis;
    }
		
		// 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
}

//인스턴스 생성 
const circle = new Circle(1);
console.log(circle); //Circle{radius:1, getDeiameter:f}
```

### 17.2.4 내부 메서드 `[[call]]`과 `[[Construct]]`
- 함수도 객체이기 때문에 일반객체처럼 내부 슬롯과 내부 메서드를 가지고 있으며 추가로 `[[Environment]]`, `[[FormalParameters]]`, `[[call]]`과 `[[Constructor]]`를 가지고 있다.
- 함수가 일반 함수로서 호출되면 `[[call]]`, 생성자 함수로 호출되면 `[[Constructor]]` 내부 메서드가 호출된다.
- 모든 함수 객체는 callable 이지만 모든 함수 객체가 constructor인 것은 아니다.
- 
### 17.2.5 constructor와 non-constructor의 구분
- constructor : 함수 선언문, 함수 표현식, 클래스
- non-constructor : 메서드, 화살표 함수

### 17.2.6 new 연산자
- 일반 함수와 생성자 함수에 특별한 형식적 차이가 없다.
- 그래서 생성자 함수로 정의하지 않은 일반 함수에 new 연산자를 붙이면 `[[constructor]]` 내부 메서드가 호출된다.
- 반대로 생성자 함수로 정의했는데 new 연산자를 붙이지 않으면 `[[call]]` 내부 메서드가 호출된다.
- 그렇기 때문에 일반함수와 생성자 함수를 구별하기 위해 생성자 함수는 파스칼 케이스로 명명한다.


