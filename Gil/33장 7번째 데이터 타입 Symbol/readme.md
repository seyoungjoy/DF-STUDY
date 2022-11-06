<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter33.%207%EB%B2%88%EC%A7%B8%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%ED%83%80%EC%9E%85%20Symbol&fontSize=40" width="100%" alt="">


# **33.1 심벌이란?**
변경 불가능한 원시 타입의 값으로, 유일한 프로퍼티 키를 만들기 위해 사용한다.

<br>

# **33.2 심벌 값의 생성**

<br>

## **33.2.1 Symbol 함수**
Symbol 함수를 호출하여 생성 해야하며 **다른 값과 절대 중복되지 않는 유일무이한 값이다.**

```js
const mySymbol = Symbol();

//외부로 노출이 되지 않아 확인할 수 없다.
console.log(mySymbol); //Symbol()

//new 연산자와 함께 쓰면 타입 에러가 발생한다.(new 연산자 사용 불가능)
new Symbol(); //TypeError

//인수로 전달된 값은 심벌 값에 대한 설명으로 디버깅 용도로만 사용된다.
const mySymbol1 = Symbol('mySymbol');
const mySymbol2 = Symbol('mySymbol');

console.log(mySymbol1 === mySymbol2); //false(유일무이한 값이다.)
```

심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.<br>
심벌값은 문자열이나 숫자 타입으로 암묵적 타입 변환이 되지 않으나 불리언 타입으로는 변환된다.

```js
const mySymbol = Symbol('mySymbol');

//래퍼객체를 생성한다.
console.log(mySymbol.description); //mySymbol
console.log(mySymbol.toString()); //Symbol(mySymbol)

//문자열, 숫자로 암묵적 타입변환 X
console.log(mySymbol + ''); //TypeError
console.log(!!mySymbol); //true
```

<br>

## **33.2.2 Symbol.for/ Symbol.keyFor 메서드**
Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

* (성공) 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환
* (실패) 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

Symbol.keyFor 메서드는 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```js
//전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for('mySymbol');
//전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // 'mySymbol'

//Symbol 함수를 호출 하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol('foo');
Symbol.keyFor(s2);// undefined
```

<br>

# **33.3 심벌과 상수**

```js
//중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
    UP: Symbol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right')
};

const myDirection = Direction.UP;

if(myDirection === Direction.UP){
    console.log('You are going Up.');
}
```

## **enum**
숫자 상수의 집합으로 열거형이라고 부른다. 자바스크립트에서 enum을 흉내내어 사용하려면 다음과 같이 객체의 변경을 방지하기 위해 객체를 동결하는 Object.freeze메서드와 심벌 값을 사용한다.

```js
const Direction = Object.freeze({
    UP: Symbol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right')
});

const myDirection = Direction.UP;
if(myDirection === Direction.UP){
    console.log('You are going Up.');
}
```

<br>

# **33.4 심벌과 프로퍼티키**
심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.
```js
//대괄호를 사용하여 생성한다. 
const obj = {
    //심벌 값으로 프로퍼티 키를 생성
    [Symbol.for('mySymbol')]: 1  
};
```

<br>

# **33.5 심벌과 프로퍼티 은닉**
심벌값을 프로퍼티키로 사용하여 생성한 프로퍼티는 for...in 문이나 object.keys, object.getOwnPropertyNames 메서드로 찾을 수 없다. 하지만 getOwnPropertySymbols메서드를 사용하면 찾을수 있다.

```js
const obj = {
    //심벌 값으로 프로퍼티 키를 생성 
    [Symbol('mySymbol')] : 1
};

//인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(obj));
//심벌 값도 찾을 수 있다.
const sybolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[sybolKey1]); // 1
```

<br>

# **33.6 심벌과 표준 빌트인 객체 확장**
심벌값으로 표준빌트인 객체를 확장하면 표준 빌트인 객체의 기존 프로퍼티 키와 충돌 하지 않는다.

```js
Array.prototype[Symbol.for('sum')] = function(){
    return this.reduce((acc, cur)=> acc + cur, 0);
}

[1,2][Symbol.for('sum')](); // 3
```

<br>

# **33.7 Well-known Symbol**
자바스크립트가 기본 제공하는 빌트인 심벌값을 Well-known Symbol이라 부른다. 자바스크립트 엔진의 내부 알고리즘에 사용된다.

이터러블은 symbol.iterator를 키로 갖는 메서드를 가지며 호출하면 이터레이터를 반환한다.(이터레이션 프로토콜 준수)