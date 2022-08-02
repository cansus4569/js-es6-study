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

### 2. Array.prototype.indexOf

### 3. Array.prototype.push

### 4. Array.prototype.pop

### 5. Array.prototype.unshift

### 6. Array.prototype.shift

### 7. Array.prototype.concat

### 8. Array.prototype.splice

### 9. Array.prototype.slice

### 10. Array.prototype.join

### 11. Array.prototype.reverse

### 12. Array.prototype.fill

### 13. Array.prototype.includes

### 14. Array.prototype.flat

## 배열 고차 함수

### 1. Array.prototype.sort

### 2. Array.prototype.forEach

### 3. Array.prototype.map

### 4. Array.prototype.filter

### 5. Array.prototype.reduce

### 6. Array.prototype.some

### 7. Array.prototype.every

### 8. Array.prototype.find

### 9. Array.prototype.findIndex

### 10. Array.prototype.flatMap