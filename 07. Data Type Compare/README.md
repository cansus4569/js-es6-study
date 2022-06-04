# 데이터 타입 비교
자바스크립트에서 [데이터 타입](../03.%20Data%20Type/README.md)(표 참조)들이 있음

* 원시 타입 값
    * 변경이 불가능한 값
    * 메모리 공간에 실제 값이 저장
        * (`생각` : C언어의 일반적인 변수 값 할당)
    * 복사시 원시 값이 전달됨
* 객체 타입 값
    * 변경이 가능한 값
    * 메모리 공간에 참조 값이 저장
        * (`생각` : C언어의 포인터 개념과 비슷한 느낌인듯?)
    * 복사시 참조 값이 전달됨

## 원시 타입 값
1. 변경이 불가능한 값 (불변성)
    * 아래 그림과 같이 변수 선언 시, 값이 특정 메모리 주소의 공간에 할당되지만
    * 값 할당 시, 기존 메모리 주소의 값이 변경되는 것이 아닌
    * 새로운 메모리 주소의 공간에 값을 할당하며
    * 변수는 새로운 메모리 주소를 가리킨다.
    * **주의 : 변수값 변경과 원시값 변경 개념 혼돈되지 말것!**

![primitive_data_store](https://user-images.githubusercontent.com/63139527/171351774-33f91601-d62c-4681-a3da-5272e322c67d.png)

2. 문자열
* 문자열도 원시 값이기에 변경이 불가능 하다.
```javascript
var text = 'hello';
text[0] = 'H';
console.log(text); // hello
```

### 원시 값 복사 (원시 값 전달)
* 원시 값을 전달(즉, 복사)했기에, 원본 값이 바뀐다고 복사값이 바뀌지 않는다.
```javascript
var num = 5;    // 원본
var copy = num; // 복사

console.log(num); // 5
console.log(copy); // 5

num += 5;   // 원본 수정

console.log(num); // 10 (수정된 원본)
console.log(copy); // 5
```

## 객체 타입 값
1. 변경이 가능한 값
    * 변수에 객체 할당 시, 특정 메모리 주소의 공간에 **참조 값**을 저장한다.
    * 참조 값으로 부터 실제 객체 데이터에 접근한다.

```javascript
// 객체 선언
var company = {
    name : 'abc'
};

// 프로퍼티 값 갱신
company.name = 'xyz';
// 프로퍼티 동적 생성
company.tel = '02-123-456';
```

![object_data_store](https://user-images.githubusercontent.com/63139527/171374813-d0f41215-d0f0-4ea0-8249-3a8e014951fa.png)

```
< 객체 타입의 값이 변경이 가능한 값으로 설계된 이유 >
 - 원시 타입의 값의 크기(메모리 할당 크키)는 어느정도 정해져 있다.
    => 숫타(8byte), 문자(2byte), 문자열(2byte * 문자열 길이)
 - 객체 타입의 크기는 크기를 갸늠할 수 없음
    => 객체 안의 프로퍼티  값이 객체일 수도 있음

 결론 : 메모리를 효율적으로 사용하기 위해 변경이 가능한 값(참조 값) 형태로 설계됨
```

### 얕은 복사 & 깊은 복사 개념
* 얕은 복사
    * 객체를 할당한 변수를 다른 변수에 할당
    ```javascript
    const a = { x: 1 }; // 객체 할당
    const b = a;
    console.log(a === b); // true
    ```

* 깊은 복사
    * 원시 값을 할당한 변수를 다른 변수에 할당
    ```javascript
    const a = 1; // 원시 값 할당
    const b = a;
    console.log(a === b); // true
    ```

### 객체 값 복사 (참조 값 전달)
* 참조 값을 전달하기에 원본과 복사된 변수는 동일한 특정 메모리 주소의 값을 바라본다.
```javascript
// 객체 선언
var company = {
    name : 'abc'
};
// 참조 값 복사 (얕은 복사)
var copy = company;

// 프로퍼티 갱신
company.name = 'xyz';
// 프로퍼티 동적 할당
copy.tel = '02-123-456';
```
![object_copy](https://user-images.githubusercontent.com/63139527/171387480-8a9f8256-ac79-41c6-9b5a-978089301894.png)