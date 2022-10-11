<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter31.%20RegExp&fontSize=60" width="100%" alt="">

# **31.1 정규 표현식이란?**
일정 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.
정규표현식은 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 **패턴 매칭 기능**을 제공한다.

```js
// 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 체크 가능하다.
// 하지만 공백,여러가지 기호를 혼합해서 가독성이 좋지않다.
const tel = '010-1234-567팔'
const regExp = /^\d{3}-\d{4}-\d{4}$/;
regExp.test(tel); //false
```

<br>

# **31.2 정규 표현식의 생성**
정규표현식은 정규표현식 리터럴, RegExp 생성자 함수를 사용할 수 있다.
```/regexp/i``` 이와 같이 시작,종료(//) 패턴(regexp), 플래그(g,i,m,u,y)으로 구성된다.

[특수문자 참고](http://yoonbumtae.com/?p=1865)

```js
//정규표현식 리터럴을 이용하는 방법
const gil = 'Is this all there is?';
const regexp = /is/i;

regexp.test(target); // true

//생성자 함수를 이용하는 방법(변수를 동적으로 RegExp 객체를 생성 가능)
const count = (str,char) => (str.match(new RegExp(char, 'gi')) ?? []).length;
```