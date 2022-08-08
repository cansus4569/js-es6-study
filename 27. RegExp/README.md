# RegExp (Regular Expression)

## 정규 표현식이란?
* 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어 (formal language)
* 자바스크립트의 고유 문법이 아니며, 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있음
* 자바스크립트는 펄(Perl)의 정규 표현식 문법을 ES3부터 도입

* 정규 표현식은 문자열을 대상으로 **패턴 매칭 기능**을 제공한다.
    * 패턴 매칭 기능 : 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능

```javascript
// 사용자로부터 입력받은 휴대폰 번호
const tel = '010-1234-567팔'

// 정규 표현식 리터럴로 휴대폰 전화번호 패턴을 정의
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴에 미챙하는지 테스트
regExp.test(tel); // false
```

* 정규 표현식을 사용하지 않는다면 반복문과 조건문을 통해 '첫 번째 문자가 숫자이고 이어지는 문자도 숫자이고 어이지는 문자도 숫자이고 다음 '-' 이고...' 와 같이 한 문자씩 연속해서 체크해야 한다.
* 정규 표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다.
* 다만 정규 표현식은 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않음

## 정규 표현식의 생성
* 정규 표현식 리터럴과 RegExp 생성자 함수를 사용하여 생성 가능
1. 일반적인 방법 : 정규 표현식 리터럴
![regexp drawio](https://user-images.githubusercontent.com/63139527/183340915-8b3c15a8-86f5-431d-bc75-33382c03d6b3.png)
```javascript
const target = 'Is this all there is?';

// 패턴: is
// 플래그: i => 대소문자를 구별하지 않고 검색
const regexp = /is/i;

// test 메서드는 target 문자열에 대해 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환
regexp.test(target); // true
```

2. RegExp 생성자 함수를 사용하여 RegExp 객체 생성
```javascript
/**
 * @param pattern 정규 표현식의 패턴
 * @param flags 정규 표현식의 플래그(g, i, m, u, y)
 */
new RegExp(pattern[, flags])

const target = 'Is this all there is?';

const regexp = new RegExp(/is/i); // ES6
// const regexp = new RegExp(/is/, 'i');
// const regexp = new RegExp('is', 'i');

regexp.test(target); // true
```

## RegExp 메서드
### 1. RegExp.prototype.exec
* 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환
* 매칭 결과가 없는 경우 `null` 반환
* 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 **첫 번째 매칭 결과만 반환**하므로 주의!
```javascript
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

### 2. RegExp.prototype.test
* 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환
```javascript
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target); // true
```

### 3. String.prototype.match
* String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환
* String.prototype.match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환
```javascript
const target = 'Is this all there is?';
const regExp1 = /is/;

target.match(regExp1);
// ["is", index: 5, input: "Is this all there is?", groups: undefined]

const regExp2 = /is/g;
target.match(regExp2); // ["is", "is"]
```

## 플래그
* 정규 표현식의 검색 방식을 설정하기 위해 사용
* 플래그는 총 6개가 있으며, 중요한 3개 살펴보자

플래그|의미|설명
-|-|-
i|Ignore case|대소문자를 구별하지 않고 패턴을 검색한다.
g|Global|대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.
m|Multi line|문자열의 행이 바뀌더라도 패턴 검색을 계속한다.

* 플래그는 옵션 값이므로 선택적으로 사용 가능
* 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수 있음
* 플래그를 사용하지 않은 경우 
    * 대소문자를 구별해서 패턴을 검색한다.
    * 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

```javascript
const target = 'Is this all there is?';

// target 문자열에서 is 문자열을 대소문자를 구별하여 한 번만 검색
target.match(/is/);

// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 한 번만 검색
target.match(/is/i);

// target 문자열에서 is 문자열을 대소문자를 구별하여 전역 검색
target.match(/is/g); // ["is", "is"]
// target 문자열에서 is 문자열을 대소문자를 구별하지 않고 전역 검색
target.match(/is/ig); // ["Is", "is", "is"]
```

## 패턴
* 정규 표현식은 패턴과 플래그로 구성
    * 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용
    * 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용
* 패턴은 `/` 로 열고 닫으며 문자열의 따옴표는 생략
    * 따옴표를 포함하면 따옴표까지도 패턴에 포함되어 검색됨
* 패턴은 특별한 의미를 가지는 메타문자 또는 기호로 표현할 수 있다.
* 어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 '정규 표현식과 매치한다'고 표현한다.

### 1. 문자열 검색
* 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.

### 2. 임의의 문자열 검색
* `.`은 임의의 문자 한 개를 의미한다.
* 문자의 내용은 무엇이든 상관 없음
* `.`을 3개 연속하여 패턴을 생성하면 문자의 내용과 상관없이 3자리 문자열과 매치한다.
```javascript
const target = 'Is this all there is?';

// 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
const regExp = /.../g;

target.match(regExp); // ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

### 3. 반복 검색
* `{m,n}` 은 앞선 패턴이 최소 m번, 최대n번 반복되는 문자열을 의미
    * 콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의!
* `{n}` 은 앞선 패턴이 n번 반복되는 문자열을 의미
    * {n,n} 과 같다
* `{n,}` 은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미
```javascript
const target = 'A AA B BB Aa Bb AAA';

// 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색
let regExp = /A{1,2}/g;
target.match(regExp); // ["A", "AA", "A", "AA", "A"]
// 'A'가 2번 반복되는 문자열을 전역 검색
regExp = /A{2}/g;
target.match(regExp); // ["AA", "AA"]
// 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색
regExp = /A{2,}/g;
target.match(regExp); // ["AA", "AAA"]
```

* `+` 는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다.
    * {1,} 과 같다

```javascript
const target = 'A AA B BB Aa Bb AAA';
// 'A' 가 최소 한 번 이상 반복되는 문자열('A', 'AA', 'AAA', ...)을 전역 검색
const regExp = /A+/g;

target.match(regExp); // ["A", "AA", "A", "AAA"]
```

* `?` 는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다.
    * {0, 1} 과 같다.

```javascript
const target = 'color colour';
// 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는
// 문자열 'color', 'colour' 를 전역 검색
const regExp = /colou?r/g;
target.match(regExp); // ["color", "colour"]
```

### 4. OR 검색
* `|` 는 or의 의미를 가짐
    * /A|B/ 는 'A' 또는 'B' 를 의미

```javascript
const target = "A AA B BB Aa Bb";
// 'A' 또는 'B' 를 전역 검색
const regExp = /A|B/g;
target.match(regExp); // ["A", "A", "A", "B", "B", "B", "A", "B"]
```
* 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용
```javascript
const target = "A AA B BB Aa Bb";
// 'A' 또는 'B' 가 한 번 이상 반복되는 문자열을 전역 검색
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /A+|B+/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```
* 위의 예제를 더 간단하게 표현하는 방식
* `[]` 내의 문자는 or로 동작한다.
* 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.
```javascript
const target = "A AA B BB Aa Bb";
// 'A' 또는 'B' 가 한 번 이상 반복되는 문자열을 전역 검색
// 'A', 'AA', 'AAA', ... 또는 'B', 'BB', 'BBB', ...
const regExp = /[AB]+/g;
target.match(regExp); // ["A", "AA", "B", "BB", "A", "B"]
```
* 범위를 지정하려면 [ ]내에 `-` 를 사용한다
```javascript
const target = 'A AA BB ZZ Aa Bb';
// 'A' ~ 'Z'가 한 번 이상 반복되는 문자열을 전역 검색
const regExp = /[A-Z]+/g; // 대문자 알파벳 검색
target.match(regExp); // ["A", "AA", "BB", "ZZ", "A", "B"]

const regExp = /[A-Za-z]+/g; // 대소문자 굴별하지 않고 알파벳 검색
const regExp = /[0-9]+/g; // 숫자 검색
```
* 위의 예제를 간단히 표현하면 다음과 같다.
* `\d`는 숫자를 의미 한다.
    * [0-9] 와 같다.
* `\D` 는 \d와 반대로 동작한다.
    * 숫자가 아닌 문자를 의미
```javascript
const target = 'AA BB 12,345';
// '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
let regExp = /[\d,]+/g;
target.match(regExp); // ["12,345"]

// '0' ~ '9' 가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
regExp = /[\D,]+/g;
target.match(regExp); // ["AA BB ", ","]
```
* `\w` 는 알파벳, 숫자, 언더스코어를 의미한다.
    * [A-Za-z0-9_] 와 같다.
* `\W` 는 \w와 반대로 동작한다.
    * 알파벳, 숫자, 언더스코어가 아닌 문자를 의미
```javascript
const target = 'Aa Bb 12,345 _$%&';
// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색
let regExp = /[\w,]+/g;
target.match(regExp); // ["Aa", "Bb", "12,345", "_"]

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
regExp = /[\W,]+/g;
target.match(regExp); // [" ", " ", ",", " $%&"]
```

### 5. NOT 검색
* **[...] 내**의 `^`은 not의 의미를 갖는다.
    * [^0-9] 는 숫자를 제외한 문자를 의미
    * \D 는 [^0-9] 와 같다.
    * \W 는 [^A-Za-z0-9_] 와 같다.
```javascript
const target = 'AA BB 12 Aa Bb';
// 숫자를 제외한 문자열을 전역 검색
const regExp = /[^0-9]+/g;
target.match(regExp); // ["AA BB Aa Bb"]
```

### 6. 시작 위치로 검색
* **[...] 밖**의 `^`은 문자열의 시작을 의미한다.
```javascript
const target = 'https://poiemaweb.com';
// 'https' 로 시작하는지 검사
const regExp = /^https/;
regExp.test(target); // true
```

### 7. 마지막 위치로 검색
* `$` 는 문자열의 마지막을 의미한다.
```javascript
cosnt target = 'https://poiemaweb.com';
// 'com' 으로 끝나는지 검사한다.
const regExp = /com$/;
regExp.test(target); // true
```

## 자주 사용하는 정규 표현식

### 1. 특정 단어로 시작하는지 검사
* [예제](./src/ex1.html)
### 2. 특정 단어로 끝나는지 검사
* [예제](./src/ex2.html)
### 3. 숫자로만 이루어진 문자열인지 검사
* [예제](./src/ex3.html)
### 4. 하나 이상의 공백으로 시작하는지 검사
* [예제](./src/ex4.html)
### 5. 아이디로 사용 가능한지 검사
* [예제](./src/ex5.html)
### 6. 메일 주소 형식에 맞는지 검사
* [예제](./src/ex6.html)
### 7. 핸드폰 번호 형식에 맞는지 검사
* [예제](./src/ex7.html)
### 8. 특수 문자 포함 여부 검사
* [예제](./src/ex8.html)