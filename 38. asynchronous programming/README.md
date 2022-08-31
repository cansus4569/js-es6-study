# 비동기 프로그래밍

## 동기 처리와 비동기 처리
* **자바스크립트 엔진은 단 하나의 실행 컨텍스트 스택을 갖는다.**
    * 동시에 2개 이상의 함수를 동시에 실행할 수 없다는 것을 의미
    * 실행 컨텍스트 스택의 최상위 요소인 "실행 중인 실행 컨텍스트"를 제외한 모든 실행 컨텍스트는 모두 실행 대기 중인 태스크들이다.

```javascript
const foo = () => {};
const bar = () => {};
foo();
bar();
```
![exec_context_stack drawio](https://user-images.githubusercontent.com/63139527/187558260-a3de3bc8-3422-4509-9696-3faffacd975d.png)

* 자바스크립트 엔진은 한 번에 하나의 태스크만 실행할 수 있는 **싱글 스레드** 방식으로 동작
* 처리 시간이 걸리는 태스크를 실행하는 경우 **블로킹**(작업중단)이 발생한다.
---
* setTimeout 함수와 유사하게 일정 시간이 경과한 이후에 콜백 함수를 호출하는 sleep 함수를 구현
```javascript
// sleep 함수는 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출한다.
function sleep(func, delay) {
    // Date.now()는 현재 시간을 숫자(ms)로 반환한다.
    const delayUntil = Date.now() + delay;

    // 현재 시간(Date.now())에 delay를 더한 delayUntil이 현재 시간보다 작으면 계속 반복한다.
    while(Date.now() < delayUntil);
    // 일정 시간(delay)이 경과한 이후에 콜백 함수(func)를 호출한다.
    func();
}
function foo() {
    console.log('foo');
}
function bar() {
    console.log('bar');
}
// sleep 함수는 3초 이상 실행된다.
sleep(foo, 3 * 1000);
// bar 함수는 sleep 함수의 실행이 종료된 이후에 호출되므로 3초 이상 블로킹 된다.
bar();
// (3초 경과 후) foo 호출 -> bar 호출
```
* 현재 실행 중인 태스크가 종료할 때까지 다음에 실행될 태스크가 대기하는 방식을 **동기 처리**라고 한다.
    * 장점 : 동기 처리 방식은 태스크를 순서대로 하나씩 처리하므로 실행 순서가 보장된다.
    * 단점 : 앞선 태스크가 종료할 때까지 이후 태스크들이 블로킹된다.

![synchronous_process drawio](https://user-images.githubusercontent.com/63139527/187559614-37d1e5fe-9fb0-493a-be5f-f94c68ce7906.png)

---
* 위 예제를 타이머 함수인 `setTimeout`을 사용하여 수정해보자.
```javascript
function foo() {
    console.log('foo');
}
function bar() {
    console.log('bar');
}
// 타이머 함수 setTimeout은 일정 시간이 경과한 이후에 콜백 함수 foo를 호출한다.
// 타이머 함수 setTimeout은 bar 함수를 블로킹하지 않는다.
setTimeout(foo, 3 * 1000);
bar();
// bar 호출 -> (3초 경과 후) foo 호출
```
* setTimeout 함수는 sleep 함수와 유사하게 일정 시간이 경과한 이후에 콜백 함수를 호출하지만
* setTimeout 함수 이후의 태스크를 블로킹하지 않고 곧바로 실행한다.
* 현재 실행중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행하는 방식을 **비동기 처리**라고 한다.
    * 장점 : 현재 실행중인 태스크가 종료되지 않은 상태라 해도 다음 태스크를 곧바로 실행함으로써 블로킹이 발생하지 않음
    * 단점 : 태스크의 실행 순서가 보장되지 않음

![asynchronous_process drawio](https://user-images.githubusercontent.com/63139527/187560264-43ed3d12-ba63-4ec8-ab95-e9e9271887c2.png)

---
* 비동기 처리를 수행하는 비동기 함수는 전통적으로 콜백 패턴을 사용한다.
* 비동기 처리를 위한 콜백 패턴은 콜백 헬을 발생시켜 
    * 가독성을 나쁘게 하고, 
    * 비동기 처리 중 발생한 에러의 예외 처리가 곤란하며,
    * 여러 개의 비동기 처리르 한 번에 처리하는 데도 한계가 있다.
* **타이머 함수인 setTimeout과 setInterval, HTTP 요청, 이벤트 핸들러는 비동기 처리 방식으로 동작한다.**
## 이벤트 루프와 태스크 큐
* 자바스크립트의 특징 중 하나는 싱글 스레드로 동작한다는 것이다.
    * 한번에 하나의 태스크만 처리할 수 있다.
* 브라우저가 동작하는 것을 살펴보면 많은 태스크가 동시에 처리되는 것처럼 느껴진다.
    * 예시
        * HTML 요소가 애니메이션 효과를 통해 움직이면서 이벤트를 처리하기도 하고,
        * HTTP 요청을 통해 서버로부터 데이터를 가지고 오면서 렌더링하기도 한다.
* 자바스크립트의 동시성을 지원하는 것이 바로 **이벤트 루프** 이다.
* 이벤트 루프는 브라우저에 내장되어 있는 기능중 하나이다.
* 브라우저 환경을 그림으로 표현하면 다음과 같다.
![event_loop_and_browser drawio](https://user-images.githubusercontent.com/63139527/187563271-e269316d-73c0-441f-bd3a-8c8f4cb22c9c.png "이벤트 루프와 브라우저 환경")