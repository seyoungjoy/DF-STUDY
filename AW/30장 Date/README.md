# 30장 Date

---

## 30.1 new Date 사용법
- 브랜든 아이크가 JS를 처음 만들때 JAVA를 참고하여 만들게됨
- 이제는 사용하지 않는 레거시 Date를 그대로 가져옴 레거시가 된 이유는
1. **현재는 사용하지 않는 RFC 형식 사용**
```jS
Date date = new Date();
System.out.printIn(date);
```
```js
var date = new Date();
console.log(date)
// Fri Oct 14 2022 14:58:19 GMT+0900 (한국 표준시) 
// --> RFC 형식
date.toISOString() // ISO 형식으로 변환 해서 많이 사용함
// 2017-03-16T17:40:00+09:00
// --> ISO 형식
```
2. **날짜 포맷 변경이 귀찮음**
```jS
var a = '${date.getFullYear()}년 ' +
    '${date.getMonth() + 1 }월 ' + // 0월이 1월임
    '${date.getDate()}일 입니다.'
// 하나 하나 땡겨와서 포맷팅 해서 표기해야함
```
3. 시간 덧셈을 JS 맘대로함
```jS
var date = new Date(2022, 9, 31) // 10월 31일 + 1개월
date.setMonth(date.getMonth() + 1) // new Date(2022,10,31)과 같음
console.log(date) // 12월 1일로 출력: 11월은 31일이 없기 때문
```

## 30.2 해결방안
1. 외부 라이브러리를 씁니다
- [day.js](https://jsikim1.tistory.com/m/196)
2. `Intl` 사용
```jS
var date = new Date(); 
var a = new Intl.DateTimeFormat('kr',{dateStyle : 'full', timeStyle: 'full'}).format(date) // 시간 포맷 별도 추가 가능
console.log(a) // 2022년 9월 19일 월요일 오후 3시 0분 47초 한국 표준시
var b = new Intl.RelativeTimeFormat().format(-10,'days')
console.log(b) //시간차 또한 쉽게 표현 가능함: 10일 전
```
3. `Temporal` 사용 (아직 정식 출시 안댐)
```js
npm install @js-temporal/polyfill // 폴리필 버전 체험가능
```
```js
var now1 = Temporal.Now.plainDateTimeISO();
consle.log(now.toString()) // 2022-10-17T15:03:01.813381812

now = now.add({days:10, months: 1}) 
now = now.subtract({month: 3})  // 날짜 덧샘뺼샘가능
now = now.round({smallestUnit: 'hour', roundingMode: 'floor'}) // 시간 반올림  

var Dday = Temporal.PlainDateTime.from('2022-09-30T12:00:00');
var today = Temporal.Now.plainDateTimeISO();

const resule = Dday.since(today)
console.log(result.toString()); // P10DT20H52M23.1443431455
console.log(result.days); // 10
```
---
# 끝