# 생성자 함수에 의한 객체 생성
* [객체 리터럴](../06.%20Basic%20Object/README.md)을 이용한 객체 생성 방식
* 이번엔 생성장 함수를 사용하여 객체를 생성하는 방식을 살펴 본다.

## Object 생성자 함수
* **new 연산자**와 **Object 생성자 함수**를 호출하면 빈 객체를 생성하여 반환
* `생성자 함수란?`
    * new 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수
    * 생성자 함수에 의해 생성된 객체를 인스턴스라고 한다.
* 생성자 함수 종류
    * Object / String / Number / Boolean / Function
    * Array / Date / RegExp / Promise / 기타 등등(빌트인 생성자 함수)

```javascript
// 빈 객체 생성
const person = new Object();

// 프로퍼티 동적 추가
person.name = 'Cansus';
person.sayHello = function () {
    console.log('Hi, My name is ' + this.name);
};

console.log(person); // {name: "Cansus", sayHello: function}
person.sayHello(); // Hi, My name is Cansus
```

## 생성자 함수

### 1. 객체 리터럴 방식의 문제점
* 장점 : 객체 생성 방식이 **직관적**이고 **간편**하다.
* 단점 : 단 하나의 객체만 생성한다.
    * 동일한 프로퍼티를 갖는 객체를 여러개 생성시 비효율적
    * ex) 동일한 프로퍼티 구조를 가진 객체 10개 생성시 -> 10번의 객체 리터럴을 사용
* 다음 코드와 같이 프로퍼티 구조가 동일함에도 불구하고 매번 같은 프로퍼티와 메서드를 기술해야 함
```javascript
const circle1 = {   // 1번째 객체 생성
    radius: 5,      // 데이터 프로퍼티만 다름
    getDiameter() { // 동일한 메서드
        return 2 * this.radius;
    }
};
console.log(circle1.getDiameter()); // 10

const circle2 = {   // 2번째 객체 생성
    radius: 10,     // 데이터 프로퍼티만 다름
    getDiameter() { // 동일한 메서드
        return 2 * this.radius;
    }
};
console.log(circle2.getDiameter()); // 20
```

### 2. 생성자 함수 방식의 장점
* <b>객체(인스턴스)</b>를 생성하기 위한 <b>템플릿(클래스)</b>처럼 프로퍼티 구조가 동일한 객체 여러개를 간편하게 생성 가능하다.
* `개인적인 생각`
    * 함수 선언문(함수 표현식)을 통해 만들어진 함수를 new 연산자와 결합되니 객체가 된다
    * [테스트 코드](./src/my_test1.html)
    * 생성자 함수에 대해 찾아본 내용
        * 생성자 함수와 일반 함수에 기술적 차이는 없음
        * 생성자 함수 이름의 첫 글자는 대문자로 시작
        * 반드시 `new` 연산자를 붙여 실행한다.
* 책에서의 생성자 함수 정의
    * 생성자 함수는 객체(인스턴스)를 생성하는 함수
    * 일반 함수와 동일한 방법으로 생성자 함수를 정의하고 **new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.**
    * new 연산자와 함께 생성자 함수를 호출하지 않으면 일반 함수로 동작한다.
```javascript
//  생성자 함수 (= 함수 선언문)
function Circle(radius) {
    // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}
// 인스턴스의 생성
const circle1 = new Circle(5);
const circle2 = new Circle(10);
console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

// new 연사자와 함께 호출하지 않으면 생성자 함수로 동작하지 않음
// 즉, 일반 함수로서 호출된다.
const circle3 = Circle(15);
// 일반 함수로 호출된 Circle은 반환문이 없으므로 undefined 반환
console.log(circle3); // undefined
// 일반 함수로 호출된 Circle 내의 this는 전역 객체를 가리킨다.
console.log(radius); // 15 --> window(=this 전역 객체).radius
```

**NOTE**
```
this
this는 객체 자신의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이다.
this 바인딩은 함수 호출 방식에 따라 동적을 결정된다.
```
함수 호출 방식|this가 가리키는 값(this 바인딩)
-|-
일반 함수로서 호출|전역 객체
메서드로서 호출|메서드를 호출한 객체(마침표 앞의 객체)
생성자 함수로서 호출|생성자 함수가 (미래에) 생성할 인스턴스
```javascript
// 함수는 다양한 방식으로 호출될 수 있다.
function foo() {
    console.log(this);
}

// 일반적인 함수로서 호출
// 전역 객체는 브라우저 환경에서는 window, Node.js 환경에서는 global을 가리킨다.
foo(); // window 전역 객체

const obj = { foo }; // ES6 프로퍼티 축약 표현
// 메서드로 호출 형태
obj.foo(); // obj
// 생성자 함수로서 호출
const inst = new foo(); // inst
```
### 3. 생성자 함수의 인스턴스 생성 과정
* 생성자 함수의 역할
    * 인스턴트를 생성 (필수)
    * 생성된 인스턴트를 초기화(프로퍼티 추가 및 초기값 할당) (옵션)
```javascript
// 생성자 함수
function Circle(radius) {
    // 인스턴스 초기화
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
    // 여기서 this 바인딩은 생성자 함수로 호출되기에, 생성자 함수가 생성할 인스턴스를 가리킨다.
}
// 인스턴스 생성 -> 반환
const circle1 = new Circle(5); // 생성자 함수로 Circle 객체 생성
```

#### 3.1 인스턴스 생성과 this 바인딩
* `바인딩`이란?
    * 식별자와 값을 연결하는 과정
    * ex) 변수 선언 : 변수 이름(식별자) ~ 확보된 메모리 공간 주소 : 바인딩
* `this 바인딩`
    * this(키워드로 분류되지만 식별자 역할을 함)와 this가 가리킬 객체를 바인딩
    * 런타임 이전에 실행된다.

### 3.2 인스턴스 초기화
* 생성자 함수 몸체 내부의 코드가 한 줄씩 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.

## 3.3 인스턴스 반환
* 생성자 함수 내부 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
* 명시적으로 객체를 반환하면 this가 반환되지 못하고 return에 정의된 객체를 반환된다.
* 명시적으로 원시 값을 반환하면 원시 값 반환은 무시되고 암묵적으로 this가 반환된다.
* **결론 : 생성자 함수 내부에서 return 문을 반드시 생략해야 한다.**

```javascript
function Circle(radius) {
    // 3.1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // Circle {}
    
    // 3.2. this에 바인딩되어 있는 인스턴스를 초기화 한다.
    this.radius = radius; // => Circle.radius = radius;
    this.getDiameter = function () { // => Circle.getDiameter = function () { }
        return 2 * this.radius; // => return 2 * Circle.radius
    };
    // 3.3-1. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    // return 생략

    // 3.3-2. 명시적으로 객체를 반환하면 암묵적인 this반환이 무시된다.
    return {};

    // 3.3-3. 명시적으로 원시 값을 반환하면 원시 값 반환이 무시되고 암묵적으로 this가 반환된다.
    return 100;
}
// 3-1 결과 : 인스턴스 생성. Circle 생성자 함수는 암묵적으로 this를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: function}
// 3-2 결과 : 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // {}
// 3-3 결과 : 인스턴스 생성. Circle 생성자 함수는 명시적으로 반환한 객체를 반환한다.
const circle = new Circle(1);
console.log(circle); // Circle {radius: 1, getDiameter: function}

```

### 4. 내부 메서드 [ [ Call ] ] 과 [ [ Construct ] ]
* 함수는 객체이지만 일반 객체와는 다르다.
    * **일반 객체는 호출할 수 없지만 함수는 호출 가능**
* 함수가 호출되는 케이스
    * **일반 함수**로 호출 : 함수 객체 내부 메서드 **[ [ Call ] ]** 이 호출됨
    * **생성자 함수**로 호출 : 함수 객체 내부 메서드 **[ [ Construct ] ]** 이 호출됨
```javascript
function foo() { }
// 일반적인 함수로서 호출: [[Call]]이 호출됨
foo();
// 생성자 함수로서 호출: [[Construct]]가 호출됨
new foo();
```
* 함수 내부 메서드를 무엇을 가지는지에 따른 함수 객체
    * callable : [ [ Call ] ] 갖는 함수 객체
        * 호출할 수 있는 객체, 즉 일반 함수
    * constructor : [ [ Construct ] ] 갖는 함수 객체
        * 생성자 함수로 호출할 수 있는 함수
    * non-constructor : [ [ Construct ] ] 갖지 않는 함수 객체
        * 생성자 함수로 호출할 수 없는 함수
* 모든 함수 객체는 callable 이지만, constructor 이거나 non-constructor 이다.

![function_object](https://user-images.githubusercontent.com/63139527/172150791-824059d6-fd47-47d7-94a4-7f60f67b81bb.png)

### 5. constructor와 non-constructor의 구분
* 자바스크립트 엔진은 함수 정의 방식에 따라 구분한다
    * constructor : 함수 선언문, 함수 표현식, 클래스
    * non-constructor : 메서드(ES6 메서드 축약 표현), 화살표 함수

```javascript
// 일반 함수 정의 : 함수 선언문, 함수 표현식
function foo() {}
const bar = function () {};
// 프로퍼티 x의 값으로 할당된 것은 일반 함수로 정의된 함수다. 메서드로 인정하지 않음
const baz = {
    x: function () {}
};

// 일반 함수로 정의된 함수: constructor
new foo(); // foo {}
new bar(); // bar {}
new baz.x(); // x {}

// 화살표 함수 정의
const arrow = () => {};
new arrow(); // TypeError: arrow is not a constructor

// 메서드 정의: ES6의 메서드 축약 표현만 메서드로 인정한다.
const obj = {
    x() {}
};
new obj.x(); // TypeError: obj.x is not a constructor
```

* **non-constructor인 함수 객체를 생성자 함수로 호출하면 에러가 발생한다.**

### 6. new 연산자
* new 연산자와 함께 함수를 호출하면 함수 객체의 내부 메서드 [ [ Construct ] ]가 호출되며 생성자 함수로 동작한다.
* new 연산자와 함께 호출하는 함수는 non-constructor가 아닌 consturctor이어야 한다.
* 생성자 함수는 일반적으로 **첫 문자를 대문자**로 기술하는 `파스칼 케이스`로 명명하여 일반 함수와 구별할 수 있도록 노력한다.

```javascript
// 생성자 함수로서 정의하지 않은 일반 함수
function add(x, y) {
    return x + y; // 원시 값 반환
}
// 일반 함수를 new 연산자로 호출
let inst = new add();
// 객체를 반환하지 않으므로 반환문이 무시. 빈 객체로 생성되어 반환
console.log(inst); // {}

// 객체를 반환하는 일반 함수
function createUser(name, role) {
    return { name, role }; // 객체 반환
}
// 일반 함수를 new 연산자로 호출
inst = new createUser('Cansus', 'admin');
// 객체를 반환
console.log(inst); // { name: 'Cansus', role: 'admin' }

// 생성자 함수 (파스칼 케이스로 명명)
function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function() {
        return 2 * this.radius;
    };
}
// new 연산자 없이 생성자 함수 호출 -> 일반 함수 호출
const circle = Circle(5);
console.log(circle); // undefined
// 일반 함수 내부의 this는 전역 객체 window를 가리킴
console.log(radius); // 5  == window.radius
console.log(getDiameter()); // 10 == window.getDiameter()

circle.getDiameter();
// TypeError: Cannot read property 'getDiameter' of undefined
```

### 7. new.target
* ES6에서 지원함 (IE에서는 지원안함)
* this와 유사하게 constructor인 모든 함수 내부에서 **암묵적인 지역 변수와 같이 사용**되며 **메타 프로퍼티**라고 부른다.
* new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.
    * new 연산자 없이 일반 함수로 호출된 함수 내부의 new.target 값은 undefined
* `개인적인 생각`
    * ES6 부터 지원하며, new.target 값을 통해 생성자 함수가 new 연산자로 호출했는지 안했는지 여부를 알수 있도록 도와주는 역할을 하는거 같다.

```javascript
// 생성자 함수
function Circle(radius) {
    // 이 함수가 new 연산자로 호출되지 않았다면, new.target = undefined
    if(!new.target) {
        // 재귀 호출 방식으로 new 연산자로 재호출하여 생성된 인스턴르를 반환
        return new Circle(radius);
    }
    // 생략
}
// new 연산자 호출 없어도 생성자 함수 내부에서 new 연산자 호출로 반환된다.
const circle = Circle(5);
```

* IE에서는 지원하지 않으므로, 스코프 세이프 생성자 패턴을 이용한 방식
```javascript
// Scope-Safe Constructor Pattern
function Circle(radius) {
    // new 연산자 호출했다면, 함수의 선두에서 빈 객체를 생성하고
    // this에 바인딩 한다. 이때 this와 Circle은 프로토타입에 의해 연결됨

    // new 연산자 호출이 아니라면, 이 시점의 this는 전역 객체 window를 가리킨다.
    // 즉, this와 Circle은 프로토타입에 의해 연결되지 않는다.
    if( !(this instanceof Circle)) {
        // 재귀 호출 방식으로 new 연산자로 재호출하여 생성된 인스턴르를 반환
        return new Circle(radius);
    }
    // 생략
}
// new 연산자 호출 없어도 생성자 함수 내부에서 new 연산자 호출로 반환된다.
const circle = Circle(5);
```

* 대부분의 빌트인 생성자 함수는 new연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환함

* ex) Object와 Function 생성자 함수는 new 연산자 호출 없어도 new 연산자로 호출한것과 동일하게 동작한다.
```javascript
let obj = new Object();
console.log(obj); // {}

obj = Object();
console.log(obj); // {}

let f = new Function('x', 'return x ** x');
console.log(f); // function anonymous(x) { return x ** x }

f = Function('x', 'return x ** x');
console.log(f); // function anonymous(x) { return x ** x }
```

* ex) String, Number, Boolean 생성자 함수는
    * new 연산자 호출시 -> 객체 반환
    * new 연산자 호출 없을시 -> 값을 반환
