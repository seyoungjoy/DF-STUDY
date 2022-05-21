# 10장 객체 리터럴
## 10.1 객체란?
- 객체는 프로퍼티와 메서드로 구성된 집합체
  - 프로퍼티 : 객체의 상태를 나타내는 data
  - 메서드 : Data를 참조하거나 조작할 수 있는 동작

## 10.2 객체 리터럴에 의한 객체 생성
- 객체 생성 방법
  - 객체 리터럴
  - Object 생성자 함수
  - 생성자 함수
  - Object.create 메서드
  - 클래스(ES6)
- 객체 리터럴로 생성하는 방법은 자바스크립트의 유연함과 강력함을 보여준다.
- 객체를 생성하기 위해 클래스를 정의하고, new 연산자를 호출할 필요가 없다.

## 10.3 프로퍼티
- 프로퍼티는 키와 값으로 구성된다.
- 프로퍼티 키는 문자열, 심벌값만 사용할 수 있고 값은 모든 형태의 타입의 값으로 사용 가능.

### 프로퍼티 키 특징
- 식별자 네이밍 규칙을 따르는 프로퍼티 키를 사용하자. 아니면 번거롭다.
- 식별자 네이밍 규칙을 따르지 않을 때는 따옴표로 감싸줌.
- 숫자 넣으면 문자열로 인식

## 10.4 메서드
- 메서드란? 객체의 프로퍼티 값으로 함수가 들어올 때 일반 함수와 구분하기 위해 메서드라 부름.

## 10.5 프로퍼티 접근
- 마침표 표기법(.)
- 대괄호 표기법([])
```jsx
//네이밍 규칙 준수하면 마침표, 대괄호 모두 사용 가능
var person ={
    name:'yang'
};

console.log(person.name); //yang
console.log(person['name']); //yang

person[name]; //따옴표를 생략하면 식별자로 인식해버린다.
```

```jsx
var person = {
    'last-name' : 'Lee',
  1:10
};

person.'last-name'; //X
person.last-name; //X

person["last-name"]; //Lee
person[last-name] //X

person.1;//X
person."1";//X
person[1];//O
person['1'];//O
```

### **식별자 네이밍 규칙**
- 문자, 숫자, _, $ 포함 가능
- 숫자로 시작 X
- 예약어 X

## 10.6 프로퍼티 값 갱신
```jsx
var person = {
    name:'yang'
}

person.name = 'kim';

console.log(person.name); //kim
```

## 10.7 프로퍼티 동적 생성
```jsx

var person = {
    name:'lee'
};

person.age = 20;

console.log(person); //{name:'Lee', age:20}
```

## 10.8 프로퍼티 삭제
```jsx
delete person.age;
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능
### 10.9.1 프로퍼티 축약 표현
- 변수를 프로퍼티 값으로 사용할 때 프로퍼티 키와 동일하면 프로퍼티 키는 생략이 가능.
```jsx
let x=1;
let y=2;

const obj = {x,y};

console.log(obj); //{x: 1, y: 2}
```
### 10.9.2 계산된 프로퍼티 이름
- 프로퍼티 키 동적 생성이 가능. 대괄호 사용
### 10.9.3 메서드 축약 표현
```jsx
const obj = {
    name: 'Lee',
  sayHi(){
        console.log('hi!' + this.name);
  }
}
```
