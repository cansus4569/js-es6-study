# strict mode
* var, let, const 키워드를 사용하여 변수를 선언한 다음 사용해야 한다!
```javascript
function foo() {
    x = 10;
}
foo();
console.log(x); // ?
```
1. 자바스크린트 엔진은 x 변수가 어디서 선언되었는지 스코프 체인을 통해 검색한다.
    * foo 함수 스코프 -> foo 함수 컨텍스트의 상위 스코프(전역 스코프)
2. x 변수 선언이 존재하지 않아 ReferenceError 발생할거 같지만  
자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성(**암묵적 전역**)한다.


## strcit mode 란?
* ES5 부터 strict mode (엄격 모드) 추가됨
    * 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

* ESLint 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있음
    * 린트 도구는 정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류만이 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 유요한 도구이다.
    * 린트 도구는 strict mode가 제한하는 오류, 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있는 강력한 효과

* 참고 : ES6에서 도입된 클래스와 모듈은 기본적으로 strict mode가 적용됨

## strict mode 적용
* 전역의 선두 또는 함수 몸체의 선두에 `'use strict';` 를 추가한다.
    * 전역의 선두에 추가하면 스크립트 전체에 strict mode가 적용된다.

```javascript
'use strict'; // 전역 선두

function foo() {
    x = 10; // ReferenceError: x is not defined
}
foo();
```

```javascript
function foo() {
    'use strict'; // 함수 몸체 선두

    x = 10; // ReferenceError: x is not defined
}
foo();
```

```javascript
function foo() {
    x = 10; // 에러 발생 안함
    'use strict'; // 함수 몸체 후두
}
foo();
```

## 전역에 strict mode 적용은 피하자
* 전역에 적용한 strict mode는 스크립트 단위로 적용된다.
```html
<!DOCTYPE html>
<html>
    <body>
        <script>
            'use stirct';
        </script>
        <script>
            x = 1; // 에러 발생 안함
            console.log(x); // 1
        </script>
        <script>
            'use strict';
            y = 1; // ReferenceError: y is not defined
            console.log(y);
        </script>
    </body>
</html>
```
* strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류 발생할 수 있음
    * 외부 라이브러리가 non-strict mode인 경우도 있기 때문
* 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용하는게 좋다.
```javascript
(function () {
    'use strict';

    // Do Something...
}());
```

## 함수 단위로 strict mode 적용도 피하자
* 함수 단위로 strict mode를 적용할 수 있다.
* strict mode가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 발생할 수 있다.
```javascript
(function () {
    // non-strict mode
    var let = 10; // 에러가 발생하지 않는다.

    function foo() {
        'use strict';

        let = 20; // SyntaxError: Unexpected strict mode reserved word
    }
    foo();
}());
```
* 결론 : strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

## strict mode가 발생시키는 에러

### 1. 암묵적 전역
* 선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.
```javascript
(function () {
    'use strict';

    x = 1;
    console.log(x);  // ReferenceError: x is not defined
}());
```
### 2. 변수, 함수, 매개변수의 삭제
* delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생한다.
```javascript
(function () {
    'use strict';

    var x = 1;
    delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.

    function foo(a) {
        delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
    }
    delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

### 3. 매개변수 이름의 중복
* 중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.
```javascript
(function () {
    'use strict';

    // SyntaxError: Duplicate parameter name not allowed in this context
    function foo(x, x) {
        return x + x;
    }
    console.log(foo(1,2));
}());
```

### 4. with 문의 사용
* with문을 사용하면 SyntaxError가 발생한다.
* with 문 : 전달된 객체를 스코프 체인에 추가한다.
    * 장점 : 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 샹략할 수 있어서 코드가 간결해짐
    * 단점 : 성능과 가독성이 나빠짐
    * 결론 : with문은 사용하지 않는 것이 좋다.
```javascript
(function () {
    'use strict';

    // SyntaxError: Strict mode code may not include a with statement
    with({ x: 1 }) {
        console.log(x);
    }
}());
```

## strict mode 적용에 의한 변화

### 1. 일반함수의 this
* 함수를 일반 함수로 호출하면 this에 undefined가 바인딩 된다.
    * 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문
    * 이때 에러는 발생하지 않는다.

```javascript
(function () {
    'use strict';

    function foo() {
        console.log(this); // undefined
    }
    foo();

    function Foo() {
        console.log(this); // Foo
    }
    new Foo();
}());
```

### 2. arguments 객체
* 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.
```javascript
(function (a) {
    'use strict';
    // 매개변수에 전달된 인수를 재할당하여 변경
    a = 2;

    // 변경된 인수가 arguments 객체에 반영되지 않는다.
    console.log(arguments); // { 0: 1, length: 1 }
}(1));
```