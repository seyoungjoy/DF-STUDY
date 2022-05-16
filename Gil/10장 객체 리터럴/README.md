 # **✍ <span style="color:#ef725e">10장 객체 리터럴</span>**


## **<span style="color:93cfcc">10.1  객체란?</span>**
**원시타입과 달리 변경 가능한 값이며, 상태(프로퍼티)와 동작(메서드)을 하나로 구성된 집합체이다.**
```js
var Gil = {
    name: 'KimYoungGil',  //프로퍼티
    say: function(){
        console.log('서울사람이세요?');
    }  //메서드(함수)
}
```
여기에서 콜론을 기준으로 왼쪽을 **프로퍼티키(key),** 오른쪽을 **프로퍼티값(value)** 이라고 한다.
<br/><br/> 

## **<span style="color:93cfcc">10.2  객체리터럴에 의한 객체 생성</span>**
**객체 리터럴은 객체를 생성하기위한 표기법이다.** 변수에 객체리터럴이 할당될때(=) 이를 해석하여 객체를 생성한다.
<br/><br/> 

*빈객체를 생성하는 방법
```js
var GilBrain = {} //아무런생각이없다....
```
<br/>

## **<span style="color:93cfcc">10.3 프로퍼티</span>**
프로퍼티키, 프로퍼티값으로 구성돼 있으며 식별자네이밍에 따르지 않은 프로퍼티키의 경우 **따옴표("",'')** 를 사용해야한다.
>**프로퍼티 키:** 빈 문자열을 포함하는 모든 문자열 또는 심벌값을 사용가능

>**프로퍼티값:** 자바스크립트에서 사용할 수 있는 모든 값을 사용가능

<br>

### **🤜🏻 10.3.1 동적으로 프로퍼티생성하기**
동적으로 프로퍼티를 생성할때는 키로 사용될 부분에는 **대괄호([...])** 를 사용해야한다.
```js
var Gil = {};
var key = '오늘저녁';
Gil[key] = '버거킹 와퍼'; //대괄호사용할것!

//이때 메모리에 저장돼 있는 객체의 상태는
// var Gil = {
//     오늘저녁 : '버거킹 와퍼'
//}


console.log(Gil); //{오늘저녁 : 버거킹 와퍼}
```

💦*객체에서 프로퍼티키 이름이 중복일 경우 마지막에 선언된 프로퍼티가 나오며(덮어쓰기됨) 에러가 안뜨니 주의할것!*

<br/>

## **<span style="color:93cfcc">10.4 메서드</span>**
함수는 프로퍼티 값으로 사용될수 있으며 이때 일반 함수들과 구분하기 위해서 **메서드**라고 불린다.

<br/>

## **<span style="color:93cfcc">10.5 프로퍼티 접근</span>**

프로퍼티 접근방법(호출)에는 2가지가 있으며 ```1.마침표를 이용하는 방법, 2.대괄호를 이용하는 방법```이 있다.

<br>

**식별자네이밍에 따른 프로퍼티키의 경우** 마침표를 이용하여 접근할수있으며 대괄호도 사용할 수 있다.
```js
var Gil = {
    burger : '2개 7천원'
}
console.log(Gil.burder); //2개 7천원
console.log(Gil[burder]); //2개 7천원
console.log(Gil['burder']); //2개 7천원
```
💦 *식별자네이밍에 따를 경우에만 대괄호 사용시 따옴표 생략가능.*

<br>

**식별자네이밍에 따르지 않은 프로퍼티키의 경우** 대괄호만 사용할 수 있다.

```js
var Gil = {
    'cheeze-burger' : '2개 7천원'
}
console.log(Gil.'cheeze-burger'); //SyntaxError: Unexpected string
console.log(Gil[cheeze-burger]); //ReferenceError: cheeze is not defined
console.log(Gil.['cheeze-burger']); //2개 7천원
```
💦 *예외로 프로퍼티키가 숫자일경우 대괄호안에 따옴표생략가능.*

<br>

## **<span style="color:93cfcc">10.6 프로퍼티 값 갱신</span>**

만들어진 객체를 갱신할 수 있다.

```js
 var Gil = {
     menu: '순찌'
 }

 Gil.menu = '김찌';
 console.log(Gil); // {menu:'김찌'}
```

## **<span style="color:93cfcc">10.7 프로퍼티 동적 생성</span>**

존재하는 객체에서 프로퍼티키, 프로퍼티값을 할당하면 프로퍼티를 추가할 수 있다.

```js
var Gil ={
    name: 'KimYoungGil'
}
Gil.hair = '짧다';
console.log(Gil); // {name: 'KimYoungGil', hair:'짧다'}
```

## **<span style="color:93cfcc">10.8 프로퍼티 삭제</span>**

객체안에 프로퍼티를 삭제할땐 delete 연산자를 이용하면 되는데 존재하지 않은 프로퍼티를 삭제를 하더라도 에러는 뜨지 않는다.

```js
var Gil ={
    name: 'KimYoungGil',
    character: '나쁜사람'
}
delete Gil.character;
```

## **<span style="color:93cfcc">10.9 ES6에서 추가된 객체 리터럴의 확장기능</span>**

### **🤜🏻 10.9.1 프로퍼티 축약 표현**
* 프로퍼티값은 변수로 할당 할수 있다.
* 프로퍼티값으로 사용한 변수와 프로퍼티키의 이름이 같을 경우 프로퍼티 키를 생략할 수 있다.

### **🤜🏻 10.9.2 계산된 프로퍼티 이름**
* string 값을 이용해서 프로퍼티를 동적으로 생성할 수 있다. 이때 프로퍼티키 부분에는 대괄호([...])를 사용한다.
* 객체리터럴 안에서도 string 값을 이용해 프로퍼티를 동적으로 생성할 수 있다.

### **🤜🏻 10.9.3 메서드 축약 표현**
* 메서드의 함수를 축약 해서 정의할 수 있다.
```js
//ES5
var Gil ={
    where: function(){
        console.log('서울사람이세여?')
    }
}

//ES6
var Gil ={
    where(){
        console.log('서울사람이세여?')
    }
}
```
💦 *축약으로 정의된 메서드는 축약하지 않은 메서드와 다르게 동작한다.*