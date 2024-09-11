## Date 생성자 함수


### 1. `new Date()`

- 인수 없이 `new` 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.

```jsx
new Data(); //Sat Sep 07 2024 16:30:32 GMT+0900 (한국 표준시)
```

### 2. `new Date(milliseconds)`

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
new Date(0); // Thu Jan 01 1970 09:00:00 GMT+0900 (한국 표준시)
```

### 3. `new Date(dateString)`

- 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
new Date('2024/09/07/16:35:00') // Sat Sep 07 2024 16:35:00 GMT+0900 (한국 표준시)
```

### 4. **`new Date(year,month[, day, hour, minute, second, millisecond])`**

- Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

```jsx
new Date(2024,8); // Sat Sep 07 2024 16:35:00 GMT+0900 (한국 표준시)

new Date(2024,8,7,16,35,00,0); // Sat Sep 07 2024 16:35:00 GMT+0900 (한국 표준시)

new Date('2024/09/07/16:35:00'); // Sat Sep 07 2024 16:35:00 GMT+0900 (한국 표준시)
```

## Date 메서드


### 1. Date.now

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

```jsx
const now = Date.now();

console.log(now) // 1725694993875
```

### 2. Date.parse

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.

```jsx
Date.parse('Jan 2, 1970 00:00:00 UTC'); // 86400000
Date.parse('Jan 2, 1970 00:00:00'); // 54000000
Date.parse('2024/09/07/10:00:00'); // 1725670800000
```

### 3. Date.UTC

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
- Date.UTC 메서드는 new Date(year, month[, day, hour, minute, second, millisecond])]와 같은 형식의 인수를 사용해야 한다.

```jsx
Date.UTC(1970, 0, 2); // 86400000
Date.UTC('2024/09/07'); // NaN
```

### 4. Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수를 반환한다.

```jsx
new Date('2024/09/07').getFullYear(); // 2024
```

### 5. Date.prototype.setFullYear

- Date 객체에 연도를 나타내는 정수를 설정한다. 월, 일도 설정할 수 있다.

```jsx
const today = new Date('2024/09/07').getFullYear(); // 2024

// 년도 지정
today.setFullYear(2020); 
today.getFullYear(); // 2020

// 년도/월/일 지정
today.setFullYear(2010,12,12);
today.getFullYear(); // 2010
```

### 6. Date.prototype.getMonth

- Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다. 1월은 0 12월은 11이다.

```jsx
new Date('2024/09/07').getMonth(); // 8
```

### 7. Date.prototype.setMonth

- Date 객체의 월을 설정한다.

```jsx
const today = new Date();

today.setMonth(0);
today.getMonth(); // 0
```

### 8. Date.prototype.getDate

- Date 객체의 날짜 (1 ~ 31)를 나타내는 정수를 반환한다.

```jsx
new Data('2024/09/07').getDate(); // 7
```

### 9. Date.prototype.setDate

- Date 객체의 날짜를 설정한다.

```jsx
const today = new Date();

today.setDate(1);
today.getDate(); // 1
```

### 10. Date.prototype.getDay

- Date 객체의 요일 (0 ~ 6)을 나타내는 정수를 반환한다.

```jsx
new Date('2024/09/07').getDay() // 6 토요일
```

### 11. Date.prototype.getHours

- Date 객체의 시간(0 ~ 23)을 나타내는 정수를 반환한다.

```jsx
new Date('2024/09/07/12:00').getHours(); // 12
```

### 12. Date.prototype.setHours

- Date 객체의 시간(0 ~ 23)을 나타내는 정수를 설정한다.

```jsx
const today = new Date();

today.setHours(7);
today.getHours() // 7
```

### 13. Date.prototype.getMinutes

- Date 객체의 분(0 ~ 59)을 나타내는 정수를 반환한다.

```jsx
new Date('2024/09/07/12:30').getMinutes(); // 30
```

### 14. Date.prototype.setMinutes

- Date 객체의 분(0 ~ 59)을 나타내는 정수를 설정한다.

```jsx
const today = new Date();

today.setMinutes(58);
today.getMinutes(); // 58
```

### 15. Date.prototype.getSeconds

- Date 객체의 초 (0 ~ 59)를 반환한다.

```jsx
new Date('2024/09/07/12:30:20').getSeconds(); // 20
```

### 16. Date.prototype.setSeconds

- Date 객체의 초 (0 ~ 59)를 설정한다.

```jsx
const today = new Date();

today.setSeconds(24);
today.getSeconds(); // 24
```

### 17. Date.prototype.getMilliseconds

- Date 객체의 밀리초(0~999)를 나타내는 정수를 반환한다.

```jsx
new Date('2024/09/07/12:30:20:150').getMilliseconds(); // 150
```

### 18. Date.prototype.setMilliseconds

- Date 객체의 밀리초(0~999)를 설정한다.

```jsx
const today = new Date();

today.setMilliseconds(150);
today.getMilliseconds(); // 150
```

### 19. Date.prototype.getTime

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.

```jsx
new Date('2024/08/08/12:30').getTime() // 1723087800000
```

### 20. Date.prototype.setTime

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 경과된 밀리초를 설정한다.

```jsx
const today = new Date();

today.setTime(86400000);
console.log(today) // Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)
```

### 21. Date.prototype.getTimezoneOffset

- UTC와 locale 시차를 분 단위로 반환한다.

```jsx
const today = new Date();

today.getTimezoneOffset() / 60; // -9
```

### 22. Date.prototype.toDateString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.

```jsx
const today = new Date('2024/09/07/12:30');

today.toDateString(); // Sat Sep 07 2024
```

### 23. Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 시간을 반환한다.

```jsx
const today = new Date('2024/09/07/12:30');

today.toTimeString(); // 12:30:00 GMT+0900 (한국 표준시)
```

### 24. Date.prototype.toISOString

- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```jsx
const today = new Date('2024/09/07/12:30');

today.toISOString(); // 2024-09-07T03:30:00.000Z
```

### 25. Date.prototype.toLocaleString

- 인수로 전달한 locale을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다. 인수를 생략한 경우 브라우저가 동작 중인 시스템의 locale을 적용한다.

```jsx
const today = new Date('2024/09/07/12:30');

today.toLocaleString(); // 2024. 9. 7. 오후 12:30:00
today.toLocaleString('ko-KR'); // 2024. 9. 7. 오후 12:30:00
```

### 26. Date.prototype.toLocaleTimeString

- 인수로 전달한 locale을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.

```jsx
const today = new Date('2024/09/07/12:30');

today.toLocaleTimeString(); // 오후 12:30:00
today.toLocaleTimeString('ko-KR'); // 오후 12:30:00
```