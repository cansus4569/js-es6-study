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

### 1. 빌트인 전역 프로퍼티

### 2. 빌트인 전역 함수

### 3. 암묵적 전역
