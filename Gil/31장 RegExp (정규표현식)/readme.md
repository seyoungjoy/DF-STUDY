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
count('Is this all there is?','is'); // 3
```

<br>

# **31.3 RegExp 메서드**

<br>

## **31.3.1 RegExp.prototype.exec**
인수로 전달 받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.
```js
const target = 'Is this all there is?';
const regExp = /is/;
regExp.exec(target);
//["is", index:5, input: "Is this all there is?", groups:undefined]
```

<br>

## **31.3.2 RegExp.prototype.test**
인수로 전달받은 문자열에 검색 결과를 불리언 값으로 변환한다.

<br>

## **31.3.3 RegExp.prototype.match**
인수로 전달받은 문자열의 검색결과를 배열로 반환한다.(exec와 비슷)
>exec와 다른점은 g 플래그 사용시 exec는 첫번째 결과값만, match는 모든 결과값을 반환한다.
 
```js
const target = 'Is this all there is?';
const regExp = /is/g;
target.match(regExp);
// ["is","is"]
```

<br>

# **31.4 플래그**
검색 방식을 설정하기 위해 사용한다.

|       플래그       |     의미      |                  설명                  |
|:---------------:|:-----------:|:------------------------------------:|
|        i        | Ignore case |         대소문자를 구별하지 않고 패턴을 검색         |
|        g        |   Global    | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
|        m        | Multi line  |      문자열의 행이 바뀌더라도 패턴 검색을 계속한다.      |

<br>

>플래그는 순서 상관없이 여러개를 사용할 수 있으며, 매칭 대상이 1개 이상 존재해도 첫 번째 대상만 검색하고 종료한다.

```js
const target = 'Is this all there is?';
target.match(/is/i);
// ["is", index:5, input:..., groups: undefined]
target.match(/is/g);
// ["is", "is"]
target.match(/is/ig);
// ["is", "is", "is"]
```

<br>

# **31.5 패턴**
정규 표현식은 일정한 규칙(패턴)을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.
패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다.
>문자열 내에 패턴과 일치하는 문자열이 존재할 때 **'정규표현식과 매치한다'** 고 표현한다.

<br>

## **31.5.1 문자열 검색**

1. 정규표현식이 매치하는지 테스트
2. 정규표현식의 매칭 결과를 구한다.(첫번째 결과만 반환)

<br>

## **31.5.2 임의의 문자열 검색**
.(마침표)은 임의의 문자 한개를 의미한다.(내용 상관 X)

```js
// 임의의 3자리 문자열을 검색, 문자열 개수가 모자랄 경우 제외한다.
const target = 'Is this all ';
const regExp = /.../g;
target.match(regExp); //["Is ", "thi", "s a", "ll "]
```

<br>

## **31.5.3 반복 검색**
* {x,y} : 앞의 패턴이 최소 x번, 최대 y번 반복되는 문자열을 의미한다.
* {y} : 앞선 패턴이 y번 반복되는 문자열을 의미한다. {y} === {y,y} (최소, 최대값이 같다.)
* {y,} : 앞선 패턴이 최소 y번 반복되는 문자열을 의미한다.
* +&nbsp;: 앞선 패턴이 최소 한번 이상 반복 되는 문자열을 의미한다.
+ ? : 앞선 패턴이 최대 한 번(0번포함)이상 반복되는 문자열을 의미한다.

```js
const target = 'A AA B BB Aa Bb AAA AAB';
const regExp = /A{1,2}/g;
const regExp2 = /A{2}/g;
const regExp3 = /A{2,}/g;
const regExp4 = /AA+/g;
const regExp5 = /AA?A/g;

target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A', 'AA']
target.match(regExp2); // ['AA', 'AA', 'AA']
target.match(regExp3); // ['AA', 'AAA','AA']
target.match(regExp4); // ['AA', 'AAA', 'AA']
target.match(regExp5); // ['AA', 'AAA', 'AA']
```

<br>

## **31.5.4 OR 검색**
```/A|B/```는 'A' 또는 'B'를 의미한다.(분해되지 않은 단어 레벨로 검색을 위해 +를 사용한다.)
```js
const target = 'A AA B BB Aa Bb';
const regExp = /A|B/g;
// /[AB]+/g; 와 동일
const regExp2 = /A+|B/g;
// A-Z(대문자) 한번이상 반복되는 패턴을 검색
const regExp3 = /[A-Z]+/g;
// A~Z, a~z 한번이상 반복되는 문자열을 전역 검색
const regExp4 = /[A-Za-z]+/g;

target.match(regExp); //["A", "A", "A", "B", "B","B", "A", "B"]
target.match(regExp2); //["A", "AA", "B", "BB", "A","B"]
target.match(regExp3); //["A", "AA", "B", "BB", "A","B"]
target.match(regExp4); //["A", "AA", "B", "BB", "Aa", "Bb"]
```
<br>

### 숫자 검색

```js
const target = 'AA BB 12,345';
const regExp = /[0-9,]+/g; // /[\d,]+/g; 과 동일
const regExp2 = /[\D,]+/g;

target.match(regExp); //["12,345"]
target.match(regExp2); //["AA BB ", ","] 공백, 특수문자 포함
```

<br>

\w는 알파벳,숫자,언더스코어를 의미한다.([A-Za-z0-9_]와 같다.)<br>
\W는 알파벳,숫자,언더스코어가 아닌 문자열을 의미한다.
```js
const target = 'Aa Bb 12,345 _$%&';
let regExp = /[\w,]+/g;
```