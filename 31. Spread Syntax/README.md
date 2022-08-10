# 스프레드 문법
* ES6에서 도입된 스프레드 문법(전개 문법) `...` 은 하나로 뭉쳐있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만듬

* 스프레드 문법 사용 가능 대상 = for...of 문으로 순회할 수 있는 이터러블
    * Array
    * String
    * Map
    * Set
    * DOM 컬렉션(NodeList, HTMLCollection)
    * arguments

* 스프레드 문법의 결과는 값이 아닌 값들의 목록이다.
    * 스프레드 문법의 결과는 변수에 할당할 수 없다.

```javascript
// 스프레드 문법의 결과는 값이 아니다.
const list = ...[1, 2, 3]; // SyntaxError: Unexpected token ...
```

* 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다.
    * 함수 호출문의 인수 목록
    * ㅈ배열 리터럴의 요소 목록
    * 객체 리터럴의 프로퍼티 목록

## 함수 호출문의 인수 목록에서 사용하는 경우
* Math.max 메서드는 매개변수 개수를 확정할 수 없는 가변 인자 함수
* 여러 개의 숫자를 인수로 전달받아 인수 중에서 최대값을 반환
* 인수에 숫자가 아닌 배열을 전달하면 최대값을 구할 수 없음 (NaN 반환)
* 스프레드 문법이 제공되기 이전에는 `Function.prototype.apply` 사용해서 해결했음
* 스프레드 문법을 사용하면 간결하고 가독성이 좋음
```javascript
Math.max(1); // 1
Math.max(1, 2); // 2
Math.max(1, 2, 3); // 3
Math.max(); // -Infinity

var arr = [1, 2, 3];
Math.max(arr); // NaN

var max = Math.max.apply(null, arr); // 3

const array = [1, 2, 3];
const maximum = Math.max(...arrary); // 3
```
* 스프레드 문법은 Rest 파라미터와 형태가 동일하여 혼동할 수 있으므로 주의!
* Rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 `...`을 붙이는 것
* 스프레드 문법은 여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것

* 즉, Rest 파라미터 <--> 스프레드 문법은 서로 반대 개념을 가진다.
```javascript
// Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
function foo(...rest) {
    console.log(rest); // 1 ,2, 3 -> [1, 2, 3]
}
// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
// [1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3]);
```

## 배열 리터럴 내부에서 사용하는 경우
* ES5에서 사용하던 방식과 스프레드를 이용한 방식을 비교하여 살펴본다.

### 1. concat
* **ES5** : 2개의 배열을 1개의 배열로 결합하고 싶을땐 **concat 메서드**를 사용
* **스프레드 문법** : **배열 리터럴**만으로 2개의 배열을 1개의 배열로 결합 가능
```javascript
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]
// ES6
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

### 2. splice
* **ES5** : 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하려면 **splice 메서드** 사용
    * splice 메서드의 세 번째 인수로 배열을 전달하면 배열 자체가 추가됨

```javascript
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

// 세 번째 인수 arr2를 해체(= 배열을 해체하여 요소 값들로..)하여 전달하고자 하는데...
// arr1에 arr2 배열 자체가 추가됨
arr1.splice(1, 0, arr2);
// 기대했던 결과 값 [1, 2, 3, 4] 가 아님...
console.log(arr1); // [1, [2, 3], 4]
// ===================================
// 이러한 경우 Function.prototype.apply 메서드를 사용하여 splice 메서드를 호출해야 함
/*
apply 메서드의 2번째 인수(배열)는 apply 메서드가 호출한 splice 메서드의 인수 목록이다.
apply 메서드의 2번째 인수 [1, 0].concat(arr2)는 [1, 0, 2, 3]으로 평가된다.
따라서 splice 메서드에 apply 메서드의 2번째 인수 [1, 0, 2, 3]이 해체되어 전달된다.
즉, arr1[1]부터 0개의 요소를 제거하고 그 자리(arr1[1])에 새로운 요소(2, 3)를 삽입한다.
*/
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]
```
* **스프레드 문법 사용**
```javascript
// ES6
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

### 3. 배열 복사
* **ES5** : 배열 복사시 **slice 메서드** 사용 (얕은 복사)
```javascript
// ES5
var origin = [1, 2];
var copy = origin.slice();
console.log(copy); // [1, 2];
console.log(copy === origin); // false
```
* **스프레드 문법 사용** (얕은 복사)
```javascript
// ES6
const origin = [1, 2];
const copy = [...origin];
console.log(copy); // [1, 2]
console.log(copy === origin); // false
```
### 4. 이터러블을 배열로 변환
* **ES5** : `Function.prototype.apply` 또는 `Function.prototype.call` 메서드를 사용하여 **slice 메서드** 호출
    * 이터러블뿐만 아니라 이터러블이 아닌 유사 배열 객체도 배열로 변환할 수 있음

```javascript
// ES5
function sum() {
    // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
    var args = Array.prototype.slice.call(arguments); // [1, 2, 3]

    return args.reduce(function (pre, cur) {
        return pre + cur;
    }, 0);
}
console.log(sum(1, 2, 3)); // 6
// 이터러블이 아닌 유사 배열 객체
const arrLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};
const arr = Array.prototype.slice.call(arrLike); // [1, 2, 3]
console.log(Array.isArray(arr)); // true
```
* **스프레드 문법 사용**
```javascript
// ES6
function sum() {
    // 이터러블이면서 유사 배열 객체인 arguments를 배열로 반환
    return [...arguments].reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3)); // 6
```
* **Rest 파라미터 사용**
```javascript
// ES6
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
console.log(sum(1, 2, 3)); // 6
```
* 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없음
* 그래서, 이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6에서 도입된 Array.from 메서드를 사용
    * Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 전달받아 배열로 변환하여 반환

```javascript
// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
    0: 1,
    1: 2,
    2: 3,
    length: 3
};
const arr = [...arrayLike]; // TypeError: object is not iterable
Array.from(arrayLike); // [1, 2, 3]
```
## 객체 리터럴 내부에서 사용하는 경우
* Rest 프로퍼티와 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.
    * 스프레드 문법의 대상은 이터러블이어야 한다.
    * 스프레드 프로퍼티 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용한다.

```javascript
// 스프레드 프로퍼티
// 객체 복사 (얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };
console.log(copy); // { x: 1, y: 2 }
console.log(copy === obj); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```

* 스프레드 프로퍼티가 제안되기 이전에는 ES6에서 도입된 `Object.assign` 메서드를 사용하여  
여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.

```javascript
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x: 1, y: 10, z: 3}

// 특정 프로퍼티 변경
const changed = Object.assign({}, { x: 1, y: 2}, { y: 100 });
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = Object.assign({}, { x: 1, y: 2}, { z: 0 });
console.log(added); // { x: 1, y: 2, z: 0 }
```

* 스프레드 프로퍼티는 Object.assign 메서드를 대체할 수 있는 간편한 문법
```javascript
// 객체 병합. 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x: 1, y: 10, z: 3}

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
// changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = { ...{ x: 1, y: 2 }, z: 0 };
// added = { ...{ x: 1, y: 2 }, ...{ z: 0 } }
console.log(added); // { x: 1, y: 2, z: 0 }
```