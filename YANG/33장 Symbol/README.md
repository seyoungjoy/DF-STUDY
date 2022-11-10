# 33장 Symbol
## 33.1 심벌이란?
- 자바스크립트 타입은 string ,number, boolean, undefined, null, object 6가지 존재
- Symbol은 ES6에서 도입된 7번째 데이터 타입으로 변경 불가능한 원시 타입의 값
- 다른값과 중복되지 않는 유일무이한 값이기 때문에 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용

## 33.2 심벌 값의 생성
### 33.2.1 Symbol 함수
- Symbol 함수를 호출하여 생성
```jsx
const mySymbol = Symbol();

console.log(mySymbol);
```

### 33.2.2 Symbol.for/Symbol.keyFor 메서드
- `Symbol.for` : 해당 키와 일치하는 심벌 값을 검색.
- `Symbol.keyFor` : symbol 값의 키를 추출.
```jsx
// mySymbol이라는 이름의 키 값이 없으면 새로운 심벌 값을 생성, 없으면 반환.
const s1 = Symbol.for('mySymbol');
// 전역에 저장된 해당 심벌 값의 키를 추출
Symbol.keyFor(s1); //mySymbol
```

### 33.3 심벌과 상수
- 변경, 중복되서는 안될 상수값을 심벌로 관리하면 안전하게 사용할 수 있다.
```jsx
const Direction = {
    UP:1,
    DOWN:2,
    LEFT:3,
    RIGHT:4
}

const myDirection = Direction.UP;

if(myDirection === Direction.UP){
    console.log("You are going UP")
}

// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성한다.
const Direction = {
    UP:Symbol('up'),
    DOWN:Symbol('down'),
    LEFT:Symbol('left'),
    RIGHT:Symbol('right')
}

const myDirection = Direction.UP;
if(myDirection === Direction.UP){
    console.log("You are going UP")
}
```

### 33.4 심벌과 프로퍼티 키
- 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.
```jsx
const obj = {
    [Symbol.for('mySymbol')]:1
};

console.log(obj[Symbol.for('mySymbol')]); //1
```

### 33.6 심벌과 표준 빌트인 객체 확장
- 표준 빌트인 객체를 확장하고 싶다면 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성할 수 있다.
```jsx
Array.prototype.sum = function(){
    return this.reduce((acc, cur) => acc + cur, 0);
};
[1,2].sum();

Array.prototype[Symbol.for('sum')] = function(){
    return this.reduce((acc, cur) => acc + cur, 0);
};
[1,2][Symbol.for('sum')];
```

- 중복되지 않는 상수 값을 생성
- 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해 도입되었다.