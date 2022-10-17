<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter29.%20Math&fontSize=60" width="100%" alt="">


Math는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다. 생성자 함수가 아니므로 정적 프로퍼티와 정적 메서드**(생성자 함수를 사용하지 않아도 참조,호출이 가능한 프로퍼티,메서드)**만 제공한다.

<br>

# **29.1 Math 프로퍼티(Math.PI)**
원주율 값을 반환한다.
```js
Math.PI; // 3.141592…
```

<br>

# **29.2 Math 메서드**

<br>

# **29.2.1 Math.abs**
인수로 전달 받은 숫자의 절대값을 반환한다.(0 또는 양수이어야 한다.)

```js
Math.abs(-1); // 1
Math.abs('-1'); // 1
Math.abs([]); // 0
Math.abs('string'); // NaN
```

<br>

# **29.2.2 Math.round**
인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.

```js
Math.round(1.4); // 1
Math.round(-1.4); // -1
Math.round(); // NaN
```

<br>

# **29.2.3 Math.ceil**
인수로 전달된 **숫자의 소수점 이하를 올림한 정수를 반환한다.(올림하면 더 큰 숫자로 반환된다. 아래 음수값 참고)**

```js
Math.ceil(1.4); // 2
Math.ceil(-1.4); // -1
Math.ceil(); // NaN
```

<br>

# **29.2.4 Math.floor**
인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.

```js
Math.floor(1.4); // 1
Math.floor(-1.4); // -2
Math.floor(); // NaN
```

<br>

# **29.2.5 Math.sqrt**
인수로 전달된 숫자의 제곱근을 반환한다.

```js
Math.sqrt(9); // 3
Math.sqrt(-9); // NaN
Math.sqrt(); // NaN
```

<br>

# **29.2.6 Math.radom**
인수로 전달된 숫자의 제곱근을 반환한다.
이때 0 ~ 1 숫자를 랜덤으로 반환하고 1은 포함되지 않는다.

```js
// 0에서 10미만 숫자(10포함 x)
console.log(Math.random()*10)
```

<br>

# **29.2.7 Math.pow**
첫번째 인수를 밑으로 두번째 인수를 지수로 하여 거듭제곱의 결과값을 반환한다.

```js
Math.pow(2,8);
```
>위 코드에서는 2<sup>8</sup>과 같다. ES7에서는 지수연산자(**)를 사용하면 가독성이 좋다.

<br>

# **29.2.8 Math.max**
전달받은 인수 중에서 가장 큰 수를 반환한다. **인수가 전달 되지않으면 -Infinity를 반환한다.**

배열요소중 최대값을 구하려면 apply메서드 혹은 스프레드 문법을 사용한다.

```js
Math.max(1,2,3);// 3
Math.max.apply(null,[1,2,3]);// 3
Math.max(...[1,2,3]);// 3
```

<br>

# **29.2.9 Math.min**
전달받은 인수중 가장 작은 수를 반환한다. **인수가 전달 되지않으면 Infinity를 반환한다.**

```js
Math.min(1,2,3);// 1
Math.min.apply(null,[1,2,3]);// 1
Math.min(...[1,2,3]);// 1
```