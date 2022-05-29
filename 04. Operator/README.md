# 연산자

연산(operation)은 연산자(operator)와 피연산자(operand)를 조합하여 이루어진다.

![연산자](https://user-images.githubusercontent.com/63139527/170853978-812bab4e-9f19-42d2-9879-9ccb91d1cc89.png)

* 연산자 : 피연산자를 연산하여 값을 만듬

* 피연산자 : 연산자를 통해 연산될 대상, 값

### 산술 연산자
* +, - , * , / , % , ++ , --

### 할당 연산자
* = , += , -= , *= , /= , %=

### 비교 연산자
* == , === , != , !== , > , < , >= , <=

### 삼항 조건 연산자
* (조건식) ? (조건이 참[ture]일 때 반환 값) : (조건식이 거짓[false]일 때 반환 값)
```javascript
var res = score >= 80 ? 'congratulation' : 'cheer up';
```
### 논리 연산자
* || , && , !

### 쉼표 연산자
* ,
```javascript
var a, b, c;
a = 1, b = 2, c = 3;
```

### 그룹 연산자
* ( )
```javascript
var res = 2 * (2 + 3); // 10
```

### typeof 연산자
* typeof
```javascript
typeof ''               // string
typeof 4                // number
typeof NaN              // number
typeof false            // boolean
typeof undefined        // undefined
typeof Symbol()         // symbol
typeof null             // object
typeof []               // object
typeof {}               // object
typeof new Date()       // object
typeof function() {}    // function
```

### 지수 연산자
- **
```javascript
// 지수 연산자 ** 는 ES7부터 지원
var res = 2 ** 3; // 8
// 지수 연산은 ES7 이전에는 Math.pow 함수를 사용
var res = Math.pow(2, 3); // 8

var res = 2 ** 2 ** 2;                 // 16
var res = Math.pow(Math.pow(2, 2), 2); // 16

// 음수의 경우 밑수를 반드시 그룹 연사자를 이용해 묶어주어야 한다.
// 안그러면 에러가 발생한다.
var res = -3 ** 2;      // 에러 발생
var res = (-3) ** 2;    // 9
```

### 그외의 연산자
- 이후에 정리해본다.
