# DOM (Document Object Model)
* **DOM (Document Object Model)은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조**

## 노드

### 1. HTML 요소와 노드 객체
* HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환
    * HTML 요소 어트리뷰트 -> 어트리뷰트 노드
    * HTML 요소 텍스트 콘텐츠 -> 텍스트 노드

![HTML_Element_Node_Object drawio](https://user-images.githubusercontent.com/63139527/184782475-4871f680-ffcb-4d3e-9a05-52f1ca119a78.png)

* HTML 문서의 구성 요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성

#### 1.1 트리 자료 구조
* 트리 자료 구조 = 노드들의 계층 구조
* 비선형 자료 구조
* 루트 노드 : 하나의 최상위 노드에서 시작
    * 최상위 노드는 부모 노드가 없음
    * 0개 이상의 자식 노드를 가짐
* 리프 노드 : 자식 노드가 없는 노드
* 노드 객체들로 구성된 트리 자료구조 = DOM (= DOM 트리)

![tree_data_structrue drawio](https://user-images.githubusercontent.com/63139527/184783369-fb506b93-bb4d-4368-9fce-34bbb29fa33c.png)

### 2. 노드 객체의 타입
* 렌더링 엔진 : HTML 문서 -- 파싱 --> DOM생성
```html
<!-- HTML 문서 -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <link rel="stylesheet" href="style.css">
    </head>
    <body>
        <ul>
            <li id="apple">Apple</li>
            <li id="banana">Banana</li>
            <li id="orange">Orange</li>
        </ul>
        <script src="app.js"></script>
    </body>
</html>
```
* **참고 : 공백 텍스트 노드 생략한 그림**
![DOM drawio](https://user-images.githubusercontent.com/63139527/184785738-4ba57a0b-9f4e-48e1-934a-b10cd448aeba.png)
* DOM은 노드 객체의 계층적인 구조로 구성
* 노드 객체는 총 12개 종류(노트 타입)

#### 2.1 문서 노드(document node)
* DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴
* 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체
    * window의 document 프로퍼티에 바인딩됨
* HTML 문서당 document 객체는 유일
* DOM 트리의 노드들에 접근하기 위한 진입점 역할 담당
    * 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 한다.

#### 2.2 요소 노드(element node)
* HTML 요소를 가리키는 객체
* HTML 요소 간의 중첩에 의해 부자 관계를 가짐
* 요소 노드는 문서의 구조를 표현

#### 2.3 어트리뷰트 노드(attribute node)
* HTML 요소의 어트리뷰트를 가리키는 객체
* 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있다.
* 어트리뷰트 노드에 접근하여 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 한다.

#### 2.4 텍스트 노드(text node)
* HTML 요소의 텍스트를 가리키는 객체
* 요소 노드의 자식 노드
* 자식 노드를 가질 수 없는 리프 노드
    * DOM 트리의 최종단
* 텍스트 노드에 접근하려면 부모 노드인 요소 노드에 접근해야 한다.

#### 2.5 그 외 노드
* Comment 노드 : 주석을 위한 노드
* DocumentType 노드 : DOCTYPE을 위한 노드
* DocumentFragment 노드 : 복수의 노드를 생성하여 추가할 떄 사용 
* 기타 등등..

### 3. 노드 객체의 상속 구조
* DOM을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API를 사용할 수 있다.
* 노드 객체는 자신의 부모, 형제, 자식을 탐색할 수 있으며, 자신의 어트리뷰트와 텍스트를 조작할 수도 있다.
* DOM을 구성하는 노드 객체는 브라우저 환경에서 추가적으로 제공하는 호스트 객체
    * 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 가짐

![Node_Object_Inheritance drawio](https://user-images.githubusercontent.com/63139527/184801509-453f2037-07e6-4285-91ff-eba1b81eb605.png)

## 요소 노드 취득
* DOM은 요소 노드를 취득할 수 있는 다양한 메서드를 제공

### 1. id를 이용한 요소 노드 취득
* `Document.prototype.getElementById` 메서드는 인수로 전달한 **id 어트리뷰트 값**을 갖는 하나의 요소 노드를 탐색하여 반환
* 인수 id 값은 HTML 문서 내에서 유일한 값
* 단 하나의 요소 노드를 반환
    * 중복된 id값을 갖는 요소가 여러 개 존재할 경우 첫 번째 요소 노드만 반환
* 전달된 id값을 갖는 HTML 요소가 존재하지 않는 경우 null 반환
* id값과 동일한 이름의 전역 변수가 암묵적으로 선언되고 해당 노드 객체가 할당되는 부수 효과 가짐
* 전역 변수가 이미 선언되어 있으면 전역 변수에 노드 객체가 재할당되지 않음

### 2. 태그 이름을 이용한 요소 노드 취득
* `Document.prototype/Element.prototype.getElementsByTagName` 메서드는 인수로 전달한 **태그 이름**을 갖는 모든 요소 노드들을 탐색하여 반환
* 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환
    * HTMLCollection 객체는 유사 배열 객체이면서 이터러블
* 하나의 값만 반환할 수 있으므로 여러 개의 값을 반환하려면 배열이나 객체와 같은 자료구조에 담아 반환해야 한다.
* 모든 요소 노드를 취득하려면 인수로 '*' 전달
* `Document.prototype.getElementsByTagName` 메서드는 DOM 전체에서 요소 노드를 탐색하여 반환
* `Element.prototype.getElementsByTagName` 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환
* 전달된 태그 이름을 갖는 요소가 존재하지 않는 경우 빈 HTMLCollection 객체 반환

### 3. class를 이용한 요소 노드 취득
* `Document.prototype/Element.prototype.getElementsByClassName` 메서드는 인수로 전달한 **class 어트리뷰트 값**을 갖는 모든 요소 노드들을 탐색하여 반환
* class 값은 공백으로 구분하여 여러 개의 class를 지정 가능
* 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 HTMLCollection 객체를 반환
* `Document.prototype.getElementsByClassName` 메서드는 DOM 전체에서 요소 노드를 탐색하여 반환
* `Element.prototype.getElementsByClassName` 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환
* 전달된 class 값을 갖는 요소가 존재하지 않는 경우 빈 HTMLCollection 객체 반환

### 4. CSS 선택자를 이용한 요소 노드 취득
* css 선택자는 스타일을 적용하고자 하는 HTML 요소를 특정할 때 사용하는 문법
```css
/* 전체 선택자 : 모든 요소를 선택 */
* { ... }
/* 태그 선택자 : 모든 p 태그 요소를 모두 선택 */
p { ... }
/* id 선택자 : id값이 'foo'인 요소를 모두 선택 */
#foo { ... }
/* class 선택자 : class 값이 'foo'인 요소를 모두 선택 */
.foo { ... }
/* 어트리뷰트 선택자 : input 요소 중에 type 어트리뷰트 값이 'text'인 요소를 모두 선택 */
input[type=text] { ... }
/* 후손 선택자 : div 요소의 후손 요소 중 p 요소를 모두 선택 */
div p { ... }
/* 자식 선택자 : div 요소의 자식 요소 중 p 요소를 모두 선택 */
div > p { ... }
/* 인접 형제 선택자 : p 요소의 형제 요소 중에 p 요소 바로 뒤에 위치하는 ul 요소를 선택 */
p + ul { ... }
/* 일반 형제 선택자 : p 요소의 형제 요소 중에 p 요소 뒤에 위치하는 ul 요소를 모두 선택 */
p ~ ul { ... }
/* 가상 클래스 선택자 : hover 상태인 a 요소를 모두 선택 */
a:hover { ... }
/* 가상 요소 선택자 : p 요소의 콘텐츠의 앞에 위치하는 공간을 선택
                일반적으로 content 프로퍼티와 함께 사용됨 */
p::before { ... }
```
* `Document.prototype/Element.prototype.querySelector` 메서드는 인수로 전달한 **CSS 선택자**를 만족시키는 하나의 요소 노드를 탐색하여 반환
    * 여러 개인 경우 첫 번째 요소 노드만 반환
    * 존재하지 않는 경우 null 반환
    * 문법에 맞지 않는 경우 DOMException 에러 발생

* `Document.prototype/Element.prototype.querySelectorAll` 메서드는 인수로 전달한 **CSS 선택자**를 만족시키는 모든 요소 노드를 탐색하여 반환
* 여러 개의 요소 노드 객체를 갖는 DOM 컬렉션 객체인 NodeList 객체를 반환
    * NodeList 객체는 유사 배열 객체이면서 이터러블
* 요소가 존재하지 않는 경우 빈 NodeList 객체 반환
* 문법에 맞지 않는 경우 DOMException 에러가 발생
* 모든 요소 노드를 취득하려면 인수로 전체 선택자 '*' 전달
* `Document.prototype.querySelector/querySelectorAll` 메서드는 DOM 전체에서 요소 노드를 탐색하여 반환
* `Element.prototype.querySelector/querySelectorAll` 메서드는 특정 요소 노드를 통해 호출하며, 특정 요소 노드의 자손 노드 중에서 요소 노드를 탐색하여 반환
* querySelector, querySelectorAll 메서드는 성능이 느리지만, CSS 선택자 문법을 사용하여 구체적인 조건으로 요소 노드를 취득 가능하다는 장점
* id 어트리뷰트가 있는 요소 노드 취득 경우 getElementById 사용하고 그 외의 경우엔 querySelector, querySelectorAll 메서드 사용 권장

### 5. 특정 요소 노드를 취득할 수 있는지 확인
* `Element.prototype.matches` 메서드는 인수로 전달한 **CSS 선택자**를 통해 특정 요소 노드를 취득할 수 있는지 확인 (true/false)
* 이벤트 위임을 사용할 때 유용

### 6. HTMLCollection과 NodeList
* HTMLCollection과 NodeList는 DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체
* HTMLCollection과 NodeList는 유사 배열 객체이면서 이터러블
    * for...of 문으로 순회 가능
    * 스프레드 문법 사용하여 간단히 배열로 변환 가능
* 중요한 특징 : 노드 객체의 상태 변화를 실시간으로 반영하는 **살아 있는 객체**(live)
    * HTMLCollection : 항상 live 객체로 동작
    * NodeList : 대부분 non-live 객체로 동작 / 경우에 따라 live 객체로 동작

#### 6.1 HTMLCollection
* getElementsByTagName, getElementsByClassName 메서드는 HTMLCollection 객체 반환
* 실시간으로 노드 객체의 상태 변경을 반영하여 요소를 제거할 수 있기 때문에  
HTMLCollection 객체를 for문으로 순회하면서 노드 객체의 상태를 변경해야 할 때 주의 해야한다.
    * for문을 역방향으로 순회하는 방법으로 회피 가능
    * while문을 사용하여 HTMLCollection 객체에 노드 객체가 남아 있지 않을 때까지 무한 반복하는 방법으로 회피 가능
    * HTMLCollection 객체를 배열로 변환하여 배열의 고차 함수(forEach, map, filter, reduce 등)를 사용

#### 6.2 NodeList
* querySelectorAll 메서드는 NodeList 객체 반환
* NodeList 객체는 NodeList.prototype 메서드를 상속받아 사용할 수 있다.
    * forEach, item, entries, keys, values 메서드 제공
    * Array.prototype.forEach 메서드와 사용방법이 동일
* **childNodes 프로퍼티가 반환하는 NodeList 객체는 HTMLCollection 객체와 같이 실시간으로 노드 객체의 상태 변경을 반영하는 live 객체로 동작하므로 주의가 필요**
* **노드 객체의 상태 변경과 상관없이 안전하게 DOM 컬렉션을 사용하려면 HTMLCollection이나 NodeList 객체를 배열로 변환하여 사용하는 것을 권장**

## 노드 탐색
* 노드 탐색 프로퍼티는 모두 접근자 프로퍼티이다.
* setter 없이 getter만 존재하여 참조만 가능한 읽기 전용 접근자 프로퍼티이다.
![tree_node_traversing drawio](https://user-images.githubusercontent.com/63139527/184816680-215f72bc-e985-4946-a211-0b3ededdcd14.png)
* Node.prototype 제공하는 프로퍼티
    * parentNode
    * previousSibling / nextSibling
    * firstChild / lastChild
    * childNodes
* Element.prototype 제공하는 프로퍼티
    * previousElementSibling / nextElementSibling
    * firstElementChild / lastElementChild
    * children

### 1. 공백 텍스트 노드
* HTML 요소 사이의 스페이스, 탭, 줄바꿈(개행) 등의 공백 문자는 텍스트 노드를 생성 = 공백 텍스트 노드
```html
<!-- HTML 문서 -->
<!DOCTYPE html>
<html>
    <body>
        <ul id="fruits">
            <li id="apple">Apple</li>
            <li id="banana">Banana</li>
            <li id="orange">Orange</li>
        </ul>
    </body>
</html>
```
![space_text_node drawio](https://user-images.githubusercontent.com/63139527/184819735-e594f1c0-0405-44b5-986d-1ecf1aa05f80.png)

### 2. 자식 노드 탐색
* 자식 노드를 탐색하기 위해서 다음과 같은 노드 탐색 프로퍼티를 사용

프로퍼티|설명
-|-
Node.prototype.childNodes|자식 노드를 모두 탐색하여 DOM 컬렉션 객체인 NodeList에 담아 반환.</br>요소 노드뿐만 아니라 텍스트 노드도 포함되어 있을 수 있다.
Element.prototype.children|자식 노드 중에서 요소 노드만 모두 탐색하여 DOM 컬렉션 객체인 HTMLCollection에 담아 반환.</br>텍스트 노드가 포함되지 않는다.
Node.prototype.firstChild|첫 번째 자식 노드를 반환.</br>텍스트 노드이거나 요소 노드이다.
Node.prototype.lastChild|마지막 자식 노드를 반환.</br>텍스트 노드이거나 요소 노드이다.
Element.prototype.firstElementChild|첫 번째 자식 요소 노드를 반환.</br>요소 노드만 반환
Element.prototype.lastElementChild|마지막 자식 요소 노드를 반환.</br>요소 노드만 반환

### 3. 자식 노드 존재 확인
* `Node.prototype.hasChildNodes` 메서드 사용
    * 존재 유무를 불리언 값으로 반환
* hasChildNodes 메서드는 childNodes 프로퍼티와 마찬가지로 텍스트 노드를 포함하여 자식 노드의 존재를 확인
* 자식 노드 중에 텍스트 노드가 아닌 요소 노드가 존재하는지 확인 방법
    * children.length
    * Element 인터페이스의 childElementCount 프로퍼티

### 4. 요소 노드의 텍스트 노드 탐색
* 요소 노드의 텍스트 노드는 요소 노드의 자식 노드이다.
* firstChild 프로퍼티로 접근할 수 있다.
* firstChild 프로퍼티는 첫 번째 자식 노드를 반환
* 반환환 노드는 텍스트 노드이거나 요소 노드이다.

### 5. 부모 노드 탐색
* Node.prototype.parentNode 프로퍼티를 사용
* 텍스트 노드는 DOM 트리의 최종단 노드인 리프 노드이므로 부모 노드가 텍스트 노드인 경우는 없다.

### 6. 형제 노드 탐색
* 형제 노드를 탐색하기 위해서 다음과 같은 노드 탐색 프로퍼티를 사용
* 어트리뷰트 노드는 요소 노드와 연결되어 있지만 부모 노드가 같은 형제 노드가 아니기 때문에 반환되지 않음
* 텍스트 노드 또는 요소 노드만 반환

프로퍼티|설명
-|-
Node.prototype.previousSibling|자신의 이전 형제 노드를 탐색하여 반환.</br>요소 노드뿐만 아니라 텍스트 노드일 수도 있다.
Node.prototype.nextSibling|자신의 다음 형제 노드를 탐색하여 반환.</br>요소 노드뿐만 아니라 텍스트 노드일 수도 있다.
Element.prototype.previousElementSibling|자신의 이전 형제 요소 노드를 탐색하여 반환.</br>요소 노드만 반환.
Element.prototype.nextElementSibling|자신의 다음 형제 요소 노드를 탐색하여 반환.</br>요소 노드만 반환.

## 노드 정보 취득
* 노드 객체에 대한 정보를 취득하려면 다음과 같은 노드 정보 프로퍼티를 사용

프로퍼티|설명
-|-
Node.prototype.nodeType|노드 객체의 종류, 즉 노드 타입을 나타내는 상수를 반환</br> - Node.ELEMENT_NODE : 요소 노드 타입, 상수 1 반환</br> - Node.TEXT_NODE : 텍스트 노드 타입, 상수 3 반환</br> - Node.DOCUMENT_NODE : 문서 노드 타입, 상수 9 반환
Node.prototype.nodeName|노드의 이름을 문자열로 반환</br> - 요소 노드 : 대문자 문자열로 태그 이름("UL", "LI" 등)을 반환</br> - 텍스트 노드 : 문자열 "#text"를 반환</br> - 문서 노드 : 문자열 "#document"를 반환

## 요소 노드의 텍스트 조작

### 1. nodeValue
* `Node.prototype.nodeValue` 프로퍼티는 setter와 getter 둘다 존재하는 접근자 프로퍼티
    * 참조와 할당 모두 가능
* 노드 객체의 nodeValue 프로퍼티를 **참조**하면 노드 객체의 값을 반환
    * 노드 객체의 값?  텍스트 노드의 텍스트
    * 텍스트 노드가 아닌 문서 노드나 요소 노드의 nodeValue 프로퍼티를 참조하면 null 반환
* 텍스트 노드의 nodeValue 프로퍼티에 값을 **할당**하면 텍스트 노드의 값, 텍스트 변경 가능
    * 요소 노드의 텍스트를 변경하려면 다음과 같은 순서 처리 필요
        1. 텍스트를 변경할 요소 노드 취득
        2. 요소 노드의 텍스트 노드 탐색
            * 텍스트 노드는 요소 노드의 자식 노드이므로 firstChild 프로퍼티를 사용하여 탐색
        3. 탐색한 텍스트 노드의 nodeValue 프로퍼티를 사용하여 텍스트 노드의 값을 변경

### 2. textContent
* `Node.prototype.textContent` 프로퍼티는 setter와 getter 둘다 존재하는 접근자 프로퍼티
* 요소 노드의 textContent 프로퍼티를 **참조**하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이) 내의 텍스트를 모두 반환
    * 요소 노드의 childNodes 프로퍼티가 반환한 모든 노드들의 텍스트 노드의 값, 텍스트를 모두 반환
    * HTML 마크업은 무시
* 요소 노드의 textContent 프로퍼티에 문자열을 **할당**하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열이 텍스트로 추가된다.
    * 할당한 문자열에 HTML 마크업이 포함되어 있더라도 문자열 그대로 인식되어 텍스트로 취급됨 (HTML 마크업 파싱되지 않음)

* 참고 : textContent 프로퍼티와 유사한 동작을 하는 innerText 프로퍼티가 있다.
* innerText 프로퍼티는 다음과 같은 이유로 사용하지 않는 것이 좋다.
    * innerText 프로퍼티는 CSS에 순종적이다.
        * 예) innerText 프로퍼티는 CSS에 의해 비표시(visibility: hidden;)로 지정된 요소 노드의 텍스트를 반환하지 않는다.
    * innerText 프로퍼티는 CSS를 고려해야 하므로 textContent 프로퍼티보다 느리다.

## DOM 조작
* 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체하는 것
* DOM 조작에 의해 리플로우와 리페인트가 발생하는 원인이 되므로 성능에 영향을 준다.
* DOM 조작은 성능 최적화를 위해 주의해서 다루어야 한다.

### 1. innerHTML
* `Element.prototype.innerHTML` 프로퍼티는 setter와 getter 둘다 존재하는 접근자 프로퍼티
* 요소 노드의 innerHTML 프로퍼티를 참조하면 요소 노드의 콘텐츠 영역(시작 태그와 종료 태그 사이)내에 포함된 모든 HTML 마크업을 문자열로 반환
```
textContent와 innerHTML 다른점
 - textContent는 HTML 마크업을 무시하고 텍스트만 반환
 - innerHTML는 HTML 마크업이 포함된 문자열을 그대로 반환
```
* 요소 노드의 innerHTML 프로퍼티에 문자열을 할당하면 요소 노드의 모든 자식 노드가 제거되고 할당한 문자열에 포함되어 있는 HTML 마크업이 파싱되어 요소 노드의 자식 노드로 DOM에 반영

* innerHTML 프로퍼티에 할당 기능으로 **크로스 사이트 스크립팅 공격**(XSS)에 취약해져 위험하다
```javascript
// 스크립트 태그를 삽입하여 사용자 정의 자바스크립트 실행하도록 함
document.getElementById('foo').innerHTML = '<script>alert(document.cookie)</script>';
```
* HTML5 에서는 innerHTML 프로퍼티로 삽입된 script 요소 내의 자바스크립트 코드를 실행하지 않도록 변경됨
* 하지만 script 요소 없이도 크로스 사이트 스크립팅 공격은 가능하다. (모던 브라우저에서도 동작 한다.)
```javascript
// 에러 이벤트를 강제로 발생시켜서 자바스크립트 코드가 실행되도록 한다.
document.getElementById('foo').innerHTML = '<img src="x" onerror="alert(document.cookie)">';
```

* 단점1 : innerHTML 프로퍼티를 사용한 DOM 조작은 구현이 간단하고 직관적이라는 장점이 있지만 XSS 공격에 취약한 단점도 있음
* **HTML 새니티제이션(HTML sanitization)**
    * 사용자로부터 입력받은 데이터에 의해 발생할 수 있는 크로스 사이트 스크립팅 공격을 예방하기 위해 잠재적 위험을 제거하는 기능
    * DOMPurify 라이브러리를 사용하는 것을 권장
    ```javascript
    // DOMPurify는 잠재적 위험을 내포한 HTML 마크업을 새니티제이션(살균)하여 잠재적 위험을 제거한다.
    DOMPurify.sanitize('<img src="x" onerror="alert(document.cookie)">');
    // => <img src="x">
    ```

* 단점2 : 요소 노드의 innerHTML 프로퍼티에 HTML 마크업 문자열을 할당하는 경우 요소 노드의 모든 자식 노드를 제거하고 할당한 HTML 마크업 문자열을 파싱하여 DOM 변경
* 아래 예제 처럼 기존 li.apple 수정없이 li.banana만 새롭게 추가하고 싶지만  
innerHTML는 무조건 모든 자식 노드를 제거하고 새롭게 요소 노드 li.apple와 li.banana를 생성하여  
fruits 요소의 자식 요소로 추가한다.
* 이러한 방식은 효율적이지 않다.
```html
<ul id="fruits">
    <li id="apple">Apple</li>
</ul>
<script>
    const $fruits = document.getElementById('fruits');
    // 기존 fruits 요소에 자식 요소로 추가 (과연..? 생각대로 될까?)
    $fruits.innerHTML += '<li id="banana">Banana</li>';
<script>
```
* 단점3 : 새로운 요소를 삽입할 때 삽입될 위치를 지정할 수 없다
* 아래 예제에서 li.apple와 li.orange 사이에 새로운 요소를 삽입하고 싶지만 삽입 위치를 지정할 수 없다.
```html
<ul id="fruits">
    <li id="apple">Apple</li>
    <li id="orange">Orange</li>
</ul>
```
* 결론 : 복잡하지 않은 요소를 새롭게 추가할 때는 유용함 / 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입해야 할 때는 사용하지 않는 것을 권장

### 2. insertAdjacentHTML 메서드
* `Element.prototype.insertAdjacentHTML`(position, DOMString) 메서드는 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 삽입
* 두 번째 인수(DOMString)로 전달한 HTML 마크업 문자열을 파싱하고 그 결과로 생성된 노드를
* 첫 번째 인수(position)로 전달한 위치에 삽입하여 DOM에 반영한다.
    * beforebegin
    * afterbegin
    * beforeend
    * afterend

![insertAdjacentHTML drawio](https://user-images.githubusercontent.com/63139527/185050295-2cfcba25-77d1-40e7-9036-5e6cc3d5057a.png)
```html
<html>
<!-- beforebegin -->
<div id="foo">
    <!-- afterbegin -->
    text
    <!-- beforeend -->
</div>
<!-- afterend -->
</html>
<script>
    const $foo = document.getElementById('foo');

    $foo.insertAdjacentHTML('beforebegin', '<p>beforebegin</p>');
    $foo.insertAdjacentHTML('afterbegin', '<p>afterbegin</p>');
    $foo.insertAdjacentHTML('beforeend', '<p>beforeend</p>');
    $foo.insertAdjacentHTML('afterend', '<p>afterend</p>');
</script>
```
* 기존의 자식 노드를 모두 제거하고 다시 처음부터 새롭게 자식 노드를 생성하여 자식 요소로 추가하는 innerHTML 프로퍼티보다 효율적이고 빠르다.
* insertAdjacentHTML 메서드는 HTML 마크업 문자열을 파싱하므로 크로스 사이트 스크립팅 공격에 취약하다.
### 3. 노드 생성과 추가
#### 3.1 요소 노드 생성
* `Document.prototype.createElement`(tagName) 메서드는 요소 노드를 생성
    * 매개변수 tagName에는 태그 이름을 나타내는 문자열을 인수로 전달
* createElement 메서드는 요소 노드를 생성할 뿐 DOM에 추가하지 않는다.
    * 생성된 요소 노드를 DOM에 추가하는 처리가 별도로 필요함!

```javascript
const $li = document.createElement('li');
```
#### 3.2 텍스트 노드 생성
* `Document.prototype.createTextNode`(text) 메서드는 텍스트 노드를 생성하여 반환
    * 매개변수 text에는 텍스트 노드의 값으로 사용할 문자열을 인수로 전달
* createTextNode 메서드는 텍스트 노드를 생성할 뿐 요소 노드에 추가하지 않는다.
    * 생성된 텍스트 노드를 요소 노드에 추가한느 처리가 별도로 필요함!

```javascript
const textNode = document.createTextNode('Banana');
```
#### 3.3 텍스트 노드를 요소 노드의 자식 노드로 추가
* `Node.prototype.appendChild`(childNode) 메서드는 매개변수 childNode에게 인수로 전달한 노드를 **appendChild 메서드를 호출한 노드의 마지막 자식 노드로 추가**한다.
* 요소 노드와 텍스트 노드는 부자관계로 연결됨
* 하지만 DOM에는 추가되지 않음
```javascript
// 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
$li.appendChild(textNode);

// $li 요소 노드에 자식 노드가 하나도 없는 경우 동일한 코딩 방법
$li.textContent = 'Banana';
// (주의) $li 요소 노드에 자식 노드가 있는 경우엔 사용하지 말것! 
```
#### 3.4 요소 노드를 DOM에 추가
* `Node.prototype.appendChild`(childNode) 메서드를 사용하여 DOM에 추가되어 있는 요소 노드의 마지막 자식 요소로 추가하면 됨
* DOM에 추가되면 DOM이 변경되므로 리플로우와 리페인트가 실행된다.
```html
<ul id='fruits'>
    <li>Apple</li>
</ul>
<script>
    const $fruits = document.getElementById('fruits');
    // $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($li);
</script>
```
### 4. 복수의 노드 생성과 추가
* 3개 요소 노드 생성 -> DOM 3번 추가 = DOM 3번 변경 => 리플로우와 리페인트 3번 실행
    * DOM 변경하는 것은 높은 비용이 드는 처리
        * 횟수를 줄이는것이 성능 향상에 유리
    * **기존 DOM에 요소 노드를 반복하여 추가하는 것은 비효율적**이다.

```html
<ul id="fruits"></ul>
<script>
    const $fruits = document.getElementById('fruits');
    ['Apple','Banana','Orange'].forEach(text => {
        // 1. 요소 노드 생성
        const $li = document.createElement('li');
        // 2. 텍스트 노드 생성
        const textNode = document.createTextNode(text);
        // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
        $li.appendChild(textNode);
        // 4. $li 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
        $fruits.appendChild($li);
    });
</script>
```

* 여러번 변경하는 문제를 회피하기 위해 **컨테이너 요소를 사용**
* 컨테이너 요소 생성 -> DOM에 추가해야할 3개 요소 노드를 컨테이너 요소에 자식 노드로 추가  
-> 컨테이너 요소를 DOM에 요소 노드 자식으로 추가
    * **DOM 1번 변경 -> 성능 장점**
    * **불필요한 컨테이너 요소 추가 -> 단점**

```html
<ul id="fruits"></ul>
<script>
    const $fruits = document.getElementById('fruits');
    // 컨테이너 요소 노드 생성
    const $container = document.createElement('div');

    ['Apple','Banana','Orange'].forEach(text => {
        // 1. 요소 노드 생성
        const $li = document.createElement('li');
        // 2. 텍스트 노드 생성
        const textNode = document.createTextNode(text);
        // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
        $li.appendChild(textNode);
        // 4. $li 요소 노드를 컨테이너 요소의 마지막 자식 노드로 추가
        $container.appendChild($li);
    });
    // 5. 컨테이너 요소 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($container);
</script>
```
* DocumentFragment 노드는 문서, 요소, 어트리뷰트, 텍스트 노드와 같은 노드 객체의 일종으로, 부모 노드가 없어서 기존 DOM과는 별도로 존재한다는 특징이 있다.
* DocumentFragment 노드는 자식 노드들의 부모 노드로서 별도의 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용
    * 기존 DOM과는 별도로 존재하므로 DocumentFragment 노드에 자식 노드를 추가하여도 기존 DOM에 어떠한 변경도 발생하지 않음
    * DocumentFragment 노드를 DOM에 추가하면 자신은 제거되고 자신의 자식 노드만 DOM에 추가됨
* `Document.prototype.createDocumentFragment` 메서드는 비어 있는 DocumentFragment 노드를 생성하여 반환한다.
```html
<ul id="fruits"></ul>
<script>
    const $fruits = document.getElementById('fruits');
    // DocumentFragment 노드 생성
    const $fragment = document.createDocumentFragment();

    ['Apple','Banana','Orange'].forEach(text => {
        // 1. 요소 노드 생성
        const $li = document.createElement('li');
        // 2. 텍스트 노드 생성
        const textNode = document.createTextNode(text);
        // 3. 텍스트 노드를 $li 요소 노드의 자식 노드로 추가
        $li.appendChild(textNode);
        // 4. $li 요소 노드를 DocumentFragment 노드의 마지막 자식 노드로 추가
        $fragment.appendChild($li);
    });
    // 5. DocumentFragment 노드를 #fruits 요소 노드의 마지막 자식 노드로 추가
    $fruits.appendChild($fragment);
</script>
```
* 실제로 DOM 변경이 발생하는 것은 한번 뿐이며 리플로우와 리페인트도 한번만 실행된다.
* **결론 : 여러개의 요소 노드를 DOM에 추가하는 경우 DocumentFragment 노드를 사용하는 것이 더 효율적이다.**

### 5. 노드 삽입
#### 5.1 마지막 노드로 추가
* `Node.prototype.appendChild` 메서드는 인수로 전달받은 노드를 자신을 호출한 노드의 마지막 자식 노드로 DOM에 추가한다.
    * 즉, 노드를 추가할 위치를 지정할 수 없고, 마지막 자식 노드로 추가함
#### 5.2 지정한 위치에 노드 삽입
* `Node.prototype.insertBefore`(newNode, childNode) 메서드는 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입한다.
* 두 번째 인수로 전달받은 노드는 반드시 insertBefore 메서드를 호출한 노드의 자식 노드이어야 한다.
    * 그렇지 않으면 DOMException 에러 발생
* 두 번째 인수로 전달받은 노그가 null이면 첫 번째 인수로 전달받은 노드를 insertBefore 메서드를 호출한 노드의 마지막 자식 노드로 추가된다.
    * appendChild 메서드와 동일하게 동작

```html
<ul id="fruits">
    <li>Apple</li>
    <li>Banana</li>
</ul>
<script>
    const $fruits = document.getElementById('fruits');

    const $li = document.createElement('li');
    $li.appendChild(document.createTextNode('Orange'));

    $fruits.insertBefore($li, $fruits.lastElementChild);
    // Apple - Orange - Banana
</script>
```
### 6. 노드 이동
* DOM에 이미 존재하는 노드를 `appendChild` 또는 `insertBefore` 메서드를 사용하여 DOM에 다시 추가하면 현재 위치에서 노드를 제거하고 새로운 위치에 노드를 추가한다.
```html
<ul id="fruits">
    <li>Apple</li>
    <li>Banana</li>
    <li>Orange</li>
</ul>
<script>
    const $fruits = document.getElementById('fruits');

    // 이미 존재하는 요소 노드를 취득
    const [ $apple, $banana, ] = $fruits.children;

    // 이미 존재하는 $apple 요소 노드를 #fruits 요소 노드의 마지막 노드로 이동
    $fruits.appendChild($apple); // Banana - Orange - Apple
    // 이미 존재하는 $banana 요소 노드를 #fruits 요소의 마지막 자식 노드 앞으로 이동
    $fruits.insertBefore($banana, $fruits.lastElementChild); // Orange - Banana - Apple
</script>
```
### 7. 노드 복사
* `Node.prototype.cloneNode`([deep: true | false]) 메서드는 노드의 사본을 생성하여 반환한다.
* 매개변수 **deep에 true를 인수로 전달**하면 노드를 **깊은 복사**하여 모든 자손 노드가 포함된 사본을 생성
* 매개변수 **deep에 false를 인수로 전달하거나 생략**하면 노드를 **얕은 복사**하여 노드 자신만의 사본을 생성
    * 얕은 복사로 생성된 요소 노드는 자손 노드를 복사하지 않으므로 텍스트 노드도 없다.

```html
<ul id='fruits'>
    <li>Apple</li>
</ul>
<script>
    const $fruits = document.getElementById('fruits');
    const $apple = $fruits.firstElementChild;

    // $apple 요소를 얕은 복사하여 사본 생성. 텍스트 노드가 없는 사본 생성
    const $shallowClone = $apple.cloneNode();
    // 사본 요소 노드에 텍스트 추가
    $shallowClone.textContent = 'Banana';
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($shallowClone);

    // #fruits 요소를 깊은 복사하여 모든 자손 노드가 포함된 사본을 생성
    const $deepClone = $fruits.cloneNode(true);
    // 사본 요소 노드를 #fruits 요소 노드의 마지막 노드로 추가
    $fruits.appendChild($deepClone);
</script>
```
### 8. 노드 교체
* `Node.prototype.replaceChild`(newChild, oldChild) 메서드는 자신을 호출한 노드의 자식 노드를 다른 노드로 교체한다.
    * 첫 번째 매개변수 newChild에는 교체할 새로운 노드를 인수로 전달
    * 두 번째 매개변수 oldChild에는 이미 존재하는 교체될 노드를 인수로 전달
    * oldChild 매개변수에 인수로 전달한 노드는 replaceChild 메서드를 호출한 노드의 자식 노드이어야 한다.
* oldChild 노드는 DOM에서 제거된다.
```html
<ul id='fruits'>
    <li>Apple</li>
</ul>
<script>
    const $fruits = document.getElementById('fruits');

    // 기존 노드와 교체할 요소 노드를 생성
    const $newChild = document.createElement('li');
    $newChild.textContent = 'Banana';

    // #fruits 요소 노드의 첫 번째 자식 요소 노드를 $newChild 요소 노드로 교체
    $fruits.replaceChild($newChild, $fruits.firstElementChild);
</script>
```
### 9. 노드 삭제
* `Node.prototype.removeChild`(child) 메서드는 child 매개변수에 인수로 전달한 노드를 DOM에서 삭제한다.
    * 인수로 전달한 노드는 removeChild 메서드를 호출한 노드의 자식 노드이어야 한다.

```html
<ul id='fruits'>
    <li>Apple</li>
    <li>Banana</li>
</ul>
<script>
    const $fruits = document.getElementById('fruits');
    const $apple = $fruits.firstElementChild;

    // #fruits 요소 노드의 마지막 요소를 DOM에서 삭제
    $fruits.removeChild($fruits.lastElementChild);
</script>
```
## 어트리뷰트
### 1. 어트리뷰트 노드와 attributes 프로퍼티
* HTML 요소는 여러 개의 어트리뷰트(속성)을 가질 수 있다.
* HTML 어트리뷰트는 HTML 요소의 시작 태그에 `어트리뷰트 이름 = "어트리뷰트 값"` 형식으로 정의
```html
<input id="user" type="text" value="grape">
```
* 공통적으로 사용할 수 있는 어트리뷰트
    * 글로벌 어트리뷰트
        * id, class, style, title, lang, tabindex, draggable, hidden 등
    * 이벤트 핸들러 어트리뷰트
        * onclick, onchange, onfocus, onblur, oninput, onkeypress, onkeydown, onkeyup, onmouseover, onsubmit, onload 등
* 특정 HTML 요소에 한정적으로 사용 가능한 어트리뷰트도 존재함
    * ex) input 요소
        * type, value, checked
* HTML 어트리뷰트는 어트리뷰트 노드로 변환되어 요소 노드와 연결된다.
    * HTML 어트리뷰트당 하나의 어트리뷰트 노드가 생성된다.
    * 위 input 요소는 3개의 어트리뷰트가 있으므로 3개의 어트리뷰트 노드가 생성됨
* 모든 어트리뷰트 노드의 참조는 유사 배열 객체이자 이터러블인 NamedNodeMap 객체에 담겨서 요소 노드의 attributes 프로퍼티에 저장된다.
![attributes_properties drawio](https://user-images.githubusercontent.com/63139527/185865456-860f3f24-2ba2-4715-ada3-911ebaba5b5f.png)
* attributes 프로퍼티는 getter만 존재하는 읽기 전용 접근자 프로퍼티이며, 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴 NamedNodeMap 객체를 반환한다.
```html
<html>
<body>
    <input id="user" type="text" value="grape">
    <script>
        // 요소 노드의 attributes 프로퍼티는 요소 노드의 모든 어트리뷰트 노드의 참조가 담긴
        // NamedNodeMap 객체를 반환한다.
        const { attributes } = document.getElementById('user');
        console.log(attributes);
        // NamedNodeMap {0: id, 1: type, 2: value, id: id, type: type, value: value, length: 3}

        // 어트리뷰트 값 취득
        console.log(attributes.id.value); // user
        console.log(attributes.type.value); // text
        console.log(attributes.value.value); // grape
    </script>
</body> 
</html>
```
### 2. HTML 어트리뷰트 조작
* `Element.prototype.getAttribute/setAttribute` 메서드를 사용하면 attributes 프로퍼티를 통하지 않고  
요소 노드에서 메서드를 통해 직접 HTML 어트리뷰트 값을 취득하거나 변경할 수 있어 편리하다.
    * 어트리뷰트 값 참조 : Element.prototype.getAttribute(attributeName)
    * 어트리뷰트 값 변경 : Element.prototype.setAttribute(attributeName, attributeValue)
* `Element.prototype.hasAttribute(attributeName)` 메서드는 특정 HTML 어트리뷰트가 존재하는 확인 
* `Element.prototype.removeAttribute(attributeName)` 메서드는 특정 HTML 어트리뷰트 삭제 

```html
<!DOCTYPE html>
    <body>
        <input id="user" type="text" value="grape">
        <script>
            const $input = document.getElementById('user');

            // value 어트리뷰트 취득
            const inputValue = $input.getAttribute('value');
            console.log(inputValue); // grape

            // value 어트리뷰트 값을 변경
            $input.setAttribute('value', 'apple');
            console.log($input.getAttribute('value')); // apple

            // value 어트리뷰트의 존재 확인
            if($input.hasAttribute('value')) {
                // value 어트리뷰트 삭제
                $input.removeAttribute('value');
            }
            // value 어트리뷰트가 삭제됨
            console.log($input.hasAttribute('value')); // false
        </script>
    </body>
</html>
```
### 3. HTML 어트리뷰트 vs DOM 프로퍼티
* 요소 노드 객체에는 HTML 어트리뷰트에 대응하는 DOM 프로퍼티가 존재한다.
    * 이 DOM 프로퍼티들은 HTML 어트리뷰트 값을 초기값으로 가지고 있다.
* 예시
    * `<input id="user" type="text" value="grape">` 
    * 위 요소가 파싱되어 생성된 요소 노드 객체에는 id, type, value 어트리뷰트에
    * 대응하는 id, type, value 프로퍼티가 존재하며
    * 이 DOM 프로퍼티들은 HTML 어트리뷰트의 값을 초기값으로 가지고 있다.
* DOM 프로퍼티는 setter, getter 둘다 가진 접근자 프로퍼티이다.
    * 참조와 변경이 가능

```html
<!DOCTYPE html>
<html>
<body>
    <input id="user" type="text" value="grape">
    <script>
        const $input = document.getElementById('user');

        // 요소 노드의 value 프로퍼티 값을 변경
        $input.value = 'foo';

        // 요소 노드의 value 프로퍼티 값을 참조
        console.log($input.value); // foo
    </script>
</body>
</html>
```
* HTML 어트리뷰트는 다음과 같이 DOM에서 중복 관리되고 있는 것처럼 보인다. -> 그렇지 않다!
    * 요소 노드의 attributes 프로퍼티에서 관리하는 어트리뷰트 노드
    * HTML 어트리뷰트에 대응하는 요소 노드의 프로퍼티(DOM 프로퍼티)
* **HTML 어트리뷰트의 역할은 HTML 요소의 초기 상태를 지정하는 것이다.**
    * **즉, HTML 어트리뷰트 값은 HTML 요소의 초기 상태를 의미하며 이는 변하지 않는다.**
* 따라서 input 요소의 요소 노드가 **생성되어 첫 렌더링이 끝난 시점**까지 어트리뷰트 노드의 어트리뷰트 값과 요소 노드의 value 프로퍼티에 할당된 값은 HTML 어트리뷰트 값과 동일하다.
```html
<!DOCTYPE html>
<html>
<body>
    <input id="user" type="text" value="grape">
    <script>
        const $input = document.getElementById('user');

        // attributes 프로퍼티에 저장된 value 어트리뷰트 값
        console.log($input.getAttribute('value')); // grape

        // 요소 노드의 value 프로퍼티에 저장된 value 어트리뷰트 값
        console.log($input.value); // grape
    </script>
</body>
</html>
```
---
* 하지만 첫 렌더링 이후 사용자가 input 요소에 무언가를 입력하기 시작하면 상황이 달라진다.
* **요소 노드는 상태를 가지고 있다.**
* 예시
    * 사용자가 input 요소의 입력 필드에 "foo" 라는 값을 입력한 경우
    * input 요소 노드는 사용자의 입력에 의해 변경된 **최신 상태**("foo")를 관리해야 하는 것은 물론
    * HTML 어트리뷰트로 지정한 **초기 상태**("grape")도 관리해야 한다.
    * 초기 상태 값을 관리하지 않으면 웹페이지를 처음 표시하거나 새로고침할 때 초기 상태를 표시할 수 없다.
* **요소 노드는 2개의 상태, 즉 초기 상태와 최신 상태를 관리해야 한다.**
* **요소 노드의 초기 상태는 어트리뷰트 노드가 관리하며, 요소 노드의 최신 상태는 DOM 프로퍼티가 관리한다.**
#### 3.1 어트리뷰트 노드
* **HTML 어트리뷰트로 지정한 HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리한다.**
    * 어트리뷰트 노드에서 관리하는 어트리뷰트 값은 사용자의 입력에 의해 상태가 변경되어도 변경되지 않고 HTML 요소의 초기 상태를 유지
* 어트리뷰트 노드가 관리하는 초기 상태값을 참조 및 변경하려면 getAttribute/setAttribute 메서드를 사용해야 한다.
    * getAttribute 메서드는 초기 상태 값을 참조한다.
    * setAttribute 메서드는 초기 상태 값을 변경한다.
#### 3.2 DOM 프로퍼티
* **사용자가 입력한 최신 상태는 HTML 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티가 관리한다.**
* **DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지한다.**
* DOM 프로퍼티에 값을 할당하는 것은 HTML 요소의 최신 상태 값을 변경하는 것을 의미한다.
    * 즉, 사용자가 상태를 변경하는 행위와 같다.
    * 이때 HTML 요소에 지정한 어트리뷰트 값에는 어떠한 영향도 주지 않는다.
* 참고 : 모든 DOM 프로퍼티가 사용자의 입력에 의해 변경된 최신 상태를 관리하는 것은 아니다.
* 예시
    * input 요소의 id 어트리뷰트에 대응하는 id 프로퍼티는 사용자의 입력과 아무런 관계가 없음
    * 사용자 입력에 의한 상태 변화와 관계없는 id 어트리뷰트와 id 프로퍼티는 항상 동일한 값을 유지함
#### 3.3 HTML 어트리뷰트와 DOM 프로퍼티의 대응 관계
* 대부분의 HTML 어트리뷰트는 HTML 어트리뷰트 이름과 동일한 DOM 프로퍼티와 1:1로 대응한다.
* 단, 다음과 같이 HTML 어트리뷰트와 DOM 프로퍼티가 언제나 1:1로 대응하는 것은 아니며,  
HTML 어트리뷰트 이름과 DOM 프로퍼티 키가 반드시 일치하는 것도 아니다.
    * id 어트리뷰트와 id 프로퍼티는 1:1 대응하며, 동일한 값으로 연동한다.
    * input 요소의 value 어트리뷰트는 value 프로퍼티와 1:1 대응한다.  
    하지만, value 어트리뷰트는 초기 상태를, value 프로퍼티는 최신 상태를 갖는다.
    * class 어트리뷰트는 className, classList 프로퍼티와 대응한다.
    * for 어트리뷰트는 htmlFor 프로퍼티와 1:1 대응한다.
    * td 요소의 colspan 어트리뷰트는 대응하는 프로퍼티가 존재하지 않는다.
    * textContent 프로퍼티는 대응하는 어트리뷰트가 존재하지 않는다.
    * 어트리뷰트 이름은 대소문자를 구별하지 않지만 대응하는 프로퍼티 키는 카멜 케이스를 따른다(maxlength -> maxLength)
#### 3.4 DOM 프로퍼티 값의 타입
* getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.
* 하지만 DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다.
    * 예시
        * checkbox 요소의 checked 어트리뷰트 값은 문자열이지만 checked 프로퍼티 값은 불리언 타입이다.

```html
<!DOCTYPE html>
<html>
<body>
    <input type="checkbox" checked>
    <script>
        const $checkbox = document.querySelector('input[type=checkbox]');

        // getAttribute 메서드로 취득한 어트리뷰트 값은 언제나 문자열이다.
        console.log($checkbox.getAttribute('checked')); // ''

        // DOM 프로퍼티로 취득한 최신 상태 값은 문자열이 아닐 수도 있다.
        console.log($checkbox.checked); // true
    </script>
</body>
</html>
```
### 4. data 어트리뷰트와 dataset 프로퍼티
* data 어트리뷰트와 dataset 프로퍼티를 사용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간에 데이터를 교환할 수 있다.
* data 어트리뷰트는 `data-` 접두사 다음에 임의의 이름을 붙여 사용한다.
    * 예시 : data-user-id, data-role
* data 어트리뷰트 값은 HTMLElement.dataset 프로퍼티로 취득할 수 있다.
* dataset 프로퍼티는 HTML 요소의 모든 data 어트리뷰트의 정보를 제공하는 DOMStringMap 객체를 반환한다.
* DOMStringMap 객체는 data 어트리뷰트의 data- 접두사 다음에 붙인 임의의 이름을 카멜 케이스로 변환한 프로퍼티를 가지고 있다.
* data 어트리뷰트의 data- 접두사 다음에 존재하지 않는 이름을 키로 사용하여 dataset 프로퍼티에 값을 할당하면 HTML 요소에 data 어트리뷰트가 추가된다.
    * 이때 dataset 프로퍼티에 추가한 카멜케이스(fooBar)의 프로퍼티 키는 data 어트리뷰트의 data- 접두사 다음에 케밥케이스(data-foo-bar)로 자동 변경되어 추가된다.

```html
<!DOCTYPE html>
<html>
<body>
    <ul class="users">
        <li id="1" data-user-id="7621" data-role="admin">Lee</li>
        <li id="2" data-user-id="9524" data-role="subscriber">Kim</li>
    </ul>
    <script>
        const users = [...document.querySelector('.users').children];

        // user-id가 '7621'인 요소 노드를 취득한다.
        const user = users.find(user => user.dataset.userId === '7621');
        // user-id가 '7621'인 요소 노드에서 data-role의 값을 취득한다.
        console.log(user.dataset.role); // "admin"

        // user-id가 '7621'인 요소 노드의 data-role의 값을 변경한다.
        user.dataset.role = 'subscriber';
        // dataset 프로퍼티는 DOMStringMap 객체를 반환한다.
        console.log(user.dataset); // DOMStringMap {userId: "7621", role: "subscriber"}

        // user-id가 '7621'인 요소 노드에 새로운 data 어트리뷰트를 추가한다.
        user.dataset.fooBar = 'test';
        console.log(user.dataset);
        /*
            DOMStringMap {userId: "7621", role: "subscriber", fooBar: "test"}
            => <li id="1" data-user-id="7621" data-role="subscriber" data-foo-bar="test">Lee</li>
        */
    </script>
</body>
</html>
```
## 스타일
### 1. 인라인 스타일 조작
* `HTMLElement.prototype.style` 프로퍼티는 setter와 getter 둘다 존재하는 접근자 프로퍼티로서 요소 노드의 **인라인 스타일**을 취득하거나 추가 또는 변경한다.
* style 프로퍼티를 참조하면 CSSStyleDeclaration 타입의 객체를 반환한다.
* CSSStyleDeclaration 객체는 다양한 CSS 프로퍼티에 대응하는 프로퍼티를 가지고 있으며, 이 프로퍼티에 값을 할당하면 해당 CSS 프로퍼티가 인라인 스타일로 HTML 요소에 추가되거나 변경된다.
* CSS 프로퍼티는 케밥 케이스를 따른다.
    * 예시 : background-color
* CSSStyleDeclaration 객체의 프로퍼티는 카멜 케이스를 따른다.
    * 예시 : backgroundColor
* 케밥 케이스의 CSS 프로퍼티를 그대로 사용하려면 객체의 마침표 표기법 대신 대괄호 표기법을 사용한다.
```javascript
$div.style['background-color'] = 'yellow';
```
* 단위 지정이 필요한 CSS 프로퍼티의 값은 반드시 단위를 지정해야 한다. 생략하면 해당 CSS 프로퍼티는 적용되지 않는다.
    * 예시 : px, em, % 등..
```html
<!DOCTYPE html>
<html>
<body>
    <div style="color: red">Hello World</div>
    <script>
        const $div = document.querySelector('div');

        // 인라인 스타일 취득
        console.log($div.style); // CSSStyleDeclaration { 0: "color", ... }

        // 인라인 스타일 변경
        $div.style.color = 'blue';

        // 인라인 스타일 추가
        $div.style.width = '100px';
        $div.style.height = '100px';
        $div.style.backgroundColor = 'yellow';
    </script>
</body>
</html>
```
### 2. 클래스 조작
* `.`으로 시작하는 클래스 선택자를 사용하여 CSS class를 미리 정의한 다음, HTML 요소의 class 어트리뷰트 값을 변경하여 HTML 요소의 스타일을 변경할 수도 있다.
* class 어트리뷰트에 대응하는 요소 노드의 DOM 프로퍼티(className, classList)를 사용한다.
#### 2.1 className
* `Element.prototype.className` 프로퍼티는 setter와 getter 둘다 존재하는 접근자 프로퍼티로서 HTML 요소의 class 어트리뷰트 값을 취득하거나 변경한다.
    * className 프로퍼티를 참조하면 class 어트리뷰트 값을 문자열로 반환
    * 요소 노드의 className 프로퍼티에 문자열을 할당하면 class 어트리뷰트 값을 할당한 문자열로 변경
* className 프로퍼티는 문자열을 반환하므로 공백으로 구분된 여러 개의 클래스를 반환하는 경우 다루기가 불편하다.
```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .box {
            width: 100px; height: 100px;
            background-color: antiquewhite;
        }
        .red { color: red; }
        .blue { color: blue; }
    </style>
</head>
<body>
    <div class="box red">Hello World</div>
    <script>
        const $box = document.querySelector('.box');

        // .box 요소의 class 어트리뷰트 값을 취득
        console.log($box.className); // 'box red'

        // .box 요소의 class 어트리뷰트 값 중에서 'red' 만 'blue'로 변경
        $box.className = $box.className.replace('red', 'blue');
    </script>
</body>
</html>
```
#### 2.2 classList
* `Element.prototype.classList` 프로퍼티는 class 어트리뷰트의 정보를 담은 DOMTokenList 객체를 반환한다.
```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .box {
            width: 100px; height: 100px;
            background-color: antiquewhite;
        }
        .red { color: red; }
        .blue { color: blue; }
    </style>
</head>
<body>
    <div class="box red">Hello World</div>
    <script>
        const $box = document.querySelector('.box');

        // .box 요소의 class 어트리뷰트 정보를 담은 DOMTokenList 객체를 취득
        // classList가 반환하는 DOMTokenList 객체는 HTMLCollection과 NodeList와 같이
        // 노드 객체의 상태 변화를 실시간으로 반영하는 살아 있는(live) 객체다.
        console.log($box.classList);
        // DOMTokenList(2) [length: 2, value: "box blue", 0: "box", 1: "blue"]

        // .box 요소의 class 어트리뷰트 값 중에서 'red' 만 'blue'로 변경
        $box.className = $box.className.replace('red', 'blue');
    </script>
</body>
</html>
```
* DOMTokenList 객체는 class 어트리뷰트의 정보를 나타내는 컬렉션 객체로서 유사 배열 객체이면서 이터러블이다.
* DOMTokenList 객체는 다음과 같이 유용한 메서드를 제공한다.

1. add(...className)
    * add 메서드는 인수로 전달한 1개 이상의 문자열을 class 어트리뷰트 값으로 추가한다.
    ```javascript
    $box.classList.add('foo'); // class="box red foo"
    $box.classList.add('bar', 'baz'); // class="box red foo bar baz"
    ```
2. remove(...className)
    * remove 메서드는 인수로 전달한 1개 이상의 문자열과 일치하는 클래스를 class 어트리뷰트에서 삭제한다.
    * 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 없으면 에러 없이 무시된다.
    ```javascript
    $box.classList.remove('foo'); // class="box red bar baz"
    $box.classList.remove('bar', 'baz'); // class="box red"
    $box.classList.remove('x'); // class="box red"
    ```
3. item(index)
    * item 메서드는 인수로 전달한 index에 해당하는 클래스를 class 어트리뷰트에서 반환한다.
    * 예시
        * index가 0이면 첫 번쨰 클래스를 반환하고 index가 1이면 두 번째 클래스를 반환한다.
    ```javascript
    $box.classList.item(0); // "box"
    $box.classList.item(1); // "red"
    ```
4. contains(className)
    * contains 메서드는 인수로 전달한 문자열과 일치하는 클래스가 class 어트리뷰트에 포함되어 있는지 확인한다.
    ```javascript
    $box.classList.contains('box'); // true
    $box.classList.contains('blue'); // false
    ```
5. replace(oldClassName, newClassName)
    * replace 메서드는 class 어트리뷰트에서 첫 번째 인수로 전달한 문자열을 두 번째 인수로 전달한 문자열로 변경한다.
    ```javascript
    $box.classList.replace('red', 'blue'); // class="box blue"
    ```
6. toggle(className[, force])
    * toggle 메서드는 class 어트리뷰트에 인수로 전달한 문자열과 일치하는 클래스가 존재하면 제거하고, 존재하지 않으면 추가한다.
    ```javascript
    $box.classList.toggle('foo'); // class="box blue foo"
    $box.classList.toggle('foo'); // class="box blue"
    ```
    * 두 번째 인수로 불리언 값으로 평가되는 조건식을 전달할 수 있다.
    * 조건식의 평가 결과가 true이면 class 어트리뷰트에 강제로 첫 번째 인수로 전달받은 문자열을 추가하고,
    * 조건식의 평가 결과가 false이면 class 어트리뷰트에서 강제로 첫 번째 인수로 전달받은 문자열을 제거한다.
    ```javascript
    // class 어트리뷰트에 강제로 'foo' 클래스를 추가
    $box.classList.toggle('foo', true); // class="box blue foo"
    // class 어트리뷰트에서 강제로 'foo' 클래스를 제거
    $box.classList.toggle('foo', false); // class="box blue"
    ```
    
* 그 외에 DOMTokenList 객체는 forEach, entries, keys, values, supports 메서드를 제공한다.
### 3. 요소에 적용되어 있는 CSS 스타일 참조
* style 프로퍼티는 인라인 스타일만 반환한다.
    * 클래스를 적용한 스타일이나 상속을 통해 암묵적으로 적용된 스타일은 style 프로퍼티로 참조할 수 없다.
* HTML 요소에 적용되어 있는 모든 CSS 스타일을 참조해야 할 경우 getComputedStyle 메서드를 사용한다.

* `window.getComputedStyle`(element[, pseudo]) 메서드는 첫 번째 인수(element)로 전달한 요소 노드에 적용되어 있는 평가된 스타일을 CSSStyleDeclaration 객체에 담아 반환한다.
    * 평가된 스타일이란?
        * 요소 노드에 적용되어 있는 모든 스타일, 즉 링크 스타일, 임베딩 스타일, 인라인 스타일, 자바스크립트에 적용한 스타일, 상속된 스타일, 기본(user agent) 스타일 등 모든 스타일이 조합되어 최종적으로 적용된 스타일을 말한다.

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            body {
                color: red;
            }
            .box {
                width: 100px;
                height: 50px;
                background-color: cornsilk;
                border: 1px solid black;
            }
        </style>
    </head>
    <body>
        <div class="box">Box</div>
        <script>
            const $box = document.querySelector('.box');

            // .box 요소에 적용된 모든 CSS 스타일을 담고 있는 CSSStyleDeclaration 객체를 취득
            const computedStyle = window.getComputedStyle($box);
            console.log(computedStyle); // CSSStyleDeclaration

            // 임베딩 스타일
            console.log(computedStyle.width); // 100px
            console.log(computedStyle.height); // 50px
            console.log(computedStyle.background-color); // rgb(255, 248, 220)
            console.log(computedStyle.border); // 1px solid rgb(0, 0, 0)

            // 상속 스타일( body -> .box )
            console.log(computedStyle.color); // rgb(255, 0, 0)

            // 기본 스타일
            console.log(computedStyle.display); // block
        </script>
    </body>
</html>
```
* getComputedStyle 메서드의 두 번째 인수(pseudo)로 :after, :before와 같은 의사 요소를 지정하는 문자열을 전달할 수 있다.
    * 의사 요소가 아닌 일반 요소의 경우 두 번째 인수는 생략한다.

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            .box:before {
                content: 'Hello'
            }
        </style>
    </head>
    <body>
        <div class="box">Box</div>
        <script>
            const $box = document.querySelector('.box');

            // 의사 요소 :before의 스타일을 취득한다.
            consst computedStyle = window.getComputedStyle($box, ':before');
            console.log(computedStyle.content); // "Hello"
        </script>
    </body>
</html>
```
## DOM 표준
* W3C(World Wide Web Consortium)과 WHATWG(Web Hyphertext Application Technology Working Group) 두 단체가 협력하면 공통된 표준을 만듬
* 이후 구글, 애플, 마이크로소프트, 모질라로 구성된 4개의 주류 브라우저 벤더사가 주도하는 WHATWG이 단일 표준을 내놓기로 합의되었다.