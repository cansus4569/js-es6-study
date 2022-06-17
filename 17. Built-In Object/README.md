# 빌트인 객체

## 자바스크립트 객체의 분류
* 크게 3가지 분류로 나눠짐
* `표준 빌트인 객체` (standard built-in object/native objects/global objects)
    * ECMAScript 사양에 정의된 객체
    * 애플리케이션 전역의 공통 기능을 제공
    * 자바스크립트 실행환경(브라우저/Node.js)과 관계없이 언제나 사용 가능
    * 전역 객체의 프로퍼티로서 제공됨
    * 선언 없이 전역 변수처럼 참조가능
* `호스트 객체` (host objects)
    * ECMAScript 사양에 정의되어있지 않음
    * 자바스크립트 실행 환경(브라우저/Node.js)에서 추가로 제공하는 객체
        * 브라우저 환경 : 클라이언트 사이드 Web API를 호스트 객체로 제공
            * ex) DOM, BOM, Canvas, XMLHttpRequest, fetch, requestAnimationFrame, SVG, Web Storage, Web Component, Web Worker
        * Node.js 환경 : Node.js 고유의 API를 호스트 객체로 제공
* `사용자 정의 객체` (user-defined objects)
    * 사용자가 직접 정의한 객체

## 표준 빌트인 객체 ([예제](./src/standard_built_in.html))
* 자바스크립트는 40여 개의 표준 빌트인 객체를 제공
    * Object, String, Number, Boolean, Symbol, Date, Math, RegExp, Array, Map/Set, WeakMap/WeakSet, Function, Promise, Reflect, Proxy, JSON, Error 등...

* Math, Reflect, JSON을 제외한 표준 빌트인 객체는 생성자 함수 객체이다.
    * 생성자 함수 객체 표준 빌트인 객체
        * 프로토타입 메서드, 정적 메서드 제공
    * !(생성자 함수 객체) 표준 빌트인 객체 (Math, Reflect, JSON)
        * 정적 메서드 제공

* 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체이다.
```javascript
// String 생성자 함수에 의한 String 객체 생성
const strObj = new String('Park'); // String {"Park"}
// String 생성자 함수를 통해 생성한 strObj 객체의 프로토타입은 String.prototype이다.
console.log(Object.getPrototypeOf(strObj) === String.prototype); // true
```

* 표준 빌트인 객체는 인스턴스 없이도 호출 가능한 빌트인 정적 메서드를 제공함
```javascript
// Number 생성자 함수에 의한 Number 객체 생성
const numObj = new Number(1.5); // Number {1.5}
// toFixed는 Number.prototype의 프로토타입 메서드다.
// Number.prototype.toFixed는 소수점 자리를 반올림하여 문자열로 반환됨
console.log(numObj.toFixed()); // 2
// isInteger는 Number의 정적 메서드
// Number.isInteger는 인수가 정수인지 검사하여 그 결과를 Boolean으로 반환
console.log(Number.isInteger(0.5)); // false
```

## 원시값과 래퍼 객체
* 문자열, 숫자, 불리언 값(**원시값**)에 대해 **객체처럼 접근**하면 생성되는 임시 객체를 **래퍼 객체**라고 한다.
    * 마침표 표기법(또는 대괄호 표기법)으로 접근
    * 자바스크립트 엔진이 암묵적으로 원시값 --> 임시 객체로 변환
    * 문자열 래퍼 객체인 String 생성자 함수의 인스턴스는 String.prototype의 메서드를 상속받아 사용할 수 있다.
    
```javascript
const str = 'hello'; // 문자열 원시값
// 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체 처럼 동작
console.log(str.length); // 5
console.log(str.toUpperCase()); // HELLO
console.log(typeof str); // string
```

![String_prototype_chain](https://user-images.githubusercontent.com/63139527/173528687-4414b1af-2d73-4a77-bac3-b4cef2bccc3c.png)

* 참조가 끝나면 원시값 형태로 되돌아간다.
    * 가비지 컬렉션 대상이 되어 메모리 해제됨

```javascript
const str = 'hello'; // 문자열 (원시값)
// 암묵적으로 생성된 래퍼 객체를 가리킴
str.name = 'Park'; // 래퍼 객체에 name 프로퍼티 동적 추가

// 이때 래퍼 객체는 아무도 참조하지 않는 상태이므로 가비지 컬렉션의 대상이 됨

// 새롭게 암묵적으로 생성된 래퍼 객체를 가리킴
console.log(str.nmae); // undefined
console.log(typeof str, str); // string hello
```

* ES6에서 새롭게 도입된 원시값인 심벌도 래퍼 객체를 생성함
    * 리터럴 표기법으로 생성 X
    * Symbol 함수를 통해 생성
    * 원시 값과는 차이가 있음

* **String, Number, Boolean 생성자 함수를 new 연산자와 함께 호출하여 문자열, 숫자, 불리언 인스턴스를 생성할 필요가 없으며 권장하지 않음**

* null과 undefined는 래퍼 객체를 생성하지 않음
    * 객체 처럼 사용하면 에러가 발생

## 전역 객체
* 코드가 실행되기 이전 **자바스크립트 엔진에 의해 생성**되는 특수한 객체이며, 어떤 객체에도 속하지 않은 **최상위 객체**

* 전역 객체는 표준 빌트인 객체, 호스트 객체, var 키워드로 선언한 전역 변수와 전역 함수를 프로퍼티로 갖는다.

* 전역 객체 특징
    * 개발자가 의도적으로 생성할 수 없음. 전역 객체를 생성할 수 있는 생성자 함수가 제공 안함
    * 전역 객체의 프로퍼티를 참조할 때 window(또는 global) 식별자를 생략할 수 있음

* let 이나 const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
    * 보이지 않는 개념적 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하기 때문

**NOTE**
```
ES11에서 도입된 globalThis는 호스트 객체를 통일한 식별자이다.
```
```javascript
// 브라우저 환경
globalThis === this     // true
globalThis === window   // true
globalThis === self     // true
globalThis === frames   // true
// Node.js 환경
globalThis === this     // true
globalThis === global   // true
```

### 1. 빌트인 전역 프로퍼티
* 빌트인 전역 프로퍼티는 전역 객체의 프로퍼티를 의미
* 주로 애플리케이션 전역에서 사용하는 값을 제공

`Infinity`
- Infinity 프로퍼티는 무한대를 나타내는 숫자값
```javascript
// 전역 프로퍼티는 window를 생략하고 참조 가능
console.log(window.Infinity === Infinity); // true

// 양의 무한대
console.log(3/0); // Infinity
// 음의 무한대
console.log(-3/0); // -Infinity
// Infinity는 숫자값이다.
console.log(typeof Infinity); // number
```

`NaN`
* NaN 프로퍼티는 숫자가 아님(Not-a-Number)을 나타내는 숫자값 (= Number.NaN 프로퍼티)
```javascript
console.log(window.NaN); // NaN
console.log(Number('xyz')); // NaN
console.log(1 * 'string'); // NaN
console.log(typeof NaN); // number
```

`undefined`
* undefined 프로퍼티는 원시타입 undefined 값
```javascript
console.log(window.undefined); // undefined
var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

### 2. 빌트인 전역 함수
* 빌트인 전역 함수는 애플리케이션 전역에서 호출할 수 있는 빌트인 함수(=전역 객체의 메서드)

`eval` ([예제](./src/eval.html))
* 자바스크립트 코드를 나타내는 문자열을 인수로 전달 받음
* 전달 받은 인수가
    * 표현식이면, 문자열 코드를 런타임에 평가하여 값 생성
    * 문이라면, 문자열 코드를 런타임에 실행
    * 여러 개의 문으로 이루어져 있다면, 모든 문을 실행한 다음 마지막 결과값을 반환
```javascript
/**
 * 주어진 문자열 코드(인수)를 런타임에 평가 또는 실행한다.
 * @param {string} code - 코드를 나타내는 문자열
 * @returns {*} 문자열 코드를 평가/실행한 결과값
 */
eval(code);
```
* eval 함수는 자신이 호출된 위치에 해당하는 기존의 스코프를 런타임에 동적으로 수정
* strict mode에서는 eval 함수 자신의 자체적인 스코프를 생성
* let, consst 키워드를 사용한 변수 선언문이면 암묵적으로 strict mode 적용

* **eval 함수 사용은 금지한다!**
    * 보안에 매우 취약함
    * 자바스크립트 엔진에 의한 최적화 적용이 안되 성능이 안좋음

`isFinite` ([예제](./src/isFinite.html))
```javascript
/**
 * 전달받은 인수가 유한수인지 확인하고 그 결과를 반환
 * @param {number} testValue - 검사 대상 값
 * @returns {boolean} 유한수 여부 확인 결과
 */
isFinite(testValue)
```
* isFinite(null)은 true 반환
    * null을 숫자로 변환하여 검사를 수행 -> null의 숫자 타입은 0 이다.

`isNaN` ([예제](./src/isNaN.html))
```javascript
/**
 * 주어진 숫자가 NaN인지 확인하고 그 결과를 반환
 * @param {number} testValue - 검사 대상 값
 * @returns {boolean} NaN 여부 확인 결과
 */
isNaN(testValue)
```

`parseFloat` ([예제](./src/parseFloat.html))
```javascript
/**
 * 전달받은 문자열 인수를 실수로 해석하여 반환
 * @param {string} string - 변환 대상 값
 * @returns {number} 변환 결과
 */
parseFloat(string)
```

`parseInt` ([예제](./src/parseInt.html))
```javascript
/**
 * 전달받은 문자열 인수를 정수로해석하여 반환
 * @param {string} string - 변환 대상 값
 * @param {number} [radix] - 진법을 나타내는 기수(2 ~ 36, 기본값 10진수)
 * @returns {number} 변환 결과
 */
parseInt(string, radix);
```
* 인수가 문자열이 아니면 문자열로 변환한 다음, 정수로 해석하여 변환
* 기수를 지정하여 10진수 숫자를 해당 기수의 문자열로 변환하여 반환하고 싶을 떈, `Number.prototype.toString` 메서드를 사용
* 16진수 리터럴("0x" 또는 "0X")이면 16진수로 해석하여 10진수 정수로 반환
* 2진수 리터럴("0b")과 8진수 리터럴("0o")는 해석하지 못한다.
* 문자열의 첫 번째 문자가 해당 지수의 숫자로 변환될 수 없다면 NaN을 반환
* 두 번째 문자부터 해당 진수를 나타내는 숫자가 아닌 문자와 마주치면 무시되며, 해석된 정수값만 반환
* 공백이 있다면 첫 번쨰 문자열만 해석하여 반환하며 앞뒤 공백은 무시됨

`encodeURI / decodeURI` ([예제](./src/encode_decode_uri.html))
* 이스케이프 처리
    * 어떤 시스템에서도 읽을 수 있는 아스키 문자 셋으로 변환
    * ex) 공백 문자 ==> %20
    * ex) 한글 "가" ==> %EC%9E%90

* URI 문법 형식 표준 RFC3986
    * URL은 아스키 문자 셋으로만 구성
    * URL내에서 의미를 갖고 있는 문자(%, ?, #)나 URL에 올 수 없는 문자(한글, 공백 등) 또는 시스템에 의해 해석될수 있는 문자(<,>)를 이스케이프 처리하여 야기될 수 있는 문제를 예방하기 위해 이스케이프 처리가 필요
    * 알파벳, 0~9 숫자, -_.!~*'() 문자는 제외

![URI](https://user-images.githubusercontent.com/63139527/174308674-ff9f3501-14b4-4fa9-8160-160df3ab29a7.png)

```javascript
/**
 * 완전한 URI를 문자열로 전달받아 이스케이프 처리를 위해 인코딩
 * @param {string} uri - 완전한 URI
 * @returns {string} 인코딩된 URI
 */
encodeURI(uri);

/**
 * 인코딩된 URI를 전달받아 이스케이프 처리 이전으로 디코딩한다.
 * @param {string} encodedURI - 인코딩된 URI
 * @returns {string} 디코딩된 URI
 */
decodeURI(encodedURI);
```
* **쿼리 스트링 구분자로 사용되는 =, ?, & 은 인코딩하지 않음** 


`encodeURIComponent / decodeURIComponent` ([예제](./src/uri_component.html))
```javascript
/**
 * URI의 구성요소를 전달받아 이스케이프 처리를 위해 인코딩
 * @param {string} uriComponent - URI 구성요소
 * @returns {string} 인코딩된 URI의 구성요소
 */
encodeURIComponent(uriComponent);

/**
 * 인코딩된 URI 구성요소를 전달받아 이스케이프 처리 이전으로 디코딩
 * @param {string} encodedURIComponent - 인코딩된 URI 구성요소
 * @returns {string} 디코딩된 URI 구성요소
 */
decodeURIComponent(encodedURIComponent);
```
* **쿼리 스트링 구분자로 사용되는 =, ?, & 까지 인코딩 한다.**

### 3. 암묵적 전역
* 자바스크립트 엔진은 y=20을 window.y=20으로 해석하여 전역 객체에 프로퍼티를 동적 생성
* y는 전역 객체의 프로퍼티가 되어 전역 변수처럼 동작
* 이러한 현상을 **암묵적 전역** 이라고 한다.
```javascript
var x = 10; // 전역 변수
function foo() {
    // 선언하지 않은 식별자에 값 할당
    y = 20; // window.y = 20;
}
foo();
// 선언하지 않은 식별자 y를 전역에서 참조할 수 있음
console.log(x + y); // 30
```

* 하지만 y는 전역 객체의 프로퍼티로 추가됨  
=> y는 변수가 아님 -> 변수 호이스팅이 발생하지 않음

```javascript
// 전역 변수 x는 호이스팅이 발생
console.log(x); // undefined
// 전역 객체의 프로퍼티인 y는 호이스팅이 발생 하지 않음
console.log(y); // ReferenceError : y is not defined

var x = 10; // 전역 변수

function foo() {
    // 선언하지 않은 식별자에게 값 할당
    y = 20; // window.y = 20;
}
foo();
// 선언하지 않은 식별자 y를 전역에서 참조할 수 있음
console.log(x + y); // 30
```

* 프로퍼티인 y는 delete 연산자로 삭제 가능
* 전역 변수는 프로퍼티이지만 delete 연산자로 삭제 불가능
```javascript
var x = 10; // 전역 변수
function foo() {
    // 선언하지 않은 식별자에 값 할당
    y = 20; // window.y = 20;
    console.log(x + y);
}
foo(); // 30

console.log(window.x); // 10
console.log(window.y); // 20

delete x; // 전역 변수는 삭제 안됨
delete y; // 프로퍼티는 삭제됨

console.log(window.x); // 10
console.log(window.y); // undefined
```