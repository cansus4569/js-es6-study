# 배열

## 배열이란?
* 배열은 여러 개의 값을 순차적으로 나열한 구조
* 배열 리터럴을 통해 생성
```javascript
const arr = ['apple', 'banana', 'orange'];
```
* 배열이 가지고 있는 값을 **요소** 라고 부름
    * 자바스크립트의 모든 값은 배열의 요소가 될 수 있다
    * 원시값, 객체, 함수, 배열
* 배열에서 자신의 위치를 나타내는 0 이상의 정수인 **인덱스**를 가짐
    * 인덱스는 배열의 요소에 접근할 때 사용함
    * 인덱스는 0부터 시작
* 요소에 접근할 때는 대괄호 표기법 사용
```javascript
arr[0] // apple
arr[1] // banana
arr[2] // orange
```
* 배열은 요소의 개수, 즉 배열의 길이를 나타내는 **length 프로퍼티**를 가짐
```javascript
for(let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 'apple' 'banana' 'orange'
}
```
* 배열은 객체 타입을 가진다.
```javascript
typeof arr // obejct
```
* 배열은 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성
    * 배열의 생성자 함수는 Array
    * 배열의 프로토타입 객체는 Array.prototype
    * Array.prototype은 배열을 위한 빌트인 메서드를 제공
```javascript
const arr = [1, 2, 3];
arr.constructor === Array // true
Object.getPrototypeOf(arr) === Array.prototype // true
```
* 배열은 객체지만 일반 객체와는 구별되는 독특한 특징이 있음
    * 명확한 차이 : "**값의 순서(인덱스)**" 와 "**length 프로퍼티**"

구분|객체|배열
-|:-:|:-:
구조|프로퍼티 키와 프로퍼티 값|인덱스와 요소
값의 참조|프로퍼티 키|인덱스
값의 순서|X|O
length 프로퍼티|X|O

## 자바스크립트 배열은 배열이 아니다
* 자바스크립트의 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체
    * 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 됨
    * 연속적으로 이어져 있지 않을 수도 있음
        * 배열의 요소가 연속적으로 이어져 있지 않는 배열 "**희소 배열**" 이라 한다.

* 자바스크립트 배열은
    * 인덱스를 나타내는 문자열을 프로퍼티 키로 가짐
    * length 프로퍼티를 갖는 특수한 객체
```javascript
console.log(Object.getOwnPropertyDescriptors([1,2,3]));
/*
{
    '0': {value: 1, writable: true, enumerable: true, configurable: true}
    '1': {value: 2, writable: true, enumerable: true, configurable: true}
    '2': {value: 3, writable: true, enumerable: true, configurable: true}
    length: {value: 3, writable: true, enumerable: false, configurable: false}
}
*/
```
* 일반적인 배열
    * 인덱스로 요소에 빠르게 접근할 수 있다.
    * 하지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않다.

* 자바스크립트 배열
    * 해시 테이블로 구현된 객체이므로 인덱스로 요소에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수 밖에 없는 구조적인 단점이 있다.
    * 하지만 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.

## length 프로퍼티와 희소 배열
* length 프로퍼티 : 요소의 개수, 즉 배열의 길이를 나타내는 0이상의 정수 값
    * 빈 배열 경우 : length 프로퍼티 값은 0
    * 빈 배열이 아닌 경우 : 가장 큰 인덱스에 1을 더한 값과 같음
```javascript
[].length // 0
[1,2,3].length // 3
```
* length 프로퍼티 값은 0 ~ 2^32 - 1(4,294,967,295) 을 가질 수 있다.
* length 프로퍼티 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신된다.
```javascript
const arr = [1, 2, 3];
console.log(arr.length); // 3

arr.push(4);
console.log(arr.length); // 4
arr.pop();
console.log(arr.length); // 3
```

* length 프로퍼티 값은 요소의 개수(배열의 길이)로 결정되지만 임의의 숫자로 값을 명시적으로 할당 할 수 있음
* 주의 : 현재 length 프로퍼티 값보다 큰 숫자 값을 할당하면 length 프로퍼티 값은 변경되지만 실제 배열의 길이가 늘어나지는 않는다.
```javascript
const arr = [1, 2, 3, 4, 5];
console.log(arr.length); // 5
arr.length = 3;
console.log(arr); // [1, 2, 3]
arr.length = 7;
// length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지 않음
console.log(arr.length); // 7
console.log(arr); // [1, 2, 3, 4, 5, empty * 2]
```
* 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 희소 배열이라 한다.
* 자바스크립트는 희소 배열을 문법적으로 허용한다.
    * 하지만 사용하지 않는 것이 좋다.
* **희소 배열은 length와 배열 요소의 개수가 일치하지 않는다. 희소 배열의 length는 희소 배열의 실제 요소 개수보다 언제나 크다.**
```javascript
// 희소 배열
const sparse = [, 2, , 4];

// 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
console.log(sparse.length); // 4
console.log(sparse); // [empty, 2, empty, 4]

// 배열 sparse에는 인덱스가 0, 2인 요소가 존재하지 않는다.
console.log(Object.getOwnPropertyDescriptors(sparse));
/*
{
    '1': {value: 2, writable: true, enumerable: true, configurable: true}
    '3': {value: 4, writable: true, enumerable: true, configurable: true}
    length: {value: 4, writable: true, enumerable: false, configurable: false}
}
*/
```

## 배열 생성

### 1. 배열 리터럴
* 배열 리터럴은 0개 이상의 요소로 쉼표로 구분하여 대괄호( [ ] )로 묶는다.
* 배열 리터럴은 객체 리터럴과 달리 프로퍼티 키가 없고 값만 존재한다.
```javascript
// 배열 리터럴로 생성
const arr1 = [1, 2, 3];
console.log(arr1.length); // 3
// 요소를 하나도 추가하지 않으면 length 프로퍼티 값이 0인 빈 배열 생성
const arr2 = [];
console.log(arr2.length); // 0
// 요소를 생략하면 희소 배열이 생성된다.
const arr3 = [1, , 3]; // 희소 배열
// 희소 배열의 length는 배열의 실제 요소 개수보다 언제나 크다.
console.log(arr3.length); // 3
console.log(arr3); // [1, empty, 3]
// 객체인 arr3에 프로퍼티 키가 '1'인 프로퍼티가 존재하지 않기 때문
console.log(arr3[1]); // undefined
```
### 2. Array 생성자 함수
* Array 생성자 함수를 통해 배열을 생성
    * 전달된 인수의 개수에 따라 다르게 동작하므로 주의 필요!
* 전달된 인수가 1개이고 숫자인 경우 length 프로퍼티 값이 인수인 배열을 생성
```javascript
const arr = new Array(10);
console.log(arr); // [empty * 10] // 희소 배열
console.log(arr.length); // 10

// 배열은 요소를 최대 4,294,967,295개 가질 수 있다.
new Array(4294967295);
new Array(4294967296); // RangeError: Invalid array length
// 전달된 인수가 음수이면 에러 발생
new Array(-1); // RangeError: Invalid array length
```
* 전달된 인수가 없는 경우 빈 배열을 생성한다. (배열 리터럴 [ ] 과 같다.)
```javascript
new Array(); // [ ]
```
* 전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열을 생성한다.
```javascript
// 전달된 인수가 2개 이상이면 인수를 요소로 갖는 배열을 생성한다.
new Array(1, 2, 3); // [1, 2, 3]
// 전달된 인수가 1개지만 숫자가 아니면 인수를 요소로 갖는 배열을 생성한다.
new Array({}); // [ { } ]
```
* new 연산자와 함께 호출하지 않더라도(=일반 함수로서 호출) 배열을 생성하는 생성자 함수로 동작한다.
    * Array 생성자 함수 내부에서 new.target을 확인하기 때문
```javascript
Array(1, 2, 3); // [1, 2, 3]
```

### 3. Array.of
* ES6에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성
* Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
```javascript
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
Array.of(1); // [1]
Array.of(1, 2, 3); // [1, 2, 3]
Array.of('string'); // ['string']
```

### 4. Array.from
* ES6에서 도입된 Array.from 메서드는 **유사 배열 객체** 또는 **이터러블 객체**를 인수로 전달받아 배열로 변환하여 반환한다.
```javascript
// 유사 배열 객체를 변환하여 배열 생성
Array.from({ length: 2, 0: 'a', 1: 'b' }); // ['a', 'b']
// 이터러블을 변환하여 배열을 생성. 문자열은 이터러블이다.
Array.from('Hi'); // ['H', 'i']
```

* Array.from을 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소를 채울 수 있다.
* 두 번째 인수로 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열을 반환한다.
```javascript
// Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined를 요소로 채운다.
Array.from({ length: 3 }); // [undefined, undefined, undefined]
// Array.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환한다.
Array.from({ length: 3}, (value, index) => index); // [0, 1, 2]
```

## 배열 요소의 참조
* 배열의 요소를 참조할 때에는 대괄호( [ ] ) 표기법을 사용
    * 대괄호 안에는 인덱스가 와야 한다.
* 정수로 평가되는 표현식이라면 인덱스 대신 사용할 수 있다.
* 인덱스는 값을 참조할 수 있다는 의미에서 객체의 프로퍼티 키와 같은 역할을 한다.
```javascript
const arr = [1, 2];
console.log(arr[0]); // 1
// 존재하지 않는 요소에 접근하면 undefined 반환
console.log(arr[2]); // undefined
```

## 배열 요소의 추가와 갱신
* 배열에도 요소를 동적으로 추가할 수 있다.
* 존재하지 않는 인덱스를 사용해 값을 할당하면 새로운 요소가 추가된다.
    * length 프로퍼티 값은 자동 갱신
* 현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 된다.
```javascript
const arr = [0];
arr[1] = 1; // 배열 요소 추가
console.log(arr); // [0, 1]
console.log(arr.length); // 2

arr[100] = 100;
console.log(arr); // [0, 1, empty * 98, 100]
console.log(arr.length); // 101
```

* 이미 요소가 존재하는 요소에 값을 재할당하면 요소값이 갱신된다.
```javascript
arr[1] = 10;
console.log(arr); // [0, 10, empty * 98, 100]
```

* 인덱스는 요소의 위치를 나타내므로 반드시 0 이상의 정수(또는 정수 형태의 문자열)를 사용해야 한다.
* 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성된다.
* 이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.
```javascript
const arr = [];

// 배열 요소의 추가
arr[0] = 1;     // 인덱스 정수
arr['1'] = 2;   // 인덱스 정수 형태의 문자열

// 프로퍼티 추가
arr['foo'] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```

## 배열 요소의 삭제
* 배열은 객체이기 때문에 배열의 특정 요소를 삭제하기 위해 delete 연산자를 사용할 수 있다.
    * delete 연산자는 객체의 프로퍼티를 삭제한다. -> 희소 배열로 변함
    * 희소 배열을 만드는 delete 연산자는 사용하지 않는 것이 좋다.

```javascript
const arr = [1, 2, 3];
// 배열 요소의 삭제
delete arr[1];
console.log(arr); // [1, empty, 3]
// length 프로퍼티에 영향을 주지 않는다. 희소 배열이 된다.
console.log(arr.length); // 3
```
* 희소 배열을 만드지 않으면서 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드를 사용한다.
```javascript
const arr = [1, 2, 3];

// Array.prototype.splice (삭제를 시작할 인덱스, 삭제할 요소 수)
// arr[1] 부터 1개의 요소 제거
arr.splice(1, 1);
console.log(arr); // [1, 3]

// length 프로퍼티가 자동 갱신된다.
console.log(arr.length); // 2
```

## 배열 메서드
* 자바스크립트 배열은 다양한 빌트인 메서드를 제공함
    * Array 생성자 함수는 정적 메서드를 제공
    * Array.prototype은 프로토타입 메서드를 제공

* 배열 메서드는 결과물을 반환하는 패턴이 2가지가 있으므로 주의!!
    * **원본 배열(배열 메서드를 호출한 배열, 즉 배열 메서드의 구현체 내부에서 this가 가리키는 객체)을 직접 변경하는 메서드**
    * **원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드**

```javascript
const arr = [1];

// push 메서드는 원본 배열(arr)을 직접 변경
arr.push(2);
console.log(arr); // [1, 2]

// concat 메서드는 원본 배열(arr)을 직접 변경하지 않고 새로운 배열을 생성하여 반환
const result = arr.concat(3);
console.log(arr); // [1, 2]
console.log(result); // [1, 2, 3]
```

### 1. Array.isArray
* Array 생성자 함수의 정적 메서드
* 전달된 인수가 배열이면 true, 배열이 아니면 false 반환
```javascript
// true
Array.isArray([]);
Array.isArray([1,2]);
Array.isArray(new Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(1);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
Array.isArray({ 0: 1, length: 1});
```

### 2. Array.prototype.indexOf
* 원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환
    * 중복되는 요소가 여러 개 있다면 첫 번째로 검색된 요소의 인덱스를 반환
    * 요소가 존재하지 않으면 -1을 반환
```javascript
const arr = [1, 2, 2, 3]; // 2 중복된 배열
// 배열 arr에서 요소 2를 검색하여 첫 번째로 검색된 요소의 인덱스를 반환
arr.indexOf(2); // 1
// 배열 arr에 요소 4가 없으므로 -1을 반환
arr.indexOf(4); // -1
// 두 번째 인수는 검색을 시작할 인덱스이다. 두 번째 인수를 생략하면 처음부터 검색한다.
arr.indexOf(2, 2); // 2

// 배열에 특정 요소가 존재하는지 확인할 때 유용하다.
const foods = ['apple', 'banana', 'orange'];

// foods 배열에 'orange' 요소가 존재하는지 확인
if(foods.indexOf('orange') === -1) {
    // foods 배열에 'orange' 요소가 존재하지 않으면 요소 추가
    foods.push('orange');
}
console.log(foods); // ["apple", "banana", "orange"]
```
* ES7에 도입된 Array.prototype.includes 메서드를 사용하면 가독성이 좋다.
```javascript
const foods = ['apple', 'banana', 'orange'];
if(!foods.includes('orange')) {
    foods.push('orange');
}
console.log(foods); // ["apple", "banana", "orange"]
```

### 3. Array.prototype.push
* 인수로 전달받은 모든 값을 **원본 배열의 마지막 요소로 추가**하고 변경된 **length 프로퍼티 값을 반환**
* 원본 배열을 직접 변경한다.
```javascript
const arr = [1, 2];
let result = arr.push(3, 4);
console.log(result); // 4
console.log(arr); // [1, 2, 3, 4]
```
* push 메서드는 성능면에서 좋지 않음
* **length 프로퍼티를 사용하여 배열의 마지막에 요소를 직접 추가할 수 있음 (push 메서드 보다 빠름)**
```javascript
const arr = [1, 2];
arr[arr.length] = 3;
console.log(arr); // [1, 2, 3]
```
* push 메서드는 원본 배열을 직접 변경하므로, **ES6의 스프레드 문법을 사용하는 편이 좋다.(직접 변경 X)**
```javascript
const arr = [1, 2];

// ES6 스프레드 문법
const newArr = [...arr, 3];
console.log(newArr); // [1, 2, 3]
```

### 4. Array.prototype.pop
* 원본 배열에서 **마지막 요소를 제거**하고 **제거한 요소를 반환**
* 원본 배열이 빈 배열이면 undefined를 반환
* 원본 배열을 직접 변경한다.
```javascript
const arr = [1, 2];
let result = arr.pop();
console.log(result); // 2
console.log(arr); // [1]
```
* 스택(LIFO) 구현 (push, pop 메서드 이용)
    * [생성자 함수](./src/stack_func.html)
    * [클래스](./src/stack_class.html)

### 5. Array.prototype.unshift
* 인수로 전달받은 모든 값을 **원본 배열의 맨 앞에 요소로 추가**하고 변경된 **length 프로퍼티 값을 반환**
* 원본 배열을 직접 변경
```javascript
const arr = [1, 2];
let result = arr.unshift(3, 4);
console.log(result); // 4
console.log(arr); // [3, 4, 1, 2]
```
* unshift 메서드는 원본 배열을 직접 변경하므로, **ES6의 스프레드 문법을 사용하는 편이 좋다.(직접 변경 X)**
```javascript
const arr = [1, 2];
// ES6 스프레드 문법
const newArr = [3, ...arr];
console.log(newArr); // [3, 1, 2]
```

### 6. Array.prototype.shift
* 원본 배열에서 **첫 번째 요소를 제거**하고 **제거한 요소를 반환**
* 원본 배열이 빈 배열이면 undefined를 반환
* 원본 배열을 직접 변경한다.
```javascript
const arr = [1, 2];
let result = arr.shift();
console.log(result); // 1
console.log(arr); // [2]
```

* 큐(FIFO) 구현 (shift, push 메서드 이용)
    * [생성자 함수](./src/queue_func.html)
    * [클래스](./src/queue_class.html)

### 7. Array.prototype.concat
* 인수로 전달된 값들(배열 또는 원시값)을 **원본 배열의 마지막 요소로 추가한 새로운 배열을 반환**
* 원본 배열은 변경되지 않음
```javascript
const arr1 = [1, 2];
const arr2 = [3, 4];

let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

result = arr1.concat(3);
console.log(result); // [1, 2, 3]

result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않음
console.log(arr1); // [1, 2]
```
* push 메서드와 unshift 메서드는 concat 메서드로 대체할 수 있다.
    * 유사하게 동작하지만 미묘한 차이가 있다.
* push, unshift 메서드를 사용할 경우, 원본 배열을 반드시 변수에 저장해 두어야 한다.
* concat 메서드를 사용할 경우, 반환 값을 반드시 변수에 할당받아야 한다.
```javascript
const arr1 = [3, 4];

arr1.unshift(1, 2);
console.log(arr1); // [1, 2, 3, 4]

arr1.push(5, 6);
console.log(arr1); // [1, 2, 3, 4, 5, 6]

const arr2 = [3, 4];

// arr1.unshift(1, 2) 대체
let result = [1, 2].concat(arr2);
console.log(result); // [1, 2, 3, 4]

// arr1.push(5, 6) 대체
result = result.concat(5,6);
console.log(result); // [1, 2, 3, 4, 5, 6]
```
* push, unshift 메서드는 배열을 그대로 원본 배열의 마지막/첫 번째 요소로 추가
* concat 메서드는 인수로 전달받은 배열을 해체하여 새로운 배열의 마지막 요소로 추가
```javascript
const arr = [3, 4];

arr.unshift([1, 2]);
arr.push([5, 6]);
console.log(arr); // [ [1, 2], 3, 4, [5, 6] ]

let result = [1, 2].concat([3, 4]);
result = result.concat([5, 6]);

console.log(result); // [1, 2, 3, 4, 5, 6]
```
* concat 메서드는 ES6의 스프레드 문법으로 대체할 수 있다.
```javascript
let result = [1, 2].concat([3, 4]);
console.log(result); // [1, 2, 3, 4]

result = [...[1, 2], ...[3, 4]];
console.log(result); // [1, 2, 3, 4]
```

### 8. Array.prototype.splice
* 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용
* 3개의 매개변수가 있으며 원본 배열을 직접 변경한다.
```javascript
/**
 * @param start 원본 배열의 요소를 제거하기 시작할 인덱스
 *              start만 지정하면 원본 배열의 start부터 모든 요소를 제거함
 *              start가 음수인 경우 배열의 끝에서의 인덱스를 나타냄
 *                  ex) -1 이면 마지막 요소를 가리키고 -n이면 마지막에서 n번째 요소를 가리킴
 * @param deleteCount 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소의 개수
 *                    deleteCount가 0인 경우 아무런 요소도 제거되지 않음(옵션)
 * @param items 제거한 위치에 삽입할 요소들의 목록
 *              생략할 경우 원본 배열에서 요소들을 제거하기만 한다(옵션)
 */
Array.splice(start, deleteCount, items);

const arr = [1, 2, 3, 4];
// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고, 그 자리에 새로운 요소 20, 30을 삽입
const result = arr.splice(1, 2, 20, 30);
// 제거한 요소가 배열로 반환
console.log(result); // [2, 3]
// 원본 배열을 직접 변경
console.log(arr); // [1, 20, 30, 4]
```
* 배열에서 특정 요소를 제거하려면 indexOf 메서드를 통해 특정 요소의 인덱스를 얻은 다음 splice 메서드를 사용
```javascript
const arr = [1, 2, 3, 1, 2];

function remove(array, item) {
    const index = arrary.indexOf(item);

    if(index !== -1) array.splice(index, 1);

    return array;
}
console.log(remove(arr, 2)); // [1, 3, 1, 2]
console.log(remove(arr, 10)); // [1, 3, 1, 2]
```
* filter 메서드를 사용하여 특정 요소를 제거할 수도 있다.
    * 특정 요소가 중복된 경우 모두 제거

```javascript
const arr = [1, 2, 3, 1, 2];

function removeAll(array, item){
    return array.filter(v => v !== item);
}
console.log(removeAll(arr, 2)); // [1, 3, 1]
```

### 9. Array.prototype.slice
* 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.
* 원본 배열은 변경되지 않음
```javascript
/**
 * @param start 복사를 시작할 인덱스
 *              음수인 경우 배열의 끝에서의 인덱스를 나타냄
 *                  ex) -2인 경우 배열의 마지막 2개의 요소를 복사하여 배열로 반환
 * @param end   복사를 종료할 인덱스
 *              이 인덱스에 해당하는 요소는 복사되지 않는다.
 *              생략 가능하며 생략 시 기본값은 length 프로퍼티 값
 */
Array.slice(start, end);

const arr = [1, 2, 3];
// arr[0] 부터 arr[1] 이전(arr[1] 미포함)까지 복사하여 반환
arr.slice(0, 1); // [1]
// arr[1] 부터 arr[2] 이전(arr[2] 미포함)까지 복사하여 반환
arr.slice(1, 2); // [2]

// 원본 배열은 변경되지 않음
console.log(arr); // [1, 2, 3]

// arr[1]부터 이후의 모든 요소를 복사하여 반환
arr.slice(1); // [2, 3]

// 배열의 끝에서부터 요소를 1개 복사하여 반환
arr.slice(-1); // [3]

// 배열의 끝에서부터 요소를 2개 복사하여 반환
arr.slice(-2); // [2, 3]

// 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환
const copy = arr.slice();
console.log(copy); // [1, 2, 3]
// 얕은 복사
console.log(copy === arr); // false
```
* slice 메서드를 이용하여 유사 배열 객체를 배열로 변환 가능
```javascript
function sum() {
    // 유사 배열 객체를 배열로 변환(ES5)
    var arr = Array.prototype.slice.call(arguments);
    console.log(arr); // [1, 2, 3]

    return arr.reduce(function (pre, cur) {
        return pre + cur;
    }, 0);
}
console.log(sum(1, 2, 3)); // 6
```
* Array.from 메서드를 사용하면 더 간단하게 유사 배열 객체(이터러블 객체)를 배열로 변환 가능
```javascript
function sum() {
    const arr = Array.from(arguments);
    console.log(arr); // [1, 2, 3]

    return arr.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3)); // 6
```
* `arguments` 객체는 유사 배열 객체이면서 이터러블 객체
* 이터러블 객체는 ES6의 스프레드 문법을 사용하면 간단하게 배열로 변환 가능
```javascript
function sum() {
    // 이터러블을 배열로 변환(ES6 스프레드 문법)
    const arr = [...arguments];
    console.log(arr); // [1, 2, 3]

    return arr.reduce((pre, cur) => pre + cur, 0);
}
console.log(sum(1, 2, 3)); // 6
```
### 10. Array.prototype.join
* 원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 문자열(구분자)로 연결한 문자열을 반환
    * 구분자는 생략 가능하며, 기본 구분자는 콤마(',')

```javascript
const arr = [1, 2, 3, 4];
// 기본 구분자 : 콤마
arr.join(); // '1,2,3,4,'
arr.join(''); // '1234'
arr.join(':'); // '1:2:3:4'
```

### 11. Array.prototype.reverse
* 원본 배열의 순서를 반대로 뒤집는다.
* 원본 배열을 직접 변경되며, 반환되는 값은 변경된 배열이다.
```javascript
const arr = [1, 2, 3];
const result = arr.reverse();

// 원본 배열이 직접 변경됨
console.log(arr); // [3, 2, 1]
// 반환 값은 변경된 배열
console.log(result); // [3, 2, 1]
```

### 12. Array.prototype.fill
* ES6에 도입된 메서드
* 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.
* 원본 배열이 변경됨
```javascript
const arr1 = [1, 2, 3];
// 인수로 전달받은 값 0을 배열의 처음부터 끝까지 요소로 채운다.
arr1.fill(0);
// 원본 배열을 직접 변경한다.
console.log(arr1); // [0, 0, 0]

const arr2 = [1, 2, 3];
// 두 번째 인수로 요소 채우기를 시작할 인덱스를 전달할 수 있음
// 인수로 전달받은 값 0을 배열의 인덱스 1부터 끝까지 요소로 채운다.
arr2.fill(0, 1);
console.log(arr2); // [1, 0, 0];

const arr3 = [1, 2, 3, 4, 5];
// 세 번째 인수로 요소 채우기를 멈출 인덱스를 전달할 수 있다.
// 인수로 전달받은 값 0을 배열의 인덱스 1부터 3이전(인덱스 3 미포함)까지 요소로 채운다.
arr3.fill(0, 1, 3);
console.log(arr3); // [1, 0, 0, 4, 5]
```
* 단점 : 모든 요소를 하나의 값만으로 채울 수밖에 없음
* Array.from 메서드를 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 요소값을 만들면서 배열을 채울 수 있다.
```javascript
// 인수로 전달받은 정수만큼 요소를 생성하고 0부터 1씩 증가하면서 요소를 채운다.
const sequences = (length = 0) => Array.from({ length }, (_, i) => i);
// const sequences = (length = 0) => Array.from(new Array(length), (_, i) => i);
console.log(sequence(3)); // [0, 1, 2]
```
### 13. Array.prototype.includes
* ES7에서 도입된 메서드
* 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환
* 첫 번째 인수로 검색할 대상을 지정
* 두 번째 인수로 검색을 시작할 인덱스를 지정
    * 생략할 경우 기본값 0이 설정됨
    * 음수를 전달하면 length 프로퍼티 값과 음수 인덱스를 합산하여(length + index) 검색 시작 인덱스 설정
```javascript
const arr = [1, 2, 3];

// 배열에 요소 2가 포함되어 있는지 확인
arr.includes(2); // true

// 배열에 요소 100이 포함되어 있는지 확인
arr.includes(100); // false

// 배열에 요소 1이 포함되어 있는지 인덱스 1부터 확인
arr.includes(1, 1); // false

// 배열에 요소 3이 포함되어 있는지 인덱스 2(arr.length - 1)부터 확인
arr.includes(3, -1); // true
```

* indexOf 메서드를 사용해도 배열 요소 포함 여부를 알 수 있지만
    * 반환값이 -1 인지 확인해야 함 => 요소가 미포함 인지 확인
    * 배열에 NaN이 포함되어 있는지 확인할 수 없음

```javascript
[NaN],indexOf(NaN) !== -1; // false
[NaN].includes(NaN); // true
```

### 14. Array.prototype.flat
* ES10에서 도입된 메서드
* 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.
```javascript
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]
```
* 중첩 배열을 평탄화할 깊이를 인수로 전달할 수 있다.
* 인수를 생략할 경우 기본값은 1이다.
* 인수로 Infinity를 전달하면 중첩 배열 모두를 평탄화 한다.
```javascript
// 중첩 배열을 평탄화하기 위한 깊이 값의 기본값은 1이다.
[1, [2, [3, [4]]]].flat(); // [1, 2, [3, [4]]]
[1, [2, [3, [4]]]].flat(1); // [1, 2, [3, [4]]]

// 중첩 배열을 평탄화하기 위한 깊이 값을 2로 지정하여 2단계 깊이까지 평탄화한다.
[1, [2, [3, [4]]]].flat(2); // [1, 2, 3, [4]]
// 2번 평탄화한 것과 동일하다.
[1, [2, [3, [4]]]].flat().flat(); // [1, 2, 3, [4]]

// 중첩 배열을 평탄화하기 위한 깊이 값을 Infinity로 지정하여 중첩 배열 모두를 평탄화한다.
[1, [2, [3, [4]]]].flat(Infinity); // [1, 2, 3, 4]
```
## 배열 고차 함수
* 고차 함수 (Higher-Order Function, HOF)는 함수를 인수로 전달받거나 함수를 반환하는 함수
    * 자바스크립트의 함수는 일급 객체 -> 함수를 값처럼 인수로 전달할 수 있고 반환 가능
    * 외부 상태의 변경이나 가변 데이터를 피하고 **불변성을 지향**하는 함수형 프로그래밍에 기반을 두고 있음

* 함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 **조건문과 반복문을 제거**하여 복잡성을 해결하고 **변수의 사용을 억제**하여 상태 변경을 피하려는 프로그래밍 패러다임이다.
    * 오류 발생의 근본적인 원인
        * 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게 하여 가독성을 해침
        * 변수는 누군가에 의해 언제든지 변경될 수 있음
* 함수형 프로그래밍은 **순수 함수를 통해 부수 효과를 최대한 억제**하여 오류를 피하고 프로그램의 안정성을 높이려는 노력의 일환

### 1. Array.prototype.sort
* 배열의 요소를 정렬하는 메서드
* 원본 배열을 직접 변경 -> 정렬된 배열 반환
* 기본적으로 오름차순으로 요소를 정렬
    * 내림차순 정렬 방법 : sort 메서드 사용후 -> reverse 메서드 사용

```javascript
const fruits = ['Banana', 'Orange', 'Apple'];
// 오름차순 정렬
fruits.sort();
// 원본 배열 직접 변경
console.log(fruits); // ['Apple', 'Banana', 'Orange']
// 내림차순 정렬 (반드시 sort 이후)
fruits.reverse();
// 원본 배열 직접 변경
console.log(fruits); // ['Orange', 'Banana', 'Apple']
```

* 배열 요소가 `숫자`인 경우 주의가 필요!
    * sort 메서드의 정렬 순서는 **유니코드 코드 포인트**의 순서를 따르기 떄문
    * 숫자의 경우 원리 : 일시적으로 문자열로 변환 후 유니코드 코드 포인트의 순서로 정렬
```javascript
const num = [40, 100, 1, 5, 2, 25, 10];
const str = ['40', '100', '1', '5', '2', '25', '10'];
num.sort();
str.sort();
// 유니코드 코드 포인트 기준으로 정렬함
// 숫자의 경우 문자열로 일시적으로 변환후 정렬됨
console.log(num); // [1, 10, 100, 2, 25, 40, 5]
console.log(str); // ['1', '10', '100', '2', '25', '40', '5']
```

* 숫자 요소를 정렬할 때는 sort 메서드에 **정렬 순서를 정의하는 비교 함수를 인수로 전달**해야 한다.
    * 비교 함수는 양수나 음수 또는 0을 반환해야 한다.
        * 0보다 작으면 비교 함수의 첫 번째 인수를 우선하여 정렬
        * 0이면 정렬하지 않음
        * 0보다 크면 두 번째 인수를 우선하여 정렬

```javascript
const points = [40, 100, 1, 5, 2, 25, 10];

// 숫자 배열의 오름차순 정렬. 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬
points.sort((a, b) => a - b);
console.log(points); // [1, 2, 5, 10, 25, 40, 100]

// 최소/최대값 획득
console.log(points[0], points[points.length - 1]); // 1 100

// 숫자 배열의 내림차순 정렬. 비교 함수의 반환값이 0보다 작으면 b를 우선하여 정렬
points.sort((a, b) => b - a);
console.log(points); // [100, 40, 25, 10, 5, 2, 1]
```

```
< 참고 >
sort 메서드는 quicksort 알고리즘을 사용해 왔었다.
ES10에서는 timesort 알고리즘을 사용하도록 바뀜
```

### 2. Array.prototype.forEach
* for문은 반복을 위한 변수 선언, 조건식과 증감식으로 이루어져 있어서 함수형 프로그래밍이 추구하는 바와 맞지 않음
* forEach 메서드는 for문을 대체할 수 있는, 반복문을 추상화한 고차 함수
* 원본 배열을 변경하지 않음 / 콜백 함수를 통해 원본 배열을 변경 가능
* 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출함
* forEach 메서드를 호출한 배열의 요소값과 인덱스, forEach 메서드를 호출한 배열(this)을 순차적으로 전달한다.
* forEach 메서드의 반환값은 undefined
```javascript
const numbers = [1, 2, 3];
const pows = [];

// forEach 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출한다.
numbers.forEach(item => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]

// forEach 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달
[1, 2, 3].forEach((item, index, arr) => {
    console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
});

// 원본 배열을 변경하지 않음 / 콜백 함수를 통해 원본 배열을 변경 가능
// 콜백 함수의 세 번째 매개변수 arr은 원본 배열 numbers를 가리킨다.
// 따라서 콜백 함수의 세 번째 매개변수 arrdㅡㄹ 직접 변경하면 원본 배열 numbers가 변경됨
numbers.forEach((item, index, arr) => { arr[index] = item ** 2; });
console.log(numbers); // [1, 4, 9]

const result = [1, 2, 3].forEach(console.log);
console.log(result); // undefined
```

* forEach 메서드의 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있음
    * 더 나은 방법 : 화살표 함수를 사용
        * 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음
        * 화살표 함수 내부에서 this를 참조하면 multiply 메서드 내부(상위 스코프)의 this를 그대로 참조 한다.

```javascript
class Numbers {
    numberArray = [];

    multiply(arr) {
        // 방법 1
        arr.forEach(function(item) {
            this.numberArray.push(item * item);
        }, this); // forEach 메서드의 콜백 함수 내부에서 this롤 사용할 객체를 전달
        // 방법 2
        // 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조
        arr.forEach(item => this.numberArray.push(item * item));
    }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

* forEach 메서드의 폴리필
    * forEach 메서드 내부에서는 반복문(for문)을 통해 순회한다.
    * 단, 반복문을 메서드 내부로 은닉하여 로직의 흐름을 이해하기 쉽게 하고 복잡성을 해결함

```
폴리필(polyfill) 이란?
최신 사양의 기능을 지원하지 않는 브라우저를 위해 누락된 최신 사양의 기능을 구현하여 추가하는 것
```

```javascript
// 만약 Array.prototype에 forEach 메서드가 존재하지 않으면 폴리필을 추가한다.
if (!Array.prototype.forEach) {
    Array.prototype.forEach = function (callback, thisArg) {
        // 첫 번째 인수가 함수가 아니면 TypeError 발생 시킴
        if (typeof callback !== 'function') {
            throw new TypeError(callback + ' is not a function');
        }

        // this로 사용할 두 번째 인수를 전달받지 못하면 전역 객체를 this로 사용
        thisArg = thisArg || window;

        // for문으로 배열을 순회하면서 콜백 함수를 호출
        for (var i = 0; i < this.length; i++) {
            // call 메서드를 통해 thisArg를 전달하면서 콜백 함수를 호출
            // 이때 콜백 함수의 인수로 배열 요소, 인덱스, 배열 자신을 전달
            callback.call(thisArg, this[i], i, this);
        }
    };
}
```

* forEach 메서드는 for문과 달리 break, continue 문을 사용할 수 없음!
    * 배열의 모든 요소를 빠짐없이 모두 순회하며 중간에 중단할 수 없음
* 희소 배열의 경우 존재하지 않는 요소는 순회 대상에서 제외함
```javascript
// 희소 배열
const arr = [1, , 3];
// for 문으로 희소 배열 순회
for(let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 1, undefined, 3
}
// forEach 메서드로 희소 배열 순회
// 존재하지 않는 요소는 순회 대상에서 제외함
arr.forEach(v => console.log(v)); // 1, 3
```

* 정리
    * forEach 메서드는 for문에 비해 성능이 좋지 않지만 가독성이 좋다.
    * 요소가 많은 배열을 순회하거나 시간이 많이 걸리는 복잡한 코드 또는 높은 성능이 필요한 경우가 아니면 forEach 메서드 사용 권장

### 3. Array.prototype.map
* 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
* **콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.**
* 원본 배열은 변경되지 않음
* map 메서드를 호출한 배열의 요소값과 인덱스, map 메서드를 호출한 배열(this)을 순차적으로 전달한다.
```javascript
const numbers = [1, 4, 9];

// map 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출
// 콜백 함수의 반환값들로 구성된 새로운 배열을 반환
const roots = numbers.map(item => Math.sqrt(item));
// const roots = numbers.map(Math.sqrt);

// map 메서드는 새로운 배열을 반환
console.log(roots); // [1, 2, 3]
// map 메서드는 원본 배열을 변경하지 않음
console.log(numbers); // [1, 4, 9]

// map 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달
[1, 2, 3].map((item, index, arr) => {
    console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
    return item;
});
```

* forEach vs map
    * 공통점 
        * 배열의 모든 요소를 순회 및 인수로 전달 받은 콜백 함수를 반복 호출
        * 고차 함수
    * 차이점
        * forEach 반환 : undefined
        * map 반환 : 새로운 배열

* **map 메서드를 호출한 배열과 map 메서드가 생성하여 반환한 배열은 1:1 매핑한다.**
    * **length 프로퍼티 값이 동일**

* map 메서드의 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있음
    * 더 나은 방법 : 화살표 함수를 사용
        * 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음

```javascript
class Prefixer {
    constructor(prefix) {
        this.prefix = prefix;
    }
    add(arr) {
        // 방법 1
        return arr.map(function (item) {
            // 외부에서 this를 전달하지 않으면 this는 undefined를 가리킨다.
            return this.prefix + item;
        }, this); // map 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달

        // 방법 2
        // 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.
        return arr.map(item => this.prefix + item);
    }
}
const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
// ['-webkit-transition', '-webkit-user-select']
```

### 4. Array.prototype.filter
* 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
* **콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환한다.**
* filter 메서드는 필터링 조건을 만족하는 특정 요소만 추출하여 새로운 배열을 만들고 싶을 때 사용함
* 원본 배열은 변경되지 않음
* filter 메서드를 호출한 배열의 요소값과 인덱스, filter 메서드를 호출한 배열(this)을 순차적으로 전달한다.
```javascript
const numbers = [1, 2, 3, 4, 5];

// filter 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출
// 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환
// numbers 배열에서 홀수인 요소만 필터링한다.(1은 true로 평가됨)
const odds = numbers.filter(item => item % 2);

// map 메서드는 새로운 배열을 반환
console.log(odds); // [1, 3, 5]

// filter 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달
[1, 2, 3].filter((item, index, arr) => {
    console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
    return item % 2;
});
```

* **filter 메서드가 생성하여 반환한 새로운 배열의 length 프로퍼티 값은 filter 메서드를 호출한 배열의 length 프로퍼티 값과 같거나 작다.**

* filter 메서드의 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있음
    * 더 나은 방법 : 화살표 함수를 사용
        * 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음

```javascript
class Users {
    constructor() {
        this.users = [
            { id: 1, name: 'Park' },
            { id: 2, name: 'Cansus' }
        ];
    }
    // 요소 추출
    findById(id) {
        // id가 일치하는 사용자만 반환
        return this.users.filter(user => user.id === id);
    }
    // 요소 제거
    remove(id) {
        // id가 일치하지 않는 사용자를 제거
        this.users = this.users.filter(user => user.id !== id);
    }
}
const users = new Users();
let user = users.findById(1);
console.log(user); // [{ id: 1, name: 'Park' }]
// id가 1인 사용자를 제거
users.remove(1);

user = users.findById(1);
console.log(user); // []
```

* filter 메서드를 사용해 특정 요소를 제거할 경우 특정 요소가 중복되어 있다면 중복된 요소가 모두 제거된다.
* 특정 요소를 하나만 제거하려면 indexOf 메서드와 splice 메서드를 사용한다.

### 5. Array.prototype.reduce
* 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
* 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 **하나의 결과값을 만들어 반환**
* 원본 배열은 변경되지 않음
* reduce 메서드는 -> 첫 번째 인수로 콜백 함수, 두 번째 인수로 초기값을 전달 받음
* reduce 메서드의 콜백 함수 -> 4개 인수 (초기값 또는 콜백 함수의 이전 반환값, 요소값, 인덱스, reduce 메서드를 호출한 배열 자체(this))가 전달됨
```javascript
// 1부터 4까지 누적을 구함
const sum = [1, 2, 3, 4].reduce((accumulator, currentValue, index, array) => accumulator + currentValue, 0);
console.log(sum); // 10
```
* 위 예제에서 reduce 메서드의 콜백 함수는 4개의 인수를 전달받아 배열의 length만큼 총 4회 호출된다.
* 이때 콜백 함수로 전달되는 인수와 콜백 함수의 반환값은 다음과 같다.

| 구분 | accumulator | currentValue | index | array     | 콜백 함수의 반환값                   |
|------|-------------|--------------|-------|-----------|--------------------------------|
| 1 round    | 0           | 1            | 0     | [1,2,3,4] | 1 (accumulator+currentValue)   |
| 2 round    | 1           | 2            | 1     | [1,2,3,4] | 3 (accumulator+currentValue)   |
| 3 round    | 3           | 3            | 2     | [1,2,3,4] | 6 (accumulator+currentValue)   |
| 4 round    | 6           | 4            | 3     | [1,2,3,4] | 10 (accumulator+ currentValue) |

* reduce 메서드를 이용한 다양한 예제
    * [평균 구하기](./src/reduce_ex1.html)
    * [최대값 구하기](./src/reduce_ex2.html)
        * Math.max 메서드 사용이 더 직관적임
    * [요소의 중복 횟수 구하기](./src/reduce_ex3.html)
    * [중첩 배열 평탄화](./src/reduce_ex4.html)
        * ES10 도입된 Array.prototype.flat 메서드 사용이 더 직관적임
    * [중복 요소 제거](./src/reduce_ex5.html)
        * filter 메서드를 사용하는 방법이 더 직관적임
        * 중복되지 않는 유일한 값들의 집합인 `Set` 을 사용할 수 있음 (권장 및 추천) 

* reduce 메서드의 두 번째 인수로 전달하는 초기값은 생략할 수 있음
    * **초기값을 전달하는 것이 안전하다.**

```javascript
// (1) 초기값 생략의 경우
// 빈 배열로 reduce 메서드 호출 시 에러가 발생함
const sum = [].reduce((acc, cur) => acc + cur);
// TypeError: Reduce of empty array with no initial value

// 객체의 특정 프로퍼티 값 합산
const products = [
    { id: 1, price: 100 },
    { id: 2, price: 200 },
    { id: 3, price: 300 }
];
// 1round : acc는 { id: 1, price: 100 }, cur은 { id: 2, price: 200 }
// 2round : acc는 300, cur은 { id: 2, price: 200 }
// 2round 에서 acc에 함수에 객체가 아닌 숫자값이 전달됨. 이때 acc.price는 undefined 다.
const priceSum = products.reduce((acc, cur) => acc.price + cur.price);
console.log(priceSum); // NaN

// (2) 초기값 전달 경우
// 빈 배열로 reduce 메서드 호출 시 정상 동작
const sum = [].reduce((acc, cur) => acc + cur);
console.log(sum); // 0

// 객체의 특정 프로퍼티 값 한산 (초기값 전달 필수)
const products = [
    { id: 1, price: 100 },
    { id: 2, price: 200 },
    { id: 3, price: 300 }
];
// 1round : acc는 0, cur은 { id: 1, price: 100 }
// 2round : acc는 100, cur은 { id: 2, price: 200 }
// 3round : acc는 300, cur은 { id: 3, price: 300 }
// 
const priceSum = products.reduce((acc, cur) => acc.price + cur.price, 0);
console.log(priceSum); // 600
```

### 6. Array.prototype.some
* 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
* 콜백 함수의 반환값이 단 한 번이라도 참이면 true, 모두 거짓이면 false 반환
* 주의 : 빈 배열인 경우 false 반환
* some 메서드를 호출한 배열의 요소값과 인덱스, some 메서드를 호출한 배열(this)을 순차적으로 전달한다.
* some 메서드의 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있음
    * 더 나은 방법 : 화살표 함수를 사용
        * 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음

```javascript
// 배열의 요소 중 10보다 큰 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some(item => item > 10); // true
// 배열의 요소 중 0보다 작은 요소가 1개 이상 존재하는지 확인
[5, 10, 15].some(item => item > 0); // false
// 배열의 요소 중 'banana'가 1개 이상 존재하는지 확인
['apple', 'banana', 'mango'].some(item => item === 'banana'); // true
// some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환
[].some(item => item > 3); // false
```

### 7. Array.prototype.every
* 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다.
* 콜백 함수의 반환값이 모두 참이면 true, 단 한 번이라도 거짓이면 false 반환
* 주의 : 빈 배열인 경우 true 반환
* every 메서드를 호출한 배열의 요소값과 인덱스, every 메서드를 호출한 배열(this)을 순차적으로 전달한다.
* every 메서드의 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있음
    * 더 나은 방법 : 화살표 함수를 사용
        * 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음

```javascript
// 배열의 모든 요소가 3보다 큰지 확인
[5, 10, 15].every(item => item > 3); // true
// 배열의 모든 요소가 10보다 큰지 확인
[5, 10, 15].every(item => item > 10); // false
// every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true 반환
[].every(item => item > 3); // true
```

### 8. Array.prototype.find
* ES6 에서 도입된 메서드
* 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소를 반환
* 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 undefined를 반환
* find 메서드를 호출한 배열의 요소값과 인덱스, find 메서드를 호출한 배열(this)을 순차적으로 전달한다.
* find 메서드의 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있음
    * 더 나은 방법 : 화살표 함수를 사용
        * 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음

```javascript
const users = [
    { id: 1, name: 'Park' },
    { id: 2, name: 'Kim' },
    { id: 2, name: 'Lee' },
    { id: 3, name: 'Cansus' }
];
// id가 2인 첫 번째 요소를 반환한다. find 메서드는 배열이 아니라 요소를 반환한다.
users.find(user => user.id === 2); // { id: 2, name: 'Kim' }
```

### 9. Array.prototype.findIndex
* ES6 에서 도입된 메서드
* 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true인 첫 번째 요소의 인덱스를 반환
* 콜백 함수의 반환값이 true인 요소가 존재하지 않는다면 -1을 반환
* findIndex 메서드를 호출한 배열의 요소값과 인덱스, findIndex 메서드를 호출한 배열(this)을 순차적으로 전달한다.
* findIndex 메서드의 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있음
    * 더 나은 방법 : 화살표 함수를 사용
        * 화살표 함수는 함수 자체의 this 바인딩을 갖지 않음

```javascript
const users = [
    { id: 1, name: 'Lee' },
    { id: 2, name: 'Kim' },
    { id: 2, name: 'Choi' },
    { id: 3, name: 'Park' }
];
// id가 2인 요소의 인덱스를 구함
users.findIndex(user => user.id === 2); // 1
// name이 'Park'인 요소의 인덱스를 구함
users.findIndex(user => user.name === 'Park'); // 3

// 위와 같이 프로퍼티 키와 프로퍼티 값으로 요소의 인덱스를 구하는 경우 다음과 같이 콜백 함수를 추상화할 수 있다.
function predicate(key, value) {
    // key와 value를 기억하는 클로저를 반환
    return item => item[key] === value;
}
// id가 2인 요소의 인덱스를 구함
users.findIndex(predicate('id', 2)); // 1
// name이 'Park'인 요소의 인덱스를 구함
users.findIndex(predicate('name', 'Park')); // 3
```

### 10. Array.prototype.flatMap
* ES10 에서 도입된 메서드
* map 메서드를 통해 생성된 새로운 배열을 평탄화 한다.
* map 메서드와 flat 메서드를 순차적으로 실행하는 효과를 가짐
* flat 메서드처럼 인수를 전달하여 평탄화 깊이를 지정할 수는 없고 1단계만 평탄화 한다.
    * 깊이 지정이 필요하면 map 메서드와 flat 메서드를 각각 호출 한다.

```javascript
const arr = ['hello', 'world'];

// map과 flat을 순차적으로 실행
arr.map(x => x.split('')).flat();
// ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

// flatMap은 map을 통해 생성된 새로운 배열을 평탄화한다.
arr.flatMap(x => x.split(''));
// ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

// flatMap은 1단계만 평탄화한다.
arr.flatMap((str, index) => [index, [str, str.length]]);
// [[0, ['hello', 5]], [1, ['world', 5]]] => [0, ['hello', 5], 1, ['world', 5]]

// 평탄화 깊이를 지정해야 하면 flatMap 메서드를 사용하지 말고 map 메서드와 flat 메서드를 각각 호출한다.
arr.map((str, index) => [index, [str, str.length]]).flat(2);
// [[0, ['hello', 5]], [1, ['world', 5]]] => [0, 'hello', 5, 1, 'world', 5]
```