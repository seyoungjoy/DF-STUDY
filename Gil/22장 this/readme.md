<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter22.%20this&fontSize=50">

# **22.1 this 키워드**
객체는 자신의 프로퍼티를 참조하고 변경할 수 있어야한다. 재귀적으로 호출하여 내부 메서드가 참조할 수 있지만 이는 올바르지 않은 방법이다.

<br>

**this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다.** this를 이용해서 자신이 속한 객체, 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.

> **객체리터럴**에서는 메서드를 호출한 객체를 가리킨다.

```js
const gil = {
    x: 1,
    getX(){
        return this.x;// this === gil{x:1, f}
    }
};
console.log(gil.getX());
```
<br>

> **생성자함수**에서는 생성될 인스턴스를 가리킨다.

```js
function gil(x){
    this.x = x; //this === gil{x: 1}
}
gil.prototype.getX = function(){
    return this.x;
}

const who = new gil(1);

console.log(who.getX());
```

📌자바스크립트 this는 함수가 호출되는 방식에 따라 this에 바인딩될 값이 결정된다.(this 바인딩이 동적으로 결정)

<br>

# **22.2 함수 호출 방식과 this 바인딩**

<br>

## **22.2.1 일반 함수 호출**
일반함수에서 호출 하면 this는 전역 객체(window)가 바인딩 된다.<br>

**모든 일반함수(중첩함수, 콜백함수 포함)가 호출되면 this에는 ```전역객체 window```가 바인딩 된다는 것을 기억하자!**

<br>

>중첩함수, 콜백함수에서 this를 일치 하는 방법

```js
var x = 3;

const gil ={
    x: 1,
    AA(){
        // this를 변수에 할당하여
        // 내부함수에서 가리키는 this를 일치시킴
        const target = this;

        function innerAA(){
            console.log(target.x);
        }
        innerAA();
    }
}
gil.AA(); // 1
```

<br>

> Function.prototype.bind 메서드를 이용하여 일치시키는 방법

```js
var x = 3;

const gil ={
    x: 1,
    AA(){
        setTimeout(function(){
            console.log(this.x);
        }.bind(this), 100);
    }
}
gil.AA(); // 1
```

<br>

> 화살표 함수를 이용하여 일치시키는 방법

```js
var x = 3;

const gil ={
    x: 1,
    AA(){
        setTimeout(()=> console.log(this.x) , 100);
    }
}
gil.AA(); // 1
```

<br>

## **22.2.2 메서드 호출**
메서드는 가리키는 함수객체에 따라 변수에 할당하여 일반함수로 호출할 수도 있고, 다른 객체의 메서드가 될 수도 있다.

<br>

```js
const gil = {
    name: 'Kim',
    AA(){
        console.log(this.name);
    }
};

const young = {name: 'none'};

//다른 객체 메서드로 할당
young.BB = gil.AA;
young.BB();


//변수에 할당, 이때 this는 일반함수 호출이므로 window를 가리킨다.
const getName = gil.AA;
getName(); // ''

/*=================== 프로토타입 메서드 사용 시====================*/

function gil(name){
    this.name = name;
}

gil.prototype.AA = function(){
    console.log(this.name);
}

const getName = new gil('Kim');
getName.AA(); //Kim

gil.prototype.name = 'none';
gil.prototype.AA(); //none
```

<br>

## ~~**22.2.3 생성자 함수 호출**~~

<br>

## **22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출**

<br>

```apply, call```

apply와 call은 본질적 기능은 함수 호출이며 이때 첫번쨰 인수로 전달한 객체를 this에 바인딩한다. 그리고 배열을 묶어 보낼수 있지만 전달 방식만 다르다.

```js
function gil(name){
    console.log(arguments);
    return this;
}
const getName = {name: 'Kim'};

//호출할 함수의 인수를 apply는 배열로 묶어 전달
console.log(gil.apply(getName, [1,2,3]));

//호출할 함수의 인수를 call은 쉼표로 구분하여 전달
console.log(gil.call(getName, 1, 2, 3));
```

📌 apply, call의 대표적 용도는 arguments 유사배열객체를 배열메서드로 사용하는 것이다.

<br>

```bind``` <br>
bind 메서드는 함수 호출을 하지 않아 명시적으로 호출 해줘야한다. 그리고 첫번째 인수값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.

```js
const gil = {
    name: 'Kim',
    AA(callback){
        //bind 메서드로 함수내부 this 바인딩 전달
        setTimeout(callback.bind(this), 100);
    }
};

gil.AA(function(){
    console.log(`nice meet you ${this.name}`);
    //nice meet you Kim
});
```