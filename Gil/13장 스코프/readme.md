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