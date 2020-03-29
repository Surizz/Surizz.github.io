---
layout: post
title: Javascript 기초 강의 정리
comments: true
---

# 자바스크립트의 사용

## 태그 안에 변수 선언 시 변수는 `전역 변수`

``` javascript
<script>
// 내용...
var val1; // 전역변수
var va12; // 전역변수
</script>
```

## 그러므로 함수 안에서 쓰자!!

**그러면 전역변수 처리가 되지 않음**

``` javascript
(function(){
  //내용
  var val1; // 지역변수
})();
```

## `use strict` 를 쓰자.

``` javascript
'use strict';
```

* 꼼꼼히 체크해줌

# DOM과 스크립트

* 자바스크립트로 HTML 문서를 다룰 수 있게 브라우저 환경 제공하는 API
* 브라우저는 HTML문서를 첫줄부터 `순서대로` 파싱하며 필요한 정보를 수집하고 렌더링한다.

## 문제 상황

``` javascript
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>JS FOR ROOKIES</title>
    <script>
      document.getElementById('clickme').addEventListener(); //Error, 스크립트 파일을 인클루드해도 마찬가지
    </script>
  </head>
  <body>
    <button id="clickme"></button>
  </body>
</html>
```

* `순서대로` 진행되기 때문에 에러

### 해결 방법 1 - 스크립트의 위치를 BODY태그 끝으로 옮긴다.

``` javascript
//...
  <body>
    <button id="clickme"></button>
    <script>
      document.getElementById('clickme').addEventListener(); //OK
    </script>
//...
```

### 해결방법 2 - window의 onload이벤트를 이용해 스크립트 실행을 지연

``` javascript
//...
    <script>
      window.onload = function() {
      document.getElementById('clickme').addEventListener(); //OK
      };
    </script>
 //...
```

# DOM상의 엘리먼트 찾기

## DOM 전체에서 엘리먼트 찾기

``` javascript
document.getElementById('myId');  //returns HTMLElement or null, 1개만 반환
document.getElementsByTagName('div'); //returns HTMLCollection or null, 유사 배열 반환
document.getElementsByClassName('myClass'); //returns HTMLCollection or null, 유사 배열 반환
```

**document**

* 브라우저상의 HTML 문서를 모델링한 객체
* DOM API의 시작점

**HTMLCollection**

* 유사 배열: Array 비슷하게 사용가능하지만 Array가 아님(Array-like Object)
    * 숫자로 인덱싱 가능, length 존재
    * Array가 아니라서 Array의 API를 사용할 수 없음
    * Array의 API를 사용하려면 Array로 변환한 뒤 사용해야함
* `live` 한 컬렉션이다.
    * 중간에 변경되었을 경우, `다시 찾지 않아도` 자연스럽게 배열에 추가됨!!

## 엘리먼트 안의 엘리먼트 찾기

``` javascript
var myContentsEl = document.getElementById('myContents');
myContentsEl.getElementByTagName('A');
```

* 성능상의 이득 얻을 수 있음
* 참조 관리를 잘해야함
    * 제거가 됐을 때!!

# 엘리먼트에서 이벤트 핸들러

**적용**

``` javascript
element.addEventListener("click", function() {
  // Your code's here
}, false);
```

**해제**

``` javascript
element.removeEventListener("click", handler, false);
```

**Prameters**

1. 이벤트명 ex) click, focus, mouseover, etc..
2. 이벤트 핸들러
3. 캡처링을 사용할지 여부

**event 핸들러에서 `this`는??**

* `element`가 된다.

# 엘리먼트 값 조작

## textbox

**html**

``` html
<input type="text" id="myInput" />
```

**js**

``` javascript
var input = document.getElementById('myInput');

input.value = "값을 설정한다"
console.log(input.value) // 값을 읽어온다.
input.type // input의 타입을 가져온다. "text" (스펙상 정의된 HTML 속성은 모두 접근가능)
input.focus() // 포커스를 준다.
input.blur() //포커스를 해제한다.
// etc..
```

## checkbox

**html**

``` javascript
<input type="checkbox" value="someValue" checked />
```

**js**

``` javascript
input.value // 값
input.checked // 체크여부(radio, checkbox)
input.type // HTML 어트리뷰트에 접근한다.(스펙상 정의된 어트리뷰트)
input.focus() // 포커스를 준다.
input.blur() //포커스를 해제한다.
// etc...
```

# 동적 엘리먼트 만들기

## 엘리먼트 만들기

**document.createElement(tagName)**

``` javascript
var newDiv = document.createElement("div");
var newP = document.createElement("p");
```

**document.createTextNode(text)**

``` javascript
var newText = document.createTextNode("텍스트 노드");
var newText2 = document.createTextNode("HELLO WORLD");
```

**만들고 BODY 엘리먼트에 붙이지 않으면 화면에 나타나지 않는다.**

## 엘리먼트에 자식 엘리먼트 추가하기

**element.appendChild(node)**

``` javascript
var newDiv = document.createElement("div");
var newP = document.createElement("p");
var newText = document.createTextNode("HELLO ROOKIES");

newP.appendChild(newText); // text를 p 태그에 넣기
newDiv.appendChild(newP); // div에 p태그 넣기

document.body.appendChild(newDiv); // div를 body에 붙이기
```

**결과**

``` html
<body>
  <div>
    <p>HELLO ROOKIES</p>
  </div>
</body>
```

``` javascript
document.body.childNodes[0].tagName == 'DIV';
document.body.childNodes[0].childNodes[0].tagName == 'P';
document.body.childNodes[0].childNodes[0].childNodes[0].nodeValue == 'HELLO ROOKIES';
```

## 자식 엘리먼트 삭제하기

**element.removeChild(node);**

``` javascript
var newDiv = document.createElement("div");
var newP = document.createElement("p");
var newText = document.createTextNode("HELLO ROOKIES");

newP.appendChild(newText);
newDiv.appendChild(newP);

document.body.appendChild(newDiv); //추가했지만
document.body.removeChild(newDiv); // 바로 삭제됨
```

**부모를 알고 자신을 알면 지우기가 쉽다!!**

## innerHTML으로 엘리먼트 만들기

``` javascript
element.innerHTML = '<div>text</div>';
// html 텍스트를 이용해 자식 노드를 생성
document.body.innerHTML = "<div><p>HELLO ROOKIES</p></div>";
<body>
  <div>
    <p>HELLO ROOKIES</p>
  </div>
</body>
```

**장점**

* 새로운 엘리먼트들을 만드는 상황에서는 보통 제일 빠름(엔진 내부에서 생성)
* DOM API를 이용하는것보다 편한 경우가 많다.

**단점**

* 엘리먼트에 작은 변화를 주는 경우 비효율적(돔트리를 `모두 지우고` 다시 만듬)
    * html 스트링을 미리 만든 후, 할당은 `1번만` 하자.
* 이벤트 처리하기가 힘듬

**할당할 때 주의할 점**

값이 세팅 될때마다 모든 자식 엘리먼트를 지우고 다시 만들기 때문에 Addition assginment(+=) 연산자를 직접 사용하는것을 피해야 한다.

``` javascript
//wrong
div.innerHTML = '<p>text1</p>';
    // 자식으로 <p>text1</p> 을 만든다.
div.innerHTML += '<p>text2</p>';
    // innerHTML === '<p>text1</p><p>text2</p>'
    // 모두 지우고 <p>text1</p> 다시만들고 <p>text2</p>도 만든다.

//correct
var content = '';

// HTML 스트링을 미리 만든다.
content += '<p>text1</p>'; // 루프에서 사용된다고 가정
content += '<p>text2</p>';

div.innerHTML = content; // innerHTML 할당은 한번만
```

## textContent

* 텍스트 노드를 변경하고 싶을 때 사용
* innerHTML과 비슷하지만 텍스트 노드를 생성하거나 참조할 수 있다.
* 자식엘리먼트들의 `모든` 텍스트 노드를 확인할 수 있다.

``` javascript
element.textContent = 'new text content';
console.log(element.textContent);
```

# 이벤트의 전파

## 이벤트 전파 순서

``` html
<body>
  <ul>
    <li><input type="checkbox" /> TODO</li>
  </ul>
</body>
```

이벤트는 특정한 방향으로 전파된다

* 위에서 아래로: `Capturing`
    * body -> ul -> li -> input
    * 하나씩 Event가 있는 지 확인
* 아래에서 위로: `Bubbling`
    * input -> li -> ul -> body
* 순서: 캡처링 -> 실제 대상 엘리먼트 -> 버블링
* 그렇기 때문에 체크박스를 눌러도 적용되지 않았던 것

## 이벤트 전파 제어

* `eventObject` 안에 캡쳐링, 버블링 취소 가능 메소드가 있다
* `addEventListener`의 세번째 인자로 캡처링/버블링 이벤트를 선택할 수 있다.
    * `true` 면 캡처링
    * `false` 면 버블링(default)
* 이벤트 핸들러에 첫번째 인자로 넘어오는 Event객체로 전파를 차단할 수 있다.

``` javascript
var useCapturing = true;

element.addEventListener('click', function(eventObject) {
  eventObject.stopPropagation(); // 캡처링이나 버블링을 취소한다.(이벤트 전파를 차단한다)

  //etc
  eventObject.preventDefault(); // 디폴트 동작을 취소한다.(ex. 링크 이동 차단)
  //체크 박스의 클릭을 취소하는경우 브라우저마다 다른 동작을 한다.
  //그밖에 많에 정보를 포함
}, useCapturing);
```

# CSS 제어

## 엘리먼트 객체의 style 어트리뷰트 이용

``` javascript
// css 속성을 그대로 사용함
element.style.width = '30px';
element.style.height = '100px';

// 실제 구조상에서 제외
element.style.display = 'block';
element.style.display = 'none';

// background-color같이 하이픈으로 이어진 속성은 카멜케이스로 변경
element.style.backgroundColor = '#f00';

// border같은 short-hand 속성도 그대로 사용함.
element.style.border = "1px solid red";

// 적용 가능 목록 확인
console.log(element.style);
```

## 엘리먼트에 CSS 아이디&클래스 적용

``` html
<style>
  .myClass {border:1px solid #f00}
  #myId {padding: 5px}
</style>
```

상위에 쓰면 적용 가능

``` javascript
//HTML의 class속성과 동일
element.className = 'myClass';

//다중 적용시 띄어쓰기
element.className = 'myClass1 myClass2';

//아이디도 동일하게 적용가능(다중 적용 X)
element.id = 'myId';
```

* **아이디** 는**한 개**의 엘리먼트에만 지정이 가능하며 엘리먼트당 한 개만 적용된다.
* 클래스 는 여러개의 엘리먼트에 적용이 가능하고 한 엘리먼트에 다중 적용이 가능하다.
* 디자인 적용은 클래스를 이용하는것을 추천(재사용)
* 아이디는 엘리먼트를 찾기 위한 식별 용도로만 사용한다.
    * 많이 안씀

## CSS 셀렉터

``` html
<style>
  .hideIncomplete li:not(.complete) {display:none}
  .hideComplete .complete {display:none}
</style>

<!--중략-->

<ul id="todolist" class="hideComplete">
  <li>TODO1</li>
  <li class="complete">TODO2</li>
  <li>TODO3</li>
</ul>
```

* 엘리먼트에 class 속성을 주면 css가 적용됨

# querySelector

``` javascript
document.querySelector('.myClass'); // 처음 찾은 한개만 리턴
document.querySelectorAll('.myClass'); // 해당되는 모든 엘리먼트 찾음
```

* element, document 모두 사용가능
* CSS 셀렉터를 이용해 엘리먼트를 찾는다.
* jQuery와 비슷
* 최신브라우저들과 IE8 이상부터 지원

# 엘리먼트 노드 속성

* DOM상의 엘리먼트들은 트리구조로 서로 연결되어 있다.
* 그래서 엘리먼트의 노드관련 속성들로 연결되어있는 엘리먼트들을 탐색할 수 있다.

``` javascript
element.firstChild // 첫번째 자식
element.lastChild // 마지막 자식
element.parentNode // 부모
element.nextSibling // 다음 형제
element.previousSibling  // 이전 형제
element.childNodes // 자식들을 모두 담고 있는 HTMLCollection
```

# DOM API 정보 검색 활용법

**Mozilla developer network**

* [http://developer.mozilla.org](http://developer.mozilla.org)
* 웹플랫폼 -> API와 DOM매뉴에서 element와 node파트 확인
* 웹 기술에 대한 거의 모든 정보를 얻을 수 있다.

---
참고: NHN 기술교육 - JavaScript 기초