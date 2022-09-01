# 프로미스
* 자바스크립트 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용한다.
    * 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리중 발생한 에러의 처리가 곤란함
    * 여러 개의 비동기 처리를 한번에 처리하는 데도 한계가 있다.
* `ES6`에서는 비동기 처리를 위한 또 다른 패턴으로 **프로미스를 도입**했다.
* 프로미스는 **콜백 패턴이 가진 단점을 보완**하며 **비동기 처리 시점을 명확하게 표현**할 수 있다는 장점이 있다.
## 비동기 처리를 위한 콜백 패턴의 단점

### 1. 콜백 헬
* 비동기 함수란 함수 내부에 비동기로 동작하는 코드를 포함한 함수를 말한다.
* **비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료된다.**
* **즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료된다.**
* **따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스포크의 변수에 할당하면 기대한 대로 동작하지 않는다.**

< GET 요청을 위한 함수 예제 >
```javascript
// GET 요청을 위한 비동기 함수
const get = url => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET',url);
    xhr.send();

    xhr.onload = () => {
        if(xhr.status === 200) {
            // (1) 서버 응답을 반환
            return JSON.parse(xhr.response);
        } else {
            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    };
};
// (2) id가 1인 post를 취득
const response = get('https://jsonplaceholder.typicode.com/posts/1');
console.log(response); // undefined
```
* get 함수가 비동기 함수인 이유는 get 함수 내부의 **onload 이벤트 핸들러가 비동기로 동작**하기 때문이다.
* get 함수를 호출하면 GET 요청을 전송하고 onload 이벤트 핸들러를 등록한 다음 undefined를 반환하고 즉시 종료된다.
* **즉, 비동기 함수인 get 함수 내부의 onload 이벤트 핸들러는 get 함수가 종료된 이후에 실행된다.**
* 따라서 get 함수의 onload 이벤트 핸들러에서 서버의 응답 결과를 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않는다.
* (1) 반환문은 get 함수의 반환문이 아닌 xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러의 반환문이다.
* (2) get 함수에 명시적인 반환문이이 없으므로 get 함수는 암묵적으로 undefined를 반환한다.
* onload 이벤트 핸들러는 get 함수가 호출하지 않기 때문에 onload 이벤트 핸들러의 반환값은 get 함수가 캐치하여 반환할 수 없다.
---
< GET 요청의 응답을 상위 스코프 변수에 할당 예제 >
```javascript
let todos; // 상위 스코프 변수
// GET 요청을 위한 비동기 함수
const get = url => {
    const xhr = new XMLHttpRequest();
    xhr.open('GET',url);
    xhr.send();

    xhr.onload = () => {
        if(xhr.status === 200) {
            // (1) 서버 응답을 상위 스코프 변수(todos)에 할당
            todos = JSON.parse(xhr.response);
        } else {
            console.error(`${xhr.status} ${xhr.statusText}`);
        }
    };
};
// id가 1인 post를 취득
get('https://jsonplaceholder.typicode.com/posts/1');
console.log(todos); // (2) undefined
```
* xhr.onload 이벤트 핸들러 프로퍼티에 바인딩한 이벤트 핸들러는 언제나 (2)의 console.log가 종료한 이후에 호출된다.
* 따라서 (2)의 시점에는 아직 전역 변수 todos에 서버의 응답 결과가 할당되기 이전이다.
* 즉, xhr.onload 이벤트 핸들러에서 서버의 응답을 상위 스코프의 변수에 할당(1)하면 **처리 순서가 보장되지 않는다.**
* 서버로 부터 응답이 도착하면 xhr 객체에서 load 이벤트가 발생한다.
* 이때 **xhr.onload 핸들러 프로퍼티에 바인딩한 이벤트 핸들러가 즉시 실행되는 것이 아니다.**
* **xhr.onload 이벤트 핸들러는 load 이벤트가 발생하면 일단 태스크 큐에 저장되어 대기하다가, 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다.**
* 이벤트 핸들러도 함수이므로 다음과 같은 과정을 거친다.
    * 이벤트 핸들러 평가 -> 이벤트 핸들러 실행 컨텍스트 생성 -> 콜 스택 푸시 -> 이벤트 핸들러 실행

<br>

* **이처럼 비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없다.**
* **따라서 비동기 함수의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 한다.**
* **이때 비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적이다.**
* **필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 비동기 처리가 처리가 실패하면 호출될 콜백 함수를 전달할 수 있다.**

<br>

* 비동기 처리 결과에 대한 후속처리를 위해 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생
    -> **콜백 헬**(Callback Hell)이라 한다.
    -> 가독성을 나쁘게하며 실수를 유발하는 원인이 된다.

< 콜백 헬 예시 >
```javascript
get('/step1', a => {
    get(`/step2/${a}`, b => {
        get(`/step3/${b}`, c => {
            get(`/step4/${c}`, d => {
                console.log(d);
            });
        });
    });
});
```
### 2. 에러 처리의 한계
* 비동기 처리를 위한 콜백 패턴의 문제점 중에서 가장 심각한 것은 에러 처리가 곤란하다는 것이다.
```javascript
try {
    setTimeout(() => { throw new Error('Error!'); }, 1000);
} catch(e) {
    // 에러를 캐치 못한다
    console.error('캐치한 에러', e);
}
```
* setTimeout 함수의 콜백 함수가 실행될 때 setTimeout 함수는 이미 콜 스택에서 제거된 상태다.
* 이것은 setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아니라는 것을 의미한다.
* **에러는 호출자(caller) 방향으로 전파된다.**
* 하지만 setTimeout 함수의 콜백 함수를 호출한 것은 setTimeout 함수가 아니다.
* 따라서 setTimeout 함수의 콜백 함수가 발생시킨 에러는 catch 블럭에서 캐치되지 않는다.

<br>

* 정리 : 비동기 처리를 위한 콜백 패턴은 콜백 헬이나 에러 처리가 곤란하다는 문제이 있다.
    * 이러한 문제를 해결하기 위해 ES6에서 프로미스가 도입되었다.

< 참고 > try ... catch ... finally 문 
```
try ... catch ... finally 문은 에러 처리를 구현하는 방법이다.
try ... catch ... finally 문을 실행하면 먼저 try 코드 블록이 실행된다.
try 코드 블록에 포함된 문 중에서 에러가 발생하면 해당 에러는 catch 문의 err 변수에 전달되고 catch 코드 블록이 실행된다.
finally 코드 블록은 에러 발생과 상관없이 반드시 한 번 실행한다.
try ... catch ... finally 문으로 에러를 처리하면 프로그램이 강제 종료되지 않는다.
```

## 프로미스의 생성
* **Promise 생성자 함수를 new 연산자와 함께 호출하면 프로미스(Promise 객체)를 생성한다.**
* ES6에서 도입된 Promise는 호스트 객체가 아닌 **ECMAScript 사양에 정의된 표준 빌트인 객체**
* Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 콜백 함수는 `resolve`와 `reject` 함수를 인수로 전달받는다.
    * resolve : 비동기 처리가 성공하면 호출되는 함수
    * reject : 비동기 처리가 실패하면 호출되는 함수
```javascript
// 프로미스 생성
const promise = new Promise((resolve, reject) => {
    // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행한다.
    if( /* 비동기 처리 성공 */ ) {
        resolve('result');
    } else { /* 비동기 처리 실패 */
        reject('failure reason');
    }
});
```

* 비동기 함수 get을 프로미스로 구현한 예제
```javascript
// GET 요청을 위한 비동기 함수
const promiseGet = url => {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.send();

        xhr.onload = () => {
            if(xhr.status === 200) {
                // 성공적으로 응답을 전달받으면 resolve 함수를 호출한다.
                resolve(JSON.parse(xhr.response));
            } else {
                // 에러 처리를 위해 reject 함수를 호출
                reject(new Error(xhr.status));
            }
        };
    });
};
// promiseGet 함수는 프로미스를 반환한다.
promiseGet('https://jsonplaceholder.typicode.com/posts/1');
```
* 프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지를 나타내는 상태 정보를 갖는다.

프로미스의 상태 정보|의미|상태 변경 조건
-|-|-
pending|비동기 처리가 아직 수행되지 않은 상태|프로미스가 생성된 직후 기본 상태
<b>fulfilled</b>|비동기 처리가 수행된 상태(성공)|resolve 함수 호출
<b>rejected</b>|비동기 처리가 수행된 상태(실패)|reject 함수 호출

* 생성된 직후의 프로미스는 기본적으로 pending 상태다.
* 이훟 비동기 처리가 수행되면 비동기 처리 결과에 따라 다음과 같이 프로미스의 상태가 변경된다.
    * 비동기 처리 성공 : resolve 함수를 호출해 프로미스를 fulfilled 상태로 변경
    * 비동기 처리 실패 : reject 함수르 호출해 프로미스를 rejected 상태로 변경
* 이처럼 **프로미스의 상태는 resolve 또는 reject 함수를 호출하는 것으로 결정된다.**

![프로미스의 상태](https://user-images.githubusercontent.com/63139527/187887481-dbae70d3-6ee6-4a31-b7db-a9216e993af6.png "프로미스의 상태")

* fulfilled 또는 rejected 상태를 settled 상태라고 한다.
* settled 상태는 fulfilled 또는 rejected 상태와 상관없이 pending이 아닌 상태로 비동기 처리가 수행된 상태를 말한다.
* 프로미스는 pending 상태에서 fulfilled 또는 rejected 상태, 즉 settled 상태로 변화할 수 있다.
    * 하지만 일단 settled 상태가 되면 더는 다른 상태로 변화할 수 없다.

![fulfilled된 프로미스](https://user-images.githubusercontent.com/63139527/187889340-2bd4530b-92f2-403e-b91b-8cafd575d23f.png "fulfilled된 프로미스")


![rejected된 프로미스](https://user-images.githubusercontent.com/63139527/187889994-9719f16a-0ae5-4e38-b45b-f5f8a200a845.png "rejected된 프로미스")

* 정리 : **프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체다.**

## 프로미스의 후속 처리 메서드
* 프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야 한다.
* 프로미스는 후속 메서드 then, catch, finally를 제공한다.
* **프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출된다.**
    * 후속 처리 메서드의 콜백 함수에 프로미스의 처리 결과가 인수로 전달된다.
* 모든 후속 처리 메서드는 프로미스를 반환하며, 비동기로 동작한다.
### 1. Promise.prototype.then
* `then` 메서드는 두 개의 콜백 함수를 인수로 전달받는다.
    * 첫 번째 콜백 함수는 프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출된다.  
    이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달받는다.
    * 두 번째 콜백 함수는 프로미스가 rejected 상태(reject 함수가 호출된 상태)가 되면 호출된다.  
    이때 콜백 함수는 프로미스의 에러를 인수로 전달받는다.

```javascript
// fulfilled
new Promise(resolve => resolve('fulfilled'))
    .then(v => console.log(v), e => console.error(e)); // fulfilled
new Promise((_, reject) => reject(new Error('rejected')))
    .then(v => console.log(v), e => console.error(e)); // Error: rejected
```
* then 메서드는 언제나 프로미스를 반환한다.
* 만약 then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스를 그대로 반환하고,  
콜백 함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 resolve 또는 reject하여 프로미스를 생성해 반환한다.
### 2. Promise.prototype.catch
* `catch` 메서드는 한 개의 콜백 함수를 인수로 전달받는다.
* catch 메서드의 콜백 함수는 프로미스가 rejected 상태인 경우만 호출된다.
```javascript
// rejected
new Promise((_, reject) => reject(new Error('rejected')))
    .catch(e => console.log(e)); // Error: rejected
// rejected
new Promise((_, reject) => reject(new Error('rejected')))
    .then(undefined, e => console.error(e));
```
* catch 메서드는 `then(undefined, onRejected)`과 동일하게 동작한다.
* 따라서 then 메서드와 마찬가지로 언제나 프로미스를 반환한다.
### 3. Promise.prototype.finally
* finally 메서드는 한 개의 콜백 함수를 인수로 전달받는다.
* finally 메서드의 콜백 함수는 프로미스의 성공(fulfilled) 또는 실패(rejected)와 상관없이 무조건 한 번 호출한다.
* finally 메서드는 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용하다.
* finally 메서드는 then/catch 메서드와 마찬가지로 언제나 프로미스를 반환한다.
```javascript
new Promise(() => {})
    .finally(() => console.log('finally')); // finally
```

< 프로미스로 구현한 비동기 함수 get을 사용해 후속 처리 구현 예제 >
```javascript
const promiseGet = url => {
    return new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest();
        xhr.open('GET', url);
        xhr.send();

        xhr.onload = () => {
            if(xhr.status === 200) {
                resolve(JSON.parse(xhr.response));
            } else {
                reject(new Error(xhr.status));
            }
        };
    });
};
promiseGet('https://jsonplaceholder.typicode.com/posts/1')
    .then(res => console.log(res))
    .catch(err => console.error(err))
    .finally(() => console.log('Bye!'));
```
## 프로미스의 에러 처리
* 프로미스는 에러를 문제없이 처리할 수 있다.
    * 1. then 메서드의 두 번째 콜백 함수로 처리 가능
    ```javascript
    promiseGet(wrongUrl).then(
        res => console.log(res),
        err => console.error(err) // then 메서드 두 번째 콜백 함수
    );
    ```
    * 2. catch 메서드를 사용해 처리 가능
    ```javascript
    promiseGet(wrongUrl)
        .then(res => console.log(res))
        .catch(err => console.error(err)); // catch 메서드
    ```

* 단, **then 메서드의 두 번째 콜백 함수**는 첫 번째 콜백 함수에서 발생한 **에러를 캐치하지 못하고** 코드가 복잡해져서 **가독성이 좋지 않다.**
* **catch 메서드**를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러(rejected 상태)뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치할 수 있다.
    * 가독성이 좋고 명확하다.

* 정리 : **에러 처리는 catch 메서드에서 하는 것을 권장한다.**
## 프로미스 체이닝
* 프로미스는 후속 처리 메서드를 통해 콜백 헬을 해결한다.

< 콜백 헬이 발생하는 예제를 프로미스를 사용해 구현한 예제 >
```javascript
const url = 'https://jsonplaceholder.typicode.com';

promiseGet(`${url}/posts/1`)
    // 취득한 post의 userId로 user 정보 취득
    .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
    .then(userInfo => console.log(userInfo))
    .catch(err => console.error(err));
```
* 위 예제에서 then -> then -> catch 순서로 후속 처리 메서드를 호출했다.
* 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출할 수 있다.
* 이를 프로미스 체이닝이라 한다.
* 위 예제에서 후속 처리 메서드의 콜백 함수는 다음과 같이 인수를 전달받으면서 호출된다.

후속 처리 메서드|콜백 함수의 인수|후속 처리 메서드의 반환값
-|-|-
then|promiseGet 함수가 반환한 프로미스가 resolve한 값(id가 1인 post)|콜백 함수가 반환한 프로미스
then|첫 번째 then 메서드가 반환한 프로미스가 resolve한 값(post의 userId로 취득한 user 정보)|콜백 함수가 반환한 값(undefined)을 resolve한 프로미스
catch<br>- 에러가 발생하지 않으면 호출되지 않는다.|promiseGet 함수 또는 앞선 후속 처리 메서드가 반환한 프로미스가 reject한 값|콜백 함수가 반환한 값(undefined)을 resolve한 프로미스

* 만약 후속 처리 메서드의 콜백 함수가 프로미스가 아닌 값을 반환하더라도 그 값을 암묵적으로 resolve 또는 reject하여 프로미스를 생성해 반환한다.
* 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 비동기 처리를 위한 콜백 패턴에서 발생하던 콜백 헬이 발생하지 않는다.
    * 다만 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 사용하지 않는 것은 아니다.
* 콜백 패턴은 가독성이 좋지 않다.
    * 이 문제는 ES8에서 도입된 async/await 를 통해 해결할 수 있다.
    * async/await를 사용하면 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현할 수 있다.
    * async/await도 프로미스를 기반으로 동작하므로 프로미스는 잘 이해하고 있어야 한다.

< async/await 예제 >
```javascript
const url = 'https://jsonplaceholder.typicode.com';

(async () => {
    // id가 1인 post의 userId를 취득
    const { userId } = await promiseGet(`${url}/posts/1`);

    // 취득한 post의 userId로 user 정보를 취득
    const userInfo = await promiseGet(`${url}/users/${userId}`);

    console.log(userInfo);
})();
```

## 프로미스의 정적 메서드

### 1. Promise.resolve / Promise.reject

### 2. Promise.all

### 3. Promise.race

### 4. Promise.allSettled

## 마이크로태스크 큐

## fetch

### 1. GET 요청

### 2. POST 요청

### 3. PATCH 요청

### 4. DELETE 요청