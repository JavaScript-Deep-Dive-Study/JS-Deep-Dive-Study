## Number 생성자 함수


- `Number` 생성자 함수에 인수를 전달하지 않고 `new` 연산자와 함께 호출하면 `[[NumberData]]` 내부 슬롯에 0을 할당한 `Number` 래퍼 객체를 생성한다.

래퍼 객체 : 문자열, 숫자, boolean 값에 대해 객체처럼 접근하면 생성되는 임시 객체

```jsx
const numObj = new Number();
console.log(numObj) // Number {[[PrimitiveValue]] : 0}
```

- `Number` 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한 후, `[[NumberData]]` 내부 슬롯에 변환된 숫자를 할당한 `Number` 래퍼 객체를 생성한다.

```jsx
let numObj = new Number('10');
console.log(numObj); // Number {[[PrimitiveValue]] : 10}

numObj = new Number('Hello');
console.log(numObj); // Number {[[PrimitiveValue]] : NaN}
```

## Number 프로퍼티


### 1. Number.EPSILON

> ES6에서 도입되어 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.
>

2진법으로 변환했을 때 무한소수가 되어 오차가 발생한다.

```jsx
0.1 + 0.2; // 0.300000000000000000004
0.1 + 0.2 === 0.3 // false
```

`Number.EPSILON` 은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

```jsx
function isEqual(a, b) {
	// a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
	return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```

### 2. Number.MAX_VALUE

> 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다.
>

```jsx
Number.MAX_VALUE // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // true
```

### 3. Number.MIN_VALUE

> 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다.
>

```jsx
Number.MIN_VALUE; // 5e-324
Number.MIN_VALUE > 0 // true
```

### 4. Number.MAX_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.
>

```jsx
Number.MAX_SAFE_INTEGER // 9007199254740991
```

### 5. Number.MIN_SAFE_INTEGER

> 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.
>

```jsx
Number.MIN_SAFE_INTEGER // -9007199254740991
```

### 6.Number.POSITIVE_INFINITY

> 자바스크립트에서 양의 무한대를 나타내는 숫자값이다.
>

```jsx
Number.POSITIVE_INFINITY // Infinity
```

### 7.Number.NEGATIVE_INFINITY

> 자바스크립트에서 음의 무한대를 나타내는 숫자값이다.
>

```jsx
Number.NEGATIVE_INFINITY // -Infinity
```

### 8. Number.NaN

> 숫자가 아님을 나타내는 숫자값이다. `window.NaN` 과 같다.
>

```jsx
Number.NaN; // NaN
```

## Number 메서드


### 1. Number.isFinite

- 인수로 전달된 숫자값이 정상적인 유한수, `Infinity`  또는 `-Infinity` 인지 아닌지 검사한다.

```jsx
Number.isFinite(0) // true
Number.isFinite(32) // true

Number.isFinite(Infinity); // false
Number.isFinite(-Infinity); // false
```

- 인수가 NaN이면 항상 false를 반환한다.

```jsx
Number.isFinite(NaN); // false
```

- Number.isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

```jsx
// Number.isFinite는 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isFinite(null); // -> false

// isFinite는 인수를 숫자로 암묵적 타입 변환한다. null은 0으로 암묵적 타입 변환된다.
isFinite(null); // -> true
```

### 2. Number.isInteger

- 인수로 전달된 숫자값이 정수인지 검사한다. 검사하기 전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```jsx
Number.isInteger(0) // true
Number.isInteger(32) // true

Number.isInteger('123') // false
Number.isInteger(Infinity) // false
```

### 3. Number.isNaN

- 인수로 전달된 숫자값이 NaN인지 검사한다.

```jsx
Number.isNaN(NaN); // true
```

- 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

```jsx
Number.isNaN(undefined) // false

isNaN(undefined) // true
```

### 4. Number.isSafeInteger

- 전달받은 인수가 안전한 정수인지 검사한다. 검사전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

```jsx
Number.isSafeInteger(0); // -> true
// 1000000000000000은 안전한 정수이다.
Number.isSafeInteger(1000000000000000); // -> true

// 10000000000000001은 안전하지 않다.
// 10의 16제곱으로 자바스크립트에서 정확하게 표현할 수 있는 정수의 범위를 벗어난다.
Number.isSafeInteger(10000000000000001); // -> false

Number.isSafeInteger(0.5); // -> false

Number.isSafeInteger('123'); // -> false
```

### 5. Number.prototype.toExponential

- 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.

```jsx
console.log((77.1234).toExponential()); // 7.71234e+1

console.log((77.1234).toExponential(4)); // 7.7123e+1

console.log((77.1234).toExponential(2)); // 7.71e+1
```

### 6. Number.prototype.toFixed

- 반올림하는 소수점 이하 자릿수를 나타내는 0~20사이의 정수값을 인수로 전달할 수 있다.

```jsx
(12345.6789).toFixed(); // 12345

(12345.6789).toFixed(1); // 12345.7

(12345.6789).toFixed(2); // 12345.68
```

### 7. Number.prototype.toPrecision

- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.

```jsx
// 전체 자릿수 유효. 인수를 생략하면 기본값 0이 지정된다.
(12345.6789).toPrecision(); // "12345.6789"
// 전체 1자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(1); // "1e+4"
// 전체 2자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // "1.2e+4"
// 전체 6자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(6); // "12345.7"
```

### 8. Number.prototype.toString

- 숫자를 문자열로 변환하여 반환한다. 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다.

```jsx
// 인수를 생략하면 10진수 문자열을 반환한다.
(10).toString(); // "10"
// 2진수
(16).toString(2); // "10000"
// 8진수
(16).toString(8); // "20
// 16진수
(16).toString(16); // "10"
```