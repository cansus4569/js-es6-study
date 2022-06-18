# this

## this 키워드
* **자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수**
* this를 통해 **자신이 속한 객체** 또는 **자신이 생성할 인스턴스**의 프로퍼티나 메서드를 참조할 수 있음
* 자바스크립트 엔진에 의해 암묵적 생성
* 코드 어디서든 참조 가능
* **this가 가리키는 값. this 바인딩은 함수 호출 방식에 의해 동적으로 결정됨**

**`this 바인딩`**
```
바인딩이랑 식별자와 값을 연결하는 과정을 의미
ex) 변수 선언 : 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것
this 바인딩 : this(키워드로 분류되지만 식별자 역할을 함)와 this가 가리킬 객체를 바인딩하는 것
```

### 1. 객체 리터럴 CASE
* 객체는 복합적 자료구조
    * 상태 : 프로퍼티
    * 동작 : 메서드
        * 메서드는 자신이 속한 객체를 가리키는 식별자를 참조 및 변경할 수 있어야 함
* 객체 리터럴은 circle 변수에 할당되기 직전에 평가 -> 평가 완료 -> 객체 생성 -> circle 식별자에 할당
#### 1.1 재귀적 참조
* `circle.radius` **재귀적 참조하는 방식은 일반적이지 않으며 바람직하지 않음**
```javascript
const circle = {
    // 프로퍼티 : 객체 고유의 상태 데이터
    radius : 5,
    // 메서드 : 상태 데이터를 참조하고 변경하는 동작
    getDiameter() {
        // 자신이 속한 객체인 circle을 참조할 수 있어야 함
        return 2 * circle.radius; // 재귀적으로 참조
    }
};
console.log(circle.getDiameter()); // 10
```
#### 1.2 this를 이용한 자기 참조(this 바인딩)
* 객체 리터럴의 메서드 내부에서의 this는 메서드를 호출한 객체, 즉 circle 가리킨다.
```javascript
const circle = {
    radius : 5,
    getDiameter() {
        return 2 * this.radius; // this 바인딩
    }
};
console.log(circle.getDiameter()); // 10
```
### 2. 생성자 함수 CASE
#### 2.1 재귀적 참조 불가
```javascript
function Circle(radius) {
    // 이 시점에서 생성자 함수 자신이 생헝할 인스턴를 가리키는 식별자를 알 수 없음
    // My생각 : ??? 자리에 넣을 재귀적 참조를 할 식별자가 없음
    ???.radius = radius;
}
Circle.prototype.getDiameter = function () {
    // 위와 동일. My생각 : ??? 자리에 넣을 재귀적 참조를 할 식별자가 없음
    return 2 * ???.radius;
}
// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 함
// My생각 : 이 시점에서 인스턴스(즉, 식별자)가 생성됐기에, 위에서 정의한 생성자 함수에서 ??? 자리를 채울수가 없음 (재귀적 참조 불가))
const circle = new Circle(5);
```
#### 2.2 this를 이용한 자기 참조(this 바인딩)
* 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
```javascript
function Circle(radius) {
    this.radius = radius; // this 바인딩
}
Circle.prototype.getDiameter = function () {
    return 2 * this.radius; // this 바인딩
}
// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```
### 3. 함수가 호출되는 방식에 따른 this 바인딩

```javascript
// this는 어디서든지 참조 가능
// (1) 전역에서 this는 전역 객체 window를 가리킴
console.log(this); // window

function square(number) {
    // (2-1) 일반 함수 내부에서 this는 전역 객체 window를 가리킴
    // (2-2) 만약 strict mode가 적용되어 있다면 undfined가 바인딩된다.
    console.log(this); // window
    return number * number;
}

const person = {
    nmae: 'Park',
    getName() {
        // (3) 메서드 내부에서 this는 메서드를 호출한 객체를 가리킴
        console.log(this); // {name: "Park", getName: f}
        return this.name;
    }
};
console.log(person.getName()); // Park

function Person(name) {
    this.name = name;
    // (4) 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    console.log(this); // Person {name: "Park"}
}
const me = new Person('Park');
```
* this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수  
-> 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.

## 함수 호출 방식과 this 바인딩
* **this 바인딩**(this에 바인딩될 값)은 **함수가 호출되는 방식에** 따라 **동적으로 결정**됨

함수 호출 방식|this 바인딩
-|-
일반 함수 호출|전역 객체
메서드 호출|메서드를 호출한 객체
생성자 함수 호출|생성자 함수가 (미래에) 생성할 인스턴스
Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체

**`렉시컬 스코프와 this 바인딩은 결정 시기가 다르다.`**
```
렉시컬 스코프 : 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정
this 바인딩 : 함수 호출 시점에 결정
```

### 1. 일반 함수 호출 ([예제](./src/function_call_this.html))
* 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.
* 단, strict mode 경우 this는 undefined가 바인딩된다.

* < 문제 발생 >  
중첩 함수와 콜백 함수는 외부 함수를 돕는 헬퍼 함수의 역할인데 this가 전역 객체를 바라보게 되면 헬퍼 함수로 동작하기 어렵게 만듬

* 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법
```javascript
// 중첩 함수와 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키는 방법
var global_val = 1;
const obj2 = {
    global_val: 100,
    foo() {
        /********** 1번째 방법 **********/
        // this 바인딩(obj2)을 변수 that에 할당
        const that = this;

        // 콜백 함수 내부에서 this 대신 that 참조
        setTimeout(function () {
            console.log("that.global_val : ",that.global_val); // 100
        },100);
        /********** 2번째 방법 **********/
        // 콜백 함수에 명시적으로 this를 바인딩
        // Function.prototype.bind 메서드 사용
        setTimeout(function () {
            console.log("this.global_val : ",this.global_val); // 100
        }.bind(this),100);
        /********** 3번째 방법 **********/
        // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
        setTimeout(() => {
            console.log("this.global_val : ",this.global_val); // 100
        },100);
    }
};
obj2.foo();
```

### 2. 메서드 호출 ([예제](./src/method_call_this.html))
* 메서드 내부의 this에는 메서드를 호출한 객체가 바인딩
    * 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩
* 주의 : 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩됨
    * 메서드는 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체
    * 그래서 
        * 다른 객체의 프로퍼티에 할당이 가능(다른 객체의 메서드가 될수 있음)
        * 일반 변수에 할당하여 일반 함수로 호출될 수 있음(this바인딩 : 전역 객체)


```javascript
const person = {
    name: 'Park',
    // 메서드는 프로퍼티에 바인딩된 함수
    getName() { // getName 프로퍼티
        return this.name; // 함수 객체
    }
    // getName 프로퍼티가 함수 객체를 가리키고 있을 뿐이다.
};
```
![method_this_binding drawio](https://user-images.githubusercontent.com/63139527/174428238-9cc41c4c-a733-4e59-bfba-98cf398ba9ba.png)

### 3. 생성자 함수 호출 ([예제](./src/constructor_function_call_this.html))
* 생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

### 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
* apply, call, bind 메서드는 Function.prototype 메서드이다.
* 모든 함수가 상속 받아 사용할 수 있다.

![apply_bind_call drawio](https://user-images.githubusercontent.com/63139527/174430644-1372bfd2-b82a-4a9e-90d7-61f710f04a05.png)

```javascript
/**
 * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
 * @returns 호출된 함수의 반환값
 */
Function.prototype.apply(thisArg[, argsArray]);
/**
 * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출한다.
 * @param thisArg - this로 사용할 객체
 * @param arg1, arg2, ... - 함수에게 전달할 인수 리스트
 * @returns 호출된 함수의 반환값
 */
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]]);
```

#### 4.1 apply와 call 메서드 ([예제](./src/apply_call_method_this.html))
* 본질적인 기능은 **함수를 호출**하는 것
* 공통점
    * 호출할 함수에 인수를 전달하는 방식만 다르고 **this로 사용할 객체를 전달하면서 함수를 호출하는 것**은 동일
* 차이점
    * apply : 인수를 **배열**로 묶어 전달
    * call : 인수를 **쉼표로 구분한 리스트 형식**으로 전달
* 대표적 용도 : arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우
    * arguments 객체는 배열이 아니기에 Array.prototype.slice 같은 배열의 메서드 사용 불가  
    => apply와 call 메서드를 이용하면 가능함

#### 4.2 bind 메서드 ([예제](./src/bind_method_this.html))
* 함수를 호출하지 않고 this로 사용할 객체만 전달함
* 메서드의 this와 메서드 내부의 중첩 함수(또는 콜백 함수)의 this가 불일치하는 문제를 해결하기 위해 유용하게 사용됨

```javascript
function getThisBinding() {
    return this;
}
// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 함수에 this로 사용할 객체를 전달
// bind 메서드는 함수를 호출하지 않는다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```
