# 데이터 타입

데이터 타입은 나중에 Typescript를 고려하여 자바스크립트 언어에서 어떠한 타입이 있는지 정리한다.

< 데이터 타입 종류 >

<table>
    <tr>
        <td> 타입</td>
        <td>데이터</td>
        <td>설명</td>
    </tr>
    <tr>
        <td rowspan="6">원시타입</td>
        <td><a href="./src/number.html">숫자(number)</a></td>
        <td>정수, 실수 구분이 없음</br>ex) 1 == 1.0</td>
    </tr>
    <tr>
        <td><a href="./src/string.html">문자열(string)</a></td>
        <td>ex) abcd</td>
    </tr>
     <tr>
        <td><a href="./src/bool.html">불리언(boolean)</a></td>
        <td>참(true) / 거짓(false)</td>
    </tr>
    <tr>
        <td><a href="./src/undefined.html">undefined</a></td>
        <td>var 키워드로 선언된 변수에 자동 할당되는 값</td>
    </tr>
    <tr>
        <td><a href="./src/null.html">null</a></td>
        <td>값이 없음을 의도적으로 명시하는 값</td>
    </tr>
     <tr>
        <td><a href="./src/symbol.html">심벌(symbol)</a></td>
        <td>중복되지 않는 유일한 값</td>
    </tr>
    <tr>
        <td colspan="2"><a href="./src/object.html">객체타입</a></td>
        <td>객체, 함수, 배열 등</td>
    </tr>
</table>

# 데이터 타입이 필요한 이유
- 참조와 할당을 위한 메모리 공간의 크기 결정
- 값의 해석을 위한 필요
    - ex) 0100 0001 (2진수)
        - 숫자의 의미 : 65
        - 문자열의 의미 : 'A'

# 자바스크립트는 동적 타입의 언어이다.
대표적으로 정적 타입의 언어인 C언어와 비교해보자.

```C
int number = 1234567890; // 4byte 정수 타입
char character = 65;    // 1byte 문자/정수 타입
// 형태
데이터타입(자료형) 변수명 = 값;
```

* 이처럼 C언어의 경우 변수를 선언할 때, 데이터 타입을 미리 정의한다.
* 변수의 타입을 변경할 수 없음 (강제 변환형으로 참조는 가능)
```C
char c = 'A';
int result = 4 + (int)c; // 강제 형 변환
printf("%d", result); // 69
```

* 자바스크립트는 동적 타입의 언어이므로, 데이터 타입을 미리 정의하지는 않는다.
```javascript
// var 키워드를 통한 변수 선언
var c = 'A';
var num = 1234567890;
console.log(typeof c);      // string
console.log(typeof num);    // number
c = 1234;
console.log(typeof c);      // number
```
* C언어(정적타입)와 다르게 변수의 데이터 타입을 언제든 변경이 가능하다.
* 즉, 할당되어지는 값에 의존하여 데이터 타입이 변경된다 !
* 같은 말로 변수가 타입을 가지는 것이 아닌 값이 타입을 가진다.

* 동적 타입 언어의 장점
    * 변수의 타입이 유동적이기에 편리함
    * 값에 의해 변수의 타입이 정해지므로 유연함

* 동적 타입 언어의 단점
    * 변수에 대한 타입이 쉽게 바뀔수 있으므로 코드의 신뢰성이 떨어짐
    * 안정성이 떨어지고, 오류가 발생할 확률이 높아짐
    * 큰 프로젝트일 수록 코드 관리가 힘들어짐(복잡성)

* 이러한 이유 자바스크립트를 정적 타입으로 사용하는 **TypeScript**가 각광받고 있다.