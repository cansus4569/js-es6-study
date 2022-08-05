# Number
* 표준 빌트인 객체
* 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드 제공

## Number 생성자 함수
* Number 객체는 생성자 함수 객체 -> new 연산자와 함께 호출하여 Number 인스턴스 생성 가능
* Number 생성자 함수에 인수를 전달하지 않고 new 연산자 호출시 [ [ NumberData ] ] 내부 슬롯에 0 할당한 Number 래퍼 객체 생성
* 인수를 숫자가 아닌 값을 전달하면 숫자로 강제 변환 후 내부 슬롯에 할당한 Number 래퍼 객체를 생성
    * 인수를 숫자로 변환할 수 없으면 NaN을 할당한 Number 래퍼 객체를 생성
```javascript
let numObj = new Number(); // 인수 : X
console.log(numObj); // Number {[[PrimitiveValue]]: 0}

numObj = new Number(10); // 인수 : 숫자
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

numObj = new Number('10'); // 인수 : 숫자형태의 문자
console.log(numObj); // Number {[[PrimitiveValue]]: 10}

numObj = new Number('Hello'); // 인수 : 문자
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
```

* new 연사자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환
    * 이러한 방식을 이용하여 명시적으로 타입 변환

```javascript
// 문자열 타입 -> 숫자 타입
Number('0'); // 0
Number('-1'); // -1
Number('10.53'); // 10.53
// 불리언 타입 -> 숫자 타입
Number(true) // 1
Number(false) // 0
```

## Number 프로퍼티 
### 1. Number.EPSILON
* ES6 도입된 프로퍼티
* 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.
```Number.EPSILON = 2.2204460492503130808472633361816 * 10^-16 = 2.220446049250313e-16```
* 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다.
    * 정수는 2진법으로 오차 없이 저장 가능
    * 부동소수점을 표현하기 위해 가장 널리 쓰이는 표준인 IEEE 754는 2진법으로 변환했을 때 무한소수가 되어 미세한 오차가 발생할 수밖에 없는 구조적 한계

```javascript
console.log(0.1+0.2); // 0.30000000000000004
console.log(0.1+0.2 === 0.3); // false
```

* Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용
```javascript
function isEqual(a, b) {
    // a와 b를 뺀 값의 절대값이 Number.EPSILON 보다 작으면 같은 수로 인정한다.
    return Math.abs(a - b) < Number.EPSILON;
}
isEqual(0.1 + 0.2, 0.3); // true
```

### 2. Number.MAX_VALUE
* 자바스크립트에서 표현할 수 있는 가장 큰 양수 값
```Number.MAX_VALUE = 1.7976931348623157 * 10^308 = 1.7976931348623157e+308```
* Number.MAX_VALUE 보다 큰 숫자는 `Infinity` 이다.
```javascript
Infinity > Number.MAX_VALUE; // true
```

### 3. Number.MIN_VALUE
* 자바스크립트에서 표현할 수 있는 가장 작은 양수 값
```Number.MIN_VALUE = 5 * 10^-324 = 5e-324```
* Number.MIN_VALUE 보다 작은 숫자는 `0` 이다.
```javascript
Number.MIN_VALUE > 0; // true
```

### 4. Number.MAX_SAFE_INTEGER
* 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값
```Number.MAX_SAFE_INTEGER = 9007199254740991```

### 5. Number.MIN_SAFE_INTEGER
* 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값
```Number.MIN_SAFE_INTEGER = -9007199254740991```

### 6. Number.POSITIVE_INFINITY
* 양의 무한대를 나타내는 숫자값
```Number.POSITIVE_INFINITY = Infinity```

### 7. Number.NEGATIVE_INFINITY
* 음의 무한대를 나타내는 숫자값
```Number.NEGATIVE_INFINITY = -Infinity```

### 8. Number.NaN
* 숫자가 아님(Not-a-Number)을 나타내는 숫자값
    * window.NaN과 같다.

```Number.NaN = NaN```

## Number 메서드
### 1. Number.isFinite
* ES6 도입된 정적 메서드
* 인수로 전달된 숫자값이 정상적인 유한수(Infinity 또는 -Infinity)가 아닌지 검사하여 결과를 불리언 값으로 반환
* 인수가 NaN 이면 false를 반환
```javascript
// 인수가 정상적인 유한수 -> true 반환
Number.isFinite(0);                 // true
Number.isFinite(Number.MAX_VALUE);  // true
Number.isFinite(Number.MIN_VALUE);  // true

// 인수가 무한수이면 false 반환
Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false

Number.isFinite(NaN);; // false
```

* `Number.isFinite 메서드`와 `빌트인 전역 함수 isFinite` 차이
    * 빌트인 전역 함수 isFinite : 인수를 숫자로 암묵적으로 타입 변환하여 검사를 수행
    * Number.isFinite : 인수를 숫자로 암묵적 타입 변환하지 않음
        * 숫자가 아닌 인수는 무조건 false 반환

```javascript
// Number.isFinite : 인수를 숫자로 암묵적 타입 변환하지 않음
Number.isFinite(null); // false
// isFinite는 인수를 숫자로 암묵적으로 타입 변환. null은 0으로 암묵적 타입 변환함
isFinite(null); // true
```

### 2. Number.isInteger
* ES6 도입된 정적 메서드
* 인수로 전달된 숫자값이 정수인지 검사하여 결과를 불리언 값으로 반환
* 인수를 숫자로 암묵적 타입 변환하지 않는다.
```javascript
// 인수가 정수이면 true 반환
Number.isInteger(0)     // true
Number.isInteger(123)   // true
Number.isInteger(-123)  // true
// 0.5는 정수가 아님
Number.isInteger(0.5)   // false
// '123'을 숫자로 암묵적 타입 변환하지 않음
Number.isInteger('123') // false
// flase를 숫자로 암묵적 타입 변환하지 않음
Number.isInteger(false) // false
// Infinity/-Infinity는 정수가 아님
Number.isInteger(Infinity)  // false
Number.isInteger(-Infinity) // false
```

### 3. Number.isNaN
* ES6 도입된 정적 메서드
* 인수로 전달된 숫자값이 NaN인지 검사하여 결과를 불리언값으로 반환
```javascript
// 인수가 NaN이면 true 반환
Number.isNaN(NaN); // true
```

* `Number.isNaN 메서드`와 `빌트인 전역 함수 isNaN` 차이
    * 빌트인 전역 함수 isNaN : 인수를 숫자로 암묵적으로 타입 변환하여 검사를 수행
    * Number.isNaN : 인수를 숫자로 암묵적 타입 변환하지 않음
        * 숫자가 아닌 인수는 무조건 false 반환

```javascript
// Number.isNaN : 인수를 숫자로 암묵적 타입 변환하지 않음
Number.isNaN(undefined); // false
// isNaN 인수를 숫자로 암묵적으로 타입 변환. undefined는 NaN으로 암묵적 타입 변환함
isNaN(undefined); // true
```

### 4. Number.isSafeInteger
* ES6 도입된 정적 메서드
* 인수로 전달된 숫자값이 안전한 정수인지 검사하여 결과를 불리언값으로 반환
    * 안전한 정수 범위 : -(2^53-1) ~ 2^53-1
* 인수를 숫자로 암묵적 타입 변환하지 않는다.
```javascript
Number.isSafeInteger(0);                // true
Number.isSafeInteger(Math.pow(2,53)-1); // true
Number.isSafeInteger(Math.pow(2,53));   // false
Number.isSafeInteger(0.5);              // false
Number.isSafeInteger('123');            // false
Number.isSafeInteger(false);            // false
Number.isSafeInteger(Infinity);         // false
```

### 5. Number.prototype.toExponential
* 숫자를 지수 표기법으로 변환하여 문자열로 반환
    * 지수 표기법?
        * 매우 크거나 작은 숫자를 표기할 때 주로 사용
        * e(Exponent) 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식
* 인수로 소수점 이하로 표현할 자릿수를 전달
* 참고
    * . 기호의 의미 : 소수 구분 or 프로퍼티 접근 연산자
    * 자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석함
    * 숫자에 소수점은 하나만 존재하므로 두 번째 .은 프로퍼티 접근 연산자로 해석함
    * (권장) 그룹 연산자를 사용할 것
    * 자바스크립트 숫자는 정수 부분과 소수 부분 사이에 공백을 포함할 수 없다.
        * 숫자 뒤의 . 앞에 공백이 오면 .을 프로퍼티 접근 연산자로 해석함

```javascript
// 인수로 소수점 이하로 표현할 자릿수를 전달
(77.1234).toExponential();  // "7.71234e+1"
(77.1234).toExponential(4); // "7.7123e+1"
(77.1234).toExponential(2); // "7.71e+1"
// 자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석함
77.toExponential(); // SyntaxError: Invalid or unexpected token
// 숫자에 소수점은 하나만 존재하므로 두 번째 .은 프로퍼티 접근 연산자로 해석함
77.1234.toExponential(); // "7.71234e+1"
// (권장) 그룹 연산자를 사용할 것
(77).toExponential(); // "7.7e+1"
// 숫자 뒤의 . 앞에 공백이 오면 .을 프로퍼티 접근 연산자로 해석함
77 .toExponential(); // "7.7e+1"
```

### 6. Number.prototype.toFixed
* 숫자를 반올림하여 문자열로 반환
* 반올림하는 소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달 가능
* 인수를 생략하면 기본값 0이 지정됨
```javascript
// 소수점 이하 반올림. 인수를 생략하면 기본값 0 지정
(12345.6789).toFixed(); // "123456"
// 소수점 이하 1자릿수 유효, 나머지 반올림
(12345.6789).toFixed(1); // "12345.7"
// 소수점 이하 2자릿수 유효, 나머지 반올림
(12345.6789).toFixed(2); // "12345.68"
// 소수점 이하 3자릿수 유효, 나머지 반올림
(12345.6789).toFixed(3); // "12345.679"
```

### 7. Number.prototype.toPrecision
* 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환
* 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환
* 전체 자릿수를 나타내는 0~21 사이의 정수값을 인수로 전달 가능
* 인수를 생략하면 기본값 0 지정
```javascript
// 전체 자릿수 유효. 인수를 생략하면 기본값 0 지정
(12345.6789).toPrecision();     // "12345.6789"
// 전체 1자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(1);     // "1e+4"
// 전체 2자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(2);     // "1.2e+4"
// 전체 6자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(6);     // "12345.7"
```

### 8. Number.prototype.toString
* 숫자를 문자열로 변환하여 반환
* 진법을 나타내는 2~36 사이의 정수값을 인수로 전달 가능
* 인수를 생략하면 기본값 10진법 지정
```javascript
// 인수 생략하면 10진수 문자열 반환
(10).toString(); // "10"
// 2진수 문자열 반환
(16).toString(2); // "10000"
// 8진수 문자열 반환
(16).toString(8); // "20"
// 16진수 문자열 반환
(16).toString(16); // "10"
```