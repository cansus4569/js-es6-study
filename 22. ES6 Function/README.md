# ES6 함수의 추가기능

## 함수의 구분
* ES6 이전까지 자바스크립트의 함수는 **별다른 구분 없이 다양한 목적으로 사용**되었다.
    * 일반적인 함수로서 호출
    * new 연산자와 함께 호출하여 인스턴스를 생성할 수 있는 생성자 함수로서 호출
    * 객체에 바인딩되어 메서드로서 호출
    * keyword : #편리함 #실수유발 #성능손해

```javascript
var foo = function() {
    return 1;
};
// 일반적임 함수 호출
foo(); // 1
// 생성자 함수로 호출
new foo(); // foo{}
// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // 1
```

* **ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.**
    * 즉, callable 이면서 constructor
    * 주의 : 메서드(객체에 바인딩된 함수)도 callable이며 constructor 이다.
    * 콜백함수도 constructor이기 때문에 불필요한 프로토타입 객체를 생성한다.

**참고**
```
호출할 수 있는 함수 객체 : callable
인스턴스를 생성할 수 있는 함수 객체 : constructor
인스턴스를 생성할 수 없는 함수 객체 : non-constructor
```

이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 3가지 종류로 명확히 구분했다.

ES6 함수의 구분|constructor|prototype|super|arguments
-|:-:|:-:|:-:|:-:
일반 함수(Normal)|O|O|X|O
메서드(Method)|X|X|O|O
화살표 함수(Arrow)|X|X|X|X

* 일반 함수는 함수 선언문이나 함수 표현식으로 정의한 함수를 말하며, ES6 이전의 함수와 차이가 없음
* ES6의 메서드와 화살표 함수는 ES6 이전의 함수와 명확한 차이가 있음

* 일반 함수는 **construcotr**이다.
* ES6의 메서드와 화살표 함수는 **non-constructor**이다.

## 메서드
* ES6 이전 일반적인 의미 : 객체에 바인딩된 함수를 일컫는 의미로 사용됨
* **ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.**
```javascript
const obj = {
    x: 1,
    // foo는 메서드다
    foo() { return this.x; },
    // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다.
    bar: function() { return this.x; }
};
console.log(obj.foo()); // 1
console.log(obj.bar()); // 1
```

* **ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor다.**
    * ES6 메서드는 생성자 함수로서 호출할 수 없다.
```javascript
new obj.foo(); // TypeError: obj.foo is not a constructor
new obj.bar(); // bar {}
```

* ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
```javascript
// obj.foo는 constructor가 아닌 ES6 메서드이므로 prototype 프로퍼티가 없다.
obj.foo.hasOwnProperty('prototype'); // false
// obj.bar는 constructor인 일반 함수이므로 prototype 프로퍼티가 있다.
obj.bar.hasOwnProperty('prototype'); // true
```

* 참고 : 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor 이다.
```javascript
String.prototype.toUpperCase.prototype; // undefined
String.fromCharCode.prototype;          // undefined

Number.prototype.toFixed.prototype;     // undefined
Number.isFinite.prototype;              // undefined

Array.prototype.map.prototype;          // undefined
Array.from.prototype;                   // undefined
```

* **ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [ [ HomeObject ] ]를 갖는다.**
    * super 참조 방식은 내부 슬롯 [ [ HomeObject ] ]을 이용함
    * ES6 메서드는 **super 키워드를 사용할 수 있음**

```javascript
const base = {
    name: 'Park',
    sayHi() {
        return `Hi! ${this.name}`;
    }
};

const derived = {
    __proto__: base,
    // sayHi는 ES6 메서드. ES6 메서드는 [[HomeObject]]를 가짐
    // sayHi의 [[HomeObject]]는 derived.prototype을 가리킨다.
    // super는 sayHi의 [[HomeObject]]의 프로토타입인 base.prototype을 가리킨다.
    sayHi() {
        return `${super.sayHi()}. how are you doing?`;
    }
};
console.log(derived.sayHi()); // Hi! Park. how are you doing?
```

* ES6 메서드가 아닌 함수는 super 키워드를 사용할 수 없다.
    * 내부 슬롯 [ [ HomeObject ] ]를 갖지 않기 때문
```javascript
const derived = {
    __proto__: base,
    // sayHi는 ES6 메서드가 아니다.
    // 따라서 sayHi는 [[HomeObject]]를 갖지 않으므로 super 키워드를 사용할 수 없다.
    sayHi: function() {
        // SyntaxError: 'super' keyword unexpected here
        return `${super.sayHi()}. how are you doing?`;
    }
};
```

* ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다.
* 메서드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.

## 화살표 함수
* 화살표 함수는 function 키워드 대신 화살표(=>, fat arrow)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있음
* 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.
### 1. 화살표 함수 정의
* 함수 정의
    * 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다.
    * 호출 방식은 기존 함수와 동일하다.
    ```javascript
    const multiply = (x, y) => x * y;
    multiply(2, 3); // 6
    ```
* 매개변수 선언
    * 매개변수가 여러 개인 경우 소괄호 ( ) 안에 매개변수를 선언한다.
    * 매개변수가 한 개인 경우 소괄호 ( ) 를 생략할 수 있다.
    * 매개변수가 없는 경우 소괄호 ( ) 를 생략할 수 없다.
    ```javascript
    const a = (x, y) => { ... };
    const b = x => { ... };
    const c = () => { ... };
    ```
* 함수 몸체 정의
    * 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 { } 를 생략할 수 있다.
    * 이때 함수 몸체 내부의 문이 값으로 평가될 수 있는 **표현식인 문**이라면 암묵적으로 반환된다.
    ```javascript
    const power = x => x ** 2;
    power(2); // 4
    // 위와 동일한 표현
    const power = => { return x ** 2; };
    ```
    * 중괄호 { }를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다.
    * 표현식이 아닌 문은 반환할 수 없기 때문
    ```javascript
    const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'
    // 위 표현은 다음과 같이 해석된다.
    const arrow = () => { return const x = 1; };
    // 중괄호 생략 못함
    const arrow = () => { const x = 1; };
    ```
    * 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호 ( )로 감싸 주어야 한다.
    ```javascript
    const create = (id, content) => ({ id, content });
    create(1, 'Javascript'); // { id: 1, content: "Javascript" }
    // 위 표현은 다음과 동일하다
    const create = (id, content) => { return { id, content }; };
    ```
    * 객체 리터럴을 소괄호 ( )로 감싸지 않으면 객체 리터럴의 중괄호 { }를 함수 몸체를 감싸는 중괄호 { }로 잘못 해석한다.
    ```javascript
    // { id, content } 를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
    const create = (id, content) => { id, content };
    create(1, 'Javascript'); // undefined
    ```
    * 함수 몸체가 여러 개의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 { }를 생략할 수 없다.
    * 반환값이 있다면 명시적으로 반환해야 한다.
    ```javascript
    const sum = (a, b) => {
        const result = a + b;
        return result;
    }
    ```
    * 화살표 함수는 즉시 실행 함수로 사용할 수 있다.
    ```javascript
    const person = (name => ({
        sayHi() { return `Hi? My name is ${name}.`; }
    }))('Park');
    console.log(person.sayHi()); // Hi? My name is Park.
    ```
    * 화살표 함수도 일급 객체이므로 고차 함수에 인수로 전달할 수 있다.
    * 콜백 함수로서 정의할 때 유용하다.
    ```javascript
    // ES5
    [1, 2, 3].map(function (v) {
        return v * 2;
    });
    // ES6
    [1, 2, 3].map(v => v * 2); // [ 2, 4, 6 ]
    ```
### 2. 화살표 함수와 일반 함수의 차이

#### 2.1 화살표 함수는 인스턴스를 생성할 수 없는 non-construct 이다.
```javascript
const Foo = () => {};
// 화살표 함수는 생성자 함수로 호출할 수 없다.
new Foo(); // TypeError: Foo is not a constructor
```
* 화살표 함수는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
```javascript
const Foo = () => {};
// 화살표 함수는 prototype 프로퍼티가 없다.
Foo.hasOwnProperty('prototype'); // false
```

#### 2.2 중복된 매개변수 이름을 선언할 수 없다.
* 일반 함수는 중복된 매개변수 이름을 선언해도 에러가 발생하지 않는다.
```javascript
function normal(a, a) { return a + a; }
console.log(normal(1, 2)); // 4
```
* 단, strict mode에서 중복된 매개변수 이름을 선언하면 에러가 발생한다.
```javascript
'use strict';
function normal(a, a) { return a + a; }
// SyntaxError: Duplicate parameter name not allowed in this context
```
* 화살표 함수에서도 중복된 매개변수 이름을 선언하면 에러가 발생한다.
```javascript
const arrow = (a, a) => a + a;
// SyntaxError: Duplicate parameter name not allowed in this context
```

#### 2.3 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.
* 화살표 함수 내부에서 this, arguments, super, new.target 을 참조하면 스코프 체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.
* 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this, arguments, super, new.target 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this, arguments, super, new.target을 참조한다.
### 3. this
* 화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 바로 this다.
* 콜백 함수 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된 것이다.

* ES6에서는 화살표 함수를 사용하여 "콜백 함수 내부의 this 문제"를 해결할 수 있다.
```javascript
class Prefixer {
    constructor(prefix) {
        this.prefix = prefix;
    }
    add(arr) {
        return arr.map(item => this.prefix + item);
    }
}
const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
// ['-webkit-transition', '-webkit-user-select']
```
* **화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexcial this라 한다.**
```javascript
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;
// 익명 함수에 상위 스코프의 this를 주입한다. 위 화살표 함수와 동일하게 동작한다.
(function () { return this.x; }).bind(this);
```

### 4. super

### 5. arguments

## Rest 파라미터

### 1. 기본 문법
* 매개변수 이름 앞에 3개의 점 ... 을 붙여서 정의한 매개변수를 의미
* **Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.**
```javascript
function foo(...rest) {
    // 매개변수 rest는 인수들의 목록을 배열로 전달받는 Rest 파라미터이다.
    console.log(rest); // [ 1, 2, 3, 4, 5 ]
}
foo(1, 2, 3, 4, 5);
```

* 일반 매개변수와 Rest 파라미터는 함께 사용 가능
    * 순차적으로 할당
```javascript
function foo (param, ...rest) {
    console.log(param); // 1
    console.log(rest); // [ 2, 3, 4, 5 ]
}
foo(1, 2, 3, 4, 5);
function bar (param1, param2, ...rest) {
    console.log(param1); // 1
    console.log(param2); // 2
    console.log(rest); // [ 3, 4, 5 ]
}
bar(1, 2, 3, 4, 5);
```

* Rest 파라미터는 반드시 마지막 파라미터이어야 한다.
```javascript
function foo(...rest, param1, param2) { }
foo(1, 2, 3, 4, 5); // SyntaxError: Rest Parameter must be last formal parameter
```
* Rest 파라미터는 단 하나만 선언할 수 있다.
```javascript
function foo(...rest1, ...rest2) { }
foo(1, 2, 3, 4, 5); // SyntaxError: Rest Parameter must be last formal parameter
```
* Rest 파라미터는 함수 객체의 length 프로퍼티에 영향을 주지 않는다.
```javascript
function foo(...rest) {}
console.log(foo.length); // 0

function bar(x, ...rest) {}
console.log(bar.length); // 1

function baz(x, y, ...rest) {}
console.log(baz.length); // 2
```

### 2. Rest 파라미터와 arguments 객체
* **ES5** 에선 가변 인자 함수의 경우 **arguments 객체**를 활용하여 인수를 전달 받았음
* arguments 객체는 전달된 인수들의 정보를 담고 있는 **유사 배열 객체 구조**를 가짐
    * 함수 내부에서 지역 변수처럼 사용 가능
    * 배열 메서드를 사용하려면 Function.prototype.call 이나 Function.prototype.apply 메서드를 사용하여 **배열로 변환해야하는 번거로움**을 가짐

```javascript
function sum() {
    // 유사 배열 객체인 arguments 객체를 배열로 변환한다.
    var array = Array.prototype.slice.call(arguments);

    return array.reduce(function(pre, cur) {
        return pre + cur;
    }, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```

* **ES6**에서는 **Rest 파라미터를 사용**하여 **인수 목록을 배열로 직접 전달받을 수 있다.**
    * 유사 배열 객체인 arguments 객체를 배열로 변환하는 과정을 피할 수 있음

```javascript
function sum(...args) {
    // Rest 파라미터 args에는 배열 [ 1, 2, 3, 4, 5 ]가 할당된다.
    return args.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3, 4, 5)); // 15
```

* 함수와 ES6 메서드는 Rest 파라미터와 arguments 객체 모두 사용 가능
* 화살표 함수는 Rest 파라미터만 사용 가능 (arguments 객체 X)

## 매개변수 기본값
* 자바스크립트 엔진이 매개변수의 개수와 인수의 개수를 체크하지 않기 떄문
    * 에러가 발생하지 않는다.
* 인수가 전달되지 않은 매개변수의 값은 undefined 이다.
```javascript
function sum (x, y) {
    return x + y;
}
console.log(sum(1)); // NaN
```

* 매개변수에 기본값을 할당할 필요가 있으며, 방어 코드가 필요함
```javascript
function sum (x, y) {
    // 인수가 전달되지 않아 매개변수의 값이 undefined인 경우 기본값을 할당
    x = x || 0;
    y = y || 0;
    
    return x + y;
}
console.log(sum(1,2)); // 3
console.log(sum(1)); // 1
```

* ES6에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간소화할 수 있다.
```javascript
function sum (x = 0, y = 0) {
    return x + y;
}
console.log(sum(1,2)); // 3
console.log(sum(1)); // 1
```

* 매개변수 기본값은 매개변수에 인수를 전달하지 않은 경우와 undefined를 전달한 경우에만 유효하다.
```javascript
function logName(name = 'Park') {
    console.log(name);
}
logName(); // Park
logName(undefined); // Park
logName(null); // null
```

* Rest 파라미터에는 기본값을 지정할 수 없다.
```javascript
function foo(...rest = []) {
    console.log(rest);
}
// SyntaxError: Rest Parameter may not have a default initializer
```

* 함수 객체의 length 프로퍼티와 arguments 객체에 아무런 영향을 주지 않는다.
```javascript
function sum(x, y = 0) {
    console.log(arguments);
}

console.log(sum.length); // 1

sum(1);     // Arguments { '0': 1 }
sum(1, 2);  // Arguments { '0': 1, '1': 2 }
```