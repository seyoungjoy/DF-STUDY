<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter28.%20Number&fontSize=60" width="100%" alt="">

# **28.1 Number 생성자 함수**
Number 객체는 생성자 함수이므로 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

Number 생성자 함수에 인수를 전달 하지 않고 new 연산자와 함께 호출하면 내부슬롯에 0을 할당한 Number 래퍼 객체를 생성한다.
```js
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```