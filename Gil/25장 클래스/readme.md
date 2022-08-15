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