# 17장 생성자 함수에 의한 객체 생성

---

## 17.1 Object 생성자 함수
- `new` 연산자로 `Object()` 호출해서 객체만드는 것
```js
const person = new Object();

person.name = "lee";
person.sayHello = function () {
    console.log('Hi Im' + this.name)
};

console.log(person);
person.sayHello();

const strObj = new String('lee');
const numObj = new Number('lee');
const boolObj = new Boolean(true);
...

```
> Object 생성자 함수를 사용해 생성하는 방식은 특별한 이유가 없다면 그다지 유용해 보이지 않는다.
- ??ㅋㅋㅋㅋㅋ

## 17.2 생성자 함수
- 동일한 구조의 객체를 여러개 생성할 때 사용
- 생성자 함수 내부에서 return 문을 반드시 생략해야함 (`this` 훼손)
- 내부 메서드
  - `[[Call]]` : 일반 함수로서 호출될때 Call 메서드가 호출됨
  - `[[Construct]]` : 생성자 함수로 호출될때 Construct가 호출됨
- Constructor 구분
  - constructor : 함수 선언문, 함수 표현식, 클래스
  - non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수
- `new` 연산자 : 객체를 반환하지 않으면 반환문이 무시됨 (빈 객체 생성)

***
# 끝