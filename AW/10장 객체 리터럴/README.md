# 10장 객체 리터럴

## 10.1 객체란?
***
- 키<sup>key</sup> + 값<sup>value</sup> 의 묶음 근데 이제 메서드를 곁들인



```js
var person = {
    age:'20', // key : value
    increase: function(){  // 메서드
	 console.log(`나이는 : ${this.age}입니다.`);   
    } 
};
```
## 10.2 객체 리터럴에 의한 객체 생성
***
- 다른언어는 함수로 객체 만들어야하는데 js는 중괄호 안에 대충 키값 넣어서 객체 생성 가능



```c++
C++

class Person
{
    int age;
public:
    Person(int age)
    {
        this->age = age;
        cout << "생성자 호출 / 나이 :" << age << endl;
    }
    ~Person()
    {
        cout << "소멸자 호출" << endl;
    }
    void GetAge()
    {
        cout << "나이는 : " << age << "입니다." << endl;
    }
};
void Test01()
{
    // 해당 함수 내에서만 메모리 유지
    Person person01(15);
    person01.GetAge();
};
```
와 극혐


***
```js
JS

var person = {
    age:'20', // key : value
    increase: function(){  // 메서드
	 console.log(`나이는 : ${this.age}입니다.`);   
    } 
};
```
편안
## 10.3 프로퍼티
***
- 프로퍼티 키는 **식별자 네이밍 규칙**에 따라 따옴표가 중요함
> 카멜케이스
> ```js
> const firstName;
> ``` 
> 스네이크 케이스
> ```js
> const first_name;
> ``` 
> 파스칼 케이스
> ```js
> const FirstName;
> ```
```js
var person ={
    firstName: 'andrew', // 규칙 따라서 에러 안남
    last-name: 'kim', // 따옴표 없어서 에러남
    'last-name':'kim' // 이제 안남
};
```

- 존재하는 키 중복 선언하면 덮어쓰기 됨
```js
var foo = {
    name: 'lee',
    name: 'kim'
};
console.log(foo); // {name: 'kim'}
```

- 프로퍼티는 int 로 넣어도 string으로 출력됨

                            
- var, function 과 같은 예약어 쓰지마셈

                        


## 10.4 메서드
***
- 프로퍼티 값안에 있는 함수를 메서드라고 부름 (객체에 묶여있음)
```js
var circle = {
    radius: 5,
    getDiameter: function (){ return 2 * this.radius;} // 이녀석인듯
};
```
## 10.5 프로퍼티 접근
***
> 주의: 프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름이 아니면 반드시 대괄호 표기법을 사용해야 한다.
- 마침표로 접근
```js
console.log(person.name);
```
- 괄호로 접근
```js
console.log(person['name']); // 키값에 따옴표 필수
```

## 10.6 프로퍼티 값 갱신
***
- 이미 존재하는 프로퍼티에 값 넣으면 덮어쓰기됨
```js
var person = {
    name: 'lee'
};

person.name='kim'

console.log(person); // {name: 'kim'}
```
## 10.7 프로퍼티 동적 생성
***
- 없는 프로퍼티 값 할당하면 생성됨
```js
var person = {
    name: 'lee'
};
person.age = 20; // 걍 막 집어넣음
console.log(person); // {name : 'lee', age: 20} 생성됨 
```
## 10.8 프로퍼티 삭제
***
- delete 연산자 쓰면 삭제됨
- 없는 프로퍼티 삭제하면 에러 없이 무시함
```js
delete person.age;
```

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능
***
1. 프로퍼티 키 생략 가능
```js
let x = 1, y = 2; 
const obj = { x, y }; //var obj = { x: x, y: y };
```
2. 프로퍼티 키 안에 backtick 기호 넣어서 동적 생성 가능
```js
const prefix = 'prop';
let i = 0;

const obj = {
    [`${prefix}-${++i}`]: i, // obj[prefix + '-' + ++i] = i;
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
};
```
3. function 생략 가능
```js
const obj = {
    name: 'lee',
    sayHi(){   // sayHi: function() { 
		console.log('hi!' + this.name);
    }
};
```
***
# 끝 
![done](https://memegenerator.net/img/instances/76848709.jpg)