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

### 2. textContent

## DOM 조작

### 1. innerHTML

### 2. insertAdjacentHTML 메서드

### 3. 노드 생성과 추가

### 4. 복수의 노드 생성과 추가

### 5. 노드 삽입

### 6. 노드 이동

### 7. 노드 복사

### 8. 노드 교체

### 9. 노드 삭제

## 어트리뷰트

### 1. 어트리뷰트 노드와 attributes 프로퍼티

### 2. HTML 어트리뷰트 조작

### 3. HTML 어트리뷰트 vs DOM 프로퍼티

### 4. data 어트리뷰트와 dataset 프로퍼티

## 스타일

### 1. 인라인 스타일 조작

### 2. 클래스 조작

### 3. 요소에 적용되어 있는 CSS 스타일 참조

## DOM 표준
