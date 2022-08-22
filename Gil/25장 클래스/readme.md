<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter25.%20%ED%81%B4%EB%9E%98%EC%8A%A4&fontSize=50" alt="">

# **25.1 클래스는 프로토타입의 문법적 설탕인가?**
> **클래스는 새로운 객체 생성 메커니즘이다.**
* 클래스를 new 연산자 없이 호출하면 에러가 발생한다.
* 클래스는 호이스팅이 발생하지 않는 것처럼 동작한다.
* 클래스 내의 모든 코드에는 암묵적으로 strict mod(엄격모드)가 지정되어 실행되며 해제할 수 없다.
* 클래스의 constructor, 프로토타입 메서드, 정적 메서든 모두 [[Enumerable]]은 false이다.(배열처럼 열거 되지 않음)

<br>

# **25.2 클래스 정의**

```js
//클래스 선언문 파스칼케이스로 작성한다(단어시작 알파벳이 대문자)
class PersonGil{}

//익명 클래스 표현식
const PersonGil = class{};

//기명 클래스 표현식
const PersonGil = class Gil{};
```

클래스는 위의 코드와 같이 정의 할수 있으며 일급객체의 특징을 갖는다.

* 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
* 변수나 자료구조(객체,배열)에 저장 가능
* 함수의 매개변수에 전달 가능
* 함수의 반환값으로 사용 가능

<br>

```js
class Person{
    constructor(name){
        this.name = name;
    }
    
    gil(){
        console.log(`Hi im ${this.name}!`);
    }
    
    static sayHi(){
        console.log('Hi~!');
    }
}

const who = new Person('gil');

console.log(who.name); //gil
who.gil(); //Hi im gil!
Person.sayHi(); //Hi~!
```

클래스 몸체에는 0개 이상의 메서드만 정의 할 수 있고, constructor(생성자), 프로토타입 메서드, 정적 메서드 3가지가 정의 될수 있다. 

<br>

# **25.3 클래스 호이스팅**
클래스 선언문은 함수 선언문처럼 같이 소스평가 이전에 평가 되어 함수객체와 프로토타입을 생성한다.<br>
하지만 선언 이전에 참조 되지 않는다.
```js
//ReferenceError
console.log(Gil);
class Gil {}
```

<br>

이처럼 클래스는 호이스팅이 발생 하지 않는것 처럼 보이지만 let이나 const 키워드 처럼 일시적 사각 지대에 빠지기 때문이다.
```js
const Gil = '';
{
    console.log(Gil); //''가 아닌 ReferenceError가 발생한다.
    class Gil {}
}
```

<br>

# **25.4 인스턴스 생성**
함수는 new연산자를 사용하지않으면 일반함수로 호출이 되지만 클래스는 new 연산자를 반드시 사용해야 한다.

```js
const Person = class Gil{}
const me = new Person;

//Gil은 함수표현식과 마찬가지로 클래스 내에서만 유효한 식별자이다. 
console.log(Gil);//ReferenceError
console.log(me);//Gil{}
```

<br>

# **25.5 메서드**
## **25.5.1 constructor**
constructor는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드이다.(이름변경 불가)<br>
클래스 정의가 평가되면 constructor의 기술된 동작을 하는 함수 객체가 생성된다.

```js
class Gil{
    //2개 이상의 constructor가 존재하면 SyntaxError가 발생한다. 즉, 최대 1개만 존재 가능하다.
    constructor(){}
    constructor(){}
}
```

<br>

constructor를 생략하면 빈 constructor가 암묵적으로 정의된다. 이때 빈객체를 생성한다.
그리고 반환문(return)을 사용 하면 this 반환이 안되므로 사용하지 않는 것이 좋다. 

```js
class Gil{
    constructor(name){
        this.name = name;
        return{}; //반환문
    }
}

const me = new Gil('gil');
console.log(me) //{} 빈객체가 반환된다.
```

<br>

하지만 원시값을 반환하면 이값은 무시되고 this가 반환된다.

```js
class Gil{
    constructor(name){
        this.name = name;
        return 1; //원시값은 무시
    }
}
const me = new Gil('gil');
console.log(me);//Gil {name:'gil'}
```

> 따라서 constructor 안에서는 return문을 생략 하길 바란다! 

<br>

## **25.5.2 프로토타입 메서드**
클래스 몸체에서 정의한 메서드는 클래스의 prototype 프로퍼티에 메서드를 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

```js
class Gil{
    //생성자
    constructor(name){
        //인스턴스 생성 및 초기화
        this.name = name;
    }

    //프로토타입 메서드
    sayHi(){
        console.log(`Hi! My name is ${this.name}`);
    }
}

const me = new Gil('gil');
me.sayHi(); //Hi! My name is gil
```

<br>

## **25.5.3 정적 메서드**
클래스에서 메서드에 static 키워드를 사용하면 정적 메서드(클래스 메서드)가 된다.
```js
class Gil{
    constructor(name){
        //인스턴스 생성 및 초기화
        this.name = name;
    }
    //정적 메서트
    static sayHi(){
        console.log('hi');
    }
} 
```

<br>

## **25.5.4 정적 메서드와 프로토타입 메서드의 차이**

<br>

1. 정적메서드와 프로토타입메서드는 자신이 속해있는 프로토타입 체인이 다름.
2. 정적메서드는 클래스로 호출, 프로토타입메서더는 인스턴스로 호출
3. 정적메서드는 인스턴스 프로퍼티 참조 X, 프로토타입메서드는 인스턴스 프로퍼티 참조 O


```js
class Square{
    constructor(width,height){
        this.width = width;
        this.height = height;
    }
    
    //정적메서드
    static area2(width,height){
        return width*height;
    }
    
    //프로토타입 메서드    
    area(){
        return this.width * this.height;
    }
}

console.log(Square.area2(10,10));

const gil = new Square(2,5);
console.log(gil.area());
```

> 인스턴스 프로퍼티를 참조 하고 싶을땐 this를 사용하고 프로토타입 메서드를 이용한다, 인스턴스 프로퍼티를 참조할 필요가 없다면 정적 메서드를 사용한다.

📌 정적메서드를 사용하면 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용하다.

<br>

## **25.5.5 클래스에서 정의한 메서드의 특징**

* function 키워드를 생략한 축약표현을 사용한다.
* 메서드를 정의할 때는 콤마가 필요 없다.
* 암묵적으로 strict mode로 실행
* 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 false이다.(열거불가능)
* new 연산자와 함께 호출할 수 없다.

<br>

# **25.6 클래스의 인스턴스 생성 과정**

<br>

```js
class Gil{
    //생성자
    constructor(name){
        //1. 암묵적으로 인스턴스가 생성, this 바인딩
        console.log(this); // Gil{}
        console.log(Object.getPrototypeOf(this) === Gil.prototype); // true
        
        //2. this에 바인딩되어 있는 인스턴스를 초기화
        this.name = name;
        
        //3. 완성된 인스턴스가 바인딩된 this가 암묵적 반환
    }
}
```

<br>

## ```1. 인스턴스 생성과 this 바인딩```
* (constructor 실행전) 암묵적으로 빈객체 생성
* 클래스가 생성한 인스턴스의 프로토타입으로 클래스의 prototype 프로퍼티가 가리키는 객체 설정
* 빈객체에 인스턴스 this 바인딩 <br>
> 📌 constructor 내부의 this는 클래스가 생선한 인스턴스를 카리킨다
 
<br>

## ```2. 인스턴스 초기화```
* this에 바인딩된 인스턴스에 프로퍼티를 추가
* constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티값을 초기화

<br>

## ```3. 인스턴스 반환```
클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.

<br>

# **25.7 프로퍼티**

<br>

# **25.7.1 인스턴스 프로퍼티**
인스턴스 프로퍼티는 constructor 내부에서 정의함으로써 클래스가 암묵적으로 생성한 빈객체(인스턴스)에 프로퍼티가 추가되어 초기화 된다.

<br>

```js
class Gil{
    constructor(name){
        //인스턴스 프로퍼티
        this.name = name; //name 프로퍼티는 public
    }
}
//name은 public
const me = new Gil('gil');
console.log(me.name);//gil
```

<br>

# **25.7.2 접근자 프로퍼티**
접근자 프로퍼티는 자체적인 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장 할때 사용하는 접근자 함수(getter,setter)로 구성된 프로퍼티다.

```js
class Gil{
    constructor(firstName, lastName){
        this.firstName = firstName;
        this.lastName = lastName;
    }
    
    //fullName은 접근자 프로퍼티이다.(get,set)
    //getter함수
    get fullName(){
        return `${this.firstName} ${this.lastName}`;
    }
    
    //setter함수
    set fullName(name){
        [this.firstName, this.lastName] = name.split('');
    }
}

const me = new Gil('gil','kim');

//데이터 프로퍼티를 통한 프로퍼티 값 참조
console.log(`${me.firstName} ${me.lastName}`);

//값저장 setter함수 호출
me.fullName = "jaehoon choi";
console.log(me); //{fistName:jaehoon, lastName:choi}

//값참조 getter함수 호출
console.log(me.fullName);



```

getter,setter 이름은 인스턴스 프로퍼티처럼 사용된다. getter는 프로퍼티처럼 참조하는 형식으로 사용하며, setter는 프로퍼티처럼 값을 할당하는 형식으로 사용된다.

>클래스의 메서드는 기본적으로 프로토타입 메서드가 되며 접근자 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.

<br>

# **25.7.3 클래스 필드 정의 제안**
클래스필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다.
<br><br>
**자바스크립트의 클래스 몸체에는 메서드만 선언할 수 있다.** 자바와 유사하게 클래스 필드를 선언하면(this 없이) 문법 에러가 발생한다.<br>
최신 브라우저나 최신 node.js에서 실행하면 정상 동작한다. (하지만 웬만하면 안쓰는 것이 좋을것 같다...)
```js
class Person{
    //클래스 필드 정의
    //최신 브라우저, node에서는 정상 동작
    name = "Lee";
}
const me = new Person();
console.log(me);//Person{name:"Lee"}
```

<br>

클래스 몸체에서 클래스 필드를 정의할때 초기화가 필요할땐 constructor 안에서 초기화 해야하며 이때는 this를 사용해야한다.<br><br>
따라서 초기화가 필요하면 클래스몸체에서 클래스필드를 정의할 필요가 없고, constructor 안에서 this를 이용해서 초기화를 할때 생선한 인스턴스에 클래스필드에 해당하는 프로퍼티가 없다면 자동 추가 된다.
```js
class Person{
    constructor(name){
        //클래스 필드 초기화
        //생성된 인스턴스에 프로퍼티가 없다면 자동 추가
        this.name = name;
    }
    
    //클래스필드에 문자열 할당
    age = '17';
    
    //클래스 필드에 함수 할당
    getName = function(){
        return this.name;
    }
    
    //화살표 함수로 정의
    getAge = () => this.name;
}
const me = new Person('gil');
console.log(me); // Person{age: '17', name: 'gil', getName: ƒ, getAge: ƒ}
```

>함수는 일급객체 이므로 클래스필드에 할당할 수 있고, 메서드를 정의할 수도 있다.<br>이때 프로토타입 메서드가 아닌 ```인스턴스 메서드```가 된다.

📌 모든 클래스 필드는 인스턴스 프로퍼티가 되기때문에 클래스 필드에 함수를 할당하는 것은 권장 하지 않는다.

<br>

```외부 초기값으로 클래스 필드를 초기화할 필요가 있는경우``` <br>
constructor에서 인스턴스 프로퍼티를 정의 하는 방식 사용(기존 방식)

<br>

```외부 초기값으로 클래스 필드를 초기화할 필요가 없는 경우```<br>
기존 방식과 클래스 필드 정의 제안(클래스 몸체에서 변수처럼 정의 하는것) 모두 사용 가능