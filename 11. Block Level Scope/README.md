# let, const 키워드와 블록 레벨 스코프

## var 키워드로 선언한 변수의 문제점
* ES5 까지 변수 선언은 오직 **var** 키워드를 통해서만 가능
* 아래의 1,2,3번은 var 키워드로 선언된 변수의 특징을 소개한다.

### 1. 변수 중복 선언 허용
* (1) 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작
    * 즉, 재할당이 이루어짐
* (2) 초기화문이 없는 변수 선언문은 무시된다.
```javascript
var x = 1;
var y = 2;

// (1) 중복선언된 초기화 문이 있는 변수 선언문 (재할당)
var x = 10; // x = 10;
// (2) 중복선언된 초기화 문이 없는 변수 선언문 (무시)
var y;

console.log(x); // 10
console.log(y); // 2
```

### 2. 함수 레벨 스코프
* 함수의 코드 블록만을 지역 스코프로 인정한다.
```javascript
var x = 1;
if(ture) { // 함수가 아닌 조건문의 코드 블록
    var x = 10; // 전역 변수로 취급됨
}
console.log(x); // 10

var i = 10;
// for문 안의 변수 선언문 또한 전역 변수로 취급된다.
for(var i = 0; i < 5 ; i++) { // 함수가 아닌 반복문의 코드 블록
    console.log(i); // 0 1 2 3 4 5
}
console.log(i); // 5
```

### 3. 변수 호이스팅
```javascript
// 이 시점에는 변수 호이스팅에 의해 이미 x 변수가 선언되었다. (1. 선언 단계)
// 변수 x는 undefined로 초기화 된다. (2. 초기화 단계)
console.log(x); // undefined

// 변수 x에 값 할당 (3. 할당 단계)
x = 1;

console.log(x); // 1

// 변수 선언은 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 실행된다.
var x;
```

## let 키워드
* ES6 부터 등장한 변수 선언 키워드
* var 키워드의 단점을 보완
### 1. 변수 중복 선언 금지
* var 키워드와 다르게 중복 선언시 에러가 발생한다.
```javascript
var x = 1;
// var 키워드 중복 선언 허용(O)
var x = 2; // 에러 없이 x = 2 로 재할당됨

let y = 3;
// let 키워드는 같은 스코프 내에 중복 선언 허용(X)
let y = 4; // error 발생 : Identifier 'y' has already been declared
```
### 2. 블록 레벨 스코프
* 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정
```javascript
let i = 10;     // 전역 스코프 및 전연 변수 선언

function foo() {            // 함수 레벨 스코프 시작
    let i = 100;            // 함수 안에 지역 변수 선언
    for(let i = 1; i < 3; i++) {    // 블록 레벨 스코프 시작
        console.log(i); // 1 2
    }                               // 블록 레벨 스코프 끝
    console.log(i);         // 100
}                           // 함수 레벨 스코프 끝
foo();
console.log(i); // 10
```
### 3. 변수 호이스팅
* let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것 처럼 동작한다.
```javascript
console.log(x); // error : x is not defined
let x;
```
* let 키워드로 선언한 변수는 "선언 단계"와 "초기화 단계"가 분리되어 진행한다.
    * `선언 단계` 런타임 이전 자바스크립트 엔진에 의해 암묵적으로 실행됨
    * `초기화 단계` 변수 선언문에 도달했을 때 실행됨
* 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생
* 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없다.
    * 이러한 구간을 **일시적 사각지대(Temporal Dead Zone; TDZ)** 라고 부른다.
```javascript
// 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
console.log(x); // error : x is not defined

let x; // 변수 선언 및 초기화 단계
console.log(x); // undefined

x = 1; // 할당
console.log(x); // 1
```

* let 키워드도 호이스팅이 발생하지 않는 것처럼 보이지만 아래 코드를 통해 호이스팅이 발생하는 것을 알 수 있다.
```javascript
let x = 1; // 전역 변수

{   // 코드 블록문 시작
        // 전역 변수를 출력이 될것 같지만, 코드 블록문 내에 x를 탐색을 해버렸고,
        // 에러를 발생시킨다.
        // 이러한 원리를 봤을 땐, let 키워드도 호이스팅이 발생하는것을 볼 수 있다.
    console.log(x); // error : cannot access 'x' before initialization
    let x = 2; // 지역 변수, 호이스팅 유도됨
}   // 코드 블록문 끝
```

### 4. 전역 객체와 let
* var 키워드로 선언한 전역 변수, 전역 함수, 암묵적 전연은 전역 객체 window의 프로퍼티가 된다.
```javascript
// 전역 변수
var x = 1;
// 암묵적 전역 : 선언하지 않는 변수에 값을 할당
y = 2;
// 전역 함수
function foo() {}

console.log(window.x); // 1
console.log(x); // 1

console.log(window.y); // 2
console.log(y); // 2

console.log(window.foo); // ƒ foo() {}
console.log(foo); // ƒ foo() {}
```
* let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
    * window.foo와 같이 접근할 수 없다.
* let 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드)내에 존재한다.
```javascript
let x = 1;
// let 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티가 아님
console.log(window.x); // undefined
console.log(x); // 1
```
## const 키워드
* const 키워드는 상수를 선언하기 위해 사용됨
* 하지만, 꼭 상수만을 위해 사용되지는 않음
* let 키워드와 대부분 동일한 특징을 가짐
### 1. 선언과 초기화
* const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화를 해야함.
```javascript
const x = 1;
const y; // error : Missing initializer in const declaration
```
* let 키워드와 동일하게 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작함.
```javascript
{   // 블록문 시작
        // 변수 호이스팅이 발생하지 않는 것처럼 동작함.
    console.log(foo); // error : Cannot access 'foo' before initialization
    const foo = 1;
    console.log(foo); // 1
}   // 블록문 끝

// 블록 레벨 스코프
console.log(foo); // error : foo is not defined
```
### 2. 재할당 금지
* const 키워드로 선언한 변수는 재할당이 금지됨
```javascript
const x = 1;
x = 2; // error : Assignment to constant variable.
```
### 3. 상수
* 변수는 언제든지 재할당을 통해 변수 값을 변경할 수 있지만 상수는 재할당이 금지된다.
    * **상수는 재할당이 금지된 변수를 말한다.**
* const 키워드를 상수를 표현하는데 사용함
* 일반적으로 상수의 이름은 대문자로 선언해 상수임을 명확히 나타낸다.
* 여러 단어로 이뤄진 경우에는 언더스코어(_)로 구분해서 스네이크 케이스로 표현하는것이 일반적
```javascript
// 세율을 의미하는 0.1은 변경할 수 없는 상수로서 사용될 값
// 변수 이름을 대문자로 선언해 상수임을 명확히 나타냄
const TAX_RATE = 0.1;

// 세전 가격
let preTaxPrice = 100;

// 세후 가격
let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

console.log(afterTaxPrice); // 110
```

### 4. const 키워드와 객체
* **const 키워드로 선언된 변수에 객체를 할당한 경우 값을 변경할 수 있다.**
    * 원시 값으로 할당한 경우는 변경이 불가능!!
```javascript
const person = {
    name: 'Kim'
};

// 객체는 변경 가능한 값이다.
// 따라서 재할당 없이 변경이 가능하다.
person.name = 'Park';

console.log(person); // {name: "Park"}
```
* **const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지 않는다.**
```
새로운 값을 재할당하는 것은 불가능하지만 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을  
통해 객체를 변경하는 것은 가능하다. 이때 객체가 변경되더라도 변수에 할당된 참조 값은  
변경되지 않는다.
```

## "var" vs "let" vs "const"
* 기본적으로 const를 사용하고 let은 재할당이 필요한 경우에 한정되서 사용 권장
    * const를 사용하면 의도치 않은 재할당을 방지함으로써 안전함

* var, let, const 키워드는 다음과 같이 사용하는 것을 권장
    * ES6를 사용한다면 var 키워드 사용하지 마라
    * 재할당이 필요한 경우에 한정해 let 키워드 사용
        * 이때 변수의 스코프는 최대한 좁게 만든다.
    * 변경이 발생하지 않고 읽기 전용으로 사용하는 원시 값과 객체에는 const 키워드를 사용한다.
        * const 키워드는 재할당을 금지하므로 var, let 키워드 보단 안전함

```
변수 선언시 재할당이 유/무를 잘모르는 경우가 많다.
객체는 의외로 재할당하는 경우가 드물다.
(SPA 프레임워크에서는 상태가 변경되었음을 명확히 하기 위해 변경된 객체를 재할당하는 경우도 있다.)
변수 선언시 const 키워드를 사용하여 선언하고, 재할당이 필요하다면 let으로 바꾸면서 사용하자!
```
