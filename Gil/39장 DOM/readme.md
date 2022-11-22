<img src="https://capsule-render.vercel.app/api?type=waving&color=gradient&customColorList=1&height=200&section=header&text=Chapter39.%20DOM&fontSize=60" width="100%" alt="">


 DOM은 HTMl 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조다.

# **39.1 노드**

<br>

## **39.1.1 HTML 요소와 노드객체 **
HTML 요소간에는 중첩 관계에 의해 계층적인 부자 관계가 형성된다. 이러한 관계를 반영하여 HTMl 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성한다.

<br>

### `트리 자료구조` <br>
트리 자료구조는 부모노드와 자식노드로 구성되어 노드간 계층 구조를 표현하는 비선형 자료구조(하나의 자료뒤에 여러 개의 자료가 존재할 수 있는 자료구조)를 말한다.

* 하나의 최상위 노드에서 시작
* 최상위 노드는 루트 노트라 한다.
* 루트 노드는 0개 이상의 자식노드를 갖는다.(자식 노드가 없으면 리프노드라 한다.)

> 노드 객체들로 구성된 트리 자료구조를 **DOM(DOM 트리)** 이라 한다.

<br>

## **39.1.2 노드 객체의 타입**
DOM은 노드객체의 계층적인 구조로 구성된다. 노드 객체는 종류가 있고 상속 구조를 갖는다.

<br>

### `문서노드`
DOM 트리의 루트 노드로서 document 객체를 가리킨다. (window.document or document로 참조할 수 있다.)

HTML 문서당 document 객체는 유일하다. 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해햐한다.

<br>

### `요소 노드`
HTML 요소를 가리키는 객체, HTML 요소간의 부자 관계를 가지며 정보를 구조화한다. (문서의 구조를 표현한다.)

<br>

### `어트리뷰트 노드`
HTML 요소의 어트리뷰트를 가리키는 객체, 부모 노드가 없으므로 요소 노드의 형제 노드는 아니다. 따라서 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 요소 노드에 접근해야 한다.

<br>

### `텍스트 노드`
HTML 요소의 어트리뷰트를 가리키는 객체다. 요소 노드의 자식 노드이며, DOM트리의 최종단이다.

<br>

## **39.1.3 노드 객체의 상속 구조**
DOM을 구성하는 노드객체는 자신의 구조와 정보를 제어할수 있는 DOM API를 사용할 수 있으며 이를 통해 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.

노드객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖는다.

* (모든 노드 객체는) Object, EventTarget, Node 인터페이스를 상속 받는다.

* 문서노드는 Document, HTMLDocument 인터페이스를 상속받고, 어트리뷰트 노드는 Attr, 텍스트노드는 CharacterData 인터페이스를 각각 상속 받는다.

* 요소노드는 ELement 인터페이스를 상속받는다. Element 인터페이스는 HTMLElement와 태그별 세분화된 인터페이스(HTMLHtmlElement, HTMLHeadELement, HTMLBodyElement, HTMLUListElement 등)를 상속받는다.

>요소노드객체는 프로토타입 체인에 있는 모든 프로토타입의 프로퍼티나 메서드를 상속받아 사용할 수 있다. (노드 객체는 공통된 기능일수록 프로토타입 체인의 상위에, 개별적 고유 기능일수록 프로토타입 체인 하위에 프로토타입 체인을 구축하여 노드객체에 필요한 프로퍼티와 메서드를 제공하는 상속 구조를 갖는다.)
> 
> **노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집한인 DOM API로 제공한다. 이 DOM API를 통해 HTMl의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있다.**

📌 노드 객체의 상속구조는 개발자 도구의 Elements 패널 우측의 Properties 패널에서 확인할 수 있다.~~(나는 왜 못찾는것인가...)~~

<br>

# **39.2 요소 노드 취득**
요소 노드의 취득은 HTML 요소를 조작하는 시작점이다.

<br>

## **39.2.1 id를 이용한 요소 노드 취득**
id값을 이용한 요소 노드취득은 중복된 id값이 있더라도 첫번쨰 요소 노드만 반환한다.(하지만 id값은 유일한 값이어야 한다.)

HTML 요소에 id 어트리뷰트를 부여하면 id값과 동일한 이름의 전역변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과가 있다.

```html
<div id="gil"></div>
<div id="gil2"></div>

<script>
    console.log(gil === document.getElementById("gil")) //true
    delete gil;
    console.log(gil); // <div id="gil"></div>
    
    //id값과 동일한 이름의 전역변수가 있으면
    //노드객체가 재할당되지 않는다.
    let gil2 = 1;
    console.log(gil2);//1
    
</script>
```

<br>

## **39.2.2 태그 이름을 이용한 요소 노드 취득**
getElementsByTagName 메서드는 인수로 전달한 태그이름을 가진 모든 요소노드들을 탐색하여 반환한다.(DOM컬렉션 객체인 HTMLCollection 객체를 반환)

<br>

## **39.2.3 class를 이용한 요소 노드 취득**
getElementsByClassName 메서드는 인수로 전달한 class 어트리뷰트 값(클래스명)을 가진 모든 요소노드들을 탐색하여 반환한다.(DOM컬렉션 객체인 HTMLCollection 객체를 반환)

<br>

## **39.2.4 CSS 선택자를 이용한 요소 노드 취득**
querySelector 메서드는 인수로 전달한 CSS 선택자를 만족 시키는 하나의 요소노드를 탐색하여 반환한다.(NodeList 객체를 반환)

* (CSS선택자를 만족시키는 요소 노드가 )여러 개인 경우 첫번째 요소노드만 반환
* (CSS선택자를 만족시키는 요소 노드가) 존재하지 않는 경우 null 반환(빈 NodeList 객체 반환)
* CSS 선택자가 문법에 맞지 않는 경우 DOMException 에러가 발생한다.
* HTML문서의 모든 요소 노드를 취득하려면 querySelectorAll 메서드에 *을 전달한다.

> id값이 있는 요소 노드를 취득할때는 getElementById를 사용하고 그외에는 querySelector, querySelectorAll를 사용하는 것을 권장한다.

```html
<div id="gil">
    <ul>
        <li></li>
        <li></li>
    </ul>
</div>

<script >
    const targetEl = document.getElementById('gil');
    //li태그의 요소들을 유사배열로 반환(이터러블)
    document.getElementsByName('li');
    targetEl.getElementsByName('li');
    //모든 태그 요소들을 유사배열로 반환
    document.getElementsByName('*');
    targetEl.getElementsByName('*');
    
    //요소가 존재하지 않는경우 빈 HTMLCollection 객체를 반환한다.
    targetEl.getElementsByName('a');
</script>
```

<br>

## **39.2.5 특정 요소 노드를 취득할 수 있는지 확인**
matches 메서드는 인수로 전달한 CSS 선택자를 특정 요소 노드로 취득할 수 있는지 확인한다.
```js
$target.matches('#fruit > .apple')//true
```

<br>

## **39.2.6 HTMLCollection과 NodeList**
HTMLCollection과 NodeList는 노드객체의 상태 변화를 실시간으로 반영하는 살아있는 객체라는 것이 중요한 특징이다.

<br>

### **`HTMLCollection`**
HTMLCollection 객체는 실시간으로 노드객체의 상태를 반영하여 요소를 제거하기때문에 for문으로 순회할때 주의해야 한다.
```html
<div class="red">A</div>
<div class="red">B</div>
<div class="red">C</div>
<script >
    //HTMLCollection은 실시간으로 반영되기 때문에
    //for문을 돌리면 2번째 배열요소를 제외한 1,3번째 배열요소만 반영된다. 
    const target = document.getElementsByClassName('red');
    for(let i = 0; i < target.length; i++){
        target[i].className = 'blue';
    }

    //for문을 역방향으로 순회하거나 while문을 쓰면 하면 해결된다.
    for(let i = target.length -1; i>=0; i--){
        target[i].className = 'blue';
    }
    
    //HTMLCollection 객체를 배열로 변환해도 해결할 수 있다.
    //고차함수(forEach, map, filter, reduce등)도 사용할 수 있다.
    [...target].forEach(el => el.className = 'blue');
</script>
```

<br>

### **`NodeList`**
HTMLCollection 객체의 부작용(실시간 반영되기 때문에 순회시 모든 노드요소가 적용 되지않을 수 있는것)을 해결하기위해 querySelectorAll 메서드를 사용하는 방법도 있다.

querySelectorAll 메서드는 DOM 컬렉션 객체인 NodeList를 반환(실시간으로 노드객체의 상태 변경을 반영하지 않음)한다.