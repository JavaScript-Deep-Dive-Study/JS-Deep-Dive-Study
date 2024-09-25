# 스프레드 문법

ES6에서 도입된 스프레드 문법 `…` 은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.

단, 이터러블에 한정돼서 사용할 수 있다.

```jsx
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다(→ 1, 2, 3)
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다.
console.log(..."Hello"); // H e l l o

// Map과 Set은 이터러블이다.
console.log(
  ...new Map([
    ["a", "1"],
    ["b", "2"],
  ])
); // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 });
// TypeError: Found non-callable @@iterator
```

…[1, 2, 3]의 결과 값인 1 2 3은 값이 아니라 값들의 목록이다.
즉 스프레드 문법은 연산자가 아니다. 따라서 스프레드 문법의 결과는 변수에 할당할 수 없다.

```jsx
// 스프레드 문법의 결과는 값이 아니다.
const list = ...[1, 2, 3]; // SyntaxError: Unexpected token ...
```

이처럼 스프레드 문법의 결과물은 값으로 사용할 수 없고, 다음과 같이 쉼표로 구분한 값의 목록을 사용하는 문백에서만 사용할 수 있다.

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

<br/>

# 함수 호출문의 인수 목록에서 사용하는 경우

```jsx
const arr = [1, 2, 3];

const max = Math.max(arr); // -> NaN
```

> Math.max 메서드는 가변 인자 함수로, 여러개의 숫자를 인수로 전달받아 인수 중에서 최대 값을 반환하는 함수다.

만약 Math.max 메서드에 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없으므로 NaN을 반환한다. 따라서 [1, 2, 3]을 1, 2, 3으로 펼쳐서 인수로 전달해야한다.

```jsx
const arr = [1, 2, 3];

const max = Math.max(...arr); // -> 3
```

> ⚠️ Rest 파라미터( ↔ 스프레드 문법과 반대 개념)  
> 이는 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 `…` 을 붙이는 것이다.
>
> ```jsx
> // Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
> function foo(...rest) {
>   console.log(rest); // 1, 2, 3 -> [ 1, 2, 3 ]
> }
>
> // 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
> foo(...[1, 2, 3]);
> ```

<br/>

# 배열 리터럴 내부에서 사용하는 경우

스프레드 문법을 사용하면 더욱 간결하고 가독성 좋게 표현할 수 있다.
ES5에서 사용하던 방식과 비교하여 살펴보자.

<br/>

## 2개의 배열을 1개의 배열로 결합하고 싶은 경우

### concat (ES5)

```jsx
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]
```

<br/>

### 스프레드 문법 (ES6)

```jsx
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

<br/>

## 어떤 배열의 중간에 다른 배열의 요소들을 추가하거나 제거하는 경우

### splice (ES5)

```jsx
var arr1 = [1, 4];
var arr2 = [2, 3];

arr1.splice(1, 0, arr2);

console.log(arr1); // [1, [2, 3], 4] -> 기대한 결과가 아님
```

```jsx
var arr1 = [1, 4];
var arr2 = [2, 3];

Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]
```

<br/>

### 스프레드 문법 (ES6)

```jsx
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

<br/>

## 배열을 복사하는 경우(얕은 복사)

### slice (ES5)

```jsx
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

<br/>

### 스프레드 문법 (ES6)

```jsx
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

<br/>

## 이터러블을 배열로 변환

### ES5

Function.prototype.apply 또는 Function.prototype.call 메서드를 사용하여 slice 메서드를 호출해야한다.

```jsx
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  var args = Array.prototype.slice.call(arguments);

  return args.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6
```

이 방법으로 이터러블이 아닌 유사 배열 객체도 배열로 변활할 수 있다.

```jsx
// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = Array.prototype.slice.call(arrayLike); // -> [1, 2, 3]
console.log(Array.isArray(arr)); // true
```

<br/>

### 스프레드 문법 (ES6)

```jsx
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

위 예제보다 나은 방법은 Rest 파라미터를 사용하는 것이다.

```jsx
// Rest 파라미터 args는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);

console.log(sum(1, 2, 3)); // 6
```

<br/>

# 객체 리터럴 내부에서 사용하는 경우

2021년 1월 TC39프로세스의 stage 4단계에 제안되어있는 스프레드 프로퍼티를 사용하면  
일반 객체를 대상으로도 스프레드 문법을 사용할 수 있다.(Rest 프로퍼티도 마찬가지)

```jsx
// 스프레드 프로퍼티
// 객체 복사(얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };
console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```

스프레드 프로퍼티가 제안되기 이전에는 ES6에서 도입된 Object.assign 메서드를 사용하여 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.

```jsx
// 객체 병합. 프로퍼티가 중복되는 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
console.log(added); // { x: 1, y: 2, z: 0 }
```
