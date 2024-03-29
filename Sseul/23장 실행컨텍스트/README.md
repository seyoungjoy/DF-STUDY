# 23. 실행 컨텍스트

## 23.1 소스코드의 타입
4가지 타입의 소스코드는 실행 컨텍스트를 실행한다.

  1. 전역코드
  2. 함수코드
  3. eval 코드
  4. 모듈코드
<br><br> 

## 23.2 소스코드의 평가와 실행
**소스코드 평가** : 실행 컨텍스트를 생성하고 변수, 함수등의 선언문만 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프에 등록한다.<br>
**소스코드 실행** : 평가가 끝나면 선언문을 제외한 코드가 순차적으로 실행된다. 이때 실행에 필요한 정보, 즉 변수나 함수의 참조를 시랭 컨텍스트가 관리하는 스코프에서 검색해서 취득한다. 
<br><br>

## 23.3 실행 컨텍스트의 역할
1. 전역 코드 평가
2. 전역 코드 실행
3. 함수 코드 평가
4. 함수 코드 실행
<br><br>

## 23.4 실행 컨텍스트 스택

```javascript
const x = 1;

function foo () {
  const y = 2;
  
  function bar () {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```
1. 전역 코드의 평가와 실행 : 전역 코드를 평가하고 실행컨텍스트를 생성해서 실행컨텍스트 스택에 푸시한다. 전역 변수 x와 전역 함수 foo는 전역 실행 컨텍스트에 등록된다. 
2. foo 함수 코드의 평가와 실행 : foo가 호출되면 전역 코드의 실행은 일시 중단되고 코드의 제어관이 foo 함수 내부로 이동한다. foo 함수 실행 컨텍스트를 실행하고 실행 컨텍스트 스택에 푸시한다.
3. bar 함수 코드의 평가와 실행 : bar가 호출되면 foo 함수 코드의 실행은 일시 중단되고 코드는 bar 함수를 평가하여 bar 함수 실행컨텍스트를 생성하고 스택에 푸시한다. 이때 bar 함수의 지역변수 z가 함수 실행 컨텍스트에 등록된다. console.log 메서드를 호출한 이후 bar 함수는 종료된다. 
4. foo 함수 코드로 복귀 : bar 함수가 종료되면 foo 함수로 이동하고 bar 함수 실행 컨텍스트를 스택에서 제거한다. 그리고 foo 함수는 종료된다.
5. 전역 코드로 복귀 : foo 함수가 종료되면 전역코드로 이동하고 foo 함수를 스택에서 제거한다. 그리고 더 이상 실행할 전역 코드가 남아 있지 않으므로 전역 실행 컨텍스트도 스택에서 제거한다.
<br><br>

## 23.5 렉시컬 환경
렉시컬 환경은 식별자와 식별자에 바인딩된 값, 그리고 상위 스코프에 대한 참조를 기록하는 자료구조로 실행 컨텍스트를 구성하는 컴포넌트다. 실행 컨텍스트 스택이 코드의 실행 순서를 관리한다면 렉시컬 환경은 스코프와 식별자를 관리한다.<br>

렉시컬 환경은 두 개의 컴포넌트로 구성된다.
 
1. 환경 레코드 : 스코프에 포함된 식별자를 등록하고 등록된 식별자에 바인딩된 값을 관리하는 저장소
2. 외부 렉시컬 환경에 대한 참조 : 상위 스코프를 가리킨다. 상위 스코프는 상위 코드의 렉시컬 환경을 말한다.
<br><br>

## 23.6 실행 컨텍스트 생성과 식별자 검색 과정

```javascript
var x = 1;
const y = 2;

function foo(a) {
  var x = 3;
  const y = 4;

  function bar(b) {
    const z = 5;
    console.log(a + b + x + y + z);
  }
  bar(10);
}

foo(20); //42
```
1. 전역 객체 생성
2. 전역 코드 평가
3. 전역 코드 실행
4. foo 함수 코드 평가
5. foo 함수 코드 실행
6. bar 함수 코드 평가
7. bar 함수 코드 실행
8. bar 함수 코드 실행 종료
9. foo 함수 코드 실행 종료
10. 전역 코드 실행 종료
<br><br>

## 23.7 실행 컨텍스트와 블록 레벨 스코프
```javascript
let x = 1;

if (true) {
  let x = 10;
  console.log(x); //10
}

console.log(x); //1
```
- 위 코드에서 if 문에 let 키워드로 변수가 선언되었기 때문에, 코드 블록이 실행되면 if 문의 코드 블록을 위한 블록 레벨 스코프를 생성해야 한다.
- 이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존 전역 렉시컬 환경을 교체한다.
- 새롭게 생성된 if 문의 코드 블록을 위한 렉시컬 환경의 외부 렉시컬 환경에 대한 참조는 if 문이 실행되기 이전 전역 렉시컬 환경을 가리킨다.
- for 문의 변수 선언문에 let 키워드를 사용한 for문은 코드 블록이 반복해서 실행될 때마다 코드 블록을 위한 새로운 렉시컬 환경을 생성한다.
