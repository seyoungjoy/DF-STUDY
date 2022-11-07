<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter34.%20%EC%9D%B4%ED%84%B0%EB%9F%AC%EB%B8%94&fontSize=50" width="100%" alt="">


# **34.1 이터레이션 프토토콜**
순회가능한 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다. 이터레이션 프로토콜에는 **이터러블 프로토콜**과, **이터레이터 프로토콜**이 있다.

<br>

```이터러블 프로토콜```<br><br>
1. Symbol.iterator를 프로퍼티키로 사용한 메서드를 직접 구현
2. 프로토타입 체인을 통해 상속 받은 Symbol.iterator메서드를 호출하면 이터레이터를 반환한다.

 >* for...of 문으로 순회할 수 있다.
 >* 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.

<br>

```이터레이터 프로토콜```<br><br>
이터러블의 Symbol.iterator메서드를 호출하면 이터레이터를 반환한다.

>* next메서드를 소유
>* next메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환
>* 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

<br>

# **34.1.1 이터러블**
이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.

* Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현한 객체
* 프로토토타입 체인을 통해 상속받은 객체

```js
const isIteravble = v => v !== null && typeof v[Symbol.iterator] === 'function';

//배열, 문자열, Map, Set 등은 이터러블이다.
isIteravble([]); //true
isIteravble(''); //true
isIteravble(new Map()); //true
isIteravble(new Set()); //true
isIteravble({}); //false
```

2021년 1월 TC39 프로세스에서는 일반객체에 스프레드 문법의 사용을 허용한다. 이외에 일반객체도 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 된다.

```js
const obj = {a:1, b:2};

console.log({...obj}); //[a:1, b:2]
```

<br>

# **34.1.2 이터레이터**
이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.
next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회결과를 나타내는 이터레이터 리절트 객체를 반환한다.
>value 프로퍼티는 현재 순회중인 이터러블의 값을 나타내며 done 프로퍼티는 이터러블의 순회 완료 여부를 나타낸다.

```js
const array = [1,2,3];

const iterator = array[Symbol.iterator]();

console.log(iterator.next()); // {value:1,done:false}
console.log(iterator.next()); // {value:2,done:false}
console.log(iterator.next()); // {value:3,done:false}
console.log(iterator.next()); // {value:undefined,done:true}
```

<br>

# **34.2 빌트인 이터러블**

<br>

# **34.3 for...of 문**

<br>

# **34.4 이터러블과 유사 배열 객체**

<br>

# **34.5 이터레이션 프로토콜의 필요성**

<br>

# **34.6 사용자 정의 이터러블**