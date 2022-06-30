# 17. 생성자 함수에 의한 객체 생성

### 17.1 Object 생성자 함수
생성자 함수란?    
new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수    
생성자 함수에 의해 생성된 객체를 인스턴스라 한다.<br><br>

### 17.2 생성자 함수
생성자 함수로 객체를 생성하면 템플릿처럼 프로퍼티의 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.
#### 17.2.1 객체 리터럴에 의한 객체 생성 방식의 문제점
- 객체 리터럴에 의한 생성 방식은 직관적이고 편하지만, 단 하나의 객체만 생성한다.
만약 동일 프로퍼티를 갖는 객체를 여러 개 생성해야 할 경우 매번 같은 프로퍼티를 기술해야하므로 수십개의 객체를 생성해야한다면??? -> 비효율
<br>

#### 17.2.2 생성자 함수에 의한 객체 생성 방식의 장점
```javascript
function Circle(radius){
    this.radius = radius;
    this.getDiameter = function(){
        return 2 * this.radius;
    };
}
const circle1 = new Circle(5);
const circle2 = new Circle(10);
console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```
- 생성자 함수에 의한 객체 생성 방식은 인스턴트를 생성하기 위한 템플릿처럼 여러개를 간편하게 생성할 수 있다.
- **new 연산자를 꼭 붙여야 생성자 함수로 동작한다**
<br>

#### 17.2.3 생성자 함수의 인스턴스 생성 과정
1. 암묵적으로 빈객체가 생성되는데 이 객체가 생성자 함수가 생성한 인스턴스! 이 인스턴스에 this를 바인딩.
2. this에 바인딩 되어있는 인스턴스를 초기화.
3. 생성자 함수 내부의 처리가 끝나면 완성된 인스턴스가 바인딩된 this를 암묵적으로 반환한다.
- 만약 this가 아닌 다른 객체를 명시적으로 반환하면 this를 반환하지 못하므로 **생성자 함수 내부에서 return문은 반드시 생략!**
<br>

#### 17.2.4 내부 메서드
함수는 생성자 함수로서 호출 할 수 있다. new 연산자와 함께..   
```javascript
function foo(){}
foo(); //[[Call]]이 호출
new foo(); //[[Construct]]가 호출
```
모든 함수 객체 : callable   
일반 함수 or 생성자 함수로서 호출 가능 객체 : constructor   
일반 함수로서만 호출 가능 객체 : non-constructor   
<br>

#### 17.2.5 constructor와 non-constructor의 구분
자바스크립트 엔진은 함수 정의 방식에 따라서 con과 non-c로 구분한다.
- constructor : 함수 선언문, 함수 표현식, 클래스(클래스도 함수다)
- non-constructor : 메서드(ES6 메서드 축약표현), 화살표 함수
<br>

#### 17.2.6 new 연산자
new연산자와 함께 함수를 호출하면 ⇒ 해당 함수는 생성자 함수로 동작. ⇒ 함수 객체의 내부 매서드 call이 호출되는 것이 아니라 Constructor가 호출.
* 일반 함수와 생성자 함수에 특별한 차이가 없기에 생성자 함수는 function Circle()과 같이 첫문자를 대문자로 표기한다!
<br>

#### 17.2.7 new.target
생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 new.target을 지원한다.   
new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다. new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.
```javascript
// 생성자 함수
  function Circle(radius) {
    // 이 함수가 new 연산자와 함께 호출되지 않았다면 new.target은 undefined다.
    if (!new.target) {
      // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
      return new Circle(radius);
    }

    this.radius = radius;
    this.getDiameter = function () {
      return 2 * this.radius;
    };
  }

  // new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
  const circle = Circle(5);
  console.log(circle.getDiameter());
  ```