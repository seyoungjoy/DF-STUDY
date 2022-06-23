# 14장 전역 변수의 문제점
## 14.1 변수의 생명주기
```jsx
function foo(){
	 var x = 'local';
	 console.log(x);
	 return x;
}

foo();
console.log(x);
```
### 14.1.1 지역 변수의 생명 주기
- 지역 변수의 생명 주기는 함수의 생명 주기와 일치한다.
- 호이스팅도 스코프 단위로 동작한다.
```jsx
var x = 'global';

function foo(){
	 console.log(x);
	 var x = 'local';
}

foo();
console.log(x);
```

### 14.1.2 전역 변수의 생명 주기
- var 키워드로 선언한 전역 변수의 생명 주기는 전역 객체의 생명 주기와 일치한다.

## 14.2 전역 변수의 문제점
- 암묵적 결합 : 모든 코드가 전역 변수를 참조하고 변경할 수 있다.
- 긴 생명주기 : 메모리 리소스를 오랜 기간 보시한다.
- 스코프 체인 상에서 종점에 존재 : 스코프 체인에서 가장 마지막에 검색되기 때문에 검색 속도가 가장 느리다.
- 네임스페이스 오염 : 전역 스코프를 공유하기 때문에 같은 이름의 변수가 존재할 수 있다.

## 14.3 전역 변수의 사용을 억제하는 방법
- 전역 변수의 무분별한 사용을 위험하기 때문에 필요한 상황이 아니라면 지역 변수 사용을 추천.
- 변수의 스코프는 좁을수록 좋다.
### 14.3.1 즉시 실행 함수
- 모든 코드를 즉시 실행 함수로 감싸면 모든 변수는 즉시 실행 함수의 지역 변수가 된다.
```jsx
(function(){
	 var foo = 10;
}());
console.log(foo);
```

### 14.3.2 네임스페이스 객채
- 유용 X

### 14.3.3 모듈 패턴
- 즉시 실행함수로 감싸 하나의 모듈을 만들어준다.
- 전역 변수의 억제 + 캡슐화 구현 가능.
- 캡슐화 : 프로퍼티와 메서드를 하나로 묶는 것.
```jsx
var Conunter  = (function(){
	 // private 변수
	 var num = 0;
	 // 외부로 공개한 데이터나 메서드를 반환.
	 return{
		  increase(){
				return ++num;
          },
         decrease(){
				return --num;
         }
     }
}())

console.log(Conunter.num); // undefined
console.log(Conunter.increase());
console.log(Conunter.increase());
console.log(Conunter.decrease());
console.log(Conunter.decrease());


```
### 14.3.4 ES6 모듈
- ES6 모듈은 파일 자체의 독자적인 모듈 스코프를 제공
```html
<script type="module" src="lib.mjs"></script>
<script type="module" src="app.mjs"></script>
```
