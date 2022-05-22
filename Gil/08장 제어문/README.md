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
