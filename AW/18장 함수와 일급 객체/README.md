# 18장 함수와 일급 객체

---

## 18.1 일 급 객 체
> 일급객체(First-class Object)란 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체를 가리킨다.

1. 변수에 할당(assignment)할 수 있다.
2. 다른 함수를 인자(argument)로 전달 받는다.
3. 다른 함수의 결과로서 리턴될 수 있다.

## 18.2 함수 객체의 프로퍼티

- Object.gOPD로 까보면`arguments`,`caller`,`length`,`name`,`prototype`,`__proto__`가 있음
- `__proto__`는 접근자 프로퍼티 (`Object.prototype`으로부터 상속)
> `arguments` 
> - 인수들의 정보를 담고있는 배열 객체이다
> - 특히, 초과된 인수들을 찾을 수 있는 상자임 (암묵적으로 모든 인수가 보관됨)
> - 가변 인자 함수에서 인자값 찾기에 유용함
> - 실제 배열이 아닌 유사 배열 객체 (forEach, map, filter, reduce 이런거 못씀)

> `caller`
> - 비표준 프로퍼티
> - 함수 자신을 호출한 함수가 출력됨

> `length`
> - 선언한 매개변수의 개수
> ```js
> function plus (x,y) {
>     return x * y ;
> }
> console.log(plus.length); // 2
> ```

> `name`
> - 함수 이름
> - 익명 함수 표현식일때 ES5에선 빈 문자열을 가지고 ES6는 함수 객체를 가리키는 식별자를 값으로 가짐

> `__proto__`
> - 모든 객체가 가지고있는 `[[Prototype]]` 내부 슬롯을 접근하기 위해 사용하는 접근자 프로퍼티
> ```js
> const obj = { a: 1};
> // 객체 리터럴으로 생성한 객체의 프로토 타입 객체는 Object.prototype 이다.
> console.log(obj.__proto__ === Object.prototype); // TRUE
> ```
 
> `prototype`
> - 생성자 함수로 호출 할수 없는 함수 객체만 소유하는 프로퍼티 (constructor)
> - 함수가 생성자 함수로 호출될때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴
***

# 끝