# 클래스

## 클래스는 프로토타입의 문법적 설탕인가?
* 자바스크립트는 프로토타입 기반 객체지향 언어
* ES5 : 클래스 없이, **생성자 함수**와 **프로토타입**을 통해 객체지향 언어의 상속을 구현
* ES6에서 도입된 `클래스(=함수)`는 클래스 기반 객체지향 프로그래밍 언어와 비슷하게 **새로운 객체 생성 메커니즘**을 제공
    * 클래스는 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕이다.

* `클래스`는 `생성자 함수`와 유사하게 동작하지만 다음과 같이 몇가지 차이가 있음

클래스|생성자 함수
-|-
new연산자 없이 호출하면 에러 발생|new연산자 없이 호출하면 일반 함수로 호출
상속을 지원하는 extends와 super 키워드 제공|extends와 super 키워드 미지원
호이스팅이 발생하지 않는 것처럼 동작|함수 선언문(함수 호이스팅), 함수 표현식(변수 호이스팅)
암묵적으로 strict mode가 지정되어 실행|암묵적으로 strict mode가 지정되지 않음
constructor, 프로토타입 메서드, 정적 메서드는 프로퍼티 어트리뷰트 [ [Enumerable] ] 값이 false다.|

## 클래스 정의
* `class` 키워드를 사용하여 정의
* 이름은 일반적으로는 `파스칼 케이스`를 사용 (생성자 함수와 동일)
* 클래스 == 함수 => 클래스는 값처럼 사용할 수 있는 `일급객체`
    * 특징
        * 무명의 리터럴로 생성가능, 런타임에 생성이 가능
        * 변수나 자료구조(객체, 배열)에 저장 가능
        * 함수의 매개변수에 전달 가능
        * 함수의 반환값으로 사용 가능

```javascript
// 클래스 선언문
class Person {} // 파스칼 케이스

// 표현식으로 클래스 정의
const Person = class {}; // 익명 클래스 표현식
const Person = class MyClass {}; // 기명 클래스 표현식
```

* 클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다.
    * 정의할 수 있는 메서드는 3가지
        * consturctor(생성자)
        * 프로토타입 메서드
        * 정적 메서드

```javascript
// 클래스 선언문
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name; // name 프로퍼티는 public
    }

    // 프로토타입 메서드
    sayHi() {
        console.log(`Hi! My Name is ${this.name}`);
    }

    // 정적 메서드
    static sayHello() {
        console.log('Hello!');
    }
}

// 인스턴스 생성
const me = new Person('Park');

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Park
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Park
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

![class_vs_consturctor_function drawio](https://user-images.githubusercontent.com/63139527/175766528-3a4b8162-6f05-4d5d-9fd5-d8998b5808f7.png)

## 클래스 호이스팅
* (1) 클래스는 함수로 평가된다.
* 클래스 선언문으로 정의한 클래스는 함수 선언문과 같이 소스코드 평가 과정, 런타임 이전에 먼저 평가되어 함수 객체를 생성한다.
* 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 consturcot 이다.
* 함수 객체를 생성하는 시점에 프토토타입도 생성된다.
* 프로토타입과 생성자 함수는 언제나 쌍(pair)로 존재한다.
* (2) 단, 클래스 정의 이전에 참조할 수 없다.
* (3) 클래스 선언문도 호이스팅이 발생한다.
    * let, const 키워드로 선언한 변수처럼 호이스팅됨
    * 일시적 사각지대(TDZ)에 빠지기 때문 -> 호이스팅이 발생하지 않는 것처럼 동작
    * 선언된 모든 식별자는 호이스팅됨 -> 모든 선언문은 런타임 이전에 먼저 실행되기 때문
```javascript
// (1) 클래스 선언문
class Person {}
console.log(typeof Person); // function

// (2)
console.log(Shop); // ReferenceError: Cannot access 'Shop' before initialization
// 클래스 선언문
class Shop {}

// (3)
const Address = '';
{
    // 호이스팅이 발생하지 않는다면 ''이 출력되야 함
    console.log(Address); // ReferenceError: Cannot access 'Address' before initialization
    // 클래스 선언문
    class Address {}
}
```

## 인스턴스 생성
* (1) 클래스는 생성자 함수이며 **new 연산자와 함께 호출되어 인스턴스를 생성**
* (2) 클래스는 인스턴스를 생성하는것이 유일한 존재 이유이므로 **반드시 new 연산자와 함께 호출**해야 한다.
* (3) 클래스 표현식으로 정의된 클래스의 경우, **식별자를 사용해 인스턴스를 생성**해야 한다.
```javascript
// (1)
class Person {}
// 인스턴스 생성
const me = new Person();
console.log(me); // Person {}

// (2) 클래스를 new 연산자 없이 호출하면 타입 에러 발생
const you = Person();
// TypeError: Class constructor Person cannot be invoked without 'new'

// (3)
const Address = class MyClass {}; // 식별자 : Address | 클래스 이름 : MyClass
// 함수 표현식과 마찬가지로 클래스를 가리키는 식별자로 인스턴스를 생성해야 한다.
const his = new Address();

// 클래스 이름 MyClass는 함수와 동일하게 클래스 몸체 내부에서만 유효한 식별자이다.
console.log(MyClass); // ReferenceError: MyClass is not defined
const her = new MyClass(); // ReferenceError: MyClass is not defined
```

## 메서드
* 클래스 몸체에는 0개 이상의 메서드만 선언할 수 있다.
* 정의할 수 있는 메서드는 3가지
    * constructor(생성자)
    * 프로토타입 메서드
    * 정적 메서드

### 1. constructor
* 인스턴스를 생성하고 초기화하기 위한 특수 메서드
    * **결론 : 인스턴스의 생성과 동시에 인스턴스 프로퍼티 추가를 통해 인스턴스의 초기화를 실행함**
* constructor는 메서드로 해석되는 것이 아니라 클래스 평가되어 생성한 함수 객체 코드의 일부가 된다.
* constructor는 클래스 내에 최대 1개만 존재 가능
    * 2개 이상일 경우 문법 에러 발생
* constructor는 생략 가능
    * 빈 constructor가 암묵적으로 정의 -> 빈 객체 생성
        * constructor() {}
* 프로퍼티가 추가되어 초기화된 인스턴스를 생성하려면 **constructor 내부에서 this에 인스턴스 프로퍼티를 추가**한다.
```javascript
constructor() {
    this.name = 'Park';
}
```
* 클래스 외부에서 프로퍼티 초기값을 전달하려면 **constructor에 매개변수 선언** 및 인스턴스를 생성할 때 초기값 전달
```javascript
constructor(name) {
    this.name = name;
}
```
* constructor 내부에서 **return 문을 반드시 생략**해야 한다.
    * 객체를 명시적으로 반환하면 this 반환이 무시되고 객체가 반환된다.
    * 원시값을 명시적으로 반환하면 원시값 반환은 무시되고 this 반환이 된다.
```javascript
constructor(name) {
    this.name = name; // 무시됨
    return {}; // 명시적으로 반환하는 빈 객체 반환
}
constructor(name) {
    this.name = name; // this 반환
    return 100; // 명시한 원시값 무시됨
}
```

### 2. 프로토타입 메서드 ([예제](./src/class_prototype_method.html))
* 클래스 몸체에서 정의한 메서드는 기본적으로 프로토타입 메서드가 된다.
```javascript
class Person {
    // 프로토타입 메서드
    sayHi() {
        console.log('Hi');
    }
}
```
* 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이다.
    * 인스턴스는 프로토타입 메서드를 상속받아 사용 가능
* 결론 : 클래스는 생성자 함수와 같이 인스턴스를 생성하는 생성자 함수
    * 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 메커니즘을 가진다.

### 3. 정적 메서드
* 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있는 메서드를 말한다.
* 클래스에서는 메서드에 `static` 키워드를 붙이면 정적 메서드가 된다.
* 정적 메서드는 인스턴스로 호출할 수 없다.
    * 인스턴스의 프로토타입 체인 상에는 클래스가 존재하지 않기 떄문에 인스턴스로 클래스의 메서드를 상속받을 수 없다.
```javascript
class Person {
    // 생성자
    constructor(name) {
        // 인스턴스 생성 및 초기화
        this.name = name;
    }

    // 정적 메서드
    static sayHi() {
        console.log('Hi');
    }
}
// 정적 메서드는 클래스로 호출한다.
// 정적 메서드는 인스턴스 없이도 호출할 수 있다.
Person.sayHi(); // Hi

// 인스턴스 생성
const me = new Person('Park');
me.sayHi(); // TypeError: me.sayHi is not a function
```

### 4. 정적 메서드와 프로토타입 메서드 차이 ([예제](./src/static_method_vs_prototype_method_in_class.html))
* 차이점
    1. 정적 메서드와 프로토타입 메서드는 자신이 속해 있는 프로토타입 체인이 다르다.
    2. 정적 메서드는 클래스로 호출하고 프로토타입 메서드는 인스턴스로 호출한다.
    3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 인스턴스 프로퍼티를 참조할 수 있다.

* 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다.
    * 프로토타입 메서드 : this는 인스턴스 객체를 가리킴
    * 정적 메서드 : this는 클래스를 가리킴
    * 프로토타입 메서드와 정적 메서드 내부의 this 바인딩이 다르다.
    -> this를 사용하지 않는 메서드는 정적 메서드로 정의하는 것이 좋다.
    -> 메서드 내부에서 인스턴스 프로퍼티를 참조할 필요가 있다면 this를 사용해야 하며, 이러한 경우 프로토타입 메서드로 정의해야한다.

* 표준 빌트인 객체엔 다양한 정적 메서드를 가지고 있다.
```javascript
// 표준 빌트인 객체의 정적 메서드
Math.max(1,2,3); // 3
Number.isNaN(NaN); // true
JSON.stringify({ a: 1 }); // "{"a":1}"
Object.is({}, {}); // false
Reflect.has({ a: 1 }, 'a'); // true
```

* 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의하지 않고 메서드로 구조화할 때 유용하다.


### 5. 클래스에서 정의한 메서드의 특징
1. **function 키워드를 생략한 메서드 축약 표현**을 사용
2. 객체 리터럴과는 다르게 **클래스에 메서드를 정의할 때는 콤마가 필요없다.**
3. **암묵적으로 strict mode로 실행**됨
4. for ... in문이나 Object.keys 메서드 등으로 **열거할 수 없다.**  
프로퍼티 어트리뷰트 **[ [ Enumerable ] ] 의 값이 false** 이다.
5. 내부 메서드 [ [ Constructor ] ]를 갖지 않는 **non-constructor** 이다.  
new 연산자와 함께 호출할 수 없다.

## 클래스의 인스턴스 생성 과정
* new 연산자와 함께 클래스를 호출하면 생성자 함수와 마찬가지로 클래스의 내부 메서드 [ [ Construct ] ]가 호출된다.
    * 즉, 클래스는 new 연산자 없이 호출할 수 없다.

* 다음과 같은 과정을 거쳐 인스턴스가 생성된다.
    1. 인스턴스 생성과 this 바인딩
    2. 인스턴스 초기화
    3. 인스턴스 반환

```javascript
class Person {
    // 생성자
    constructor(name) {
        // 1. 암묵적으로 인스턴스가 생성되고 this에 바인딩된다.
        console.log(this); // Person {}
        console.log(Object.getPrototypeOf(this) === Person.prototype); // true

        // 2. this에 바인딩되어 있는 인스턴스를 초기화한다.
        // 즉, 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로
        // 인스턴스의 프로퍼티 값을 초기화 한다.
        // 만약, constructor가 생략되었다면 이 과정도 생략된다.
        this.name = name;

        // 3. 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    }
}
// new 연산자와 함께 클래스 호출
const me = new Person('Park');
```

## 프로퍼티
### 1. 인스턴스 프로퍼티
* **constructor 내부에서 this에 인스턴스 프로퍼티를 추가**한다.
* constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.
* private, public, protected 키워드와 같은 접근 제한자를 지원하지 않는다.
    * **인스턴스 프로퍼티는 언제나 public 하다.**
    * private한 프로퍼티를 정의할 수 있는 사양이 제안중에 있다.
        * "4. private 필드 정의 제안" 참조

```javascript
class Person {
    constructor(name) {
        // 인스턴스 프로퍼티
        this.name = name; // public
    }
}
const me = new Person('Park');
console.log(me.name); // Park
```

### 2. 접근자 프로퍼티 ([예제](./src/accessor_property_in_class.html))
* 접근자 프로퍼티
    * 자체적으로 값( [ [ Value ] ] )을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수(getter, setter 함수)로 구성된 프로퍼티
    * 인스턴스 프로퍼티처럼 사용됨 = 바로 호출하는 것이 아닌 참조를 통해 내부 호출 발생
    * getter : 인스턴스 프로퍼티에 접근할 때마다 프로퍼티 값 조작 또는 별도 행위
        * 메서드 이름 앞에 get 키워드 사용
    * setter : 인스턴스 프로퍼티에 값을 할당할 때마다 프로퍼티 값 조각 또는 별도 행위
        * 메서드 이름 앞에 set 키워드 사용
* 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.

### 3. 클래스 필드 정의 제안
* 클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어
* 자바스크립트에서...
    * 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 반드시 **constructor 내부에서 this에 프로퍼티를 추가**해야함
    * 클래스에서 인스턴스 프로퍼티를 참조하려면 반드시 **this를 사용하여 참조**해야함.
    * 클래스 몸체에는 **메서드만 선언 가능**, **클래스 필드 선언하면 문법 에러 발생**

* 하지만!! **새로운 표준 사양 "클래스 필드 정의"가 제안되면서 클래스 필드 정의가 가능**해졌다.
```javascript
class Person {
    // 클래스 필드 정의
    name = 'Lee';
}
const me = new Person();
console.log(me); // Person { name: 'Lee' }
```
* 클래스 필드를 정의하는 경우 this에 클래스 필드를 바인딩해서는 안된다.
    * this는 클래스의 constructor와 메서드 내에서만 유효하다.
```javascript
class Person {
    // this에 클래스 필드를 바인딩해서는 안됨
    this.name = ''; // SyntaxError: Unexpected token '.'
}
```
* 클래스 필드를 참조하는 경우, this를 반드시 사용
```javascript
class Person {
    // 클래스 필드
    name = 'Lee';

    constructor() {
        console.log(name); // ReferenceError: name is not defined
    }
}
new Person();
```
* 클래스 필드에 초기값을 할당하지 않으면 undefined를 갖는다.
```javascript
class Person {
    // 클래스 필드를 초기화하지 않으면 undefined를 갖는다.
    name;
}
const me = new Person();
console.log(me); // Person { name: undefined }
```
* 외부의 초기값으로 클래스 필드를 초기화해야할 필요가 있다면 constructor에서 클래스 필드를 초기화 해야한다.
```javascript
class Person {
    name;

    constructor(name) {
        // 클래스 필드 초기화
        this.name = name;
    }
}
const me = new Person('Lee');
console.log(me); // Person {name: "Lee"}
```
* 어차피 constructor 내부에서 클래스 필드를 참조하여 초기값을 할당해야 한다.
    * 즉, 클래스가 생성한 인스턴스에 클래스 필드에 해당하는 프로퍼티가 없다면 자동 추가되기 때문
```javascript
class Person {
    constructor(name) {
        this.name = name;
    }
}
const me = new Person('Lee');
console.log(me); // Person {name: "Lee"}
```
* 함수는 일급 객체이므로 함수를 클래스 필드에 할당할 수 있다.
    * 클래스 필드를 통해 메서드를 정의할 수 있다.
    * 클래스 필드에 함수를 할당하는 경우, 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.
    * 클래스 필드에 함수를 할당하는 것은 권장하지 않는다.
```javascript
class Person {
    // 클래스 필드에 문자열을 할당
    name = 'Park';

    // 클래스 필드에 함수 할당
    getName = function () {
        return this.name;
    }
    // 화살표 함수로 정의할 수도 있다.
    // getName = () => this.name;
}
const me = new Person();
console.log(me); // Person {name: "Park", getName: function}
console.log(me.getName()); // Park
```

### 4. private 필드 정의 제안
* 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public 함
```javascript
class Person {
    name = 'Park'; // 클래스 필드도 기본적으로 public
}
const me = new Person();
console.log(me.name); // Park
```

* private 필드를 정의할 수 있는 새로운 표준 사양이 제안됨
    * private 필드의 선두에 `#`을 붙여준다.
    * private 필드를 참조할 때도 `#`을 붙여주어야 한다.
* public 필드는 어디서든 참조 가능하지만 private 필드는 클래스 내부에서만 참조할 수 있다.

접근 가능성|public|private
-|:-:|:-:
클래스 내부|O|O
자식 클래스 내부|O|X
클래스 인스턴스를 통한 접근|O|X

```javascript
class Person { 
    // private 필드 정의
    #name = '';

    constructor(name) {
        // private 필드 참조
        this.#name = name;
    }
}
const me = new Person('Park');
// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

* 클래스 외부에서 private 필드에 **직접 접근**할 수 있는 방법은 없다.
* **접근자 프로퍼티**를 통해 **간접적으로 접근하는 방법은 유효**하다.
```javascript
class Person {
    // private 필드 정의
    #name = '';

    constructor(name) {
        this.#name = name;
    }

    // name은 접근자 프로퍼티
    get name() {
        // private 필드를 참조하여 trim한 다음 반환
        return this.#name.trim();
    }
}
const me = new Person(' Park ');
console.log(me.name); // Park
```

* private 필드는 반드시 클래스 몸체에 정의해야 한다.
    * private 필드를 constructor에 정의하면 에러가 발생한다.
```javascript
class Person {
    constructor(name) {
        // private 필드는 클래스 몸체에서 정의해야 한다.
        this.#name = name;
        // SyntaxError: Private field '#name' must be declared in an enclosing class
    }
}
```

### 5. static 필드 정의 제안
* static 키워드를 사용하여 정적 필드를 정의할 수 없었다.
* 새로운 표준 사양 "Static class features" 가 제안되면서 정의 가능해짐
    * static public 필드
    * static private 필드
    * static private 메서드

```javascript
class MyMath {
    // static public 필드 정의
    static PI = 22 / 7;

    // static private 필드 정의
    static #num = 10;

    // static private 메서드
    static #okkk(a, b) {
        // something
    }

    // static 메서드
    static increment() {
        return ++MyMath.#num;
    }
}
console.log(MyMath.PI); // 3.14...
console.log(MyMath.increment());
```

## 상속에 의한 클래스 확장

### 1. 클래스 상속과 생성자 함수 상속

### 2. extends 키워드

### 3. 동적 상속

### 4. 서브클래스의 constructor

### 5. super 키워드

### 6. 상속 클래스의 인스턴스 생성 과정

### 7. 표준 빌트인 생성자 함수 확장