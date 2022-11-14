# 32장 String

---

## 32.1 String 생성자 함수
- String 객체는 생성자 함수 객체다. (`new` 연산자와 함께 호출하여 `String` 인스턴스 생성가능)
## 32.2 length 프로퍼티
- String 래퍼 객체는 배열과 마찬가지로 `length` 프로퍼티를 갖는다. 즉, 유사 배열 객체이다.
## 32.3 String 메서드
 1. String.prototype.indexOf
    - 인수로 전달받은 문자열을 검색하여 첫번째 인덱스를 반환한다
    - 검색에 실패하면 `-1`을 반환하므로 특정 문자열이 존재하는지 확인할 떄 유용하다.
    - ES6에서는 `String.prototype.includes`메서드를 사용을 추천(가독성이 좋다)
 2. String.prototype.search
    -  인수로 전달받은 "정규표현식"과 매치하는 문자열을 검색하여 해당 문자열의 인덱스를 반환한다.
    - 검색에 실패하면 `-1`을 반환한다
 3. String.prototype.includes
    - 인수로 전달받은 문자열이 포함되어 있는지 확인하여 `true` or `false` 로 반환
4. String.prototype.startsWith
    - 인수로 전달받은 문자열로 시작하는지 확인하여 `true` or `false` 로 반환
    - 두번째 인수는 검색할 문자열의 길이(처음부터 n자리까지)
5. String.prototype.endsWith
    - 인수로 전달받은 문자열로 끝나는지 확인하여 `true` or `false` 로 반환
    - 두번째 인수는 검색할 문자열의 길이(처음부터 n자리까지)
6. String.prototype.charAt
    - 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환
    - `0 ~ (문자열 길이 - 1)` 사이의 정수여야함, 아니면 빈문자열 반환
7. String.prototype.substring
    - 첫번째 인수와 두번째 인수 사이의 문자열을 반환
    - 첫번째 인수가 두번째 인수보다 큰 경우 두 인수는 교환된다
    - 인수가 `0`보다 작거나 `NaN`인 경우 0으로 취급
    - 인수가 문자열의 길이(`str.length`)인 경우 인수는 문자열의 길이(`str.length`)로 취급
    - String.prototype.indexOf와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할수 있다(substring안에 인수로 indexOf사용)
8. String.prototype.slice
   - substring 메서드와 동일하게 동작하지만 음수인 인수를 전달할수있다
   - 음수는 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환
9. String.prototype.toUpperCase
   - 전부 대문자
10. String.prototype.toLowerCase
    - 전부 소문자
11. String.prototype.trim
    - 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다
    - `string.prototype.trimStart`, `string.prototype.trimEnd` 도 사용가능
12. String.prototype.repeat
    - ES6
    - 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환
    - 0이면 빈 문자열 반환, 음수면 RangeError 발생, 생략시 기본값 0
13. String.prototype.replace
    - 대상 문자열에서 첫번째 인수로 전달받은 문자열 또는 정규식을 검색하여 두번째 인수로 변환
    - 케이스 변환에 유용한듯
14. String.prototype.split
    - 첫번째 인수로 전달한 문자열 또는 정규식을 검색하여 문자열을 구분한뒤 각 쪼개진 문자열로 이루어진 배열을 반환
    - 두번째 인수로 배열의 길이를 지정할 수 있다
    - `Array.prototype.reverse`, `Array.prototype.join`메서드로 문자열을 역순으로 정렬 가능
## 
---
# 끝