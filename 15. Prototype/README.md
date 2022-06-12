# 프로토타입
* 자바스크립트는 명령형, 함수형, **프로토타입 기반 객체지향 프로그래밍**을 지원하는 멀티 패러다임 프로그래밍 언어이다.

* 자바스크립트를 이루고 있는 거의 "모든 것"이 객체다. (원시 타입 값 제외)

**NOTE**
```
ES6에서 클래스가 도입됨
기존의 프로토타입 기반 객체지향 모델을 폐지하고 새로운 객체지향 모델을 제공하는것은 아님
클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 동일하게 동작하지는 않음
클래스는 생성자 함수보다 엄격하며, 생성자 함수에서는 제공하지않는 기능을 제공함
```

## 객체지향 프로그래밍
* 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임

* 속성 : 실체(사물이나 개념)의 특징이나 성질
* 추상화 : 프로그램에 필요한 속성만 간추려내어 표현하는 것
* 객체 : ([예제](./src/object_oriented_programming.html))
    * 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조
    * **상태** 데이터와 **동작**을 하나의 논리적인 단위로 묶은 복합적인 자료구조 
        * 상태 데이터 == 프로퍼티
        * 동작 == 메서드

## 상속과 프로토타입 ([예제](./src/inheritance.html))
* 상속
    * 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

* [생성자 함수](../13.%20Object%20creation%20by%20constructor%20function/src/my_test1.html) : 동일한 프로퍼티(메서드 포함) 구조를 갖는 객체를 여러개 생성할 때 유용함
    * 문제점 : 메서드는 모든 인스턴스가 동일한 내용의 메서드를 사용함 (중복)
        * 중복으로 인한 불필요한 메모리 낭비 발생  
        => 상속을 통해 불필요한 중복 제거!

![method_duplication](https://user-images.githubusercontent.com/63139527/172409651-124d98d4-478a-4303-bf64-38ad46f7fa34.png)

* **자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거**

![inheritance](https://user-images.githubusercontent.com/63139527/172409726-4c269d4b-f93a-432c-aec3-e991373e2e1c.png)

* Circle 생성자 함수가 생성한 모든 인스턴스는 상위(부모) 객체 역할을 하는  
Circle.prototype의 모든 프로퍼티와 메서드를 상속 받는다.

## 프로토타입 객체
* 프로토타입 객체 : 객체 간 상속을 구현하기 위해 사용
* 모든 객체는 생성 방식에 따라 프로토타입이 결정되고 [ [ Prototype ] ] 내부 슬롯에 저장됨
    * 객체 리터럴 : Object.prototype
    * 생성자 함수 : 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체
* [ [ Prototype ] ] 내부 슬롯은 __ proto __ 접근자 프로퍼티를 통해서만 가능하다.
* 프로토타입 constructor 프로퍼티 ---- 접근 ----> 생성자 함수
* 프로토타입 <---- 접근 ---- 생성자 함수 prototype 프로퍼티

![relationship](https://user-images.githubusercontent.com/63139527/172636381-f79eb6a4-0909-4f9a-b319-8887cf2e55b0.png)

### 1. __ proto __ 접근자 프로퍼티
* __ proto __ 접근자 프로퍼티를 통해 간접 접근
```javascript
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter 함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

* __ proto __ 접근자 프로퍼티는 객체가 직접 소유한 프로퍼티가 아닌 Object.prototype의 프로퍼티이다.
    * 모든 객체는 상속을 통해 Object.prototype.__ proto __ 접근자 프로퍼티를 사용 가능

```javascript
const person = { name: 'Park' }; // 객체 리터럴 방식의 객체 생성

// person 객체는 __proto__ 프로퍼티를 소유하지 않음을 증명
console.log(person.hasOwnProperty('__proto__')); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: f, set: f, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

* __ proto __ 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유
    * 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지
    * 프로토타입 체인은 계층 구조의 단방향 링크드 리스트 형태를 가짐 (순환 구조 X)
```javascript
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

* __ proto __ 접근자 프로퍼티를 코드 내에 직접 사용하는것을 권장하지 않음
```javascript
// obj는 프로토타입 체인의 종점. Object.__proto__를 상속받을 수 없음
const obj_root = Object.create(null);
console.log(obj_root.__proto__); // undefined

const obj = {};
const parent = { x: 1};

// obj 객체의 프로토타입 취득 (참조) , ES5
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입 교체 , ES6
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

### 2. 함수 객체의 prototype 프로퍼티
* 함수 객체 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.
```javascript
// 함수 객체 : prototype 프로퍼티 소유
(function() {}).hasOwnProperty('prototype'); // true
// 일반 객체 : prototype 프로퍼티 소유하지 않음
({}).hasOwnProperty('prototype'); // false
```
* non-constructor (화살표 함수), ES6 메서드 축약 표현은 prototype 프로퍼티 X, 프로토타입을 가지지 않음
```javascript
const Person = name => { // non-constructor
    this.name = name;
};
console.log(Person.hasOwnProperty('prototype')); // false
console.log(Person.prototype); // undefined
const obj = { // ES6 메서드 축약 표현
    foo() {}
};
console.log(obj.foo.hasOwnProperty('prototype')); // false
console.log(obj.foo.prototype); // undefined
```
* 모든 객체가 가지고 있는 __ proto __ 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프러프티는 결국 동일한 프로토타입을 가리킨다.
```javascript
// 생성자 함수
function Person(name) {
    this.name = name;
}
const me = new Person('Park');
// 결국 Person.prototype과 me.__proto__는 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
```

### 3. 프로토타입의 constructor 프로퍼티와 생성자 함수
* 모든 프로토타입은 constructor 프로퍼티를 갖는다.
* constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.
* 이러한 연결은 함수 객체가 생성될 떄 이뤄진다.
```javascript
// 생성자 함수
function Person(name) {
    this.name = name;
}
const me = new Person('Park');
// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

## 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입
* 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요
* 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다.
* 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.

리터럴 표기법|생성자 함수|프로토타입
-|-|-
객체 리터럴|Object|Object.prototype
함수 리터럴|Function|Function.prototype
배열 리터럴|Array|Array.prototype
정규 표현식 리터럴|RegExp|RegExp.prototype

## 프로토타입의 생성 시점
* 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성됨

### 1. 사용자 정의 생성자 함수와 프로토타입 생성 시점
* constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 생성됨
```javascript
// 함수 호이스팅
console.log(Person.prototype); // {constructor: f}

// 생성자 함수 (함수 선언문)
function Person(name) {
    this.name = name;
}
```
### 2. 빌트인 생성자 함수와 프로토타입 생성 시점
* 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성
* 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 [ [ Prototype ] ] 내부 슬롯에 할당됨

## 객체 생성 방식과 프로토타입의 결정
* 프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다.
* 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정됨
### 1. 객체 리터럴에 의해 생성된 객체의 프로토타입
* 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 **Object.prototype**
```javascript
const obj = { x: 1 };
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```
### 2. Object 생성자 함수에 의해 생성된 객체의 프로토타입
* 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 **Object.prototype**
```javascript
const obj = new Object();
obj.x = 1;
console.log(obj.constructor === Object); // true
console.log(obj.hasOwnProperty('x')); // true
```

**NOTE**
```
차이점 : 객체 리터럴 생성 방식 vs Object 생성자 함수 방식
: 프로퍼티를 추가하는 방식이 다름
 -> 객체 리터럴 방식 : 리터럴 내부에 프로퍼티를 추가
 -> Object 생성자 함수 방식 : 빈 객체 생성한 이후 동적 프로퍼티 추가
```
### 3. 생성자 함수에 의해 생성된 객체의 프로토타입
* 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 **생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객채**
* **하지만**, 사용자 정의 생성자 함수로 생성된 프로토타입의 프로퍼티는 **constructor 뿐**이다.
```javascript
function Person(name) {
    this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
    console.log('Hi! My name is ' + this.name);
};

const me = new Person("Park");
const you = new Person("Cansus");

me.sayHello(); // Hi! My name is Park
you.sayHello(); // Hi! My name is Cansus
```

## 프로토타입 체인
* 객체의 프로퍼티(메서드 포함)에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [ [ Prototype ] ] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다.
* 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.
```javascript
function Person(name) {
    this.name = name;
}

Person.prototype.sayHello = function () {
    console.log('Hi! My name is ' + this.name);
};

const me = new Person('Park');
```
![prototype_chain](https://user-images.githubusercontent.com/63139527/173193623-14f85bdc-67b7-4aa9-abe3-cb4857b0aa2d.png)


## 오버라이딩과 프로퍼티 섀도잉 ([예제]())
* 오버라이딩 : 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식
    
* 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토 타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는것이 아닌 인스턴스 프로퍼티로 추가함
    * 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 **오버라이딩** 했다.
    * 프로토타입 메서드 sayHello는 가려지는 현상을 **프로퍼티 섀도잉** 이라 함

![overriding](https://user-images.githubusercontent.com/63139527/172873995-7412bb63-3138-43f7-8e4e-78f95284099c.png)

## 프로토타입의 교체
* 프로토타입은 임의의 다른 객체로 변경이 가능
* 방법 : 생성자 함수, 인스턴스로 교체 가능
* 이런게 있다는 정도만 알아두자!
    * 이유 : **직접상속** 또는 **ES6 도입된 클래스**를 사용하는것을 권장
### 1. 생성자 함수에 의한 프로토타입의 교체 ([예제](./src/exchange_1.html))
![exchange1](https://user-images.githubusercontent.com/63139527/173193748-8ff3af77-fc5b-41dc-80fd-505c72b751bd.png)

### 2. 인스턴스에 의한 프로토타입의 교체 ([예제](./src/exchange_2.html))
![exchange2](https://user-images.githubusercontent.com/63139527/173193751-cb1c37a7-7ff5-42aa-83bc-db128a4b96bb.png)

## instanceof 연산자 ([예제](./src/instanceof.html))
* 이항 연산자
* 좌변 : 객체 식별자
* 우변 : 생성자 함수 식별자
```
객체 instanceof 생성자 함수
```
* 우변 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 **프로토타입 체인 상에 존재**하면 true 반환, 그렇지 않은 경우 false 반환
## 직접 상속

### 1. Object.create에 의한 직접 상속 ([예제](./src/object_create.html))
* Object.create 메서드 : 명시적으로 프로토타입을 지정하여 새로운 객체를 생성
    * 추상 연산 OrdinaryObjectCreate를 호출하여 생성함

```javascript
/**
 * 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체를 생성하여 반환
 * @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
 * @param {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
 * @return {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
 */
Object.create(prototype[, propertiesObject])
```

* Object.create 장점
    * new 연산자 없이도 객체 생성 가능
    * 프로토타입을 지정하면서 객체 생성 가능
    * 객체 리터럴에 의해 생성된 객체도 상속 가능

* Object.create 단점
    * 두 번째 인자로 프로퍼티를 정의하는 것이 번거로움
        * __ proto __ 접근자 프로퍼티를 사용한 직접 상속 방식 사용

* ESLint 에서는 Object.prototype 빌트인 메서드 직접 호출을 권장하지 않음
    * 이유 : Object.create로 프로토타입 체인 종점 객체를 생성할 수 있기 때문
        * 체인 종점 객체는 Object.prototype 빌트인 메서드 사용 불가

```javascript
// 프로토타입 체인 종점 객체 생성
const obj = Object.create(null);
obj.a = 1;

// obj는 Object.prototype 빌트인 메서드 사용 불가
console.log(obj.hasOwnProperty('a'));
// TypeError: obj.hasOwnProperty is not a function
```
* 이러한 위험요소를 없애기 위해 빌트인 메서드는 간접적으로 호출하여 사용하는것을 권장
```javascript
// 프로토타입 체인 종점 객체 생성
const obj = Object.create(null);
obj.a = 1;

// 빌트인 메서드 간접 호출
console.log(Object.prototype.hasOwnProperty.call(obj, 'a')); // ture
```

### 2. 객체 리터럴 내부에서 __ proto __에 의한 직접 상속
* ES6에서 객체 리터럴 내부에서 __ proto __ 접근자 프로퍼티를 사용하여 직접 상속 구현함

```javascript
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성 & 프로토타입 지정하여 직접 상속 받음
const obj = {
    y: 20,
    // 객체를 직접 상속 받음
    // obj -> myProto -> Object.prototype -> null
    __proto__: myProto
};
console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 정적 프로퍼티/메서드 ([예제](./src/static_prop_method.html))
* 정적(static) 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.
* 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근 불가능
```javascript
Person.staticMethod(); // staticMethod
me.staticMethod(); // TypeError: me.staticMethod is not a function
```
![static_prop_method](https://user-images.githubusercontent.com/63139527/173227964-41d5f15a-4d69-40c2-91eb-1bf393b05c1a.png)

```javascript
// Object.create는 정적 메서드
// Object 생성자 함수가 생성한 객체로 호출 불가능
const obj = Object.create({ name: 'Park' });
// Object.prototype.hasOwnProperty는 프로토타입 메서드
// Object.prototype의 메서드 이므로 모든 객체가 호출 가능
obj.hasOwnProperty('name');; // false
```

**NOTE**
```
프로토타입 프로퍼티/메서드를 표기할 때 prototype --> # 으로 표기
ex)
Object.prototype.isPrototypeOf --> Object#isPrototypeOf
```

## 프로퍼티 존재 확인

### 1. in 연산자
* `in 연산자`는 객체 내에 특정 프로퍼티가 **존재하는지 여부**를 확인한다.
```javascript
/**
 * key: 프로퍼티 키를 나타내는 문자열
 * object: 객체로 평가되는 표현식
 */
key in object
```
* `주의` : 객체가 **상속받은 모든 프로토타입 프로퍼티**도 확인함
```javascript
const person = {
    name: 'Park',
    address: 'Panggyo'
};

console.log('name' in person); // true
console.log('address' in person); // true
console.log('age' in person); // false
console.log('toString' in person); // true (프로토타입 체인 검색)
```
* ES6에서 도입된 `Reflect.has` 메서드는 in 연산자와 동일하게 동작함
```javascript
const person = { name: 'Park' };
console.log(Reflect.has(person, 'name')); // true
console.log(Reflect.has(person, 'toString')); // true (프로토타입 체인 검색)
```
### 2. Object.prototype.hasOwnProperty 메서드
* `Object.prototype.hasOwnProperty` 메서드 : 객체에 특정 프로퍼티가 존재하는지 확인
    * 객체 고유의 프로퍼티 키인 경우에만 true 반환
    * **상속받은 프로토타입의 프로퍼티 키인 경우 false 반환**
```javascript
console.log(person.hasOwnProperty('toString')); // false
```
## 프로퍼티 열거

### 1. for ... in 문
* 객체의 프로퍼티 개수만큼 순회
* 변수 선언문 : 선언한 변수에 프로퍼티 키를 할당
```javascript
for (변수선언문 in 객체) { ... }
```
* `for ... in 문`은 **객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [ [ Enumerable ] ]의 값이 true인 프로퍼티를 순회하며 열거한다.**
* 프로퍼티 키가 **심벌**인 프로퍼티는 열거하지 않는다.
```javascript
const person = {
    name: 'Park',
    address: 'Panggyo',
    __proto__: { age: 30 },
    [sym]: 10 // 심벌 프로퍼티
};
console.log('toString' in person); // true

for (const key in person) {
    console.log(key + ' : ' + person[key]);
}
// name : Park
// address : Panggyo
// age: 30

// toString 프로퍼티가 열거가 안되는 이유 (enumerable: false)
console.log(Object.getOwnPropertyDescriptor(Object.prototype, 'toString'));
// {value: f, writable: true, enumerable: false, configurable: true}

// 객체 고유 프로퍼티만 열거하려면 Object.prototype.hasOwnProperty 메서드를 이용한다.
for (const key2 in person) {
    // 객체 자신의 프로퍼티인지 확인(체크)
    if ( !person.hasOwnProperty(key2)) continue;
    console.log(key2 + ' : ' + person[key2]);
}
// name : Park
// address : Panggyo
```

* 배열도 객체이므로 프로퍼티와 상속받은 프로퍼티가 포함된다.
* 배열의 경우 for ... in문을 사용하지 말고 **for문, for ... of문, Array.prototype.forEach 메서드** 사용을 권장

```javascript
const arr = [1, 2, 3];
arr.x = 10; // 배열(=객체) 이므로 프로퍼티 가짐

for (const i in arr) {
    // 프로퍼티 x도 출력됨
    console.log(arr[i]); // 1 2 3 10
}

// arr.length는 3
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 1 2 3
}

// forEach 메서드는 요소가 아닌 프로퍼티는 제외함
arr.forEach(v => console.log(v)); // 1 2 3

// for ... of는 변수 선언문에서 선언한 변수에 키가 아닌 값을 할당한다.
for (const value of arr) {
    console.log(value); // 1 2 3
}
```

### 2. Object.keys/values/entries 메서드
* 객체 자신의 고유 프로퍼티만 열거하기 위해서 Object.keys/values/entries 메서드 사용 권장

* `Object.keys` 메서드 : **프로퍼티 키**를 **배열**로 반환
```javascript
const person = {
    name: 'Park',
    address: 'Panggyo',
    __proto__: { age: 20 }
};
console.log(Object.keys(person)); // ["name", "address"]
```

* (ES8) `Object.values` 메서드 : **프로퍼티 값**을 **배열**로 반환
```javascript
console.log(Object.values(person)); // ["Park", "Panggyo"]
```

* (ES8) `Object.entries` 메서드 : **프로퍼티 키와 값의 쌍의 배열**을 **배열**로 반환
```javascript
console.log(Object.entries(person)); 
// [["name", "Park"], ["address", "Panggyo"]]
Object.entries(person).forEach(([key, value]) => console.log(key, value));
/*
name Park
address Panggyou
*/
```