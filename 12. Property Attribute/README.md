# 프로퍼티 어트리뷰트

## 내부 슬롯과 내부 메서드 ([예제](./src/internal_slot_method.html))
* ECMAScript 사양에 등장하는 이중 대괄호( [ [...] ] )로 감싼 이름들이 내부 슬롯과 내부 메서드다.
* 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는
    * 의사 프로퍼티 = 내부 슬롯
    * 의사 메서드 = 내부 메서드
* 개발자가 직접 접근할 수 있도록 외부로 공개된 객체의 프로퍼티는 아니다.
    * 자바스크립트 엔진의 내부 로직
* 모든 객체는 [ [ Prototype ] ] 이라는 내부 슬롯을 가진다.
* 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로
    * 원칙적으로 직접 접근할 수 없지만
    * [ [ Prototype ] ] 내부 슬롯의 경우, __ proto __를 통해 간접적으로 접근 가능
```javascript
const o = {};

// 내부 슬롯은 자바스크립트 엔진의 내부 로직이므로 직접 접근 불가능
o.[[Prototype]] // error : Unexpected token '['
// 단, 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근할 수 있는 수단을 제공함
o.__proto__ // Object.prototype
```

## 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체 ([예제](./src/get_own_property_descriptor.html))
* **자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.**
    * 프로퍼티 상태란?
        * 프로퍼티의 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부
    * 프로퍼티 어트리뷰트란?
        * 자바스크립트 엔진이 관리하는 내부 상태 값인 내부 슬롯
            * [ [ Value ] ]
            * [ [ Writable ] ]
            * [ [ Enumerable ] ]
            * [ [ Configurable ] ]
* 프로퍼티 어트리뷰트에 직접 접근할 수 없지만 **Object.getOwnPropertyDescriptor** 메서드를 사용하여 간접적으로 확인할 수 있다.
```javascript
const person = {
    name: 'Kim'
};
// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// {value: "Kim", writable: true, enumerable: true, configurable: true}
```
프로퍼티 어트리뷰트 | 프로퍼티 상태
-|-
value | "Kim"
writeable | true
enumerable | true
configurable | true

```
Object.getOwnPropertyDescriptor ( obj, property_key )
```
* 첫번째 매개변수 : 객체의 참조를 전달
* 두번째 매개변수 : 프로퍼티 키를 문자열로 전달

참조 : [MDN 공식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/getOwnPropertyDescriptor)

* ES8 도입된 **`Object.getOwnPropertyDescriptors`**
    * 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환
```javascript
const person = {
    name: "Park"
};
// 프로퍼티 동적 생성
person.age = 30;

// 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환
console.log(Object.getOwnPropertyDescriptors(person));
/*
{
    name: {value: "Lee", writable: true, enumerable: true, configurable: true},
    age: {value: 30, writable: true, enumerable: true, configurable: true}
}
*/
```

## 데이터 프로퍼티와 접근자 프로퍼티
* 프로퍼티는 2개의 프로퍼티로 구분할 수 있다.
    * 데이터 프로퍼티
        * **키**와 **값**으로 구성된 일반적인 프로퍼티
    * 접근자 프로퍼티
        * 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 **접근자 함수**로 구성된 프로퍼티

### 1. 데이터 프로퍼티 ([예제](./src/get_own_property_descriptor.html))
* 프로퍼티 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 기본값으로 자동 정의된다.
<table>
    <tr>
        <td bgcolor="#BAE6FD" align="center">프로퍼티</br>어트리뷰트</td>
        <td bgcolor="#BAE6FD" align="center">프로퍼티 디스크립터</br>객체의 프로퍼티</td>
        <td bgcolor="#BAE6FD" align="center">설명</td>
    </tr>
    <tr>
        <td>[[Value]]</td>
        <td>value</td>
        <td>
            <ul>
                <li>프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값</li>
                <li>프로퍼티 키를 통해 프로퍼티 값을 변경하면 [[Value]]에 값을 재할당</li>
                <li>프로퍼티가 없으면 프로퍼티를 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>[[Writable]]</td>
        <td>writable</td>
        <td>
            <ul>
                <li>프로퍼티 값의 변경 가능 여부를 나타냄</li>
                <li>Boolean 데이터 타입을 가짐</li>
                <li>[[Writable]]의 값이 false인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 된다.</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>[[Enumerable]]</td>
        <td>enumerable</td>
        <td>
            <ul>
                <li>프로퍼티 열거 가능 여부를 나타냄</li>
                <li>Boolean 데이터 타입을 가짐</li>
                <li>[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for... in문이나 Object.keys 메서드 등으로 열겨할 수 없음</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>[[Configurable]]</td>
        <td>configurable</td>
        <td>
            <ul>
                <li>프로퍼티의 재정의 가능 여부를 나타냄</li>
                <li>Boolean 데이터 타입을 가짐</li>
                <li>[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다.</li>
                <li>단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다.</li>
            </ul>
        </td>
    </tr>
</table>

* 프로퍼티가 생성될 때 [ [ Value ] ]의 값은 **프로퍼티 값으로 초기화**됨
* [[ Writable ] ], [ [ Enumerable ] ], [ [ Configurable ] ]의 값은 **true로 초기화**됨
* 프로퍼티를 동적 추가해도 동일하게 초기화된다.

### 2. 접근자 프로퍼티 ([예제](./src/accessor_property.html))
* 자체적으로는 값을 갖지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 **접근자 함수**로 구성된 프로퍼티
* 접근자 함수는 getter/setter 함수라고도 부른다.
<table>
    <tr>
        <td bgcolor="#BAE6FD" align="center">프로퍼티</br>어트리뷰트</td>
        <td bgcolor="#BAE6FD" align="center">프로퍼티 디스크립터</br>객체의 프로퍼티</td>
        <td bgcolor="#BAE6FD" align="center">설명</td>
    </tr>
    <tr>
        <td>[[Get]]</td>
        <td>get</td>
        <td>
            <ul>
                <li>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 <strong>읽을 때</strong> 호출되는 접근자 함수</li>
                <li>즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]]의 값</li>
                <li>getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환됨</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>[[Set]]</td>
        <td>set</td>
        <td>
            <ul>
                <li>접근자 프로퍼티를 통해 데이터 프로퍼티의 값을 <strong>저장할 때</strong> 호출되는 접근자 함수</li>
                <li>즉, 접근자 프로퍼티 키로 프로퍼티 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값</li>
                <li>setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장됨</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>[[Enumerable]]</td>
        <td>enumerable</td>
        <td>
            <ul>
                <li>프로퍼티 열거 가능 여부를 나타냄</li>
                <li>Boolean 데이터 타입을 가짐</li>
                <li>[[Enumerable]]의 값이 false인 경우 해당 프로퍼티는 for... in문이나 Object.keys 메서드 등으로 열겨할 수 없음</li>
            </ul>
        </td>
    </tr>
    <tr>
        <td>[[Configurable]]</td>
        <td>configurable</td>
        <td>
            <ul>
                <li>프로퍼티의 재정의 가능 여부를 나타냄</li>
                <li>Boolean 데이터 타입을 가짐</li>
                <li>[[Configurable]]의 값이 false인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다.</li>
                <li>단, [[Writable]]이 true인 경우 [[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용된다.</li>
            </ul>
        </td>
    </tr>
</table>

```javascript
const person = {
    // 데이터 프로퍼티
    firstName: 'cansus',
    lastName: 'Parak',

    // fullName은 접근자 함수로 구성된 접근자 프로퍼티
    // getter 함수
    get fullName() {
        return this.firstName + " " + this.lastName;
    },
    // setter 함수
    set fullName(name) {
        // 배열 디스트럭처링 할당 --> 이름을 공백으로 분리
        [this.firstName, this.lastName] = name.split(" ");
    }
};
```
* 접근자 프로퍼티 fullName으로 프로퍼티 값에 접근하면 내부적으로 [ [ Get ] ] 내부 메서드가 호출되어 다음과 같이 동작한다.
    * 1. 프로퍼티 키가 유효한지 확인(프로퍼티 키는 문자열 또는 심벌이어야함)  
    -> 프로퍼티 키 "fullName"은 문자열이므로 유효한 프로퍼티 키
    * 2. 프로토타입 체인에서 프로퍼티를 검색한다.  
    -> person객체에 fullName 프로퍼티가 존재한다.
    * 3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인한다.  
    -> fullName 프로퍼티는 접근자 프로퍼티이다.
    * 4. 접근자 프로퍼티 fullName의 프로퍼티 어트리뷰트 [ [ Get ] ]의 값, 즉 getter 함수를 호출하여 그 결과를 반환한다.  
    -> 프로퍼티 fullName의 프로퍼티 어트리뷰트 [ [ Get] ]의 값은 Object.getOwnPropertyDescriptor 메서드가 반환하는 프로퍼티 디스크립터 객체의 get 프로퍼티 값과 같다.

**NOTE**
```
프로토타입(prototype)이란?

어떤 객체의 상위(부모) 객체의 역할을 하는 객체이다.
하위(자식) 객체에게 자신의 프로퍼티와 메서드를 상속한다.
프로토타입 객체의 프로퍼티나 메서드를 상속받은 하위 객체는  
자신의 프로퍼티 또는 메서드인 것처럼 자유롭게 사용할 수 있다.

프로토타입 체인이란?
프로토타입이 단방향 링크드 리스트 형태로 연결되어 있는 상속 구조를 말한다.
객체의 프로퍼티나 메서드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메서드가 없다면
프로토타입 체인을 따라 프로토타입의 프로퍼티나 메서드를 차례대로 검색한다.
```

## 프로퍼티 정의 ([예제](./src/property_define.html))
* 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

* `Object.defineProperty` 메서드를 사용하면 프로퍼티의 어트리뷰트를 정의할 수 있다.
    * 참조 : [MDN 공식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)
```javascript
Object.defineProperty(obj, prop, descriptor)
```

프로퍼티 디스크립터 객체의 프로퍼티|대응하는 프로퍼티 어트리뷰트|생략했을 떄의 기본값
-|-|-
value|[ [ Value ] ]|undefined
get|[ [ Get ] ]|undefined
set|[ [ Set ] ]|undefined
writable|[ [ Writable ] ]|false
enumerable|[ [ Enumerable ] ]|false
configurable|[ [ Configurable ] ]|false

* `Object.defineProperties` 메서드를 사용하면 여러 개의 프로퍼티를 한 번에 정의할 수 있다.
    * 참조 : [MDN 공식](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperties)
```javascript
Object.defineProperties(obj, props)
```

## 객체 변경 방지
* 객체는 변경 가능한 값이므로 재할당 없이 직접 변경이 가능하다.
    * 프로퍼티 추가, 프로퍼티 삭제, 프로퍼티 값 갱신 가능
    * Object.defineProperty, Obejct.defineProperties 메서드 사용하여 프로퍼티 어트리뷰트 재정의 가능

* 자바스크립트는 객체의 변경을 방지하는 다양한 메서드를 제공함
    * 객체 변경 방지 메서드들은 객체의 변경을 금지하는 강도가 다르다.

구분|메서드|프로퍼티 추가|프로퍼티 삭제|프로퍼티 값 읽기|프로퍼티 값 쓰기|프로퍼티 어트리뷰트 재정의
-|-|:-:|:-:|:-:|:-:|:-:
객체 확장 금지|Object.preventExtensions|X|O|O|O|O
객체 밀봉|Object.seal|X|X|O|O|X
객체 동결|Object.freeze|X|X|O|X|X

### 1. 객체 확장 금지 ([예제](./src/object_preventExtensions.html))
* `Object.preventExtensions` 메서드는 객체의 확장을 금지한다.
* **확장이 금지된 객체는 프로퍼티 추가가 금지된다.**
    * 프로퍼티 동적 추가할 수 없음
    * Object.defineProperty 메서드로 추가할 수 없음
* 확장이 가능한 객체인지 여부는 `Object.isExtensible` 메서드로 확인 가능
### 2. 객체 밀봉 ([예제](./src/object_seal.html))
* `Object.seal` 메서드는 객체를 밀봉한다.
* **밀봉된 객체는 읽기와 쓰기만 가능하다.**
    * 프로퍼티 추가 금지
    * 프로퍼티 삭제 금지
    * 프로퍼티 어트리뷰트 재정의 금지
* 밀봉된 객체인지 여부는 `Object.isSealed` 메서드로 확인 가능

### 3. 객체 동결 ([예제](./src/object_freeze.html))
* `Object.freeze` 메서드는 객체를 동결한다.
* **동결된 객체는 읽기만 가능하다.**
    * 프로퍼티 추가 금지
    * 프로퍼티 삭제 금지
    * 프로퍼티 어트리뷰트 재정의 금지
    * 프로퍼티 값 갱신 금지
* 동결된 객체인지 여부는 `Object.isFrozen` 메서드로 확인 가능

### 4. 불변 객체 ([예제](./src/immutable_object.html))
* 확장 금지(preventExtensions), 밀봉(seal), 동결(freeze) 메서드들은 **얕은 변경 방지**로 직속 프로퍼티만 변경이 방지되고 **중첩 객체까지는 영향을 주지 못한다.**
* Object.freeze 메서드로 객체를 동결하여도 중첩 객체까지 동결할 수 없다.
```javascript
const person = {
    name: 'Park',
    address: { city: 'Seoul' }
};

// 얕은 객체 동결
Object.freeze(person);

// 직속 프로퍼티만 동결한다.
console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결하지 못한다.
console.log(Object.isFrozen(person.address)); // false

person.address.city = 'Daegu';
console.log(person); // {name: "Park", address: {city: "Daegu"}}
```
* 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 **불변 객체**를 구현하려면 객체를 값으로 갖는 모든 프로퍼티에 대해 **재귀적으로 Object.freeze 메서드를 호출**해야 한다.
    * Object.freeze + 재귀함수 호출
