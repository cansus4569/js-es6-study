# 함수와 일급 객체

## 일급 객체 ([예제](./src/first_class_object.html))
* 다음 조건을 만족하는 객체는 **일급 객체**이다.
    * 1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
    * 2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
    * 3. 함수의 매개변수에 전달할 수 있다.
    * 4. 함수의 반환값으로 사용할 수 있다.

* 결론 : 자바스크립트의 함수는 일급 객체이다.
    * 함수형 프로그래밍을 가능케 함
    * 함수 객체는 호출이 가능하며, 함수 고유의 프로퍼티를 소유한다.

## 함수 객체의 프로퍼티
![function_obejct_property](https://user-images.githubusercontent.com/63139527/172164948-812b8678-8ddf-4896-8fec-998339558312.png)

* 함수 객체의 고유 프로퍼티 (일반 객체엔 없음)
    * arguments
    * caller
    * length
    * name
    * prototype
* __ proto __ 는 Object.prototype 객체의 프로퍼티를 상속받은 접근자 프로퍼티
    * 즉, 함수 객체 고유의 프로퍼티가 아님
    * Object.prototype 객체의 __ proto __ 접근자 프로퍼티는 모든 객체가 상속받아 사용 가능

### 1. arguments 프로퍼티
* 함수 객체의 arguments 프로퍼티 값은 arguments 객체
* arguments 객체는 함수 호출시 전달된 인수들의 정보를 담고 있는 유사 배열 객체
* 함수 내부에서 지역 변수처럼 사용 (외부에서 참조 불가능)
* ES3부터 표준 폐지되었지만, 현재 일부 브라우저에서 지원

![arguments](https://user-images.githubusercontent.com/63139527/172168258-80a3c8da-7636-4418-bd5c-247d55c4a18b.png)

* <font color="red" style="font-weight: bold;">빨간 네모 박스</font>
    * arguments 객체는 인수를 프로퍼티 값으로 소유하며 프로퍼티 키는 인수의 순서를 나타냄
* <font color="green" style="font-weight: bold;">초록 네모 박스</font>
    * legnth 프로퍼티는 인수의 개수를 나타냄
* <font color="blue" style="font-weight: bold;">빨간 네모 박스</font>
    * callee 프로퍼티는 호출되어 arguments 객체를 생성한 함수를 나타냄

* arguments 객체의 Symbol(Symbol.iterator) 프로퍼티는 arguments 객체를 순회 가능한 자료구조인 이터러블로 만들기 위한 프로퍼티 
    * [예제](./src/symbol_iterator.html )

* arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용함
    * 자바스크립트 특징 : 함수 호출 시 인수의 개수를 확인하지 않음
    * [예제](./src/var_args.html)

* arguments 객체는 배열 형태로 정보를 담고 있지만 실제 배열이 아닌, 유사 배열 객체
    * 유사 배열 객체는 배열 메서드를 사용하면 에러 발생함(실제 배열이 아님)
    * Function.prototype.apply/call/bind 메서드로 간접 호출해서 사용해야 함
    * [예제](./src/array_like_object.html)

**NOTE**
```
ES5에서 arguments 객체는 유사 배열 객체로 구분함
ES6부터 이터러블이 도입되면서 arguments 객체는 유사 배열 객체이면서 동시에 이터러블이다.

이터러블 : 이터레이션 프로토콜을 준수하며 순회 가능한 자료구조
```

* ES6에서 Rest 파라미터 도입
    * Function.prototype.apply/call/bind 메서드로 간접 호출의 번거로움을 해결함
    * [예제](./src/rest_parameter.html)

### 2. caller 프로퍼티
* ECMAScript 사양에 포함되지 않은 비표준 프로퍼티
    * 표준화 예정 없음
    * 관심이 없다면 지나쳐도 좋다...(?!)
* 함수 객체의 caller 프로퍼티는 함수 자신을 호출한 함수를 가리킨다.
```javascript
function foo (func) {
    return func();
}

function bar() {
    return 'caller : ' + bar.caller;
}
console.log(foo(bar)); // caller : function foo(func) {...}
console.log(bar()); // caller : null
```

### 3. length 프로퍼티
* **함수 객체의 length 프로퍼티**는 함수를 정의할 때 선언한 **매개변수의 개수**를 가리킨다.
    * arguments 객체의 length 프로퍼티와 혼동되지 말것!
    * **arguments 객체의 length 프로퍼티**는 **인자의 개수**를 가리킨다.

```javascript
function foo() {}
console.log(foo.length); // 0

function bar (x) {
    return x;
}
console.log(bar.length); // 1

function baz(x,y) {
    return x * y;
}
console.log(baz.length); // 2
```

### 4. name 프로퍼티
* 함수 객체의 name 프로퍼티는 함수 이름을 나타낸다.
    * ES6에서 정식 표준이 되었다.
* name 프로퍼티는 ES5와 ES6에서 동작이 다르다.
    * ex) 익명 함수 표현식 case
    * ES5 : name 프로퍼티는 빈 문자열 값
    * ES6 : name 프로퍼티는 함수 객체를 가리키는 식별자 값
```javascript
// 기명 함수 표현식
var a = function foo() {};
console.log(a.name); // foo

// 익명 함수 표현식
var b = function() {};
// ES5 : name 프로퍼티는 빈 문자열 값
// ES6 : name 프로퍼티는 함수 객체를 가리키는 식별자 값
console.log(b.name); // b

// 함수 선언문
function bar() {}
console.log(bar.name); // bar
```

### 5. __ proto __ 접근자 프로퍼티
* 모든 객체는 [ [ Prototype ] ] 이라는 내부 슬롯을 갖는다.
* [ [ Prototype ] ] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가리킨다.
* **__ proto __ 프로퍼티**는 [ [ Prototype ] ] 내부 슬롯이 가리키는 **프로토타입 객체에 접근**하기 위해 사용하는 **접근자 프로퍼티**이다.
    * [ [ Prototype ] ] 내부 슬롯에 직접 접근은 불가능하다.
    * __ proto __ 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근할 수 있다.

```javascript
const obj = { a: 1 };

// 객체 리터럴 방식으로 생성한 객체의 프로토타입 객체는 Object.prototype이다.
console.log(obj.__proto__ === Object.prototype); // true

// 객체 리터럴 방식으로 생성한 객체는 프로토타입 객체인 Object.prototype의 프로퍼티를 상속받는다.
// hasOwnProperty 메서드는 Object.prototype의 메서드다.
console.log(obj.hasOwnProperty('a')); // true
console.log(obj.hasOwnProperty('__proto__')); // false
```

**NOTE**
```
hasOwnProperty(prop) 메서드

인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우 : true 반환
인수로 전달받은 프로퍼티 키가 상속받은 프로토타입의 프로퍼티 키인 경우 : false 반환
```

### 6. prototype 프로퍼티
* 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor만 소유하는 프로퍼티이다.
* 일반 객체와 non-constructor 에는 prototype 프로퍼티가 없다.

```javascript
// 함수 객체는 prototype 프로퍼티를 소유 함
(function () {}).hasOwnProperty('prototype'); // true

// 일반 객체는 prototype 프로퍼티가 없음
({}).hasOwnProperty('prototype'); // false
```
* prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때  
생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킨다.