<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter13.%20%EC%8A%A4%EC%BD%94%ED%94%84&fontSize=55">

# **13.1 스코프란?**
**식별자의 유효한 범위**이다. 함수의 매개변수의 유효범위가 함수 내 까지 이다. 이처럼 식별자는 식별자 결정을 통해서 변수가 선언된 위치에 따라 유효범위가 결정된다.

<br>

# **13.2 스코프의 종류**

<br>

## **13.2.1 전역과 전역 스코프**
```js
var Gil = 'global';

function Where(){
    console.log(Gil);
}
console.log(Gil); // global
```

>Gil이라는 변수는 함수 안이 아닌 전역에 위치하고 전역변수에 해당한다. 따라서 모든 범위(전역스코프)에서 참조할 수 있다.

<br>

## **13.2.2 지역과 지역 스코프**
```js
function travel(){
    var kim = 'contry';

    function Where(){
        console.log(kim);
    }
}

console.log(kim); // Uncaught ReferenceError
```
>kim이라는 변수는 travel 함수 안에 위치하고 지역변수에 해당한다. 따라서 kim변수를 전역에서 호출할경우 레퍼런스에러가 발생한다.

<br>

# **13.3 스코프 체인**
지역스코프에서 전역스코프까지 계층이 연결돼 있는것을 말한다.

```js
//전역스코프
function three(){
    //지역스코프
    var G = 3;
    function two(){
        //지역스코프
        var I = 2;
        function one(){
            //지역스코프
            var L = 1;
            console.log(G);
        }
        one();
    }
    two();
}
three(); //3
```
>이때 변수를 참조할때 코드 실행시점(코드가 실행되는 스코프)부터 상위 스코프로 이동 하여 선언된 변수를 검색한다.

<br>

## **13.3.1 스코프 체인에 의한 *'변수'* 검색**

>스코프체인은 하위에서 상위로 검색을 하는것을 생각하고 아래 코드를 살펴보자!

```js
var A = 1;

function X(){
    var B = 2;

    function Y(){
        var B = 3;

        function Z(){
            console.log(A); //1
            console.log(B); //3
        }
        Z();
    }
    Y();
}
X();
```

<br>

## **13.3.2 스코프 체인에 의한 *'함수'* 검색**

>함수도 변수와 마찬가지로 하위에서 상위로가는 스코프계층을 가진다. 하지만 가독성이 떨어지니 함수명은 중복되지 않도록 하자!

```js
function X(){
    console.log('global X!');
}
function Y(){
    function X(){
        console.log('inner X!');
    }
    X(); // inner X!
}
Y();
```

<br>

# **13.4 함수 레벨 스코프**
함수몸체내에서만 유효한 특성을 **함수레벨 스코프**라고 하고, 블록안('{}', for문, if문 등등)에서까지 유효한 특성을 **블록레벨 스코프**라고 한다.

```js
//함수레벨 스코프(var)
var x = 1;

if(true){
    var x = 2;
}
console.log(x); // 2

//블록레벨 스코프(const, let)
const y = 1;
if(true){
    const y = 3;
}
console.log(y); // 1
```

<br>

# **13.4 렉시컬 스코프**

함수를 호출될때 상위스코프를 결정되는 것을 동적스코프, 함수가 정의될때(만들어질때) 상위스코프가 결정되는 것을 **정적스코프,렉시컬스코프**라고 한다. <br>

<br>

```js
var x = 1;

function Gil(){
   var x = 3;
   young();
}
function young(){
    console.log(x);
}
Gil(); // 1
young(); // 1
```

> 자바스크립트는 기본 렉시컬스코프를 가지기 떄문에 정의될때 상위스코프가 결정된다.