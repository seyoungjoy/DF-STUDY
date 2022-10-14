# 22장 this
## 22.1 this 키워드
- this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
- this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- this가 가리키는 값은, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
  <br><br>
1. 객체 리터럴의 메서드 내부의 this는 메서드를 호출한 객체 circle을 가리킨다.

```jsx
const circle = {
  radius:5,
  getDiameter(){
    return 2*this.radius;
  }
}
console.log(circle.getDiameter());
```
<br><br>
2. 생성자 함수 내부의 this 는 생성자 함수가 생성할 인스턴스를 가리킨다.

```jsx
function Circle(radius){
  this.radius = radius;
}
Circle.prototype.getDiameter = function(){
  return 2*this.radius;
}

const circle = new Circle(5);
console.log(circle.getDiameter());//10
```

<br><br>
3. this는 코드 어디서에든 참조가 가능하다.

```jsx
console.log(this); //window
```

## 22.2 함수 호출 방식과 this 바인딩
- this 바인딩은 함수 호출 방식, 즉 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.
1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 22.2.1 일반 함수 호출
- 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.

```jsx
function foo(){
  console.log(this) //window
  function bar(){
    console.log(this); //window
  }
  bar()
}
foo();
```

### 22.2.2 메서드 호출
- 메서드 내부에서 this는 메서드를 호출한 객체를 가리키게 된다.
- 주의한 점! this의 메서드를 소유한 객체가 아닌, 메서드를 호출한 객체에 바인딩된다.

```jsx
const person = {
  name:'Lee',
  getName(){
    return this.name;
  }
}
console.log(person.getName()) //Lee
```
- getName이라는 메서드는 person에 종속된 함수 객체가 아니라 독립적으로 존재하는 별도의 객체다.

```jsx
const anotherPerson = {
  name:'Kim'
}

anotherPerson.getName = person.getName;

console.log(anotherPerson.getName()); //Kim

const getName = person.getName;

console.log(getName()); // ''
//this.name은 브라우저 환경의 window.name을 불러온다.
```

### 22.2.3 생성자 함수 호출
- 생성자 함수 내부의 this에는 생성자 함수가 생성할 인스턴스가 바인딩된다.

```jsx
function Circle(radius){
  this.radius = radius;
  this.getDiameter = function(){
    return 2*this.radius;
  }
}

//반지름이 5인 Circle 객체를 생성.
const circle1 = new Circle(5);

//반지름이 10인 Circle 객체를 생성.
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); //10
console.log(circle2.getDiameter()); //20
```


### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출
- apply와 call 메서드의 본질적인 기능은 함수를 호출하는 것이고
- 첫번째 인수로 전달한 객체를 this에 바인딩한다.
```jsx
function getThisBinding(){
  return this;
}

const thisArg = {a:1};

console.log(getThisBinding());//window

console.log(getThisBinding.apply(thisArg)); //{a:1}
console.log(getThisBinding.call(thisArg)); //{a:1}
```
- bind 메서드는 apply, call 메서드와 달리 함수를 호출하지 않고
- 첫번째 인수로 전달된 값으로 this 바인딩을 한 후 함수를 새롭게 생성해서 반환한다.

```jsx
function getThisBinding(){
		return this;
}

// this로 사용할 객체
const thisArg = {a:1};

//getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); //getThisBinding
// bind 메서드는 함수를 호출하지 않기때문에 명시적으로 호출해줘야한다.
console.log(getThisBinding.bind(thisArg)()); //{a:1}


```

```jsx
const person = {
		name:'Lee',
    foo(callback){
				setTimeout(callback.bind(this), 100);
    }
}

person.foo(function(){
		console.log(this.name);
})
```
