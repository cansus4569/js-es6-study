# 7번째 데이터 타입 Symbol

## 심벌이란?
* ES6에서 도입된 7번째 데이터 타입(변경 불가능한 원시 타입의 값)
* 심벌 값은 다른 값과 중복되지 않는 유일무이한 값
* 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용

## 심벌 값의 생성
### 1. Symbol 함수
* 심벌 값은 Symbol 함수를 호출로만 생성한다.
    * 다른 원시값은 리럴 표기법으로 생성 가능
* 심벌 값은 외부로 노출되지 않아 확인할 수 없으며, **다른값과 절대 중복되지 않는 유일무이한 값**
* Symbol 함수는 new 연산자와 함께 호출하지 않는다.
```javascript
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol
// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()

// Symbol 함수는 new 연산자와 함께 호출하지 않는다.
new Symbol(); // TypeError: Symbol is not a constructor
```
* 선택적으로 문자열을 인수로 전달 가능
    * 생성된 심벌값에 대한 설명(description)으로 디버깅 용도로만 사용
    * 심벌 값에 영향을 주지 않음
* 심벌 값에 대한 설명이 같더라도 생성된 심벌값은 유일무이한 값
```javascript
const mySymbol1 = Symbol('my');
const mySymbol2 = Symbol('my');
console.log(mySymbol1 === mySymbol2); // false
```
* 심벌 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성함
* description 프로퍼티와 toString 메서드(Symbol.prototype 프로퍼티)
* 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않음
* 불리언 타입으로는 암묵적으로 타입 변환된다.
```javascript
const mySymbol = Symbol('mySymbol');
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)

console.log(mySymbol + ''); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol); // TypeError: Cannot convert a Symbol value to a string

console.log(!!mySymbol); // true
if(mySymbol) console.log('mySymbol is not empty.');
```
### 2. Symbol.for / Symbol.keyFor 메서드
* Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
    * 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환
    * 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환

```javascript
// 심벌 값 생성
const s1 = Symbol.for('test');
// 심벌 값 반환
const s2 = Symbol.for('test');
console.log(s1 === s2); // true
```
* Symbol 함수 호출은 유밀무이한 값을 생성하지만 전역 심벌 레지스트리에 등록되어 관리되지 않음
* Symbol.for 메서드는 애플리케이션 전역에서 중복되지 않는 유일무이한 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.
* Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.
```javascript
const s1 = Symbol.for('test');
Symbol.keyFor(s1); // test
const s2 = Symbol('fail');
Symbol.keyFor(s2); // undefined
```
## 심벌과 상수
* 4방향을 나타내는 상수를 정의한다고 가정
* 아래의 코드에서 문제점 발생
    * 상수 값 1, 2, 3, 4는 변경/중복될 수 있음
    * 그래서 유밀 무이한 심벌 값을 이용하여 해결
```javascript
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의
// 이때 값 1, 2, 3, 4 에는 특별한 의미가 없고 상수 이름에 의미가 있음
const Direction = {
    UP: 1,
    DOWN: 2,
    LEFT: 3,
    RIGHT: 4
};
const test = Direction.UP;
if(test === Direction.UP) {
    console.log('Go to UP');
}
```
* 심벌 값을 이용한 예제
```javascript
// 중복될 가능성이 없는 심벌 값으로 상수 값을 생성
const Direction = {
    UP: Symbol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right')
};
const test = Direction.UP;
if(test === Direction.UP) {
    console.log('Go to UP');
}
```
* **enum**
```
enum은 명명된 숫자 상수의 집합으로 열거형이라고 부른다.
자바스크립트는 enum을 지원하지 않는다.
자바스크립트에서 enum을 흉내 내어 사용하려면 다음과 같이 객체의 변경을 방지하기 위해
객체를 동결하는 Object.freeze 메서드와 심벌 값을 사용한다.
```
```javascript
// Javascript enum
const Direction = Object.freeze({
    UP: Symbol('up'),
    DOWN: Symbol('down'),
    LEFT: Symbol('left'),
    RIGHT: Symbol('right')
});
const test = Direction.UP;
if(test === Direction.UP) {
    console.log('Go to UP');
}
```
## 심벌과 프로퍼티 키
* 심벌 값으로 프로퍼티 키를 동적 생성하여 프로퍼티를 만들어보자
* 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.
* 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.
* **심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.**
* 기존 프로퍼티 키와 충돌하지 않는 것은 물론, 미래에 추가될 어떤 프로퍼티 키와도 충돌할 위험이 없다.
```javascript
const obj = {
    // 심벌 값으로 프로퍼티 키를 생성
    [Symbol.for('mySymbol')]: 1
};
obj[Symbol.for('mySymbol')]; // 1
```
## 심벌과 프로퍼티 은닉
* 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 `for...in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 찾을 수 없다.
* 심벌 값을 프로퍼티 키로 사용하여 프로퍼티를 생성하면 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.
```javascript
const obj = {
    // 심벌 값으로 프로퍼티 키를 생성
    [Symbol.for('mySymbol')]: 1
};
for(const key in obj) {
    console.log(key); // 아무것도 출력되지 않음
}
console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```
* 프로퍼티를 완전하게 숨길 수 있는 것은 아니다.
* ES6에서 도입된 `Object.getOwnPropertySymbols` 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.
```javascript
const obj = {
    // 심벌 값으로 프로퍼티 키를 생성
    [Symbol.for('mySymbol')]: 1
};
// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]
// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```
## 심벌과 표준 빌트인 객체 확장
* 일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.
* 표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다.
* 확장 금지 이유
    * 미래에 표준 사양으로 추가될 메서드의 이름과 중복될 가능성
* 중복될 가능성이 없는 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 안전하게 표준 빌트인 객체를 확장할 수 있다.
```javascript
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for('sum')] = function () {
    return this.reduce((acc, cur) => acc + cur, 0);
};
[1, 2][Symbol.for('sum')](); // 3
```
## Well-known Symbol
* 자바스크립트가 기본 제공하는 빌트인 심벌 값을 ECMAScript 사양에서는 Well-known Symbol 이라고 부른다.
* 빌트인 심벌 값은 Symbol 함수의 프로퍼티에 할당되어 있다.
* Well-known Symbol 은 자바스크립트 엔진의 내부 알고리즘에 사용된다.

```
Array, String, Map, Set, TypedArray, arguments, NodeList, HTMLCollection과 같이  
for...of 문으로 순회 가능한 빌트인 이터러블은 Symbol.iterator를 키로 갖는 메서드를 가지며,
Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.
빌트인 이터러블은 이터레이션 프로토콜을 준수한다.

만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.
ECMAScript 사양에 규정되어 있는 대로 Symbol.iterator를 키로 갖는 메서드를 객체에 추가하고
이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다.
```
```javascript
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
    // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
    [Symbol.iterator]() {
        let cur = 1;
        const max = 5;
        // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
        return {
            next() {
                return { value: cur++, done: cur > max + 1 };
            }
        };
    }
};
for(const num of iterable) {
    console.log(num); // 1 2 3 4 5
}
```
## Symbol 결론
* 심벌은 중복되지 않는 상수 값을 생성하는 것을 물론 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉 하위 호환성을 보장하기 위해 도입되었다.