# 제너레이터와 async/await

## 제너레이터란?
* ES6에서 도입된 제너레이터는 코드 블록의 실행을 일시 중지했다가 필요한 시점에 재개할 수 있는 특수한 함수이다.
* 제너레이터와 일반 함수의 차이는 다음과 같다.
    1. **제너레이터 함수는 함수 호출자에게 함수 실행의 제어권을 양도할 수 있다.**
        * 일반 함수를 호출하면 제어권이 함수에게 넘어가고 함수 코드를 일괄 실행한다.
        * 즉, 함수 호출자는 함수를 호출한 이후 함수 실행을 제어할 수 없다.
        * 제너레이터 함수는 함수 실행을 함수 호출자가 제어할 수 있다.
        * 다시 말해, 함수 호출자가 함수 실행을 일시 중지시키거나 재개시킬 수 있다.
        * 이는 **함수의 제어권을 함수가 독점하는 것이 아니라 함수 호출자에게 양도(yield)할 수 있다**는 것을 의미한다. 
    2. **제너레이터 함수는 함수 호출자와 함수의 상태를 주고받을 수 있다.**
        * 일반 함수를 호출하면 매개변수를 통해 함수 외부에서 값을 주입받고 함수 코드를 일괄 실행하여 결과값을 함수 외부로 반환한다.
        * 즉, 함수가 실행되고 있는 동안에는 함수 외부에서 함수 내부로 값을 전달하여 함수의 상태를 변경할 수 없다.
        * 제너레이터 함수는 함수 호출자와 양방향으로 함수의 상태를 주고받을 수 있다.
        * 다시 말해, **제너레이터 함수는 함수 호출자에게 상태를 전달할 수 있고 함수 호출자로부터 상태를 전달받을 수도 있다.** 
    3. **제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.**
        * 일반 함수를 호출하면 함수 코드를 일괄 실행하고 값을 반환한다.
        * **제너레이터 함수를 호출하면 함수 코드를 실행하는 것이 아니라 이터러블이면서 동시에 이터레이터인 제너레이터 객체를 반환한다.**

## 제너레이터 함수의 정의
* 제너레이터 함수는 `fuction*` 키워드로 선언한다.
* 그리고 하나 이상의 yield 표현식을 포함한다.
* 이것을 제외하면 일반 함수를 정의하는 방법과 같다.
```js
// 제너레이터 함수 선언문
function* genDecFunc() {
    yield 1;
}
// 제너레이터 함수 표현식
const genExpFunc = function* () {
    yield 1;
};
// 제너레이터 메서드
const obj = {
    * genObjMethod() {
        yield 1;
    }
};
// 제너레이터 클래스 메서드
class MyClass {
    * genClsMethod() {
        yield 1;
    }
}
```
* 애스터리스크(*)의 위치는 function 키워드와 함수 이름 사이라면 어디든지 상관없다.
    * 다음 예제의 제너레이터 함수는 모두 유효하다.
    * 일관성을 유지하기 위해 function 키워드 바로 뒤에 붙이는 것을 권장한다.

```js
function* genFunc() { yield 1; } // 권장 방식
function * genFunc() { yield 1; }
function *genFunc() { yield 1; }
function*genFunc() { yield 1; }
```
* 제너레이터 함수는 화살표 함수로 정의할 수 없다.
```js
const getArrowFunc = * () => {
    yield 1;
} // SyntaxError: Unexpected token '*'
```
* 제너레이터 함수는 new 연산자와 함께 생성자 함수로 호출할 수 없다.
```js
function* genFunc() {
    yield 1;
}
new genFunc(); // TypeError: genFunc is not a constructor
```
## 제너레이터 객체
* **제너레이터 함수를 호출하면 일반 함수처럼 함수 코드 블록을 실행하는 것이 아니라 제너레이터 객체를 생성해 반환한다.**
* **제너레이터 함수가 반환한 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.**
* 다시 말해, 제너레이터 객체는 Symbol.iterator 메서드를 상속받는 이터러블이면서 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환하는 next 메서드를 소유하는 이터레이터다.
* 제너레이터 객체는 next 메서드를 가지는 이터레이터이므로 Symbol.iterator 메서드를 호출해서 별도로 이터레이터를 생성할 필요가 없다.
```js
// 제너레이터 함수
function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
}
// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
const generator = genFunc();
// 제너레이터 객체는 이터러블이면서 동시에 이터레이터다.
// 이터러블은 Symbol.iterator 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체다.
console.log(Symbol.iterator in generator); // true
// 이터레이터는 next 메서드를 갖는다.
console.log('next' in generator); // true
```
* 제너레이터 객체는 next 메서드를 갖는 이터레이터이지만 이터레이터에는 없는 return, throw 메서드를 갖는다.
* 제너레이터 객체의 세 개의 메서드를 호출하면 다음과 같이 동작한다.
    * next 메서드를 호출하면 제너레이터 함수의 yield 표현식까지 코드 블록을 실행하고 yield된 값을 value 프로퍼티 값으로, false를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    * return 메서드를 호출하면 인수로 전달받은 값을 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    ```js
    function* genFunc() {
        try {
            yield 1;
            yield 2;
            yield 3;
        } catch(e) {
            console.error(e);
        }
    }
    const generator = genFunc();
    console.log(generator.next()); // {value: 1, done: false}
    console.log(generator.return('End!')); // {value: "End!", done: true}
    ```
    * throw 메서드를 호출하면 인수로 전달받은 에러를 발생시키고 undefined를 value 프로퍼티 값으로, true를 done 프로퍼티 값으로 갖는 이터레이터 리절트 객체를 반환한다.
    ```js
    function* genFunc() {
        try {
            yield 1;
            yield 2;
            yield 3;
        } catch (e) {
            console.error(e);
        }
    }
    const generator = genFunc();
    console.log(generator.next()); // {value: 1, done: false}
    console.log(generator.throw('Error!')); // {value: undefined, done: true}
    ```

## 제너레이터의 일시 중지와 재개
* 제너레이터는 yield 키워드와 next 메서드를 통해 실행을 일시 중지했다가 필요한 시점에 다시 재개할 수 있다.
* 일반 함수는 호출 이후 제어권을 함수가 독점하지만 제너레이터는 함수 호출자에게 제어권을 양도(yield)하여 필요한 시점에 함수 실행을 재개할 수 있다.
* 제너레이터 함수를 호출하면 제너레이터 함수의 코드 블록이 실행되는 것이 아니라 제너레이터 객체를 반환한다고 했다.
* 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
* 제너레이터 객체의 next 메서드를 호출하면 제너레이터 함수의 코드 블록을 실행한다.
* 단, 일반 함수처럼 한 번에 코드 블록의 모든 코드를 일괄 실행하는 것이 아니라 yield 표현식까지만 실행한다.
* **yield 키워드는 제너레이터 함수의 실행을 일시 중지시키거나 yield 키워드 뒤에 오는 표현식의 평가 결과를 제너레이터 함수 호출자에게 반환한다.**

```js
// 제너레이터 함수
function* genFunc() {
    yield 1;
    yield 2;
    yield 3;
}
// 제너레이터 함수를 호출하면 제너레이터 객체를 반환한다.
// 이터러블이면서 동시에 이터레이터인 제너레이터 객체는 next 메서드를 갖는다.
const generator = genFunc();

// 처음 next 메서드를 호출하면 첫 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 첫 번째 yield 표현식에서 yield된 값 1이 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 faluse가 할당된다.
console.log(generator.next()); // {value: 1, done: false}

// 다시 next 메서드를 호출하면 두 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 두 번째 yield 표현식에서 yield된 값 2가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 2, done: false}

// 다시 next 메서드를 호출하면 세 번째 yield 표현식까지 실행되고 일시 중지된다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 세 번째 yield 표현식에서 yield된 값 3가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 false가 할당된다.
console.log(generator.next()); // {value: 3, done: false}

// 다시 next 메서드를 호출하면 남은 yield 표현식이 없으므로 제너레이터 함수의 마지막까지 실행한다.
// next 메서드는 이터레이터 리절트 객체({value, done})를 반환한다.
// value 프로퍼티에는 제너레이터 함수의 반환값 undefined가 할당된다.
// done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true 할당된다.
console.log(generator.next()); // {value: undefined, done: true}
```
* **제너레이터 객체의 next 메서드를 호출하면 yield 표현식까지 실행되고 일시 중지(suspend)된다.**
* **이때 함수의 제어권이 호출자로 양도(yield)된다.**
* 이후 필요한 시점에 호출자가 또 다시 next 메서드를 호출하면 일시 중지된 코드부터 실행을 재개(resume)하기 시작하여 다음 yield 표현식까지 실행되고 또 다시 일시 중지된다.
* **이때 제너레이터 객체의 next 메서드는 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다.**
* **next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티에는 yield 표현식에서 yield된 값(yield 키워드 뒤의 값)이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었는지를 나타내는 불리언 값이 할당된다.**
* 이처럼 next 메서드를 반복 호출하여 yield 표현식까지 실행과 일시 중지를 반복하다가 제너레이터 함수가 끝까지 실행되면 next 메서드가 반환하는 이터레이터 리절트 객체의 value 프로퍼티에는 제너레이터 함수의 반환값이 할당되고 done 프로퍼티에는 제너레이터 함수가 끝까지 실행되었음을 나타내는 true가 할당된다.
```
generator.next() -> yield -> generator.next() -> yield -> 
    ... -> generator.next() -> return
```
* 이터레이터의 next 메서드와 달리 제너레이터 객체의 next 메서드에는 인수를 전달할 수 있다.
* **제너레이터 객체의 next 메서드에 전달한 인수는 제너레이터 함수의 yield 표현식을 할당받는 변수에 할당된다.**
* yield 표현식을 할당받는 변수에 yield 표현식의 평가 결과가 할당되지 않는 것에 주의하기 바란다.
    * [예제](./src/generator.html)
* 제너레이터의 특성을 활용하면 비동기 처리를 동기 처리처럼 구현할 수 있다.
## 제너레이터의 활용
### 1. 이터러블의 구현
* 제너레이터 함수를 사용하면 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.

< 이터레이션 프로토콜을 준수하여 무한 피보나치 수열을 생성하는 함수 예제 >
```js
// 무한 이터러블을 생성하는 함수
const infiniteFibonacci = (function () {
    let [pre, cur] = [0, 1];

    return {
        [Symbol.iterator]() { return this; },
        next() {
            [pre, cur] = [cur, pre + cur];
            // 무한 이터러블이므로 done 프로퍼티를 생략한다.
            return { value: cur };
        }
    };
}());
// infiniteFibonacci는 무한 이터러블이다.
for(const num of infiniteFibonacci) {
    if (num > 10000) break;
    console.log(num); // 1 2 3 5 8 ... 2584 4181 6765
}
```

< 제너레이터를 사용하여 무한 피보나치 수열을 생성하는 함수 예제 >
* 이터레이션 프로토콜을 준수해 이터러블을 생성하는 방식보다 간단히 이터러블을 구현할 수 있다.
```js
// 무한 이터러블을 생성하는 제너레이터 함수
const infiniteFibonacci = (function* () {
    let [pre, cur] = [0, 1];

    while(true) {
        [pre, cur] = [cur, pre + cur];
        yield cur;
    }
}());
// infiniteFibonacci는 무한 이터러블이다.
for(const num of infiniteFibonacci) {
    if(num > 10000) break;
    console.log(num); // 1 2 3 5 8 ... 2584 4181 6765
}
```
### 2. 비동기 처리
* 제너레이터 함수는 next 메서드와 yield 표현식을 통해 함수 호출자와 함수의 상태를 주고받을 수 있다.
* 이러한 특성을 활용하면 프로미스를 사용한 비동기 처리를 동기 처리처럼 구현할 수 있다.
* 다시 말해, 프로미스의 후속 처리 메서드 then/catch/finally 없이 비동기 처리 결과를 반환하도록 구현할 수 있다.
    * [예제](./src/generator_async.html)

```js
// node-fetch는 Node.js 환경에서 window.fetch 함수를 사용하기 위한 패키지다.
// 브라우저 환경에서 이 예제를 실행한다면 아래 코드는 필요 없다.
// https://github.com/node-fetch/node-fetch
// const fetch = require('node-fetch');

// 제너레이터 실행기
const async = genFunc => {
    const generator = genFunc(); // (2)

    const onResolved = arg => {
        const result = generator.next(arg); // (5)

        return result.done
            ? result.value // (9)
            : result.value.then(res => onResolved(res)); // (7)
    };
    return onResolved; // (3)
};
(async(function* fetchTodo() { // (1)
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = yield fetch(url); // (6)
    const todo = yield response.json(); // (8)
    console.log(todo);
    // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
})()); // (4)
```

* async/await 를 사용하면 async 함수와 같은 제너레이터 실행기를 사용할 필요가 없지만 혹시 제너레이터 실행기가 필요하다면 직접 구현하는 것보다 co 라이브러리를 사용하기 바란다.
```js
const fetch = require('node-fetch');
// https://github.com/tj/co
const co = require('co');

co(function* fetchTodo() {
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = yield fetch(url);
    const todo = yield response.json();
    console.log(todo);
});
```
## async/await
* 제너레이터를 사용해서 비동기 처리를 동기 처리처럼 동작하도록 구현했지만 코드가 무척이나 장황해지고 가독성도 나빠졌다.
* ES8(ECMAScript 2017)에서는 제너레이터보다 간단하고 가독성 좋게 비동기 처리를 동기 처리처럼 동작하도록 구현할 수 있는 async/await 가 도입되었다.
* async/await 는 프로미스를 기반으로 동작한다.
* async/await를 사용하면 프로미스의 then/catch/finally 후속 처리 메서드에 콜백 함수를 전달해서 비동기 처리 결과를 후속 처리할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있다.
* 다시 말해, 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.

< async/await 구현 예제 >
```js
async function fetchTodo() {
    const url = 'https://jsonplaceholder.typicode.com/todos/1';

    const response = await fetch(url);
    const todo = await response.json();
    console.log(todo);
    // {userId: 1, id: 1, title: 'delectus aut autem', completed: false}
}
fetchTodo();
```
### 1. async 함수
* await 키워드는 반드시 async 함수 내부에서 사용해야 한다.
* async 함수는 `async` 키워드를 사용해 정의하며 언제나 **프로미스를 반환**한다.
* async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve 하는 프로미스를 반환한다.

```js
// async 함수 선언문
async function foo(n) { return n; }
foo(1).then(v => console.log(v)); // 1

// async 함수 표현식
const bar = async function (n) { return n; };
bar(2).then(v => console.log(v)); // 2

// async 화살표 함수
const baz = async n => n;
baz(3).then(v => console.log(v)); // 3

// async 메서드
const obj = {
    async foo(n) { return n; }
};
obj.foo(4).then(v => console.log(v)); // 4

// async 클래스 메서드
class MyClass {
    async bar(n) { return n; }
}
const myClass = new MyClass();
myClass.bar(5).then(v => console.log(v)); // 5
```
* 클래스의 constructor 메서드는 async 메서드가 될 수 없다.
* 클래스의 constructor 메서드는 인스턴스를 반환해야 하지만 async 함수는 언제나 프로미스를 반환해야 한다.
```js
class MyClass {
    async constructor() { }
    // SyntaxError: Class constructor may no be an async method
}
const myClass = new MyClass();
```
### 2. await 키워드
* `await` 키워드는 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환한다.
* await 키워드는 반드시 프로미스 앞에서 사용해야 한다.
```js
const getGithubUserName = async id => {
    const res = await fetch(`https://api.github.com/users/${id}`); // (1)
    const { name } = await res.json(); // (2)
    console.log(name); // Ungmo Lee
};
getGithubUserName('ungmo2');
```
* **프로미스가 settled 상태가 되면 프로미스가 resolve한 처리 결과가 res 변수에 할당된다.**
* 이처럼 await 키워드는 다음 실행을 일시 중지시켰다가 프로미스가 settled 상태가 되면 다시 재개한다.
---
**< 서로 연관 없이 개별적으로 수행되는 비동기 처리 예제(Bad) >**
* 순서 보장이 필요한 경우, 속도는 느려지지만...
```js
async function foo() {
    const a = await new Promise(res => setTimeout(() => res(1), 3000));
    const b = await new Promise(res => setTimeout(() => res(2), 2000));
    const c = await new Promise(res => setTimeout(() => res(3), 1000));

    console.log([a, b, c]); // [1, 2, 3]
}
foo(); // 약 6초 소요됨
```
* 모든 프로미스에 await 키워드를 사용하는 것은 주의해야 한다.
* foo 함수가 수행하는 3개의 비동기 처리는 서로 연관이 없어 개별적으로 수행되는 비동기 처리이므로 앞선 비동기 처리가 완료될 때까지 대기해서 **순차적으로 처리**할 필요가 없다.

**< 서로 연관 없이 개별적으로 수행되는 비동기 처리 예제(Good) >**
* 순서 보장이 필요 없는 경우, 속도는 빨라짐...
```js
async function foo() {
    const res = await Promise.all([
        new Promise(resolve => setTimeout(() => resolve(1), 3000)),
        new Promise(resolve => setTimeout(() => resolve(2), 2000)),
        new Promise(resolve => setTimeout(() => resolve(3), 1000))
    ]);
    console.log(res); // [1, 2, 3]
}
foo(); // 약 3초 소요됨
```
### 3. 에러 처리
* 비동기 처리를 위한 콜백 패턴의 단점 중 가장 심각한 것은 에러 처리가 곤란하다는 것이다.
* 에러는 호출자 방향으로 전파된다.
* 즉, 콜 스택의 아래 방향(실행 중인 실행 컨텍스트가 푸시되기 직전에 푸시된 실행 컨텍스트 방향)으로 전파된다.
* 하지만 비동기 함수의 콜백 함수를 호출한 것은 비동기 함수가 아니기 때문에 try...catch 문을 사용해 에러를 캐치할 수 없다.
```js
try {
    setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch (e) {
    // 에러를 캐치하지 못한다.
    console.error('캐치한 에러', e);
}
```
* async/await 에서 에러 처리는 try...catch문을 사용할 수 있다.
* 콜백 함수를 인수로 전달받는 비동기 함수와는 달리 프로미스를 반환하는 비동기 함수는 명시적으로 호출할 수 있기 때문에 호출자가 명확하다.
```js
const foo = async () => {
    try {
        const wrongUrl = 'https://wrong.url';

        const response = await fetch(wrongUrl);
        const data = await response.json();
        console.log(data);
    } catch (err) {
        console.error(err); // TypeError: Failed to fetch
    }
};
foo();
```
* foo 함수의 catch 문은 HTTP 통신에서 발생한 네트워크 에러뿐 아니라 try 코드 블록 내의 모든 문에서 발생한 일반적인 에러까지 모두 캐치할 수 있다.
* **async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환한다.**
* 따라서 async 함수를 호출하고 Promise.prototype.catch 후속 처리 메서드를 사용해 에러를 캐치할 수도 있다.
```js
const foo = async () => {
    const wrongUrl = 'https://wrong.url';

    const response = await fetch(wrongUrl);
    const data = await response.json();
    return data;
};
foo()
    .then(console.log)
    .catch(console.error); // TypeError: Failed to fetch
```





