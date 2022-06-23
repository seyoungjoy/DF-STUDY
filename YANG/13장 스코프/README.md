# 13장 스코프
## 13.1 스코프란?
- 식별자가 유효한 범위
- 식별자를 검색할 때 사용하는 규칙
- 스코프 내에서 식별자는 유일해야 하지만 다른 스코프에는 같은 이름의 식별자를 사용할 수 있다. 
```jsx
var x = 'global';

function foo(){
	 var x = 'local';
	 console.log(x); //(1)
}
foo();

console.log(x); //(2)
```

## 13.2 스코프의 종류
```jsx
var x = 'global x';
var y = 'global y';

function outer(){
	 var z = "outer's local z";
	 
	 console.log(x);
	 console.log(y);
	 console.log(z);
	 
	 function inner(){
		  var x = "inner's local x";
		  
		  console.log(x);
		  console.log(y);
		  console.log(z);
     }
	  inner();
}

outer();

console.log(x);
console.log(z);
```
### 13.2.1 전역과 전역 스코프
- 전역에 변수를 선언하면 어디서든지 참조할 수 있다.

### 13.2.2 지역과 지역 스코프
- 함수 몸체 내부에 선언한 변수는 지역 스코프를 가지게된다.
- 지역 변수는 자신의 지역 스코프와 하위 지연 스코프에서 유요하다.

## 13.3 스코프 체인
- 스코프가 계층적으로 연결된 것
- 변수를 참조할 때 자바스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여
- 상위 스코프 방향으로 이동하며 선언된 변수를 검색한다.
### 13.3.1 스코프 체인에 의한 변수 검색
- 상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조할 수 있지만
- 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없다.
### 13.3.2 스코프 체인에 의한 함수 검색
```jsx
function foo(){
	 console.log('global function foo')
}
function bar(){
	 function foo(){
		  console.log('local function foo')
     }
	 foo();
}
bar();
```

## 13.4 함수 레벨 스코프
- 블록 레벨 스코프 : 함수 몸체만이 아니라 모든 코드 블록 (if, for, while)이 지역 스포크를 만드는 것.
- 함수 레벨 스코프 : 함수의 코드 블록만을 지역 스코프로 인정 (var)
```jsx
var x = 1;

if(true){
	 var x = 10;
}
console.log(x); //10
```
## 13.5 렉시컬 스코프(정적 스코프)
- 렉시컬 스코프 : 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정된다.
- 동적 스코프 : 함수가 호출되는 시점에서 상위 스코프가 결정됨.

```jsx
var x = 1;
function foo(){
	 var x = 10;
	 bar();
}

function bar(){
	 console.log(x);
}

foo(); //1
bar(); //1
```
