# String
* 표준 빌트인 객체 String
* 문자열을 다룰 때 유용한 프로퍼티와 메서드를 제공

## String 생성자 함수
* new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있음
* 인수를 전달하지 않고 new 연산자와 함께 호출하면 [ [ StringData ] ] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체 생성
* 인수로 문자열을 전달하여 new 연산자와 함께 호출하면 [ [ StringData ] ] 내부 슬롯에 전달받은 문자열을 할당한 String 래퍼 객체 생성
```javascript
const strObj = new String();
console.log(strObj); // String { length: 0, [[PrimitiveValue]]: ""} 
// [[PrimitiveValue]] 접근이 불가한 프로퍼티는 [[StringData]] 내부 슬롯을 가리킨다.

const strObject = new String("Kim");
console.log(strObject);
// String { 0: "K", 1: "i", 2: "m", length: 3, [[PrimitiveValue]]: "Kim" }
```
* String 래퍼 객체는 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 **유사 배열 객체이면서 이터러블**이다.
* 문자열은 원시 값이므로 변경할 수 없다. 에러가 발생하지 않음
* 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성
* new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 문자열을 반환한다.
    * 이를 이용하여 명시적으로 타입을 변환하기도 한다.

```javascript
const strObj = new String("Kim");
// 유사 배열 객체이면서 이터러블
console.log(strObj[0]); // K
// 원시값 변경 불가능 (에러 발생 X)
strObj[0] = 'S';
console.log(strObj); // 'Kim'
// 인수를 문자가 아닌 다른 타입
let test = new String(123);
console.log(test); // String {0: "1", 1: "2", 2: "3", length: 3, [[PrimitiveValue]]: "123"}
test = new String(null);
console.log(test); // String {0: "n", 1: "u", 2: "l", 3: "l", length: 4, [[PrimitiveValue]]: "null"}
// new 연산자 없이 사용
// 숫자 타입 => 문자열 타입
String(1); // "1"
String(NaN); // "NaN"
String(Infinity); // "Infinity"
// 불리언 타입 => 문자열 타입
String(true); // "true"
String(false); // "false"
```
## length 프로퍼티
* length 프로퍼티는 문자열의 문자 개수를 반환
```javascript
'Hello'.length; // 5
```
## String 메서드
* String 객체에는 원본 String 래퍼 객체(String 메서드를 호출한 String 래퍼 객체)를 **직접 변경하는 메서드는 존재하지 않음**
* **String 객체의 메서드는 언제나 새로운 문자열을 반환**
* 문자열은 변경 불가능한 원시 값이기 때문에 **String 래퍼 객체도 읽기 전용 객체로 제공**된다.
```javascript
const strObj = new String('Kim');
console.log(Object.getOwnPropertyDescriptors(strObj));
/*
String 래퍼 객체는 읽기 전용 객체. writable 프로퍼티 어트리뷰트 값이 false 
{
    '0': { value: 'K', writable: false, enumerable: true, configurable: false },
    '1': { value: 'i', writable: false, enumerable: true, configurable: false },
    '2': { value: 'm', writable: false, enumerable: true, configurable: false },
    legnth: { value: 3, writable: false, enumerable: false, configurable: false }
}
```
### 1. String.prototype.indexOf
* 대상 문자열(메서드를 호출한 문자열)에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환
* 검색에 실패하면 -1 반환
* 2번째 인수로 검색을 시작할 인덱스를 전달 가능
```javascript
const str = 'Hello World';
str.indexOf('l'); // 2
str.indexOf('or'); // 7
str.indexOf('x'); // -1

str.indexOf('l', 3); // 3
```
* 대상 문자열에 특정 문자열이 존재하는 확인할 때 유용하다.
* ES6에서 도입된 String.prototype.includes 메서드를 사용하면 가독성이 좋다.
```javascript
if(str.indexOf('Hello') !== -1) {
    // TODO: 포함되어 있는 경우 처리할 내용
}

if(str.includes('Hello')) {
    // TODO: 포함되어 있는 경우 처리할 내용
}
```
### 2. String.prototype.search
* 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환
* 검색에 실패하면 -1 반환
```javascript
const str = 'Hello World';
str.search(/o/); // 4
str.search(/x/); // -1
```
### 3. String.prototype.includes
* ES6에서 도입된 메서드
* 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 결과를 true 또는 false 반환
* 2번째 인수로 검색을 시작할 인덱스를 전달 가능
```javascript
const str = 'Hello World';

str.includes('Hello'); // true
str.includes(''); // true
str.includes('x'); // false
str.includes(); // false

str.includes('l', 3); // true
str.includes('H', 3); // false
```
### 4. String.prototype.startsWith
* ES6에서 도입된 메서드
* 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 결과를 true 또는 false 반환
* 2번째 인수로 검색을 시작할 인덱스를 전달 가능
```javascript
const str = 'Hello World';
str.startsWith('He'); // true
str.startsWith('x'); // false
```
### 5. String.prototype.endsWith
* ES6에서 도입된 메서드
* 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 결과를 true 또는 false 반환
* 2번째 인수로 검색할 문자열의 길이를 전달 가능
```javascript
const str = 'Hello World';
str.endsWith('ld'); // true
str.endsWith('x'); // false
```
### 6. String.prototype.charAt
* 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환
* 인덱스는 문자열의 범위 0 ~ (문자열 길이 -1) 사이의 정수
    * 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환
* 유사한 문자열 메서드 : String.prototype.charCodeAt 과 String.prototype.codePointAt 있다.
```javascript
const str = 'Hello';

for(let i = 0; i < length; i++) {
    console.log(str.charAt(i)); // H e l l o
}

str.charAt(5); // ''
```
### 7. String.prototype.substring
* 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환
* 두 번째 인수는 생략 가능
    * 생략 시 마지막 문자까지 부분 문자열을 반환
```javascript
const str = 'Hello World';
str.substring(1,4); // ell
str.substring(1); // 'ello world'
```
* 첫 번째 인수는 두 번째 인수보다 작은 정수이어야 정상이다.
* 다음과 같이 인수를 전달하여도 정상 동작
    * 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
    * 인수 < 0 또는 NaN인 경우 0으로 취급된다.
    * 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급된다.

```javascript
const str = 'Hello World'; // str.length == 11
// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(4, 1); // 'ell'
// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-2); // 'Hello World'
// 인수 > 문자열의 길이(str.length)인 경우 인수는 문자열의 길이(str.length)로 취급된다.
str.substring(1, 100); // 'ello World'
str.substring(20); // ''
```
* String.prototype.indexOf 메서드와 함께 사용하면 특정 문자열을 기준으로 앞뒤에 위치한 부분 문자열을 취득할 수 있다.
```javascript
const str = 'Hello World';
str.substring(0, str.indexOf(' ')); // 'Hello'
str.substring(str.indexOf(' ') + 1, str.length); // 'World'
```
### 8. String.prototype.slice
* substring 메서드와 동일하게 동작함
* 단, slice 메서드에는 인수에 음수를 전달 가능
    * 음수 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환

```javascript
const str = 'hello world';
str.substring(0, 5); // 'hello'
str.slice(0, 5); // 'hello'

str.substring(2); // 'llo world'
str.slice(2); // 'llo world'

// 인수 < 0 또는 NaN인 경우 0으로 취급
str.substring(-5); // 'hello world'
// 음수 인수 전달 가능. 뒤에서 5자리르 잘라내어 반환
str.slice(-5); // 'world'
```
### 9. String.prototype.toUpperCase & String.prototype.toLowerCase
* 대상 문자열을 모두 대문자로 변경한 문자열을 반환
* 대상 문자열을 모두 소문자로 변경한 문자열을 반환
```javascript
const str = 'Hello World!';
str.toUpperCase(); // 'HELLO WORLD!'
str.toLowerCase(); // 'hello world!'
```
### 10. String.prototype.trim
* 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환
* String.prototype.trimStart, String.prototype.trimEnd 를 사용하면 대상 문자열 앞 또는 뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환
```javascript
const str = '      foo    ';
str.trim(); // 'foo'

str.trimStart(); // 'foo    '
str.trimEnd(); // '      foo'
```
* String.prototype.replace 메서드에 정규 표현식을 인수로 전달하여 공백 문자를 제거할 수 있음
```javascript
const str = '      foo    ';
str.replace(/\s/g, ''); // 'foo'
str.replace(/^\s+/g, ''); // '      foo'
str.replace(/\s+$/g, ''); // 'foo    '
```
### 11. String.prototype.repeat
* ES6에서 도입된 메서드
* 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환
    * 인수가 0이면 빈 문자열 반환
    * 인수가 음수이면 RangeError 발생
    * 인수를 생략하면 기본값 0 설정

```javascript
const str = 'abc';
str.repeat(); // ''
str.repeat(0); // ''
str.repeat(1); // 'abc'
str.repeat(2); // 'abcabc'
str.repeat(2.5); // 'abcabc' (2.5 -> 2)
str.repeat(-1); // RangeError: Invalid count value
```
### 12. String.prototype.replace
* 대상 문자열에서 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 **치환한 문자열을 반환**
* 검색된 문자열이 여럿 존재할 경우 첫 번째로 검색된 문자열만 치환
* 특수한 교체 패턴 사용 가능
    * ex) $&는 검색된 문자열을 의미

Pattern|Inserts
:-:|-
$$|`$` 기호 삽입
$&|매치된 문자열을 삽입
$`|매치된 문자열 앞쪽까지의 문자열을 삽입
$'|매치된 문자열의 문자열을 삽입
$n|`n`이 1이상 99이하의 정수라면, 첫 번째 매개변수로 넘겨진 RegExp 객체에서 소괄호로 묶인 `n`번째의 부분 표현식으로 매치된 문자열을 삽입

```javascript
const str1 = 'Hello world';
str1.replace('world', 'Park'); // 'Hello Park'

const str2 = 'Hello Hello';
str2.replace(/hello/gi, 'Park'); // 'Park Park'

const str3 = 'Hello world world';
str3.replace('world', 'Park'); // 'Hello Park world'

const str4 = 'Hello world';
str4.replace('world', '<strong>$&</strong>'); // 'Hello <strong>world</strong>'
```
* 두 번째 인수로 치환 함수를 전달 가능
    * 첫 번째 인수로 매치한 결과를 두 번째 인수로 전달한 치환 함수의 인수로 전달하면서 호출하고 치환 함수가 반환한 결과와 매치 결과를 치환한다.

* [예제](./src/replace_example.html)
    * 카멜 케이스 < -- > 스네이크 케이스 변경

### 13. String.prototype.split
* 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 각 문자열로 이루어진 배열을 반환
    * 인수로 빈 문자열을 전달하면 각 문자를 모두 분리
    * 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환
* 두 번째 인수로 배열의 길이를 지정 가능
```javascript
const str = 'How are you doing?';
str.split(' '); // ["How", "are", "you", "doing?"]
str.split(/\s/); // ["How", "are", "you", "doing?"]
str.split('');
// ["H", "o", "w", " ", "a", "r", "e", " ", "y", "o", "u", " ", "d", "o", "i", "n", "g", "?"]
str.split(); // ["How are you doing?"]
```
* Array.prototype.reverse, Array.prototype.join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.
```javascript
function reverseString(str) {
    return str.split('').reverse().join('');
}
reverseString('Hello world!'); // '!dlrow olleH'
```