# 31장 RegExp

---

## 31.4 플래그
| 플래그 | 의미          | 설명                                  | 
|:----:|:------------|:------------------------------------|
|   i   | ignore case | 대소문자를 구별하지 않고 패턴을 검색한다              |  
|   g   | global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다 | 
|   m   | multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다           | 

## 31.6 자주 사용하는 정규표현식
1. 특정 단어로 시작하는지 검사
```js
const url = 'https://example.com';

/^https?:\/\//.test(url); // => true
```
2. 특정 단어로 끝나는지 검사
```js
const fileName = 'index.html';

/html$/.test(fileName); // => true
```

3. 숫자로만 이루어진 문자열인지 검사
```js
const number = '1234';

/^\d+$/.test(number); // => true
```

4. 하나 이상의 공백으로 시작하는지 검사
```js
const blank = 'hi!';

/^[\s]+/.test(blank); // => true
```

5. 아이디로 사용 가능한지 검사
```js
const id = 'abcd1234';

/^[A-za-z0-9]{4,10}$/.test(id); // => true
```

6. 메일 주소 형식에 맞는지 검사
```js
const email = 'andreww@designfever.com';

/^[A-Za-z0-9]([-_\.]?[A-Za-z0-9]([-_\.]?[A-Za-z0-9])*\.[A-Za-z]]{2,3})$/.test(email); // => true
```

7. 핸드폰 번호 형식에 맞는지 검사
```js
const phone = '010-1234-1234';

/^\d{3}-\d{3,4}-\d{4}$/.test(phone); // => true
```

8. 특수 문자 포함 여부 검사
```js
const special = 'abcd#123';

(/[^A-Za-z0-9]/gi).test(special); // => true

special.replace(/[^A-Za-z0-9]/gi, ''); // abcd123
```
---
# 끝