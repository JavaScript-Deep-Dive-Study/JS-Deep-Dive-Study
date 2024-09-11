## Math 프로퍼티


### Math.PI

> 원주율 값을 반환한다.
>

```jsx
Math.PI // 3.141592653589793
```

## Math 메서드


### 1. Math.abs

- 인수로 전달된 숫자의 절대값을 반환한다.

```jsx
Math.abs(-1);        // -> 1
Math.abs('-1');      // -> 1
Math.abs('');        // -> 0
Math.abs([]);        // -> 0

Math.abs(null);      // -> 0
Math.abs(undefined); // -> NaN

Math.abs({});        // -> NaN
Math.abs('string');  // -> NaN
Math.abs();          // -> NaN
```

### 2. Math.round

- 인수로 전달된 숫자의 소수점 이하를 반올림한 정수를 반환한다.

```jsx
Math.round(1.4);  // -> 1
Math.round(1.6);  // -> 2
Math.round(-1.4); // -> -1
Math.round(-1.6); // -> -2
Math.round(1);    // -> 1
Math.round();     // -> NaN
```

### 3. Math.ceil

- 인수로 전달된 숫자의 소수점 이하를 올림한 정수를 반환한다.

```jsx
Math.ceil(1.4);  // -> 2
Math.ceil(1.6);  // -> 2
Math.ceil(-1.4); // -> -1
Math.ceil(-1.6); // -> -1
Math.ceil(1);    // -> 1
Math.ceil();     // -> NaN
```

### 4. Math.floor

- 인수로 전달된 숫자의 소수점 이하를 내림한 정수를 반환한다.

```jsx
Math.floor(1.9);  // -> 1
Math.floor(9.1);  // -> 9
Math.floor(-1.9); // -> -2
Math.floor(-9.1); // -> -10
Math.floor(1);    // -> 1
Math.floor();     // -> NaN
```

### 5. Math.sqrt

- 인수로 전달된 숫자의 제곱근을 반환한다.

```jsx
Math.sqrt(9);  // -> 3
Math.sqrt(-9); // -> NaN
Math.sqrt(2);  // -> 1.414213562373095
Math.sqrt(1);  // -> 1
Math.sqrt(0);  // -> 0
Math.sqrt();   // -> NaN
```

### 6. Math.random

- 임의의 난수를 반환한다. 반환한 난수는 0에서 1미만의 실수다.

```jsx
Math.random(); // 0에서 1 미만의 랜덤 실수(0.8208720231391746)
```

### 7. Math.pos

- 첫 번째 인수를 밑으로, 두 번째 인수를 지수로 거듭제곱한 결과를 반환한다.

```jsx
Math.pow(2, 8);  // -> 256
Math.pow(2, -1); // -> 0.5
Math.pow(2);     // -> NaN
```

### 8. Math.max

- 전달받은 인수중에서 가장 큰 수를 반환한다.

```jsx
Math.max(1); // -> 1
Math.max(1, 2); // -> 2
Math.max(1, 2, 3); // -> 3
Math.max(); // -> -Infinity

Math.max(...[1,2,3]); // -> 3
```

### 9. Math.min

- 전달받은 인수중에서 가장 작은 수를 반환한다.

```jsx
Math.min(1); // -> 1
Math.min(1, 2); // -> 1
Math.min(1, 2, 3); // -> 1
Math.min(); // -> Infinity

Math.min(...[1,2,3]); // -> 1
```