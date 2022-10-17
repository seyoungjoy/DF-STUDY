# 31장 RegExp(Regular Expression)
## 31.1 정규 표현식이란?
- 패턴 매칭 기능 제공
- 특정 패턴으로 문자열이 조건에 일치하는지 검색 or 치환가능
- 가독성이 좋지 않다는 단점

## 31.2 정규 표현식의 생성
- `/패턴/플래그`
- 정규 표현식 리터럴
```jsx
const target = "is this all there is?";

const regexp = /is/i;
console.log(regexp.test(target))```
```

- RegExp 생성자 함수
```jsx
new RegExp(pattern[, flags]);
```

## 31.3 RegExp 메서드
### 31.3.1 RegExp.prototype.exec
- 매칭 결과가 있으면 배열로 반환, 없으면 null

### 31.3.2 RegExp.prototype.test
- 정규 표현식의 패턴을 검사하여 결과를 불리언 값으로 반환
```jsx
const target = "is this all there is?";

const regexp = /is/i;
console.log(regexp.test(target))
```

### 31.3.3 String.prototype.match
- 매칭 결과를 배열로 반환
```jsx
const target = "is this all there is?";

const regexp = /is/g;
console.log(target.match(regexp))
```

## 31.4 플래그
- 검색 방식을 설정
- i : ignore case : 대소문자를 무시하고 검색하겠다.
- g : global : 패턴과 일치하는 모든 문자열을 검색하겠다.
- m : multi line : 문자열의 행이 바뀌어도 계속 검사.

## 31.5 패턴
### 31.5.1 문자열 검색
### 임의의 문자열 검색
- `.`
```jsx
const target = "Is this all there is?";

const regExp = /.../g;
target.match(regExp);
```

### 반복 검색
- `{m,n}` : 중괄호 앞 문자가 최소 m번, 최대 n번 반복되는 문자열
- `{n}` : 중괄 호 앞 문자가 최소 n번 반복
- `A{n,}`, `/A+/` : 중괄호 앞 문자가 최소 n번 이상 반복
- `?` : 
```jsx
const target = "A AA B BB Aa Bb AAA";

const regExp = /A{1,2}/g;
console.log(target.match(regExp))
```

### NOT 검색
```jsx
const target = 'AA BB 12 Aa Bb';

const reg = /[^0-9]+/g;
console.log(target.match(reg))

```

### 시작 위치로 검색
```jsx
const target = `https://poiemaweb.com`;

const reg = /^http/;
console.log(reg.test(target))
```

### 마지막 위치로 검색
```jsx
const target = 'https:/poiemaweb.com';

const reg = /com$/;
console.log(reg.test(target))
```

## 31.6 자주 사용하는 정규표현식
### 특정 단어로 시작하는지 검사
```jsx
const url = 'https://example.com';

// const reg = /^https?:\/\//;
const reg = /^(http|https):\/\//;

console.log(reg.test(url))
```

### 특정 단어로 끝나는지 검사
```jsx
const fileName = 'index.html';

const reg = /$html/;
```

### 숫자로만 이루어진 문자열인지 검사
```jsx
console.log(/^\d+$/.test("12345"))
```

