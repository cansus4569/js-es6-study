# 이터러블

## 이터레이션 프로토콜
* ES6에서 도입됨
* 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

* **ES6 이전**의 순회 가능한 데이터 컬렉션(배열, 문자열, 유사 배열 객체, DOM 컬렉션 등)은 **통일된 규약 없이** 각자 나름의 구조를 가지고 `for문`, `for...in문`, `forEach 메서드` 등 다양한 방법으로 순회할 수 있었다.

* **ES6에서**는 순회 가능한 데이터 컬렉션을 **이터레이션 프로토콜을 준수하는 이터러블로 통일**하여 `for...of문`, `스프레드 문법`, `배열 디스트럭처링 할당의 대상`으로 사용할 수 있도록 **일원화**했다.

* 이터레이션 프로토콜에는 **이터러블 프로토콜**과 **이터레이터 프로토콜**이 있다.

* 이터러블 프로토콜 (iterable protocol)
    * Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환
    * 이러한 규약을 이터러블 프로토콜이라 한다.
    * **이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.**
    * **이터러블은 for...of문으로 순회할 수 있으며 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.**
* 이터레이터 프로토콜 (iterator protocol)
    * 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 **이터레이터**를 반환
    * 이터레이터는 next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 **이터레이터 리절트 객체**를 반환
    * 이러한 규약을 이터레이터 프로토콜이라 한다.
    * **이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다.**
    * 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

![iteration_protocol drawio](https://user-images.githubusercontent.com/63139527/183581148-039a0a65-31cd-4e35-9ded-b467add6e962.png)

### 1. 이터러블
* 이터러블 프로토콜을 준수한 객체를 이터러블이라 한다.
* Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체
* 이터러블인지 확인하는 함수 구현
```javascript
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';

isIterable([]);         // true
isIterable('');         // true
isIterable(new Map());  // true
isIterable(new Set());  // true
isIterable({});         // true
```
* 예) 배열은 Array.prototype의 Symbol.iterator 메서드를 상속받는 이터러블이다.
    * for...of문 순회 가능
    * 스프레드 문법 사용 가능
    * 배열 디스트럭처링 할당의 대상 사용 가능

```javascript
const array = [1, 2, 3];
// 상속 확인
console.log(Symbol.iterator in array); // true
// for...of
for(const item of array) {
    console.log(item);
}
// 스프레드
console.log([...array]); // [1, 2, 3]
// 디스트럭처링 할당 대상
const [a, ...rest] = array;
console.log(a, rest); // 1, [2, 3]
```
* 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다.
    * for...of 문 순회 불가
    * 디스트럭처링 할당 대상 불가
    * 단, 스프레드 문법은 사용 가능
        * TC39 프로세스의 stage4 단계에 제안되어 ECMAScript 표준에 적용됨
    * 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 된다.(사용자 정의 이터러블)

```javascript
const obj = {a:1, b:2};
console.log(Symbol.iterator in obj); // false
for(const item of obj) { // TypeError: obj is not iterable
    console.log(item);
}
const [a, b] = obj; // TypeError: obj is not iterable

console.log({...obj}); // {a:1, b:2}
```
### 2. 이터레이터
* **이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.**
* 이터레이터 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다.
    * next 메서드 호출시 이터러블을 순차적으로 한 단계식 순회하며 순회 결과를 나타내는 **이터레이터 리절트 객체**를 반환한다.
* 이터레이터 리절트 객체의 value 프로퍼티는 현재 순회 중인 이터러블의 값을 나타내며 done 프로퍼티는 이터러블의 순회 완료 여부를 나타낸다.
```javascript
const array = [1, 2, 3];
const iterator = array[Symbol.iterator]();

console.log('next' in iterator); // true

console.log(iterator.next());   // { value: 1, done: false }
console.log(iterator.next());   // { value: 2, done: false }
console.log(iterator.next());   // { value: 3, done: false }
console.log(iterator.next());   // { value: undefined, done: true }
```
## 빌트인 이터러블
* 자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다.

빌트인 이터러블|Symbol.iterator 메서드
-|-
Array|Array.prototype[Symbol.iterator]
String|String.prototype[Symbol.iterator]
Map|Map.prototype[Symbol.iterator]
Set|Set.prototype[Symbol.iterator]
TypedArray|TypedArray.prototype[Symbol.iterator]
arguments|arguments[Symbol.iterator]
DOM 컬렉션|NodeList.prototype[Symbol.iterator]</br>HTMLCollection.prototype[Symbol.iterator]

## for...of 문
* `for...of` 문은 이터러블을 순회하면서 이터러블의 요소를 변수에 할당한다.
```javascript
for ( 변수선언문 of 이터러블 ) { ... }
```
* for...of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for...of문의 변수에 할당한다.
* 그리고 이터레이터 리절트 객체의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단한다.
```javascript
for(const item of [1, 2, 3]) {
    console.log(item); // 1 2 3
}

// for...of 문의 내부 동작을 for 문으로 표현
// 이터러블
const iterable = [1, 2, 3];

// 이터레이터 생성
const iterator = iterable[Symbol.iterator]();

for(;;) {
    // 이터레이터의 next 메서드를 호출하여 이터러블을 순회
    // 이때 next 메서드는 이터레이터 리절트 객체를 반환
    const res = iterator.next();

    // 이터레이터 리절트 객체의 done 프로퍼티 값이 true이면 이터러블의 순회를 중단
    if(res.done) break;

    // 이터레이터 리절트 객체의 value 프로퍼티 값을 item 변수에 할당
    const item = res.value;
    console.log(item); // 1 2 3
}
```
## 이터러블과 유사 배열 객체
* 유사 배열 객체는 배열 처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다.
    * 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for문으로 순회할 수 있고
    * 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
* 유사 배열 객체는 이터러블이 아닌 일반 객체이다.
    * Symbol.iterator 메서드가 없기 때문에 for...of문으로 순회할 수 없다.

```javascript
// 유사 배열 객체
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};
for(let i = 0; i < arrayLike.length; i++) {
    console.log(arrayLike[i]); // 1 2 3
}

for(const item of arrayLike) { // TypeError: arrayLike is not iterable
    console.log(item);
}
```
* 단, arguments, NodeList, HTMLCollection 은 유사 배열 객체이면서 이터러블이다.
    * ES6에서 이터러블이 도입되면서 Symbol.iterator 메서드가 구현되어 이터러블이 되었다.
    * 이터러블이 된 이후에도 length 프로퍼티를 가지며 인덱스로 접근할 수 있는 것에는 변함이 없으므로 유사 배열 객체이면서 이터러블인 것이다.
* 배열도 마찬가지로 ES6에서 이터러블이 도입되면서 이터러블이 되었다.
* ES6에서 도입된 Array.from 메서드를 사용하여 배열로 간단히 변환 가능
    * Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환한다.

```javascript
// 유사 배열 객체
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};
// Array.from은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
const arr = Array.from(arrayLike);
console.log(arr); // [1, 2, 3]
```
## 이터레이션 프로토콜의 필요성
* 이터러블은 for...of문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로 데이터 공급자의 역할을 한다고 할 수 있다.
* 만약 다양한 데이터 공급자가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 데이터 공급자의 순회 방식을 모두 지원해야 한다. -> 효율적이지 않음
* 다양한 데이터 공급자가 이터레이션 프로토콜을 준수하도록 규정하면 데이터 소비자는 이터레이션 프로토콜만 지원하도록 구현하면 된다.
* 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 **데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할을 한다.**

![iteration_interface drawio](https://user-images.githubusercontent.com/63139527/183616682-2dac4612-ba18-4f4d-b44b-9f35ca8de215.png)

## 사용자 정의 이터러블

### 1. 사용자 정의 이터러블 구현
* 이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다.
* 사용자 정의 이터러블은
    * 이터레이션 프로토콜을 준수하도록 Symbol.iterator 메서드를 구현하고
    * Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터를 반환하도록 한다.
    * 이터레이터의 next 메서드는 done과 value 프로퍼티를 가지는 이터레이터 리절트 객체를 반환한다.
    * for...of문은 done 프로퍼티가 true가 될 때까지 반복하며 done 프로퍼티가 true가 되면 반복을 중지한다.

```javascript
// 피보나치 수열(1, 2, 3, 5, 8, ...) 구현한 사용자 정의 이터러블
const fibo = {
    // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
    [Symbol.iterator]() {
        let [pre, cur] = [0, 1]; // 배열 디스트럭처링 할당
        const max = 10; // 수열의 최대값

        // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환해야 하고
        // next 메서드는 이터레이터 리절트 객체를 반환해야 한다.
        return {
            next() {
                [pre, cur] = [cur, pre + cur]; // 배열 디스트럭처링 할당
                // 이터레이터 리절트 객체를 반환
                return { value: cur, done: cur >= max };
            }
        };
    }
};
// 이터러블인 fibo 객체를 순회할 때마다 next 메서드가 호출된다.
for(const num of fibo) {
    console.log(num); // 1 2 3 5 8
}

// 이터러블은 스프레드 문법의 대상이 될 수 있다.
const arr = [...fibo];
console.log(arr); // [1, 2, 3, 5, 8]
// 이터러블은 배열 디스트럭처링 할당의 대상이 될 수 있다.
const [first, second, ...rest] = fibo;
console.log(first, second, rest); // 1, 2, [3, 5, 8]
```
### 2. 이터러블을 생성하는 함수
* 위 예제에서 수열의 최대값은 (내부)고정된 값으로 외부에서 전달한 값으로 변경할 방법이 없다.
* 수열의 최대값을 (외부)인수로 전달받아 이터러블을 반환하는 함수를 만들면 된다.
```javascript
// 피보나치 수열을 구현한 사용자 정의 이터러블을 반환하는 함수
// 수열의 최대값을 인수로 전달받는다.
const fiboFunc = function (max) {
    let [pre, cur] = [0, 1];

    // Symbol.iterator 메서드를 구현한 이터러블을 반환
    return {
        [Symbol.iterator]() {
            return {
                next() {
                    [pre, cur] = [cur, pre + cur];
                    return { value: cur, done: cur >= max };
                }
            };
        }
    };
};
// 이터러블을 반환하는 함수에 수열의 최대값을 인수로 전달하면서 호출한다.
// fiboFunc(10)은 이터러블을 반환한다.
for (const num of fiboFunc(10)) {
    console.log(num); // 1 2 3 5 8
}
```
### 3. 이터러블이면서 이터레이터인 객체를 생성하는 함수
* 위의 예제에서 fiboFunc 함수는 이터러블을 반환한다.
* 만약 이터레이터를 생성하려면 이터러블의 Symbol.iterator 메서드를 호출해야 한다.
```javascript
// fiboFunc 함수는 이터러블을 반환
const iterable = fiboFunc(5);
// 이터러블의 Symbol.iterator 메서드는 이터레이터 반환
const iterator = iterable[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: 5, done: true }
```
* 이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator 메서드를 호출하지 않아도 된다.
* 다음 객체는 Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터다.
```javascript
// 이터러블이면서 이터레이터인 객체
// 이터레이터를 반환하는 Symbol.iterator 메서드와 이터레이션 리절트 객체를 반환하는 next 메서드를 소유한다.
{
    [Symbol.iterator]() { return this; },
    next() {
        return { value: any, done: boolean };
    }
}
```
* fiboFunc 함수를 이터러블이면서 이터레이터인 객체를 생성하여 반환하는 함수로 변경
```javascript
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fiboFunc = function (max) {
    let [pre, cur] = [0, 1];

    // Symbol.iterator 메서드와 next 메서드를 소유한 이터러블이면서 이터레이터인 객체를 반환
    return {
        [Symbol.iterator]() { return this; },
        // next 메서드는 이터레이터 리절트 객체를 반환
        next() {
            [pre, cur] = [cur, pre + cur];
            return { value: cur, done: cur >= max };
        }
    };
};

// iter는 이터러블이면서 이터레이터다.
let iter = fiboFUnc(10);

for(const num of iter) {
    console.log(num); // 1 2 3 5 8
}

iter = fiboFunc(10);

console.log(iter.next()); // { value: 1, done: false }
console.log(iter.next()); // { value: 2, done: false }
console.log(iter.next()); // { value: 3, done: false }
console.log(iter.next()); // { value: 5, done: false }
console.log(iter.next()); // { value: 8, done: false }
console.log(iter.next()); // { value: 13, done: true }
```
### 4. 무한 이터러블과 지연 평가
* 무한 이터러블을 생성하는 함수를 정의 해보자
    * 무한 수열을 간단히 구현할 수 있다.

```javascript
const fiboFunc = function () {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() { return this; },
        next() {
            [pre, cur] = [cur, pre + cur];
            // 무한을 구현해야 하므로 done 프로퍼티를 생략한다.
            return { value: cur };
        }
    };
};
for(const num of fiboFunc()) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8 ... 4181 6765
}
const [f1, f2, f3] = fiboFunc();
console.log(f1, f2, f3); // 1 2 3
```
* 위 예제의 이터러블은 **지연 평가**를 통해 데이터를 생성한다.
* 지연 평가는 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법이다.
* 즉, 평가 결과가 필요할 때까지 평가를 늦추는 기법이 지연 평가다.

* 지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 
    * **빠른 실행 속도**를 기대할 수 있고 
    * **불필요한 메모리를 소비하지 않으며**
    * **무한도 표현**할 수 있다는 장점이 있다.