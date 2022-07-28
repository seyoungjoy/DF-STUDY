# 24장 클로저
```jsx
const x= 1;
function outerFunc(){
		const x = 10;
		function innerFunc(){
				console.log(x);
    }
		innerFunc();
}

outerFunc();
```
- outerFunc 함수 내부에 innerFunc가 중첩함수로 정의되고 호출되었다.
- 이때 innerFunc함수의 상위 스코프는 outerFunc가 되며 outerFunc에 있는 변수 x 에 접근이 가능하다.
 

```jsx
const x= 1;
function outerFunc(){
		const x = 10;
		innerFunc();
}
function innerFunc(){
		console.log(x);
}

outerFunc();
```
- innerFunc함수가 outerfunc내에 정의된 중첩 함수가 아니라면 outerFUnc함수 내부에서 innerFunc를 호출하더라도
- outerfuncs 내에 있는 x 변수에는 접근할 수 없다.
- 이는 자바스크립트가 렉시컬 스코프를 따르는 프로그래밍 언어이기 때문에 함수는 자신이 정의된 스코프를 기준으로 상위 스코를 정하게 된다.
- 
## 24.1 렉시컬 스코프
- 렉시컬 환경이 만들어질 때 자신의 상위 스코프를 정하게 되고, 식별자를 참조할 때 자신의 렉시컬 환경에 검색이 안되면
- 참조해두었던 상위 렉시컬 환경으로 들어가 식별자를 검색한다.
- 이것이 렉시컬 스코프이다.

## 24.2 함수 객체의 내부 슬롯 `[[Environment]]`
- 함수는 상위 스코프를 기억하기 위해 내부 슬롯인 `[[Environment]]` 에 저장한다.
- 상위 스코프에 대한 참조는 평가되는 시점을 기준으로 정해진다.

## 24.3 클로저와 렉시컬 환경
```jsx
const innerFunc = outer();
innerFunc() //10
```

- ourter 함수는 innerFunc에 값을 반환하고 생명주기를 마감한다.
- 그런데 innerFunc를 실행했을 때 이미 생명주기를 마감한 outer 함수의 지역 변수를 잘 가져오는 것을 볼 수 있다.
- 이렇게 중첩함수가 더 오래 유지되는 경우 생명주기를 마감한 외부 함수의 변수를 참조할때
- 이러한 중첩함수를 클로저라고 부른다.

- outer 함수는 실행컨텍스트에서는 제거되지만 렉시컬 환경까지 사라진것은 아니다.
- innerFunc가 참조하고 있기 때문에 가비지컬렉션이 제거대상으로 보지 않기 때문.

- 모든 함수는 자신의 상위 스코프를 기억하기 때문에 클로저라고 볼수있나?
- 그것은 아니다.
- 상위 스코프의 식별자를 참조하지 않는다.
- 외부함수보다 중첩 함수가 더 빨리 소멸된다.

즉 클로저는!!
- 상위 스코프의 식별자를 참조하면서
- 외부함수보다 더 오래 살아남는 함수만을 클로저라고 부른다!
- 특징 : 참조하고 있는 상위 스코프의 데이터만 기억한다.
- 그럼 대체 이런걸 왜 사용하는거지?

## 24.4 클로저의 활용
```jsx
const increase = (function(){
		let num = 0;
		//클로저
		return function(){
				return num = num+1;
		}
}());

console.log(increase());

console.log(increase());
```
- 이렇게 increase 함수를 만들게 되면 외부에서 num 상태값을 함부로 변경할수도 없고
- increase 함수만이 상태 변경을 할 수 있도록 만들 수 있다.

- 클로저는 상태의 의닉과 특정 함수에게만 상태변경을 허용할 때 사용한다.


## 24.5 캡슐화와 정보 은닉
- 캡슐화 : 객체의 상태나 프로퍼티를 묶어서 외부에서 변경할 수 없도록 만드는 것.
