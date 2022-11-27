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

```html
<ul id="fruits">
    <li>A</li>
    <li>B</li>
</ul>
<script >
    const $fruits = document.getElementById('fruits');
    
    //childNodes프로퍼티는 NodeList를 실시간 반영하므로
    //주의가 필요하다.
    const{childNodes} = $fruits;
    console.log(childNodes); //NodeList(5)[text, li, text, li, text]
    
    for(let i =0; i < childNodes.length; i++){
        $fruits.removeChild(childNodes[i]);
    }
    //모든 자식노드가 삭제되지 않았다.
    console.log(childNodes); //NodeList(2)[li,li]
</script>
```

> 예상과 다르게 동작할 우려가 있어 **노드객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 반환하여 사용하는 것을 권장한다.**

```js
//스프레드 문법을 이용한 자식 노드요소 삭제
[...childNodes].forEach(childNode =>{
    $fruits.removeChild(childNodes);
});
```

<br>

# **39.3 노드 탐색**
취득한 요소 노드를 기점으로 DOM 트리의 노드를 옮겨 다니며 부모, 형제, 자식 노드 등을 탐색해야 할 때가 있다.

```html
// 1. ul 요소 노드 취득
<ul id="fruit">
    //2. 자식노드 모두 탐색 or 자식노드 중 하나만 탐색
    <li class="a">A</li>
    //3. b 요소는 2개의 형제 요소와 부모 요소를 가지므로 b요소노드를 취득 후
    //형제 노드를 탐색하거나 부모 노드를 탐색한다.
    <li class="b">B</li>
    <li class="c">C</li>
</ul>
```

<img src="./img1.PNG" width="100%">

>이처럼 DOM 트리 상의 노드를 탐색할 수 있도록 Node, Element 인터페이스는 트리 탐색 프로퍼티를 제공한다.

parentNode, previousSibling, firstChild, childNodes 프로퍼티는 Node.prototype이 제공하고, 프로퍼티 키에 Element가 포함된 previousElementSibling, nextElementSibling과 children 프로퍼티는 Element.prototype이 제공한다.

📌 노드 탐색 프로퍼티는 모두 접근자 프로퍼티다. 단, 노드 탐색 프로퍼티는 setter없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티다.

<br>

## **39.3.1 공백 텍스트 노드**
HTML요소 사이의 스페이스, 탭, 줄바꿈(개행)등의 공백문자는 텍스트 노드(공백텍스트노드)를 생성한다.

<br>

## **39.3.2 자식 노드 탐색**
| 프로퍼티                                | 설명                                                         |
|:------------------------------------|:-----------------------------------------------------------|
| Node.prototype.childNodes           | 자식노드를 모두 탐색하여 NodeList에 담아 반환(텍스트 노드 포함)                   |
| Element.prototype.children          | 자식 노드 중에서 요소 노드만 모두 탐색하여HTMLCollection에 담아 반환(텍스트 노드 포함 X) |
| Node.prototype.firstChild           | 첫번째 자식 노드를 반환(텍스트노드 or 요소노드다.)                             |
| Node.prototype.lastChild            | 마지막 자식 노드를 반환(텍스트노드 or 요소노드다.)                             |
| Element.prototype.firstElementChild | 첫번째 자식 '요소' 노드를 반환(요소노드만 반환)                               |
| Element.prototype.lastElementChild  | 마지막 자식 '요소' 노드를 반환(요소노드만 반환) |

```html
<ul id="fruits">
    <li class="a">A</li>
    <li class="b">B</li>
    <li class="c">C</li>
</ul>
<script >
    const $fruit = document.getElementById('fruit');

    //NodeList(7)[text, li.a, text, li.b, text, li.c, text]
    console.log($fruit.childNodes);
    
    //HTMLCollection(3)[li.a, li.b, li.c]
    console.log($fruit.children);
    
    console.log($fruit.firstChild);//#text
    console.log($fruit.lastChild); //#text
    console.log($fruit.firstElementChild); //li.a
    console.log($fruit.lastElementChild); //li.c
</script>
```

<br>

## **39.3.3 자식 노드 존재 확인**
Node.prototype.hasChildNodes 메서드는 텍스트 노드를 포함한 자식 노드가 존재하는지 확인 한다. 

자식노드들 중 텍스트 노드가 아닌 요소 노드가 존재 하는지 확인은 children.length, childElementCount를 사용하면된다.

```js
$fruit.hasChildNodes(); //true or false
!!$fruit.children.length; //0 (false)
!!$fruit.childElementCount; //0 (false)
```

<br>

## **39.3.4 요소 노드의 텍스트 노드 탐색**
firstChild 프로퍼티는 요소노드의 텍스트 노드로 접근할 수 있다. (텍스트 노드나 요소노드를 반환)

<br>

## **39.3.5 부모 노드 탐색**
Node.prototype.parentNode 프로퍼티를 사용하여 부모노드(리프노드)를 탐색한다.

<br>

## **39.3.6 형제 노드 탐색**
아래 노드 탐색 프로퍼티는 어트리뷰트 노드를 제외한 텍스트노드, 요소노드만 반환한다.

| 프로퍼티       | 설명                                 |
|:-----------------|:-----------------------------------|
| Node.prototype.previousSibling           | 자신의 이전 형제 노드를 탐색하여 반환(텍스트노드, 요소노드) |
| Node.prototype.nextSibling               | 자신의 다음 형제 노드를 탐색하여 반환(텍스트노드, 요소노드) |
| Element.prototype.previousElementSibling | 자신의 이전 형제 '요소'노드를 탐색하여 반환(요소노드)    |
| Element.prototype.nextElementSibling     | 자신의 다음 형제 '요소'노드를 탐색하여 반환(요소노드     |

<br>

# **39.4 노드 정보 취득**

| 프로퍼티                    | 설명                                                                                                                                                                     |
|:------------------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Node.prototype.nodeType | 노드 타입을나타내는 상수를 반환한다.<br/>* Node.ELEMENT_NODE: 요소 노드 타입을 나타내는 상수 1을 반환<br/>* Node.TEXT_NODE: 텍스트 노드 타입을 나타내는 상수 3을 반환<br/>* Node.DOCUMENT_NODE: 문서 노드 타입을 나타내는 상수 9를 반환 |
| Node.prototype.nodeName | 노드의 이름을 문자열로 반환<br/>* 요소노드: 대문자 문자열로 태그이름을 반환<br/>* 텍스트 노드: 문자열 "#text"를 반환<br/>* 문서 노드: 문자열 "#document"를 반환                                                           |

```js
console.log(document.nodeType); //9
console.log(document.nodeName); // #document
console.log(idTag.nodeName); // 1
console.log(idTag.nodeType); // DIV
console.log(idTag.firstChild.nodeType); // 3
console.log(idTag.firstChild.nodeName); // #text
```

<br>

# **39.5 요소 노드의 텍스트 조작**

<br>

## **39.5.1 nodeValue**
nodeValue 프로퍼티는 setter, getter 모두 존재하는 접근자 프로퍼티다.(지금까지 살펴본 프로퍼티는 모두 읽기 전용 접근자 프로퍼티)

노드객체의 nodeValue 프로퍼티를 참조하면 노드객체의 값을 반환한다.(텍스트 노드의 텍스트 반환) 문서노드나 요소노드의 nodeValue 프로퍼티를 참조하면 null을 반환한다.

<br>

### `요소노드의 텍스트 변경 방법`<br>
1. 텍스트를 변경할 요소 노드를 취득한 다음, 취득한 요소 노드의 텍스트 노드를 탐색한다.(firstChild 프로퍼티를 사용하여 탐색)
2. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경한다.

<br>

## **39.5.2 textContent**
setter, getter 모두가 존재하는 접근자 프로퍼티다. 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 모두 취득하거나 변경한다.(HTML 마크업은 무시된다.)

콘텐츠 영역에 자식 노드가 없고 텍스트만 존재한다면 nodeValue 프로퍼티보단 textContent 프로퍼티를 사용하는것이 더 간단하다. **(nodeValue 프로퍼티를 사용하면 텍스트 노드만 취득하지 않으면 null을 반환하기 때문이다.)**

```html
<div id="target">hi!</div>

<script >
    document.getElementById('target').textContent // hi
    
    //하지만 HTML 마크업이 파싱 되지않아 태그명을 텍스트 노드(텍스트)로 인식한다.
    document.getElementById('target').textContent = 'hi! <span>there</span>';
    //hi! <span>there</span> (span 태그는 태그가 아니라 문자열이다.)
    document.getElementById('target').textContent
</script>
```

<img src="./img2.PNG">
<br>HTML 마크업은 문자열이다.

<br>

> **innerText 프로퍼티를 사용을 권장하지 않는 이유.**
> * CSS에 순종적이다. 예를 들어 visibility:hidden으로 지정된 요소노드의 텍스트를 반환하지 않는다.
> * CSS를 고려해야 하므로 textContent 프로퍼티보다 느리다.

<br>

# **39.6 DOM 조작**
DOM 조작은 성능에 영향(리플로우, 리페인트)을 주기 때문에 성능 최적화를 위해 주의해서 다루어야 한다.

<br>

## **39.6.1 innerHTML**
HTML마크업이 포함된 문자열을 그대로 반환한다. 요소노드의 모든자식노드가 제거되고 할당된 값을 HTML마크업이 파싱되어 자식노드로 DOM에 반영된다.

```js
//노드추가
target.innerHTML += '<li class="banana">A</li>';
//노드교체
target.innerHTML = '<li class="banana">A</li>';
//노드삭제
target.innerHTML = '';

//크로스 사이트 스크립팅 공격에 취약하므로 위험할 수 있다.
//HTML5는 innerHTML 프로퍼티로 삽입된 script요소 내의 js를 실행하지 않는다.
target.innerHTML = '<script>alert(document.cookie)</script>';
```

>sanitize를 사용하면 잠재적 위험을 내포한 HTML 마크업을 제거한다.
> `DOMPurify.sanitize('<a onerror="alert(document.cookie)"></a>') // => <a></a>`

**단점1.** innerHTML은 요소노드를 추가를 하더라도 기존에 있던 요소 즉 모든 요소를 다 삭제후 다시 생성하여 자식요소로 추가한다.

```js
//노드추가
//자식요소를 모두 삭제하고 다시 생성한다.
target.innerHTML += '<li class="banana">A</li>';
```

<br>

**단점2.** 새로운 요소를 삽입할 떄 위치를 지정할 수 없다.

<br>

## **39.6.2 insertAdjacentHTML 메서드**
insertAdjacentHTML(position, DOMString)메서드는 **기존 요소를 제거하지 않고, 위치를 지정해 새로운 요소를 삽입한다.**

첫번째 인수로 위치지정은 beforebegin, afterbegin, beforeend, afterend 4가지를 할당 할수 있다.

두번째 인수로 HTML 마크업 문자열을 할당한다.

<img src="./img3.PNG" width="100%">

```js
$target.insertAdjacentHTML('beforeend','<p>beforeend</p>');
```

<br>

## **39.6.3 노드 생성과 추가**