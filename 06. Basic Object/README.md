# 객체의 개념
* 자바스크립트에서 [원시 타입](../03.%20Data%20Type/README.md)(표 참조) 데이터를 제외한 나머지는 객체(객체, 함수, 배열 등)이다.
* 다양한 타입의 데이터 값(원시값, 객체)을 하나로 묶은 구조 형태를 가짐
```javascript
var store = {
    food: 'mara-tang',      // string
    price: 8000,            // number
    soldout: false,          // boolean
    order: function() {     // function (=object)
        if(soldout)
            console.log(this.food + " is soldout!");
        else
            console.log(this.food + " : " + this.price + "₩");
    }
    // 키(key): 값(value)       ==> 프로퍼티
    // 키(key): 함수(function)  ==> 메서드(method)
};
```
* 0개 이상의 프로퍼티로 구성된 집합 (프로퍼티는 키(key)와 값(value)으로 구성)
    * 프로퍼티 : 객체의 상태를 나타내는 값
    * 메서드 : 프로퍼티를 참조하고 조작하는 함수

## 객체 생성 (객체 리터럴 방식)
* 자바스크립트는 **프로토타입 기반 객체지향** 언어
* 객체 생성 방법
    * **객체 리터럴**
    * Object 생성자 함수
    * 생성자 함수
    * Object.create 메서드
    * 클래스(ES6)
* 객체 리터럴 방식 : { } 중괄호 안에 프로퍼티 정의
    * 중괄호 뒤에 세미콜론(;)을 붙여줘야 한다.
    * NOTE : 블록문과 혼동하지 말것
* 샘플코드([링크](./src/object_literal.html))

## 프로퍼티
* 객체는 프로퍼티의 집합
* 프로퍼티는 키와 값으로 구성
```javascript
var store = {
    food: 'mara-tang',  // 키: 'food',  값: 'mara-tang'
    price: 6000,        // 키: 'price', 값: 6000
    price: 8000         // 중복시 나중에 선언된 값으로 덮어쓰여진다.
};

console.log(store.pirce); // 8000

// 프로퍼티 키 : 모든 문자열(빈 문자 포함), 심벌 값
// 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값
```

## 메서드
* 자바스크립트 함수는 일급 객체이다 -> 함수는 값으로 취급 가능 -> 프로퍼티 값으로 사용 가능
```javascript
var store = {
    food: 'mara-tang',
    price: 8000,
    soldout: false,
    order: function() {
        if(soldout)
            console.log(this.food + " is soldout!");
        else
            console.log(this.food + " : " + this.price + "₩");
    }
};

console.log(store.order()); // mara-tang : 8000₩
```

## 프로퍼티 접근
* 프로퍼티 접근 방법
    * 마침표(.)를 이용한 방법
    * 대괄호( [ ] )를 이용한 방법
```javascript
var skill = {
    lang: 'javascript'
};

// 마침표(.)를 이용한 방법
console.log(skill.lang); // javascript
// 대괄호( [ ] )를 이용한 방법
console.log(skill['lang']); // javascript
// 대괄호 안에 문자열로 안넣으면, 식별자로 인식함
console.log(skill[lang]); // Reference error : lang is not defined
// 존재하지 않는 프로퍼티를 접근하면 undefined 반환
console.log(skill.level); // undefined
```
## 프로퍼티 값 갱신
* 존재하는 프로퍼티에 값을 할당하면 새로운 값으로 갱신됨
```javascript
var skill = {
    lang: 'javascript'
};

skill.lang = 'pyscript'; // 프로퍼티 값 갱신

console.log(skil); // {lang: "pyscript"}
```
## 프로퍼티 동적 생성
* 존재하지 않는 프로퍼티(키:값)를 할당하면 동적으로 생성함
```javascript
var skill = {
    lang: 'javascript'
};

skill.level = 2; // 프로퍼티 동적 생성

console.log(skil); // {lang: "pyscript", level: 2}
```

## 프로퍼티 삭제
* delete 키워드로 객체 프로퍼티를 삭제
```javascript
var skill = {
    lang: 'javascript'
};

skill.level = 2; // 동적 생성

delete skill.level; // 프로퍼티 삭제
console.log(skil); // {lang: "pyscript"}
```
## 확장 기능 (ES6 이후)
#### 프로퍼티 축약(생략) 방법
* 식별자를 이용한 객체 리터럴 생성
* 샘플코드 ([링크](./src/property_shorthand.html))

#### 식별자(변수값)를 이용하여 프로퍼티 키 동적 생성
* 식별자를 이용한 프로퍼티 키 동적 생성
* 대괄호 ( [ ] ) 접근 방식을 사용
* 샘플코드 ([링크](./src/computed_property_name.html))

#### 메서드 축약(생략) 방법
* function 키워드 생략 가능
* 샘플코드 ([링크](./src/method_shorthand.html))