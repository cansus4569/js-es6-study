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
### 1. 클래스 상속과 생성자 함수 상속 ([예제](./src/class_extends.html))
* 프로토타입 기반 상속과는 다른 개념
* 프로토타입 기반 상속  
: 프로토타입 체인을 통해 다른 객체의 자산을 상속 받음
* 상속에 의한 클래스 확장  
: **기존 클래스를 상속받아 새로운 클래스를 확장하여 정의**
* 클래스와 생성자 함수는 인스턴스를 생성할 수 있는 함수라는 점에서 유사
    * 차이점 : 클래스는 상속을 통해 기존 클래스를 확장할 수 있는 문법이 제공
        * `extends` 키워드 사용
    * 코드 재사용 관점에서 매우 유용

![class_extends drawio](https://user-images.githubusercontent.com/63139527/176683766-c04fa8c4-3413-48eb-af99-c565c16f9665.png)

### 2. extends 키워드
* 상속을 통해 클래스를 확장하려면 `extends` 키워드를 사용하여 상속받을 클래스를 정의
* 상속을 통해 확장된 클래스를 명명
    * 서브 클래스 or 파생 클래스 or 자식 클래스
* 상속할 때 상위 클래스를 명명
    * 수퍼 클래스 or 베이스 클래스 or 부모 클래스

```javascript
// 수퍼(베이스/부모) 클래스
class Base {}
// 서브(파생/자식) 클래스
class Derived extends Base {}
```

![extends_keyword drawio](https://user-images.githubusercontent.com/63139527/176688430-19ff5bfb-3574-438a-8d3d-2041dd486ab3.png)

* 클래스도 프로토타입을 통해 상속 관계를 구현함
* 수퍼 클래스와 서브 클래스는 인스턴스의 프로토타입 체인뿐 아니라 클래스 간의 프로토타입 체인도 생성한다.
    * 프로토타입 메서드, 정적 메서드 모두 상속 가능

### 3. 동적 상속
* extends 키워드 앞에는 반드시 클래스가 와야한다.
* extends 키워드는 생성자 함수를 상속받아 클래스를 확장할 수도 있음
```javascript
// 생성자 함수
function Base (a) {
    this.a = a;
}
// 생성자 함수를 상속 받는 서브 클래스
class Derived extends Base {}

const derived = new Derived(1);
console.log(derived); // Derived { a: 1 }
```

* extends 키워드 다음에는 [ [ Construct ] ] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.
    * 동적으로 상속받을 대상을 결정할 수 있다.

```javascript
function Base1() {}

class Base2 {}

let condition = true;

class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

### 4. 서브클래스의 constructor
* 서브 클래스에도 `constructor` 를 생략하면 암묵적으로 정의된다.
```javascript
constructor(...args) { super(...args); }
```
* args는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트
* super()는 수퍼 클래스의 constructor(super-constructor)를 호출하여 인스턴스를 생성

**NOTE : Rest 파라미터**
```
매개변수에 ... 를 붙이면 Rest 파라미터가 된다.
Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
```

* 예제
```javascript
// 수퍼 클래스
class Base {
    // constructor 생략시 암묵적으로 정의됨
    // constructor() {}
}
// 서브 클래스
class Derived extends Base {
    // constructor 생략시 암묵적으로 정의됨
    // constructor(...args) { super(...args); }
}
const derived = new Derived();
console.log(derived); // Derived {}
```
* 프로퍼티를 소유하는 인스턴스를 생성하려면 constructor 내부에서 인스턴스에 프로퍼티를 추가해야 한다.

### 5. super 키워드
#### 5.1 super 호출
* 수퍼 클래스의 constructor(super-constructor)를 호출 

* **수퍼 클래스의 constructor 내부에서 추가한 프로퍼티를 그대로 갖는 인스턴스를 생성한다면**, **서브 클래스의 constructor 생략 가능**
    * 서브 클래스를 호출하면서 전달한 인수는 암묵적 정의된 constructor의 super 호출을 통해 수퍼 클래스의 constructor에 전달됨
    * [예제](./src/super_call_1.html)
* **수퍼 클래스의 constructor 내부에서 추가한 프로퍼티와 서브 클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면**, **서브 클래스의 constructor 생략 불가능**
    * 서브 클래스를 호출하면선 전달한 인수 중에서 수퍼 클래스의 constructor에 전달할 필요가 있는 인수는 서브 클래스의 constructor에서 호출하는 super를 통해 전달해야 함
    * [예제](./src/super_call_2.html)
* super 호출 시 주의 사항
    1. 서브 클래스에서 constructor를 생략하지 않는 경우, 서브 클래스의 constructor에서는 반드시 super를 호출해야 한다.
    2. 서브 클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없다.
    3. super는 반드시 서브 클래스의 constructor에서만 호출한다.

#### 5.2 supoer 참조
* 수퍼 클래스의 메서드를 호출 가능

1. 서브 클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼 클래스의 프로토타입 메서드 sayHi를 가리킨다. ([예제](./src/super_refer_1.html))
    * Base.prototype.sayHi를 호출할 때 call 메서드를 사용해 this를 전달해야 한다.
        * [예제](./src/super_refer_2.html)
    * 메서드 내부 슬롯 [ [ HomeObject ] ] 를 가지며, 자신을 바인딩하고 있는 객체를 가리킨다.
    ```javascript
    // 의사 코드 표현
    super = Object.getPrototypeOf([[HomeObject]])
    ```
    * 주의 : ES6 메서드 축약 표현으로 정의된 함수만이 [ [ HomeObject ] ]를 갖는다.
        * 즉, [ [ HomeObject ] ]를 가지는 함수만이 super를 참조할 수 있다.
    * super 참조는 수퍼 클래스의 메서드를 참조하기 위해 사용하므로 서브 클래스의 메서드에서 사용해야 한다.
    * 객체 리터럴에서도 super를 참조를 사용할 수 있다.
        * [예제](./src/super_refer_3.html)

2. 서브 클래스의 정적 메서드 내에서 super.sayHi는 수퍼 클래스의 정적 메서드 sayHi를 가리킨다. ([예제](./src/super_refer_4.html))

### 6. 상속 클래스의 인스턴스 생성 과정
#### 6.1 서브클래스의 super 호출
* 자바스크립트 엔진은 클래스를 평가할 때 수퍼클래스와 서브클래스를 구분하기 위해 "base" 또는 "derived"를 값으로 갖는 내부 슬롯 [ [ ConstructorKind ] ]를 갖는다.
    * 상속 받지 않는 클래스(그리고 생성자 함수) : [ [ ConstructorKind ] ] = base
    * 다른 클래스를 상속 받는 서브클래스 : [ [ ConstructorKind ] ] = derived
    * new 연산자와 함께 호출되었을 때 동작 구분된다.

* 다른 클래스를 상속받지 않는 클래스(그리고 생성자 함수)는 new 연산자와 함께 호출되었을 때 암묵적으로 빈 객체, 즉 인스턴스를 생성하고 이를 this에 바인딩한다.
* **서브클래스는 자신이 직접 인스턴스를 생성하지 않고 수퍼클래스에게 인스턴스 생성을 위임한다.**
    * **서브클래스의 constructor에서 반드시 super를 호출해야하는 이유**

* 서브클래스 constructor 내부에 super 호출이 없으면 에러가 발생
    * 실제로 인스턴스를 생성하는 주체는 수퍼클래스이기 때문

#### 6.2 수퍼클래스의 인스턴스 생성과 this 바인딩
* 수퍼클래스의 constructor 내부의 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성함
* 빈 객체가 바로(아직 완성되지는 않았지만) 클래스가 생성한 인스턴스이다.
* 암묵적으로 생성된 빈 객체, 즉 인스턴스는 this에 바인딩된다.
* 수퍼클래스의 constructor 내부의 this는 생성된 인스턴스를 가리킨다.

```javascript
// 수퍼클래스
class Rectangle {
    constructor(width, height) {
        // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
        console.log(this); // ColorRectangle {}
        // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle 이다.
        console.log(new.target); // ColorRectangle

        // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
        console.log(Obejct.getPrototypeOf(this) === Color.Rectangle.prototype); // true
        console.log(this instanceof ColorRectangle); // true
        console.log(this instanceof Rectangle); // true
    }
}
```
* 이때 인스턴스는 수퍼클래스가 생성한 것이다.
* new 연산자와 함께 호출된 함수를 가리키는 new.target은 서브클래스를 가리킨다.
* **인스턴스는 new.target이 가리키는 서브클래스가 생성한 것으로 처리된다.**

#### 6.3 수퍼 클래스의 인스턴스 초기화
* 수퍼클래스의 constructor가 실행되어 this에 바인딩되어 있는 인스턴스를 초기화한다.
* this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.
```javascript
// 수퍼클래스
class Rectangle {
    constructor(width, height) {
        // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
        console.log(this); // ColorRectangle {}
        // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle 이다.
        console.log(new.target); // ColorRectangle

        // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
        console.log(Obejct.getPrototypeOf(this) === Color.Rectangle.prototype); // true
        console.log(this instanceof ColorRectangle); // true
        console.log(this instanceof Rectangle); // true

        // 인스턴스 초기화
        this.width = width;
        this.height = height;

        console.log(this); // ColorRectangle { width: 2, height: 4 }
    }
}
```

#### 6.4 서브클래스의 constructor로의 복귀와 this 바인딩
* super의 호출이 종료되고 제어 흐름이 서브클래스 constructor로 돌아온다.
* **super가 반환한 인스턴스가 this에 바인딩된다.**
* **서브클래스는 별도의 인스턴스를 생성하지 않고 super가 반환한 인스턴스를 this에 바인딩하여 그대로 사용한다.**
```javascript
class ColorRectangle extends Rectangle {
    constructor(width, height, color) {
        super(width, height);

        // super가 반환한 인스턴스가 this에 바인딩된다.
        console.log(this); // ColorRectangle {width: 2, heigth: 4}
    }
}
```
* **super가 호출되지 않으면 인스턴스가 생성되지 않으며, this 바인딩도 할 수 없다.**
* **서브클래스의 constructor에서 super를 호출하기 전에는 this를 참조할 수 없는 이유가 이러한 것 때문**
* `결론 : 서브클래스 constructor 내부의 인스턴스 초기화는 반드시 super 호출 이후에 처리되어야 한다.`

#### 6.5 서브 클래스의 인스턴스 초기화
* super 호출 이후, 서브클래스의 constructor에 기술되어 있는 인스턴스 초기화가 실행된다.
* 즉, this에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 constructor가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.

#### 6.6 인스턴스 반환
* 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
```javascript
class ColorRectangle extends Rectangle {
    constructor(width, height, color) {
        super(width, height);

        // super가 반환한 인스턴스가 this에 바인딩된다.
        console.log(this); // ColorRectangle {width: 2, heigth: 4}

        // 인스턴스 초기화
        this.color = color;

        // 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
        console.log(this); // ColorRectangle { width: 2, heigth: 4, color: "red" }
    }
}
```

### 7. 표준 빌트인 생성자 함수 확장
* extends 키워드 다음에는 클래스뿐만 아니라 [ [ Construct ] ] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다.
* 표준 빌트인 객체(String, Number, Array)도 [ [ Construct ] ] 내부 메서드를 갖는 생성자 함수이므로 extends 키워드를 사용하여 확장할 수 있다.

```javascript
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
    // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
    uniq() {
        return this.filter((v, i, self) => self.indexOf(v) === i);
    }

    // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
    average() {
        return this.reduce((pre, cur) => pre + cur, 0) / this.length;
    }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
// MyArray.prototype.average 호출
console.log(myArray.average()); // 1,75
```

* Array 생성자 함수를 상속받아 확장한 MyArray 클래스가 생성한 인스턴스는 Array.prototype과 MyArray.prototype의 모든 메서드를 사용할 수 있다.

* 주의할 것
    * Array.prototype의 메서드 중에서 map, filter와 같이 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환한다는 것
    ```javascript
    console.log(myArray.filter(v => v % 2) instanceof MyArray); // true
    ```

* 만약 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환하지 않고 Array의 인스턴스를 반환하면 MyArray 클래스의 메서드와 메서드 체이닝이 불가능하다.
```javascript
// 메서드 체이닝
// [1, 1, 2, 3] => [1, 1, 3] => [1, 3] => 2
console.log(myArray.filter(v => v % 2).uniq().average()); // 2
```
* myArray.filter 반환 인스턴스 = MyArray 클래스 생성한 인스턴스
* uniq 메서드가 반환하는 인스턴스도 MyArray 타입
* uniq 메서드가 반환하는 인스턴스로 average 메서드를 연이어 호출(메서드 체이닝)할 수 있다.

* 만약 **uniq 메서드**가 **Array가 생성한 인스턴스를 반환**하게 하려면 `Symbol.species`를 사용하여 **정적 접근자 프로퍼티를 추가**한다.

```javascript
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
    // 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
    static get [Symbol.species]() { return Array; }

    // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
    uniq() {
        return this.filter((v, i, self) => self.indexOf(v) === i);
    }

    // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
    average() {
        return this.reduce((pre, cur) => pre + cur, 0) / this.length;
    }
}
const myArray = new MyArray(1, 1, 2, 3);

console.log(myArray.uniq() instanceof MyArray); // false
console.log(myArray.uniq() instanceof Array); // true

// 메서드 체이닝
// uniq 메서드는 Array 인스턴스를 반환하므로 average 메서드를 호출할 수 없다.
console.log(myArray.uniq().average());
// TypeError: myArray.uniq(...).average is not a function
```