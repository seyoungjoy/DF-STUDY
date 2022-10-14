# 16장 프로퍼티 어트리뷰트
## 16.1 내부 슬롯과 내부 메서드
```jsx
const o = {};
// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근할 수 없다.
// o.[[Prototype]] // SyntaxError

// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공.
o.__proto__ //Object.prototype
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체
- 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
- `Object.getOwnPropertyDescriptor` 메서드를 통해 간접적으로 확인 가능하다.
```jsx
const person = {
		name:'Lee'
}

console.log(Object.getOwnPropertyDescriptor(person,'name'));
//{value: 'Lee', writable: true, enumerable: true, configurable: true}
```

## 16.3 데이터 프로퍼티와 접근자 프로퍼티
- 데이터 프로퍼티 : 키와 값으로 구성된 일반적인 프로퍼티. value, writable, enumerable, configuable이 있다.
- 접근자 프로퍼티 : 데이터 프로퍼티의 값을 읽거나 저장할 때 관여하는 getter/setter 함수
```jsx
const person = {
    firstName:'Ungmo',
    lastName:'Lee',
    // getter 함수
    get fullName(){
				return `${this.firstName} ${this.lastNAme}`;
    },
    // setter 함수
    set fullName(name){
				[this.firstName, this.lastName] = name.split(' ');
    }
}
```

## 16.4 프로퍼티 정의
- `Object.defineProperty` 메서드를 통해 프로퍼티의 어트리뷰트를 정의할 수 있다.

## 16.5 객체 변경 방지
- 객체의 변경을 방지하는 세가지 메서드를 제공한다.

1. 객체 확장 금지 : `Object.preventExtensions` -> 프로퍼티 추가 금지
2. 객체 밀봉 : `Object.seal`-> 프로퍼티 추가, 삭제, 재정의 금지
3. 객체 동결 : `Object.freeze` -> 읽기만 가능ㅇ해짐.
