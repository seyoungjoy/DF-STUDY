# 22. this

### 22.1 this 키워드

```this```는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다. ```this```를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
**this는 함수 호출 방식에 의해 동적으로 결정된다.**
<br><br>

### 22.2 함수 호출 방식과 this 바인딩

렉시컬 스코프는 함수가 정의될때 결정, this 바인딩은 호출 될때 결정!!
<br><br>

#### 22.2.1 일반 함수 호출

기본적으로 this에는 전역 객체가 바인딩 된다.

```javascript
function foo(){
    console.log("foo's this :" , this); //window
    function bar(){
        console.log("bar's this:", this);
    }
    bar();//window
}
foo();
```
일반 함수로 호출된 모든 함수(중첩,콜백함수) 내부의 this에는 **전역 객체가 바인딩된다.**<br>
중첩 함수의 this와 외부 함수의 this가 다를 수 있다는 점에서 전역 객체로 바인딩 되는 것은 문제가 있다.

```javascript
var value = 1;
const obj = {
    value : 100,
    //1
    foo(){
        // this 바인딩(obj)을 변수 that에 할당
        const that = this;
        setTimeout(function(){
            console.log(that.value); //100
        }, 100);

    }
    //2
    foo(){
        setTimeout(function(){
            console.log(this.value); //100
        }, bind(this),100);

    }
    //3
    foo(){
        setTimeout(() => console.log(this.value),100);
    }
}
obj.foo();
```
위의 방법과 같이 this 바인딩을 일치 시킨다.
<br><br>

#### 22.2.2 메서드 호출

메서드 내부의 있는 this는 그 메서드를 실행시킨 객체 인스턴스가 this가 된다.
```javascript
const person = {
    name : 'Lee',
    getName(){
        //메서드 내부의 this는 메서드를 호출한 객체에 바인딩
        return this.name;
    }
};
console.log(person.getName()); //Lee

const anotherPerson = {
    name : 'kim'
};

//getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

//getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); //kim

//getName 메서드를 변수에 할당
const getName = person.getName;
console.log(getName());
//일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name가 같다.
```
따라서 메서드 내부의 this는 프로퍼티로 메서드를 가리키고 있는 객체와는 관계 없고 메서드를 호출한 객체에 바인딩된다.
<br><br>

#### 22.2.3 생성자 함수 호출

생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
new연산자를 쓰지 않으면 일반 함수로 동작하기때문에 -> this는 전역 객체를 가리킨다.
<br><br>

#### 22.2.4 Function의 prototype인 apply/call/bind 메서드에 의한 간접호출
<br>

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

//getThisBiding함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩
console.log(getThisBinding.apply(thisArg)); // { a: 1 }
console.log(getThisBinding.call(thisArg)); // { a: 1 }
//
```
apply 메서드는 호출할 함수의 인수를 배열로 묶어 전달. call 메서드는 쉼표로 구분한 리스트 형식으로 전달.
둘의 대표적인 용도는 유사 배열 객체에 배열 메서드를 사용하는 경우.
<br><br>

```javascript
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding.bind(thisArg)); // getThisBinding

console.log(getThisBinding.bind(thisArg)()); // { a: 1 }
```
bind 메서드는 this와 메서드 내부의 중첩,콜백 함수의 this가 불일치하는 문제를 해결한다.






