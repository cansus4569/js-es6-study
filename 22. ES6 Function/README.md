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

### 1. 화살표 함수 정의

### 2. 화살표 함수와 일반 함수의 차이

### 3. this

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