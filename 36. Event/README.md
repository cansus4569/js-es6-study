# 이벤트

## 이벤트 드리븐 프로그래밍
* 브라우저는 처리해야 할 특정 사건이 발생하면 이를 감지하여 특정한 타입의 이벤트를 발생시킨다.
    * 클릭, 키보드 입력, 마우스 이동 등
* 이벤트가 발생했을 때 호출될 함수를 **이벤트 핸들러**라고 하며
* 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 **이벤트 핸들러 등록**이라 한다.
* 프로그램의 흐름을 이벤트 중심으로 제어하는 프로그래밍 방식을 **이벤트 드리븐 프로그래밍**이라 한다.

## 이벤트 타입
* 이벤트 타입은 이벤트의 종류를 나타내는 문자열이다.
### 1. 마우스 이벤트
이벤트 타입|이벤트 발생 시점
-|-
click|마우스 버튼을 클릭했을 때
dblclick|마우스 버튼을 더블 클릭했을 때
mousedown|마우스 버튼을 눌렀을 때
mouseup|누르고 있던 마우스 버튼을 놓았을 때
mousemove|마우스 커서를 움직였을 때
mouseenter|마우스 커서를 HTML 요소 안으로 이동했을 때(버블링 되지 않는다)
mouseover|마우스 커서를 HTML 요소 안으로 이동했을 때(버블링 된다)
mouseleave|마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링 되지 않는다)
mouseout|마우스 커서를 HTML 요소 밖으로 이동했을 때(버블링 된다)
### 2. 키보드 이벤트
이벤트 타입|이벤트 발생 시점
-|-
keydown|모든 키를 눌렀을 때 발생한다.
keypress|폐지 되었으므로 사용하지 않을 것을 권장한다.
keyup|누르고 있던 키를 놓았을 때 한 번만 발생한다.
### 3. 포커스 이벤트
이벤트 타입|이벤트 발생 시점
-|-
focus|HTML 요소가 포커스를 받았을 때(버블링되지 않는다)
blur|HTML 요소가 포커스를 잃었을 때(버블링되지 않는다)
focusin|HTML 요소가 포커스를 받았을 때(버블링된다)
focusout|HTML 요소가 포커스를 잃었을 때(버블링된다)
### 4. 폼 이벤트
이벤트 타입|이벤트 발생 시점
-|-
submit|form 요소 내의 submit 버튼을 클릭했을 때
reset|form 요소 내의 reset 버튼을 클릭했을 때(최근에는 사용 안함)
### 5. 값 변경 이벤트
이벤트 타입|이벤트 발생 시점
-|-
input|input(text, checkbox, radio), select, textarea 요소의 값이 입력되었을 때
change|input(text, checkbox, radio), select, textarea 요소의 값이 변경되었을 때</br>사용자가 입력하고 있을 때는 input 이벤트가 발생하고 사용자 입력이 종료되어 값이 변경되면 change 이벤트가 발생한다.
readystatechange|HTML 문서의 로드와 파싱 상태를 나타내는 document.readyState 프로퍼티 값('loading', 'interactive', 'complete')이 변경될 때
### 6. DOM 뮤테이션 이벤트
이벤트 타입|이벤트 발생 시점
-|-
DOMContentLoaded|HTML 문서의 로드와 파싱이 완료되어 <b>DOM 생성이 완료</b>되었을 때
### 7. 뷰 이벤트
이벤트 타입|이벤트 발생 시점
-|-
resize|브라우저 윈도우(window)의 크기를 리사이즈할 때 연속적으로 발생한다.</br>오직 window 객체에서만 발생한다.
scroll|웹페이지(document) 또는 HTML 요소를 스크롤할 때 연속적으로 발생한다.
### 8. 리소스 이벤트
이벤트 타입|이벤트 발생 시점
-|-
load|<b>DOMContentLoaded</b> 이벤트가 발생한 이후, 모든 리소스(이미지, 폰트 등)의 로딩이 완료되었을 때(주로 window 객체에서 발생)
unload|리소스가 언로드될 때(주로 새로운 웹페이지를 요청한 경우)
abort|리소스 로딩이 중단되었을 때
error|리소스 로딩이 실패했을 때

## 이벤트 핸들러 등록
* 이벤트가 발생하면 브라우저에 의해 호출될 함수가 이벤트 핸들러이다.
* 이벤트가 발생했을 때 브라우저에게 이벤트 핸들러의 호출을 위임하는 것을 **이벤트 핸들러 등록**이라 한다.
### 1. 이벤트 핸들러 어트리뷰트 방식
* HTML 요소의 어트리뷰트 중에는 이벤트에 대응하는 **이벤트 핸들러 어트리뷰트**가 있다.
* 이벤트 핸들러 어트리뷰트의 이름은 `on` 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져 있다.
    * 예시 : onclick
* 이벤트 핸들러 어트리뷰트 값으로 함수 호출문 등의 문(statement)을 할당하면 이벤트 핸들러가 등록된다.
    * 주의할 점 : 어트리뷰트 값으로 함수 참조가 아닌 함수 호출문 등의 문을 할당해야 함

```html
<!DOCTYPE html>
<html>
    <body>
        <button onclick="sayHi('Park')">Click me!</button>
        <script>
            function sayHi(name) {
                console.log(`Hi ${name}.`);
            }
        </script>
    </body>
</html>
```
* 함수가 아닌 값을 반환하는 함수 호출문을 이벤트 핸들러로 등록하면 브라우저가 이벤트 핸들러를 호출할 수 없다.
* 위 예제에서는 이벤트 핸들러 어트리뷰트 값으로 함수 호출문을 할당했다.
* **이벤트 핸들러 어트리뷰트 값은 사실 암묵적으로 생성될 이벤트 핸들러의 함수 몸체를 의미한다.**
```javascript
// onclick="sayHi('Park')" 어트리뷰트는 파싱되어 함수를 암묵적으로 생성
// 이벤트 핸들러 어트리뷰트 이름과 동일한 키 onclick 이벤트 핸들러 프로퍼티에 할당
function onclick(event) {
    sayHi('Park');
}
```
* 이처럼 동작하는 이유는 이벤트 핸들러에 인수를 전달하기 위해서이다.
* 만약 이벤트 핸들러 어트리뷰트 값으로 함수 참조를 할당해야 한다면 이벤트 핸들러에 인수를 전달하기 곤란한다.
* 이벤트 핸들러 어트리뷰트 값으로 여러 개의 문을 할당 가능 (함수 몸체를 의미하기 때문)
* CDB(Component Based Development) 방식의 Angular/React/Svelte/Vue.js 에서 이벤트 핸들러 어트리뷰트 방식으로 처리한다.
```html
<!-- Angular -->
<button (click)="handleClick($event)">Save</button>
{ /* React */ }
<button onClick={handleClick}>Save</button>
<!-- Svelte -->
<button on:click={handleClick}>Save</button>
<!-- Vue.js -->
<button v-on:click="handleClick($event)">Save</button>
```
### 2. 이벤트 핸들러 프로퍼티 방식
* window 객체와 DOM 노드 객체는 이벤트에 대응하는 이벤트 핸들러 프로퍼티를 가지고 있다.
* 이벤트 핸들러 프로퍼티의 키는 이벤트 핸들러 어트리뷰트와 마찬가지로 `on` 접두사와 이벤트의 종류를 나타내는 이벤트 타입으로 이루어져있다.
* 이벤트 핸들러 프로퍼티에 함수를 바인딩하면 이벤트 핸들러가 등록된다.
```html
<!DOCTYPE html>
<html>
    <body>
        <button>Click me!</button>
        <script>
            const $button = document.querySelector('button');

            // 이벤트 핸들러 프로퍼티에 이벤트 핸들러를 바인딩
            $button.onclick = function() {
                console.log('button click');
            };
        </script>
    </body>
</html>
```
* **이벤트 타깃** : 이벤트 핸들러를 등록하기 위한 이벤트를 발생시킬 객체
* **이벤트 타입** : 이벤트의 종류를 나타내는 문자열
* **이벤트 핸들러** : 타깃에 대한 이벤트 타입에 대한 동작될 함수
![event handler property method drawio](https://user-images.githubusercontent.com/63139527/186297998-04621477-9b02-4b9a-b3e7-f2203b7581ef.png)

* 이벤트 핸들러는 이벤트 타깃 또는 전파된 이벤트를 캐치할 DOM 노드 객체에 바인딩한다.
* "이벤트 핸들러 어트리뷰트 방식"도 결국 DOM 노드 객체의 이벤트 핸들러 프로퍼티로 변환되므로 결과적으로 이벤트 핸들러 프로퍼티 방식과 동일하다고 할 수 있다.
* "이벤트 핸들러 프로퍼티 방식"은 "이벤트 핸들러 어트리뷰트 방식"의 HTML과 자바스크립트가 뒤섞이는 문제를 해결할 수 있다.
    * 하지만 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만 바인딩할 수 있다는 단점이 있다.

```html
<!DOCTYPE html>
<html>
    <body>
        <button>Click me!</button>
        <script>
            const $button = document.querySelector('button');

            // 이벤트 핸들러 프로퍼티 방식은 하나의 이벤트에 하나의 이벤트 핸들러만을 바인딩할 수 있다.
            // 첫 번째로 바인딩된 이벤트 핸들러는 두 번째 바인딩된 이벤트 핸들러에 의해 재할당되어 실행되지 않는다.
            $button.onclick = function() {
                console.log('button clicked 1');
            };

            // 두 번째로 바인딩된 이벤트 핸들러
            $button.onclick = function() {
                console.log('button clicked 2');
            };
        </script>
    </body>
</html>
```
### 3. addEventListener 메서드 방식
* DOM Level 2에서 도입된 `EventTarget.prototype.addEventListener` 메서드를 사용하여 이벤트 핸들러를 등록할 수 있다.
    *" 이벤트 핸들러 어트리뷰트 방식"과 "이벤트 핸들러 프로퍼티 방식"은 DOM Level 0부터 제공되던 방식

![addEventListener method drawio](https://user-images.githubusercontent.com/63139527/186346523-a6614082-b14b-45df-8078-c9ea358f543b.png)

* addEventListener 메서드의 **첫 번째 매개변수**에는 이벤트의 종류를 나타내는 문자열인 **이벤트 타입**을 전달한다.
    * 이벤트 핸들러 프로퍼티 방식과는 달리 on 접두사를 붙이지 않는다.
* **두 번째 매개변수**에는 **이벤트 핸들러**를 전달한다.
* **마지막 매개변수**에는 이벤트를 캐치할 **이벤트 전파 단계**(캡처링 또는 버블링)를 지정한다.
    * 생략하거나 false를 지정하면 버블링 단계에서 이벤트를 캐치하고
    * true를 지정하면 캡처링 단계에서 이벤트를 캐치한다.

* addEventListener 메서드 방식은 이벤트 핸들러 프로퍼티에 바인딩된 이벤트 핸들러에 아무런 영향을 주지 않는다.
    * 버튼 요소에서 클릭 이벤트가 발생하면 2개의 이벤트 핸들러가 모두 호출된다.

```html
<!DOCTYPE html>
<html>
    <body>
        <button>Click me!</button>
        <script>
            const $button = document.querySelector('button');

            // 이벤트 핸들러 프로퍼티 방식
            $button.onclick = function() {
                console.log('[이벤트 핸들러 프로퍼티 방식] button click');
            };

            // addEventListener 메서드 방식
            $button.addEventListener('click', function() {
                console.log('[addEventListener 메서드 방식] button click');
            });
        </script>
    </body>
</html>
```

* addEventListener 메서드는 하나 이상의 이벤트 핸들러를 등록할 수 있다.
    * 이때 이벤트 핸들러는 등록된 순서대로 호출된다.
* addEventListener 메서드를 통해 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 이벤트 핸들러만 등록된다.
```html
<!DOCTYPE html>
<html>
    <body>
        <button>Click me!</button>
        <script>
            const $button = document.querySelector('button');

            // addEventListener 메서드는 동일한 요소에서 발생한 동일한 이벤트에 대해
            // 하나 이상의 이벤트 핸들러를 등록할 수 있다.
            $button.addEventListener('click', function() {
                console.log('[1] button click');
            });

            // addEventListener 메서드 방식
            $button.addEventListener('click', function() {
                console.log('[2] button click');
            });

            const handleClick = () => console.log('button click');

            // 참조가 동일한 이벤트 핸들러를 중복 등록하면 하나의 핸들러만 등록된다.
            $button.addEventListener('click', handleClick);
            $button.addEventListener('click', handleClick);
        </script>
    </body>
</html>
```
## 이벤트 핸들러 제거
* addEventListener 메서드로 등록한 이벤트 핸들러를 제거하려면 `EventTarget.prototype.removeEventListener` 메서드를 사용한다.
* removeEventListener 메서드에 전달할 인수는 addEventListener 메서드와 동일하다.
    * addEventListener 메서드에 전달한 인수와 removeEventListener 메서드에 전달한 인수가 일치하지 않으면 이벤트 핸들러가 제거되지 않는다.
## 이벤트 객체
* 이벤트가 발생하면 이벤트에 관련한 다양한 정보를 담고 있는 이벤트 객체가 동적으로 생성된다.
* **생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.**
```html
<!DOCTYPE html>
<html>
    <body>
        <p>클릭하세요.</p>
        <em class="message"></em>
        <script>
            const $msg = document.querySelector('.message');
            // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
            function showCoords(e) {
                $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
            }
            document.onclick = showCoords;
        </script>
    </body>
</html>
```
* 브라우저가 이벤트 핸들러를 호출할 때 이벤트 객체를 인수로 전달하기 때문
* 이벤트 객체를 전달받으려면 이벤트 핸들러를 정의할 때 이벤트 객체를 전달받을 매개변수를 명시적으로 선언해야 한다.
* 예제서는 e라는 이름으로 매개변수를 선언했으나 다른 이름을 사용하여도 상관없다.

* 이벤트 핸들러 어트리뷰트 방식으로 이벤트 핸들러를 등록했다면 다음과 같이 `event`를 통해 이벤트 객체를 전달받을 수 있다.
    * 이벤트 핸들러 어트리뷰트 방식의 경우 이벤트 객체를 전달받으려면 이벤트 핸들러의 첫 번째 매개변수 이름이 반드시 event 이어야 한다.

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            html, body { height: 100%; }
        </style>
    </head>
    <!-- 이벤트 핸들러 어트리뷰트 방식의 경우 event가 아닌 다른 이름으로는 이벤트 객체를 전달받지 못한다. -->
    <body onclick="showCoords(event)">
        <p>클릭하세요.</p>
        <em class="message"></em>
        <script>
            const $msg = document.querySelector('.message');
            // 클릭 이벤트에 의해 생성된 이벤트 객체는 이벤트 핸들러의 첫 번째 인수로 전달된다.
            function showCoords(e) {
                $msg.textContent = `clientX: ${e.clientX}, clientY: ${e.clientY}`;
            }
        </script>
    </body>
</html>
```
### 1. 이벤트 객체의 상속 구조
![event prototype struct drawio](https://user-images.githubusercontent.com/63139527/186348514-4e990aba-b78f-4faa-9400-53b09230919c.png)
* Event, UIEvent, MouseEvent 등 모두는 생성자 함수이다.
* 다음과 같이 생성자 함수를 호출하여 이벤트 객체를 생성할 수 있다.
```html
<!DOCTYPE html>
<html>
    <body>
        <script>
            // Event 생성자 함수를 호출하여 foo 이벤트 타입의 Event 객체를 생성
            let e = new Event('foo');
            console.log(e);
            console.log(e.type); // "foo"
            console.log(e instanceof Event); // true
            console.log(e instanceof Object); // true

            // FocusEvent 생성자 함수를 호출하여 focus 이벤트 타입의 FocusEvent 객체를 생성
            e = new FocusEvent('focus');
            console.log(e);

            // MouseEvent 생성자 함수를 호출하여 click 이벤트 타입의 MouseEvent 객체를 생성
            e = new MouseEvent('click');
            console.log(e);

            // KeyboardEvent 생성자 함수를 호출하여 keyup 이벤트 타입의 KeyboardEvent 객체를 생성
            e = new KeyboardEvent('keyup');
            console.log(e);

            // InputEvent 생성자 함수를 호출하여 change 이벤트 타입의 InputEvent 객체를 생성
            e = new InputEvent('change');
            console.log(e);
        </script>
    </body>
</html>
```
* 이처럼 이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체도 생성자 함수에 의해 생성된다.
* 이벤트 객체 중 일부는 사용자의 행위에 의해 생성된 것이고 일부는 자바스크립트 코드에 의해 인위적으로 생성된 것이다.
    * MouseEvent 타입의 이벤트 객체는 사용자가 마우스를 클릭하거나 이동했을 때 생성되는 이벤트 객체
    * CustomEvent 타입의 이벤트 객체는 자바스크립트 코드에 의해 인위적으로 생성한 이벤트 객체
* Event 인터페이스는 DOM 내에서 발생한 이벤트에 의해 생성되는 이벤트 객체를 나타낸다.
* Event 인터페이스에는 모든 이벤트 객체의 공통 프로퍼티가 정의되어 있고 FocusEvent, MouseEvent, KeyboardEvent, WheelEvent 같은  
하위 인터페이스에는 이벤트 타입에 따라 고유한 프로퍼티가 정의되어 있다.
### 2. 이벤트 객체의 공통 프로퍼티
* Event 인터페이스(=Event.prototype)에 정의되어 있는 이벤트 관련 프로퍼티는 UIEvent, CustomEvent, MouseEvent 등 모든 파생 이벤트 객체에 상속된다.
    * Event 인터페이스의 이벤트 관련 프로퍼티는 모든 이벤트 객체가 상속받는 공통 프로퍼티다.

공통 프로퍼티|설명|타입
-|-|-
type|이벤트 타입|string
target|이벤트를 발생시킨 DOM 요소|DOM 요소 노드
currentTarget|이벤트 핸들러가 바인딩된 DOM 요소|DOM 요소 노드
eventPhase|이벤트 전파 단계</br>0: 이벤트 없음, 1: 캡처링 단계, 2: 타깃 단계, 3: 버블링 단계|number
bubbles|이벤트를 버블링으로 전파하는지 여부.</br>다음 이벤트는 bubbles: false로 버블링하지 않는다.</br><ul><li>포커스 이벤트 focus/blur</li><li>리소스 이벤트 load/unload/abort/error</li><li>마우스 이벤트 mouseenter/mouseleave</li></ul>|boolean
cancelable|preventDefault 메서드를 호출하여 이벤트의 기본 동작을 취소할 수 있는지 여부.</br>다음 이벤트는 cancelable: fase로 취소할 수 없다.</br><ul><li>포커스 이벤트 focus/blur</li><li>리소스 이벤트 load/unload/abort/error</li><li>마우스 이벤트 mouseenter/mouseleave</li></ul>|boolean
defaultPrevented|preventDefault 메서드를 호출하여 이벤트를 취소했는지 여부|boolean
isTrusted|사용자의 행위에 의해 발생한 이벤트인지 여부.</br>예를 들어, click 메서드 또는 dispatchEvent 메서드를 통해 인위적으로 발생시킨 이벤트인 경우 isTrusted는 false다.|boolean
timeStamp|이벤트가 발생한 시각(1970/01/01/00:00:0 부터 경과한 밀리초)|number

* [예제](./src/checkbox_chage.html)
### 3. 마우스 정보 취득
* click, dblclick, mousedown, mouseup, mousemove, mouseenter, mouseleave 이벤트가 발생하면 생성되는 MouseEvent 타입의 이벤트 객체는 다음과 같은 고유 프로퍼티를 갖는다.
    * 마우스 포인터의 좌표 정보를 나타내는 프로퍼티
        * screenX/screenY, clientX/clientY, pageX/pageY, offsetX/offsetY
    * 버튼 정보를 나타내는 프로퍼티
        * altKey, ctrlKey, shiftKey, button

* DOM 요소를 드래그하여 이동시키는 [예제](./src/mouse_drag.html)
    * 드래그
        * 시작 : mousedown 이벤트 발생한 상태에서 mousemove 이벤트가 발생한 시점
        * 종료 : mouseup 이벤트가 발생한 시점
    * clientX/clientY 프로퍼티는 뷰포트(viewport), 즉 웹페이지의 가시 영역을 기준으로 마우스 포인터 좌표를 나타낸다.
### 4. 키보드 정보 취득
* keydown, keyup, keypress 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 altKey, ctrlKey, shiftKey, metaKey, key, keyCode 같은 고유의 프로퍼티를 갖는다.
* [예제]() : input 요소의 입력 필드에 엔터 키가 입력되면 현재까지 입력 필드에 입력된 값을 출력하는 
* keyup 이벤트가 발생하면 생성되는 KeyboardEvent 타입의 이벤트 객체는 입력한 키 값을 문자열로 반환하는 key 프로퍼티를 제공한다.
    * 엔터 키의 경우 key 프로퍼티는 'Enter'를 반환한다.
    * 참조 : https://keycode.info
## 이벤트 전파

## 이벤트 위임

## DOM 요소의 기본 동작 조작

### 1. DOM 요소의 기본 동작 중단

### 2. 이벤트 전파 방지

## 이벤트 핸들러 내부의 this

### 1. 이벤트 핸들러 어트리뷰트 방식

### 2. 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

## 이벤트 핸들러에 인수 전달

## 커스텀 이벤트

### 1. 커스텀 이벤트 생성

### 2. 커스텀 이벤트 디스패치
