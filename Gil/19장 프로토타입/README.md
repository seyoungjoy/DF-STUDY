 <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter.19%20%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85&fontSize=50" style="width:100%;" alt=""/>


# **19.1  객체지향 프로그래밍**
객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임을 **객체지향 프로그래밍**이라고 한다.
<br>
객체의 다양한 속성 중 프로그램에 필요한 속성을 간추려 내어 표현하는 것을 **추상화**라고 한다.

<br>

# **19.2  상속과 프로토타입**
생성자 함수로 만들어진 인스턴스는 생성때 마다 동일한 메서드를 중복 소유한다. 자바스크립트는 프로토타입을 기반으로 상속을 구현해서 중복 소유를 제거 가능하다.

```js
//생성자 함수
function CIrcle(radius){
    this.radius = radius;
}

Circle.porototype.getArea = function(){
    return Math.PI*this.radius**2;
};

//인스턴스 생성
const circle1 = new Circle(1);
const circle2 = new Circle(2);

//두개의 인스턴스는 Circle.prototype으로부터 같은 메서드를 상속 받는다.
conosole.log(circle1.getArea === circle2.getArea); //true
```

>radius 프로퍼티(상태)만 개별 소유하고 동일한 메서드는 Circle.porototype으로 부터 상속받아 공유하고 있다.

<br>

# **19.3  프로토타입 객체**

<br>

## **19.3.1  __ proto __ 접근자 프로퍼티**
접근자 프로퍼티는 값을 호출(getter함수), 값 저장(setter 함수) [[GET]],[[SET]] 프로퍼티 어트리뷰트로 구성된 프로퍼티다.

<br>

### ```__prorto__는 접근자 프로퍼티다.```

```js
const obj = {};
const prrent = {x:1};

obj.__proto__ = parent;

//proto 접근자를 통해 paraent 객체 프로퍼티를 obj객체가 상속받았다.
console.log(obj.x); // 1
```

<br>

### ```__prorto__ 접근자 프로퍼티는 상속을 통해 사용된다.```
__ prorto __ 접근자 프로퍼티는 객체가 직접 소유한 프로퍼티가 아니라 Object.prototype의 프로퍼티다. 모든 객체는 상속을 통해 사용할 수 있다.

<br>

### ```__prorto__ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유```

접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.
순환 참조가 되면 프로토타입체인의 종점이 없기 때문에 무한루프를 방지 하는 것이다.

```js
const parent = {};
const child = {};

child.__proto__ = parent;
parent.__proto__ = child; // TypeError
```

<br>

### ```__prorto__ 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.```
모든 객체가 __ proto __ 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문에 Object.getPrototypeOf, Object.setPrototypeOf 메서드 사용을 권장한다.

```js
const obj = {};
const parent = {x:1};

//obj.__proto__;
Object.getPrototypeOf(obj);

//obj.__proto__ = parent;
Object.setPrototypeOf(obj, parent);

console.log(obj.x); //1
```

<br>

## **19.3.2  함수 객체의 prototype 프로퍼티**
함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

<br>

## **19.3.3  프로토타입의 constructor 프로퍼티와 생성자 함수**
모든 프로토타입은 constructor 프로퍼티를 가지며 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.(생성자 함수가 생성될때 연결이 이루어진다.)

<br>

# **19.4  리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입**
리터럴 표기법에 의해 생성된 객체는 프로토타입 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 함수라 단정할 수 없다.

<br>

```js
const obj = {};
//Object 생성자 함수이다.(constructor 프로퍼티로 연결 되어있다.)
console.log(obj.constructor === Object) //ture
```

Object 생성자함수에 인수를 전달하지 않거나 undefined, null을 인수로 전달하면서 호출하면 추상연산을 호출하여 Object.prototype을 프토로타입으로 갖는 빈객체를 생성한다.



