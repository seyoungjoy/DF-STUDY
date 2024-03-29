# 11. 원시 값과 객체의 비교

원시타입과 객체타입의 다른점   
+ 원시 값은 변경 **불가능한 값**, 객체는 **변경 가능한 값**
+ 원시 값을 변수에 할당하면 메모리 공간에는 **실제 값이 저장**되지만 객체는 **참조 값이 저장**된다.
+ 원시 값을 갖는 변수를 다른 변수에 할당하면 **원시값이 복사** 되지만 객체를 다른변수에 할당하면 **참조 값이 복사**된다.
<br><br><br>

# 11.1 원시 값

### 11.1.1 변경 불가능한 값   
원시값은 읽기 전용 값으로 변경할 수 없다.
원시값이 변경 불가능하다 는 것은 변수가 아니라 값에 대한 말. 상수는 재할당이 금지된 변수.
```javascript
var score;
score = 80; //0x0001332 메모리주소
score = 90; //0x0669F913 메모리주소
```
값은 그대로이고 메모리 주소를 변경한다. 이러한 특성을 불변성이라 한다.<br><br>

### 11.1.2 문자열과 불변성
문자열은 다른 원시값과 비교할때 특징이 있는데 문자열의 길이에 따라 메모리 공간의 크기가 결정된다.
문자열은 **유사 배열 객체이면서 이터러블**이므로 배열과 유사하게 각문자에 접근할 수 있다. (유사배열의 경우 배열의 메서드를 쓸 수 없다!)<br><br>
   

### 11.1.3 값에 의한 전달
```javascript
var score = 80;
var copy = score;
console.log(score, copy); //80,80
```
위와 같이 원시값이 복사되어 전달되는데 이를 값에 의한 전달이라 한다.   
변수에는 값이 전달되는 것이 아니라 메모리 주소를 전달하기 때문에 전달된 메모리 주소를 통해 메모리 공간에 접근하면 값을 참조할 수 있다.
**원시 값을 갖는 변수를 할당하면 변수 할당 시점이든, 재할당 시점이든 결국 두 변수는 다른 메모리 공간에 별개의 값이 된다.**<br><br><br>


# 11.2 객체
객체는 동적으로 추가되고 삭제할 수 있기 때문에 사전에 크기를 정해 둘 수 없다.<br>

### 11.2.1 변경 가능한 값
객체는 변경 가능한 값이다.   
객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리를 접근하면 참조 값에 접근할 수 있다.
```javascript
var person = {
    name : 'Joo'
}
// 변수명(식별자): person
// 해당 값의 위치(메모리 주소): 0x00000F2
// 참조 값: 0x0001332
```
이렇게 변수가 값을 갖는것이 아니기 때문에 변수는 객체를 가리키고 있다, 참조하고 있다는 표현을 쓴다.   
객체를 생성하고 관리하는 방식은 복잡하고 비용이 많이든다.
그래서 메모리를 효율적으로 사용하기 위해 객체를 변경 가능한 값으로 만들었다.<br><br>

### 11.2.2 참조에 의한 전달
객체의 이러한 구조는 여러개의 식별자가 하나의 객체를 공유할 수 있는 단점을 가지고 있다.   
이게 왜 단점일까?
```javascript
var person = {
    name : 'Joo'
}
var copy = person
copy.name = 'joooo'
console.log(person); //{name:'joooo}
```
person과 copy는 저장된 메모리 주소는 다르지만 동일한 객체를 가리킨다. **두 개의 식별자가 하나의 객체를 공유하기 때문에 한쪽에서 객체를 변경하면 서로 영향을 주고받는다.**