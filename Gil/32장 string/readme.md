<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter32.%20String&fontSize=60" width="100%" alt="">


# **32.1 String 생성자 함수**
표준 빌트인 객체인 String 객체는 **생성자 함수 객체**다. new 연산자와 함께 호출 하여 String 인스턴스를 생성할 수 있고, [[StringData]] 내부슬롯에 빈 문자열을 할당한 래퍼객체를 생성한다.

```js
const gil = new String();
console.log(gil); // String{length:0, [[PrimitiveValure]]:""}

//인수로 문자열을 전달하면 문자열을 할당한 래퍼객체를 생성한다.
const gil2 = new String('hi');
console.log(gil2); // String{0:"h", 1:"i", length:2, [[PrimitiveValure]]:"hi"}

//문자열은 원시 값이므로 변경할 수 없고, 이때 에러가 발생하지 않는다.
gil2[0] = "a";
console.log(gil2); //'hi'

//문자열이 아닌 값을 전달하면 인수를 강제로 문자열로 변환후 내부슬롯에 반환된 래퍼객체를 생성한다.
const gil3 = new String(null);
conole.log(gil3); // String{0:"n", 1:"u", 2:"l", 3:"l" length:4, [[PrimitiveValure]]:"null"}

//new 연산자를 사용하지 않으면 인스턴스가 아닌 문자열로 반환한다.
String(true); //"true"
String(NaN); //"NaN"
```

<br>

# **32.2 length 프로퍼티**
length 프로퍼티는 문자열의 문자 개수를 반환한다.

```js
const gil = "hi";
gil.length; // 2
```

<br>

# **32.3 String 메서드**
String객체의 메서드는 언제나 새로운 문자열을 반환한다.(래퍼객체도 읽기 전용 객체로 제공된다.)

<br>

# **32.3.1 String.prototype.indexOf**
indexOf 메서드는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면(존재하지 않으면) -1을 반환한다.

```js
const ssd = "https://ssd.designfever.com/kr";

ssd.indexOf("kr"); //28

if(ssd.indexOf("kr") === -1){
    //영문일경우
    console.log('en');
}

//includes는 불리언 값으로 반환된다.
if(ssd.includes("kr")){
    //국문인경우
    console.log('en');
}
```

<br>

# **32.3.2 String.prototype.search**
대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.

```js
const gil = 'Hello world 가';
gil.search(/[ㄱ-ㅎ|ㅏ-ㅣ|가-힣]/); // 12 (한글찾기)
gil.search(/x/); // -1 (검색에 실패하면 -1을 반환한다.)
```

<br>

# **32.3.3 String.prototype.includes**
문자열 포함 유무를 확인하여 불리언 값으로 반환한다.
```js
const gil = "hello world";
gil.includes("hello"); //true
gil.includes(" "); //true
gil.includes(); //false

//두번째 인수로 검색 시작 인덱스를 전달할 수 있다.
gil.includes("l",5); //true
```

<br>

# **32.3.4 String.prototype.startsWidth**
대상 문자열이 전달 받은 인수로 시작 유무를 불리언값으로 반환한다.

```js
const str = "nice meet you";
str.startsWith("H"); //false
str.startsWith("ni"); //true

//검색을 시작할 위치를 전달할 수 있다.
str.startsWith(" ", 4); //true
```

<br>

# **32.3.5 String.prototype.endsWidth**
대상 문자열이 전달 받은 인수로 끝나는지 유무를 불리언값으로 반환한다. 

```js
const str = "nice meet you";
str.endsWith("H"); //false
str.endsWith("ou"); //true

//검색할 문자열의 길이를 전달할 수 있다.
//"nice "
//true
str.endsWith(" ", 4);
```

<br>

# **32.3.6 String.prototype.charAt**
전달받은 인수 인덱스의 문자열을 반환한다.

```js
const str = "hello";
str.charAt(0); //h

//문자열 범위가 벗어난 정수인 경우 빈 문자열을 반환한다.
str.charAt(7); // ''
```

<br>

# **32.3.7 String.prototype.substring**
첫번째 인수로 받은 인덱스 부터 두번째 인수로 받은 인덱스 **전**까지를 문자열로 반환한다.

```js
const str = "hello~";
//1 ~ 2 까지
str.substring(1,3); //"el"

//첫번째 인수가 두번째 보다 높을경우 인수가 교환된다.
str.substring(3,1); //"el"

//인수가 0보다 작거나 NaN인경우 0으로 취급한다.
str.substring(-2); //"hello~"

//인수가 length보다 길경우 length로 취급한다..
str.substring(30); //""  === str.substring(6)
str.substring(1, 100); //"ello~" === str.substring(1, 6)

//indexOf로 특정문자 기준 앞뒤를 취득할 수있다.
str.substring(0, str.indexOf("~")); //"hello"
```

<br>

# **32.3.8 String.prototype.slice**
subString 과 동일 하지만 인수로 음수를 전달할 수 있다.

```js
const str ="hello world";
str.slice(-5); //"world"
```

<br>

# **32.3.9 String.prototype.toUpperCase**
대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

<br>

# **32.3.10 String.prototype.toLowerCase**
대상 문자열 모두 소문자로 변경한 문자열을 반환한다.

<br>

# **32.3.11 String.prototype.trim**
대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.
trimStart()를 이용해 문자열 앞, trimEnd()를 이용해 문자열 뒤 공백만 제거할 수도 있다.

```js
//replace 메서드에 정규 표현식을 인수로 전달하여 공백을 제거할 수도 있다.

const str = "   foo   ";
str.replace(/\s/g, '');// "foo"  
```

<br>

# **32.3.12 String.prototype.repeat**
인수로 전달받은 정수 만큼 문자열을 반복 해준다. 음수의 경우 RangeError를 발생시킨다.

<br>

# **32.3.13 String.prototype.replace**
첫번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두번쨰 인수로 전달 받은 문자열로 치환한 문자열을 반환한다.

```js
const str = 'Hello world';

//특수 교체 패턴을 사용할 수 있다.(아래참고)
str.replace('world','<strong>$&</strong>'); //'Hello <strong>world</strong>'

//정규표현식을 전달할 수 있다.
str.replace(/hello/gi, 'lee'); //'lee world'
```
>[참고](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/replace)

<br>

두번째 인수로 치환함수를 전달하면
1. 첫번째로 인수로 전달한 문자열 또는 정규표현식에 매치한 결과를 두번째 인수의 치환함수의 인수값으로 전달한다. ex) match(첫번째 매치 결과값)
2. 두번째 인수(치환함수)가 호출되고 반환한 결과와 매치 결과를 치환한다.

```js
function cameToSnake(camelCase){
    return camelCase.replace(/.[A-Z]/g, matchFn => {
        console.log(matchFn); //"oW"
        return matchFn[0] + '_' + matchFn[1].toLowerCase();
    });
}

const camelCase = 'helloWorld';
cameToSnake(camelCase); //'hello_world'
```

<br>

# **32.3.14 String.prototype.split**
1. 첫번째 인수로 전달된 문자열 또는 정규표현식을 검색하여 문자열을 구분
2. 분리된 각 문자열로 이루어진 배열을 반환.

빈 문자열을 전달하면 문자 모두를 분리하며 인수를 생략하면 단일 요소가 있는 배열을 반환한다.

```js
const str = "How are you doing?";

str.split(' '); //["How", "are", "you", "doing?"]
//여러가지 공백을 의미하는 정규표현식
str.split(/\s/); //["How", "are", "you", "doing?"]
str.split(""); //["H", "o", "w", ~ "?"]
str.split(); //["How are you doing?"]

//두번째 인수로 전달시 배열의 길이를 지정한다.
str.split(' ', 2); //["How", "are"]

//reverse, join 메서드를 사용하여 문자열을 역순으로 뒤집을수 있다.
function reverseString(str){
    return str.splite('').reverse().join('');
}

reverseString("hello world!");
//"!dlrow olleh" 
```