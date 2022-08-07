# Date
* 표준 빌트인 객체
* 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체 & 생성자 함수
* UTC는 국제 표준시 
* KST는 UTC에 9시간을 더한 시간
    * UTC 00:00 AM = KST 09:00 AM
* 현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정됨

## Date 생성자 함수
* Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가짐
    * 기본적으로 현재 날짜와 시간을 나타내는 정수값을 가짐
    * 다른 날짜와 시간을 다루고 싶은 경우엔 인수로 전달해서 지정해야 함
* 이 값은 1970년 1월 1일 00:00:00(UTC) 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타냄
    * 모든 시간의 기점인 1970년 1월 1일 0시를 나타내는 Date 객체는 내부적으로 정수값 0을 가지며  
    19070년 1월 1일 0시를 기점으로 하루가 지난 1970년 1월 2일 0시를 나타내는 Date 객체는  
    내부적으로 정수값 86,400,000(24h * 60m * 60s * 1000ms)를 가진다.

### 1. new Date()
* 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환
* new 연산자 없이 호출하면 날짜와 시간 정보를 나타내느 문자열을 반환
```javascript
// new 연산자 호출 O
new Date(); // Mon jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)
// new 연산자 호출 X
Date(); // "Mon jul 06 2020 01:03:18 GMT+0900 (대한민국 표준시)"
```

### 2. new Date(milliseconds)
* 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:00(UTC) 기점으로 인수로 전달된 밀리초만큼 날짜와 시간을 나타내는 Date 객체 반환
```javascript
new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (대한민국 표준시)

/*
86400000ms는 1day를 의미
1s = 1000ms
1m = 60s * 1000ms = 60000ms
1h = 60m * 60000ms = 3600000ms
1d = 24h * 3600000ms = 86400000ms
*/
new Date(86400000); // Fri Jan 02 1970 09:00:00 GMT+0900 (대한민국 표준시)
```

### 3. new Date(dateString)
* 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환
* 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이어야 한다.
```javascript
new Date('May 26, 2020 10:00:00');
new Date('2020/03/26/10:00:00');
```

### 4. new Date(year, month[, day, hour, minute, second, millisecond])
* 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체 반환
* 연, 월은 반드시 지정해야 한다.
    * 지정하지 않은 경우 1970년 1월 1일 00:00:00(UTC) 나타내는 Date 객체를 반환
* 지정하지 않은 옵션 정보는 0 또는 1로 초기화된다.

인수|내용
-|-
year|연을 나타내는 1900년 이후의 정수, 0부터 99는 1900부터 1999로 처리됨
month|월을 나타내는 0~11까지의 정수(주의: 0부터 시작 0 = 1월)
day|일을 나타내는 1~31까지의 정수
hour|시를 나타내는 0~23까지의 정수
minute|분을 나타내는 0~59까지의 정수
second|초를 나타내는 0~59까지의 정수
millisecond|밀리초를 나타내는 0~999까지의 정수

```javascript
// 월을 나타내는 2는 3월을 의미
new Date(2020, 2);
// Sun Mar 01 2020 00:00:00 GMT+0900 
new Date(2020, 2, 26, 10, 00, 00, 0);
// Thu Mar 26 2020 10:00:00 GMT+0900

// 가독성 좋은 형태
new Date('2020/3/26/10:00:00:00');
// Thu Mar 26 2020 10:00:00 GMT+0900
```

## Date 메서드

### 1. Date.now
* 1970년 1월 1일 00:00:00(UTC) 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환
```javascript
Date.now(); // 1593971539112
```

### 2. Date.parse
* 1970년 1월 1일 00:00:00(UTC) 기점으로 인수로 전달된 지정 시간(new Date(dateString)의 인수와 동일한 형식)까지의 밀리초를 숫자로 반환
```javascript
// UTC
Date.parse('Jan 2, 1970 00:00:00 UTC'); // 86400000
// KST
Date.parse('Jan 2, 1970 09:00:00'); // 86400000
// KST
Date.parse('1970/01/02/09:00:00'); // 86400000
```

### 3. Date.UTC
* 1970년 1월 1일 00:00:00(UTC) 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환
* new Date(year, mont[, day, hour, minute, second, millisecond])와 같은 형식의 인수를 사용해야 함
* 로컬 타임(KST)이 아닌 UTC로 인식
```javascript
Date.UTC(1970, 0, 2); // 86400000
Date.UTC('1970/1/2'); // NaN
```

### 4. Date.prototype.getFullYear & Date.prototype.setFullYear
* 연도를 나타내는 정수를 반환
* 연도를 나타내는 정수를 설정
```javascript
const today = new Date();
today.setFullYear(1900, 0, 1);
today.getFullYear(); // 1900
```

### 5. Date.prototype.getMonth & Date.prototype.setMonth
* 월을 나타내는 0~11의 정수를 반환
* 월을 나타내는 0~11의 정수를 설정
    * 월 이외에 옵션으로 일도 설정 가능

```javascript
const today = new Date();

today.setMonth(11, 1); // 12월 1일
today.getMonth(); // 11
```

### 6. Date.prototype.getDate & Date.prototype.setDate
* 날짜(1~31)을 나타내는 정수를 반환
* 날짜(1~31)을 나타내는 정수를 설정
```javascript
const today = new Date();
today.setDate(1);
today.getDate(); // 1
```

### 7. Date.prototype.getDay
* 요일(0~6)을 나타내는 정수를 반환

요일|일요일|월요일|화요일|수요일|목요일|금요일|토요일
:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:
반환값|0|1|2|3|4|5|6
```javascript
new Date('2020/07/24').getDay(); // 5
```

### 8. Date.prototype.getHours & Date.prototype.setDate
* 시간(0~23)을 나타내는 정수를 반환
* 시간(0~23)을 나타내는 정수를 설정
    * 시간 이외에 옵션으로 분, 초, 밀리초도 설정 가능

```javascript
const today = new Date();
today.setHours(0, 0, 0, 0); // 00:00:00:00
today.getHours(); // 0
```

### 9. Date.prototype.getMinutes & Date.prototype.setMinutes
* 분(0~59)을 나타내는 정수를 반환
* 분(0~59)을 나타내는 정수를 설정
    * 분 이외에 옵션으로 초, 밀리초도 설정 가능

```javascript
const today = new Date();
today.setMinutes(5, 10, 999); // HH:05:10:999
today.getMinutes(); // 5
```

### 10. Date.prototype.getSeconds & Date.prototype.setSeconds
* 초(0~59)를 나타내는 정수를 반환
* 초(0~59)를 나타내는 정수를 설정
    * 초 이외에 옵션으로 밀리초도 설정 가능

```javascript
const today = new Date();
today.setSeconds(10, 0); // HH:MM:10:000
today.getSeconds(); // 10
```

### 11. Date.prototype.getMilliseconds & Date.prototype.setMilliseconds
* 밀리초(0~999)를 나타내는 정수를 반환
* 밀리초(0~999)를 나타내는 정수를 설정
```javascript
const today = new Date();
today.setMilliseconds(123);
today.getMilliseconds(); // 123
```

### 12. Date.prototype.getTime & Date.prototype.setTime
* 1970년 1월 1일 00:00:00(UTC) 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환
* 1970년 1월 1일 00:00:00(UTC) 기점으로 Date 객체의 시간까지 경과된 밀리초를 설정
```javascript
new Date('2020/07/24/12:30').getTime(); // 1595561400000

const today = new Date();
today.setTime(86400000); // 86400000은 1day를 나타냄
console.log(today); // Fri Jan 02 1970 09:00:00 GMT+0900
```

### 13. Date.prototype.getTimezoneOffset
* UTC와 Date 객체에 지정된 locale 시간과의 차이를 분 단위로 반환
* KST는 UTCdp 9tlrksdmf ejgks tlrks
* UTC = KTC - 9
```javascript
const today = new Date();
today.getTimezoneOffset() / 60; // -9
```

### 14. Date.prototype.toDateString
* 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 **날짜**를 반환
```javascript
const tdoay = new Date('2020/07/24/12:30');
today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toDateString(); // Fri Jul 24 2020
```

### 15. Date.prototype.toTimeString
* 사람이 읽을 수 있는 형식으로 Date 객체의 **시간**을 표현한 문자열을 반환
```javascript
const today = new Date('2020/07/24/12:30');
today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toTimeString(); // 12:30:00 GMT+0900 (대한민국 표준시)
```

### 16. Date.prototype.toISOString
* ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환
```javascript
const today = new Date('2020/07/24/12:30');
today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toISOString(); // 2020-07-24T03:30:00.000Z
today.toISOString().slice(0, 10); // 2020-07-24
today.toISOString().slice(0, 10).replace(/-/g, ''); // 20200724
```

### 17. Date.prototype.toLocaleString
* 인수로 전달한 locale 기준으로 Date 객체의 **날짜와 시간**을 표현한 문자열을 반환
* 인수를 생략한 경우 브라우저가 동작 중인 시스템의 locale을 적용
```javascript
const today = new Date('2020/07/24/12:30');
today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleString(); // 2020. 7. 24. 오후 12:30:00
today.toLocaleString('ko-KR'); // 2020. 7. 24. 오후 12:30:00
today.toLocaleString('en-US'); // 7/24/2020, 12:30:00 PM
today.toLocaleString('ja-JP'); // 2020/7/24 12:30:00
```

### 18. Date.prototype.toLocaleTimeString
* 인수로 전달한 locale 기준으로 Date 객체의 **시간**을 표현한 문자열을 반환
* 인수를 생략한 경우 브라우저가 동작 중인 시스템의 locale을 적용
```javascript
const today = new Date('2020/7/24/12:30');
today.toString(); // Fri Jul 24 2020 12:30:00 GMT+0900 (대한민국 표준시)
today.toLocaleTimeString(); // 오후 12:30:00
today.toLocaleTimeString('ko-KR'); // 오후 12:30:00
today.toLocaleTimeString('en-US'); // 12:30:00 PM
today.toLocaleTimeString('ja-JP'); // 12:30:00
```

## Date를 활용한 시계 예제
* [예제](./src/example.html)