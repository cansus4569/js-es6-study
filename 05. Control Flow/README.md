# 제어문
* 조건문에 따라 수행하는 조건문과 반복문에 대한 정리
* 다른 언어에서의 내용과 비슷하기에 간단히 정리한다.

## 블록문
* { } 중괄호로 묶어서 블록을 지정한다.
* 블록문은 중괄호 끝에 세미콜론(;)을 붙이지 않는다.
* NOTE : 객체 리터럴과 혼동하지 말것!

```javascript
// 블록문
{
    var res = 1;
}
// 제어문
if(res > 0) {
    res++;
}
// 함수 선언문
function add(x, y) {
    return x + y;
}
```

## 조건문
* 조건식이 참과 거짓에 따라 코드 블록문을 수행한다.
* 조건문의 종류 : if ~ else문, switch문

### if ~ else문
```javascript
if(조건식1) {
    // 조건식 1이 참일 경우 해당 블록문 수행
} else if (조건식2) {
    // 조건식 2가 참일 경우 해당 블록문 수행
} else {
    // 조건식1과 조건식2가 거짓일 때 해당 블록문 수행
}
```
* 참고 : if ~ else문은 삼항 조건 연산자로도 표현이 가능하다.
* (주관적 의견) : 조건이 2개일 경우 삼항 조건 연산자가 가독성이 좋지만, 조건이 많아 질 수록 삼항 조건 연산자로 표현하기에는 복잡하다.

### switch문
* 주어진 표현식에 일치하는 case로 이동하여 코드들을 수행한다.
* 일치하는 case가 없을 경우 default 아래에 정의된 코드들을 수행한다.
* default는 생략 가능하다.
```javascript
switch(표현식) {
    case 표현식1 :
        // case 표현식1에 대한 코드들을 정의 및 실행되는 코드 영역 
        break; // switch 문을 빠져나온다.
    case 표현식2 :
        // case 표현식2에 대한 코드들을 정의 및 실행되는 코드 영역
        //
        break; // switch 문을 빠져나온다.
    default :
    // default에 대한 코드들을 정의 및 실행되는 코드 영역
}
```

## 반복문
* 주어진 조건식이 결과가 참일 경우 반복 수행된다.
* 반대로 조건식의 결과가 거짓일 경우 반복을 멈추고 빠져나온다.
* 반복문의 종류 : for문, while문, do~while문

### for문
* 선언한 변수가 증감식을 통해 영향을 받고 조건식이 참일 경우 반복한다.
```javascript
for(변수 선언문 또는 할당문; 조건식; 증감식) {
    // 조건식이 참일 경우 실행되는 코드 영역
}
// for문을 이용한 반복문
for(var i=0; i<10; i++) {
    console.log(i);
}
// 이중 for문
for(var i=0;i<5;i++) {
    for(var j=0;j<5;j++) {
        console.log(i+j);
    }
}
// for문을 이용한 무한반복
for(;;) {
    console.log('Not the End');
}
```

### while문
* 주어진 조건식이 참일 경우 반복한다.
* for문과 다르게 증감식이 없기 때문에 while문 안에서 증감식 코드가 있어야 무한반복에 빠지지 않는다.
```javascript
var i = 0;
while(i < 10) { // while(조건식)
    console.log(i);
    i++;    // 증감식
}
// while문을 이용한 무한반복
while(1) {
    console.log('Not the End');
}
```

### do~while문
* 주어진 조건이 참일 경우 반복한다.
* while문과 동일하게 동작하지만, 차이점은 조건식의 위치가 선행이 아닌 후행이다.
* 맨 끝에 세미콜론(;)을 붙인다.
* 먼저 한번 수행하고 조건식을 참과 거짓에 여부에 따라 반복문이 수행된다.
```javascript
var i = 0;
do {
    console.log(i);
    i++;    // 증감식
} while (i < 10); // while(조건식)
// do~while문을 이용한 무한반복
do {
    console.log('Not the End');
} while (1); // 세미콜론
```

## break문
* 주로 for문, switch문, while문, do~while문 에서 사용되어진다.
* break문의 역할은 해당 조건문 또는 반복문에서 실행을 그만두고 빠져나오는 의미를 나타낸다.
```javascript
for(var i=0; i<10; i++) {
    if(i==4) break; // i=4 일 경우, for문을 빠져나온다.
}

switch(i) {
    case 1:
        break;
    case 4:
        break; // i=4 일 경우, switch문을 빠져나온다.
    default:
}

while(i < 10) {
    if(i==9) break; // i=9일 경우 while문을 빠져나온다.
    i++;
}
```

## continue문
* break문과 다르게 빠져나오는 형태가 아니다.
* continue문은 현 지점에서 중단하고 증감식을 통해 다음 반복 조건식의 단계를 진행하도록 한다.
```javascript
var res = 0;
for(var i=0;i<5;i++) {
    if(i==3) continue; // i=3일 경우, 증감식을 통해 i=4의 단계로 넘어감.
    res += i; // i=3일 경우, continue문에 의해 덧셈 연산이 제외됨.
}
```