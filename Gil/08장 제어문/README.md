 <img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter8.%20%EC%A0%9C%EC%96%B4%EB%AC%B8&fontSize=60" style="width:100%;"/>

  # **8.1 블록문**
 0개 이상의 문을 중괄호로 묶는 것으로 블록문을 **1개의 실행단위로 취급한다.**

<br>

# **8.2 조건문**
## **8.2.1 if...else문**

<br>

>* 조건식이 **true일 경우 if의 코드블록**이 실행되고 **false일 경우 else의 코드블록**이 실행된다.
>* **else if**로 조건식을 2개이상 더 늘릴 수 있다.

```javascript
var Gil = 6;

if(Gil >= 0){
    console.log('if 코드블록');
}else if(Gil === 0){
    console.log('else if 코드블록');
}else{
    console.log('else 코드블록');
}
// Gil = 6 이므로 'if 코드블록'이 출력된다.
```

<br>

>* 코드블록안에 문이 1개 뿐이라면 중괄호을 생략할 수 있다.
>* if...else문은 **삼항조건연산자**로 바꿀수 있다.
```javascript
var Tomorrow = '월욜';
var Day;

if(Tomorrow === '일욜')      Day = '주말';
else if(Tomorrow === '월욜') Day = '출근';
else                         Day = '평일';

// 삼항조건연산자 변환

var Day = Tomorrow === '월욜' ? (Tomorrow === '일욜' ? '주말':'출근'):'평일';
```
<br>

## **8.2.2 switch 문**
표현식을 평가하여 참과거짓이 아닌 값과 일치하는 표식을 갖는 case 문으로 실행하는 문
>* 케이스문 뒤에 break를 사용해야 폴스루가 발생하지 않는다.
>* 참과 거짓으로 구분이 가능하다면 if...else문을 이용하자!

<br>

# **8.3 반복문**
조건식이 거짓일때까지 반복하여 코드블럭을 실행하는 문

<br>

## **8.3.1 for 문**

```js
for(var i=0; i<2; i++){
    console.log(i);
}
```
> * 초기값 0으로 조건식을 평가 후 코드블록을 한번 실행 하고 i는 1로 증가한다.
> * 다시 조건식을 평가하고 코드블록을 실행 하고 i는 다시 2로 증가한다.
> * 2는 조건식에서 거짓으로 평가 되어 반복문은 종료 된다.

<br>

```js
//무한루프 반복문
for (;;){...} 

//중첩 for문
for(var i=0; i<2; i++){
    for(var j=0; j<2; j++){
        if(i+j === 2){
            console.log(`[${i}], [${j}]`);
            // [1, 1]
        }
   }
}
```
>변수선언, 조건식, 증감을 빼면 무한 반복문을 만들 수 있고 for문 안에 for문을 다시 사용하여 중첩for문도 만들 수 있다.

<br>

## **8.3.2 while 문**
반복 횟수가 불확실할때 사용하는 반복문
```js
var Gil = 3;

while(Gil < 10){
    console.log(Gil); //3,4,5,6,7,8,9
    Gil++
}
//무한 반복문
while(Gil){
    console.log(Gil) //3, 3, 3, 3, 3, 3, …
}
```
<br>

## **8.3.3 do...while 문**
무조건 한번 코드블록을 실행(do) 후 조건식을 평가 하는 반복문
```js
var gil = '비빔밥';

do{
    console.log(gil);
    gil = '돈가스';
}while(gil === '비빔밥')
```
<br>

# **8.4 break 문**
레이블문, 반복문, switch문에서 코드블록을 벗어날때 사용하는 문

<br>

# **8.5 continue 문**
반복문에서 사용했을때 다음 문을 실행하지않고 반복문의 증감식으로 되돌아간다.(반복문을 벗어나지않는다.)

```js
var gil = 'hhhiiiii';
var search = 'i';
var count = 0;

for(i=0; i<gil.length; i++){
    if(gil[i] === search) continue
    count++;
}
console.log(count); // 3
```
>정규표현식(RegExp)과, match메서드를 이용하여 동일하게 동작 가능하다.