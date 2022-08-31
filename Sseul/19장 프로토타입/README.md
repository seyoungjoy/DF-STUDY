# 19. 프로토타입

## 19.1 객체지향 프로그래밍
구현하려는 프로그램에 필요한 속성만 간추려 내어 표현하는 것을 **추상화**라고 한다.   
객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.
<br><br>

## 19.2 상속과 프로토타입
프로토타입을 기반으로 상속을 구현하고 불필요한 중복을 제거하고 기존의 코드를 재사용한다.

```js
function Circle(radius) {
	this.radius = radius;
	this.getArea = function() {
		return Math.PI * this.radius ** 2;
	}
}

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // false
```
- 같은 메소드이지만 다르게 인식
- 다르게 인식하게 되면 메모리를 불필요하게 낭비한다.
<br>

```js
function Circle(radius) {
	this.radius = radius;
}

Circle.prototype.getArea = function () {
	return Math.PI * this.radius ** 2;
};

const circle1 = new Circle(1);
const circle2 = new Circle(2);

console.log(circle1.getArea === circle2.getArea); // true
```
- getArea 메서드는 하나만 생성되어 Circle 생성자 함수가 생성하는 모든 인스턴스는 getArea 메서드를 상속받아 사용할 수 있다.
<br><br>

## 19.3 프로토타입 객체
모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 여기에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다. 즉, 객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다.
- 객체 리터럴에 의해 생성된 객체의 프로토타입은 Object.prototype이다.
- 생성자 함수에 의해 생성된 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.
<br><br>

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
```js
const obj = {}; // 객체 리터럴로 생성

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); //true
```
문제는 리터럴 표기법에 의해 생성된 객체의 생성자 함수는 프로토타입이 존재하나, **constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수와 같지는 않다.**
추상 연산에 의해 다르다고 하는데 결론은 리터럴 표기법에 의해 생성된 객체나 생성자 함수로 생성한 객체나 본질적인 면에서 큰 차이가 없다.   
리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖고 프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문이다. 
<br><br>

## 19.5 프로토타입의 생성 시점
```"프로토타입과 생성자 함수는 언제나 쌍으로 존재하므로 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다."```
1. 사용자 정의 생성자 함수와 프로토타입 생성 시점
- 생성자 함수로 호출 가능한 함수인 constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 생성된다.
- 이 때 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.
2. 빌트인 생성자 함수와 프로토타입 생성 시점
- 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성되므로, 이 때 프로토타입도 생성된다.
- 이 후 생성된 객체는 프로토타입을 상속받는다.
<br><br>

## 19.6 객체 생성 방식과 프로토타입의 결정
객체를 생성하는 방법
- 객체 리터럴
- Object 생성자 함수
- 생성자 함수
- Object.create 메서드
- 클래스(ES6)
<br><br>

## 19.7 프로토타입 체인
프로토타입 체인이란?
자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 매커니즘이다.
- 체인의 최상위에 위치하는 객체는 Object.prototype 이다. 모든 객체는 Object.protytpe을 상속받는다. Object.prototype 를 프로토타입 체인의 종점이라고 한다.
- 프로토타입 체인은 <u>상속과 프로퍼티 검색</u>을 위한 메커니즘이다. 이해 반해 스코프 체인은 <u>식별자 검색</u>을 위한 메커니즘이다.
- 스코프 체인과 프로토타입 체인은 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용한다.
<br><br>

## 19.8 오버라이딩과 프로퍼티 섀도잉
```js
const Person = (function () {
    //생성자 함수
    function Person(name) {
    	this.name = name;
    }
    
    //프로토타입 메서드
    Person.prototype.sayHello = function () {
   	console.log(`hi! my name is ${this.name}`);
     };
     
     return Person
}());

//인스턴스 생성
const me = new Person('Lee');

//인스턴스 프로퍼티(메서드) 오버라이딩(재정의)
me.sayHello = function() {
     console.log('Hey! my name is ${this.name}');
};

me.sayHello();
```
- 상위 프로토타입이 소유한 프로퍼티(메서드 포함) : <b>프로토타입 프로퍼티</b>
- 인스턴스가 소유한 프로퍼티: <b>인스턴스 프로퍼티</b>    
위 예시 속 <b>인스턴스 메서드 sayHello는 프로토타입 메서드의 sayHello 메서드를 오버라이딩</b> 했다 하며, 이러한 <b>상속관계에 의해 프로퍼티가 가려지는 현상을 프로퍼티 섀도잉</b>이라 한다.
<br><br>

## 19.9 프로토타입의 교체
프로토타입은 임의의 다른 객체로 변경 할 수 있다. 자신의 부모 객체를 동적으로 변경할 수 있으며, 객체 간의 상속관계를 변경할 수 있다. 생성자함수, 인스턴스에 의해 교체된다.   
프로토타입 교체를 통해 객체간의 상속관계를 동적으로 변경하는 것은 매우 번거롭다! **그래서 프로토타입을 직접 교체하지 않는 것이 좋다.** 상속관계를 인위적으로 설정하려면 다른 방법을 이용하는 것이 더 편리하고 안전하다!
<br><br>

## 19.10 instanceof 연산자
```js 
//객체 instanceof 생성자 함수
function Person(name) {
   this.name = name;
}
const me = new Person('Lee');
console.log(me instanceof Person); // true
console.log(me instanceof Object); // true
```
우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true, 아니면 false로 평가된다.   
생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인해주는 것이다.
<br><br>

## 19.11 직접 상속
```js
//Object.create에 의한 직접 상속
let obj = Object.create(Object.prototype, {
  x : { value:1, writable: true, enumerable:true, configurable:true}
});
```
new 연산자 없이도 객체 생성 가능하고 프로토타입을 지정하면서 객체 생성도 하며,객체 리터럴에 의해 생성된 객체도 상속 가능하다는 장점이있다.
```js
//객체 리터럴 내부에서 __proto__에 의한 직접 상속
const myProto = {x:10};
const obj = {
  y:20,
  __proto__:myProto;
};

console.log(Object.getPrototypeOf(obj) === myProto); //true
```
Object.create 메서드에 의한 직접 상속은 두 번째 인자로 프로퍼티를 정의하기 번거롭다는 단점이 있는데
ES6에서는 객체 리터럴 내부에서 proto 접근자 프로퍼티를 사용해 직접 상속을 구현할 수 있다.
<br><br>

## 19.12 정적 프로퍼티/메서드
```js
function Foo() {}

// 프로토타입 메서드
Foo.prototype.x = function () {
	console.log('x');
};

// 프로토타입 메서드 호출하려면 인스턴스 생성해야 함.
const foo = new Foo();
foo.x(); // x

// 정적 메서드
Foo.x = function() {
	console.log('x');
};

// 인스턴스 생성 없이 호출 가능
Foo.x(); // x
```
- 정적(static) 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출 할 수 있는 프로퍼티/메서드를 말한다.
- 생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼티/메서드라 한다.
- 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출 할 수 없다.

## 19.13 프로퍼티 존재 확인
```in 연산자```<br/>
상속받은 모든 프로토타입 프로퍼티를 열거한다. ES6에서 도입된 Reflect.has 메서드를 사용할 수도 있다.   
```hasOwnProperty 메서드```
 자기가 직접 가지고 있는 프로퍼티만 나온다.
 <br><br>

## 19.14 프로퍼티 열거
```js
//for (변수선언문 in 객체) {...}
for(key in Person){
  //검색
}
```
순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거한다.<br>
```Object.keys/values/entries 메서드```는 객체 자신의 고유 프로퍼티만 열거한다.