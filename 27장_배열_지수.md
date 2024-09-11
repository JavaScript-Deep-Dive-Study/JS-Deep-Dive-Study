## 27.1 배열이란?

- 여러 개의 값을 순차적으로 나열한 자료구조
- 사용 빈도가 매우 높은 가장 기본적인 자료구조
  ```jsx
  const arr = ["apple", "banana", "orange"];
  ```
- 요소(element): 배열이 가지고 있는 값 / 인덱스(index): 배열에서 어떤 요소의 위치(0 이상의 정수)
- 대괄호 표기법을 통해 요소에 접근할 수 있음
  ```jsx
  arr[0]; // 'apple'
  arr[1]; // 'banana'
  arr[2]; // 'orange'
  ```
- length 프로퍼티: 배열의 길이를 나타내는 프로퍼티

  ```jsx
  arr.length; // 3

  // 배열의 순회
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 'apple' 'banana' 'orange'
  }
  ```

- 배열 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성 가능

  ```jsx
  const arr = [1, 2, 3];

  arr.constructor === Array; // true
  Object.getPrototypeOf(arr) === Array.prototype; // true
  ```

- 자바스크립트에서 배열은 객체 타입
  ```jsx
  typeof arr; // object
  ```
- “값의 순서”와 “length 프로퍼티”에서 둘은 명확한 차이가 있음

  ```jsx
  const arr = [1, 2, 3];

  // 반복문으로 자료구조를 순서대로 순회하기 위해서는
  // 자료구조의 요소에 순서대로 접근할 수 있어야 하며
  // 자료구조의 길이를 알 수 있어야 함
  for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]); // 1 2 3
  }
  ```

<br>

## 27.2 자바스크립트 배열은 배열이 아니다

- 밀집 배열(dense array)

  - 배열의 요소가 하나의 데이터 타입으로 통일되어 있고 연속적으로 인접
  - 인덱스를 통해 단 한번의 연산으로 특정 요소에 접근 가능 (`O(1)`)
  - 정렬되지 않은 배열의 경우 처음 위치부터 선형적으로 검색해야 함 (`O(n)`)

    ```jsx
    // 선형 검색을 통해 배열(array)에 특정 요소(target)가 존재하는지 확인
    // 배열에 특정 요소가 존재하면 특정 요소의 인덱스를 반환, 존재하지 않으면 -1을 반환
    function linearSearch(array, target) {
      const length = array.length;

      for (let i = 0; i < length; i++) {
        if (array[i] === target) return i;
      }

      return -1;
    }

    console.log(linearSearch([1, 2, 3, 4, 5, 6], 3)); // 2
    console.log(linearSearch([1, 2, 3, 4, 5, 6], 0)); // -1
    ```

- 희소 배열(sparse array)
  - 배열 요소 각각의 메모리 공간은 서로 다를 수 있고 연속적이지 않을 수 있음
  - 자바스크립트의 배열은 일반적인 배열을 흉내낸 특수한 객체
    ```jsx
    // "16.2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체" 참고
    console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
    /*
    {
      '0': {value: 1, writable: true, enumerable: true, configurable: true}
      '1': {value: 2, writable: true, enumerable: true, configurable: true}
      '2': {value: 3, writable: true, enumerable: true, configurable: true}
      length: {value: 3, writable: true, enumerable: false, configurable: false}
    }
    */
    ```
    ```jsx
    const arr = [
      "string",
      10,
      true,
      null,
      undefined,
      NaN,
      Infinity,
      [],
      {},
      function () {},
    ];
    ```

<br>

## 27.3 length 프로퍼티와 희소 배열

- 배열의 길이를 나타내는 0 이상의 정수
  ```jsx
  [].length[(1, 2, 3)].length; // 0 // 3
  ```
- 현재 길이보다 더 작은 값을 length 프로퍼티에 할당하면 길이가 줄어듦

  ```jsx
  const arr = [1, 2, 3, 4, 5];

  // 현재 length 프로퍼티 값인 5보다 작은 숫자 값 3을 length 프로퍼티에 할당
  arr.length = 3;

  // 배열의 길이가 5에서 3으로 줄어듦
  console.log(arr); // [1, 2, 3]
  ```

  ```jsx
  const arr = [1, 2, 3];
  console.log(arr.length); // 3

  // 요소 추가
  arr.push(4);
  // 요소를 추가하면 length 프로퍼티의 값이 자동 갱신됨
  console.log(arr.length); // 4

  // 요소 삭제
  arr.pop();
  // 요소를 삭제하면 length 프로퍼티의 값이 자동 갱신됨
  console.log(arr.length); // 3
  ```

  ```jsx
  // 희소 배열
  const sparse = [, 2, , 4];

  // 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않음
  console.log(sparse.length); // 4
  console.log(sparse); // [empty, 2, empty, 4]

  // 배열 sparse에는 인덱스가 0, 2인 요소가 존재하지 않음
  console.log(Object.getOwnPropertyDescriptors(sparse));
  /*
  {
    '1': { value: 2, writable: true, enumerable: true, configurable: true },
    '3': { value: 4, writable: true, enumerable: true, configurable: true },
    length: { value: 4, writable: true, enumerable: false, configurable: false }
  }
  */
  ```

- 희소 배열은 length와 배열 요소 개수가 일치하지 않음, length가 항상 더 큼

<br>

## 27.4 배열 생성

- 배열 리터럴

  - 가장 일반적이고 간편한 배열 생성 방식

    ```jsx
    const arr = [1, 2, 3];
    console.log(arr.length); // 3
    ```

    ```jsx
    const arr = [];
    console.log(arr.length); // 0
    ```

    ```jsx
    const arr = [1, , 3]; // 희소 배열

    // 희소 배열의 length는 배열의 실제 요소 개수보다 언제나 큼
    console.log(arr.length); // 3
    console.log(arr); // [1, empty, 3]
    console.log(arr[1]); // undefined
    ```

- Array 생성자 함수

  - 예시

    ```jsx
    const arr = new Array(10);

    console.log(arr); // [empty × 10]
    console.log(arr.length); // 10
    ```

    ```jsx
    // 배열은 요소를 최대 4,294,967,295개 가질 수 있음
    new Array(4294967295);

    // 전달된 인수가 0 ~ 4,294,967,295를 벗어나면 RangeError가 발생
    new Array(4294967296); // RangeError: Invalid array length

    // 전달된 인수가 음수이면 에러가 발생
    new Array(-1); // RangeError: Invalid array length
    ```

    ```jsx
    new Array(); // []
    Array(1, 2, 3); // [1, 2, 3]
    ```

    ```jsx
    // 전달된 인수가 2개 이상이면 인수를 요소로 갖는 배열을 생성
    new Array(1, 2, 3); // [1, 2, 3]

    // 전달된 인수가 1개지만 숫자가 아니면 인수를 요소로 갖는 배열을 생성
    new Array({}); // [{}]
    ```

- Array.of

  - 전달된 인수를 요소로 갖는 배열 생성

    ```jsx
    // 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성
    Array.of(1); // [1]

    Array.of(1, 2, 3); // [1, 2, 3]

    Array.of("string"); // ['string']
    ```

- Array.from

  - 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열 생성

    ```jsx
    // 유사 배열 객체를 변환하여 배열을 생성
    Array.from({ length: 2, 0: "a", 1: "b" }); // ['a', 'b']

    // 이터러블을 변환하여 배열을 생성, 문자열은 이터러블
    Array.from("Hello"); // ['H', 'e', 'l', 'l', 'o']
    ```

    ```jsx
    // Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined를 요소로 채움
    Array.from({ length: 3 }); // [undefined, undefined, undefined]

    // Array.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환
    Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
    ```

<br>

## 27.5 배열 요소의 참조

- 대괄호에 인덱스를 넣어 배열의 요소를 참조

  ```jsx
  const arr = [1, 2];

  // 인덱스가 0인 요소를 참조
  console.log(arr[0]); // 1
  // 인덱스가 1인 요소를 참조
  console.log(arr[1]); // 2
  ```

  ```jsx
  const arr = [1, 2];

  // 인덱스가 2인 요소를 참조
  // 배열 arr에는 인덱스가 2인 요소가 존재하지 않음
  console.log(arr[2]); // undefined
  ```

  ```jsx
  // 희소 배열
  const arr = [1, , 3];

  // 배열 arr에는 인덱스가 1인 요소가 존재하지 않음
  console.log(Object.getOwnPropertyDescriptors(arr));
  /*
  {
    '0': {value: 1, writable: true, enumerable: true, configurable: true},
    '2': {value: 3, writable: true, enumerable: true, configurable: true},
    length: {value: 3, writable: true, enumerable: false, configurable: false}
  */

  // 존재하지 않는 요소를 참조하면 undefined가 반환
  console.log(arr[1]); // undefined
  console.log(arr[3]); // undefined
  ```

<br>

## 27.6 배열 요소의 추가와 갱신

- 존재하지 않는 인덱스를 통해 값을 할당하면 새로운 요소가 추가됨
- 동시에 length 프로퍼티의 값이 자동 갱신됨

  ```jsx
  const arr = [0];

  // 배열 요소의 추가
  arr[1] = 1;

  console.log(arr); // [0, 1]
  console.log(arr.length); // 2
  ```

  ```jsx
  arr[100] = 100;

  console.log(arr); // [0, 1, empty × 98, 100]
  console.log(arr.length); // 101
  ```

  ```jsx
  // 명시적으로 값을 할당하지 않은 요소는 생성되지 않음
  console.log(Object.getOwnPropertyDescriptors(arr));
  /*
  {
    '0': {value: 0, writable: true, enumerable: true, configurable: true},
    '1': {value: 1, writable: true, enumerable: true, configurable: true},
    '100': {value: 100, writable: true, enumerable: true, configurable: true},
    length: {value: 101, writable: true, enumerable: false, configurable: false}
  */
  ```

  ```jsx
  // 요소값의 갱신
  arr[1] = 10;

  console.log(arr); // [0, 10, empty × 98, 100]
  ```

<br>

## 27.7 배열 요소의 삭제

- 배열은 “객체”이기 때문에 delete 연산자를 사용할 수 있음

  - length 프로퍼티에 영향이 없고 희소 배열이 되기 때문에 사용하지 않는 것이 좋긴 함

    ```jsx
    const arr = [1, 2, 3];

    // 배열 요소의 삭제
    delete arr[1];
    console.log(arr); // [1, empty, 3]

    // length 프로퍼티에 영향을 주지 않음 -> 희소 배열이 됨
    console.log(arr.length); // 3
    ```

- Array.prototype.splice 메서드를 사용하는 것을 권장

  ```jsx
  const arr = [1, 2, 3];

  // Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
  // arr[1]부터 1개의 요소를 제거
  arr.splice(1, 1);
  console.log(arr); // [1, 3]

  // length 프로퍼티가 자동 갱신
  console.log(arr.length); // 2
  ```

<br>

## 27.8 배열 메서드

- 원본 배열을 직접 변경하는 메서드(mutator method)와 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드(accessor method)가 있음 → 후자 사용을 더 권장

  ```jsx
  const arr = [1];

  // push 메서드는 원본 배열(arr)을 직접 변경
  arr.push(2);
  console.log(arr); // [1, 2]

  // concat 메서드는 원본 배열(arr)을 직접 변경하지 않고 새로운 배열을 생성하여 반환
  const result = arr.concat(3);
  console.log(arr); // [1, 2]
  console.log(result); // [1, 2, 3]
  ```

- Array.isArray

  - 전달된 인수의 배열 여부를 boolean으로 반환

    ```jsx
    // true
    Array.isArray([]);
    Array.isArray([1, 2]);
    Array.isArray(new Array());

    // false
    Array.isArray();
    Array.isArray({});
    Array.isArray(null);
    Array.isArray(undefined);
    Array.isArray(1);
    Array.isArray("Array");
    Array.isArray(true);
    Array.isArray(false);
    Array.isArray({ 0: 1, length: 1 });
    ```

- Array.prototype.indexOf

  - 원본 배열에서 인수로 전달된 요소의 인덱스를 반환

    ```jsx
    const arr = [1, 2, 2, 3];

    // 배열 arr에서 요소 2를 검색하여 첫 번째로 검색된 요소의 인덱스를 반환
    arr.indexOf(2); // 1
    // 배열 arr에 요소 4가 없으므로 -1을 반환
    arr.indexOf(4); // -1
    // 두 번째 인수는 검색을 시작할 인덱스, 두 번째 인수를 생략하면 처음부터 검색
    arr.indexOf(2, 2); // 2
    ```

    ```jsx
    const foods = ["apple", "banana", "orange"];

    // foods 배열에 'orange' 요소가 존재하는지 확인
    if (foods.indexOf("orange") === -1) {
      // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가
      foods.push("orange");
    }

    console.log(foods); // ["apple", "banana", "orange"]
    ```

    ```jsx
    const foods = ["apple", "banana"];

    // foods 배열에 'orange' 요소가 존재하는지 확인
    if (!foods.includes("orange")) {
      // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가
      foods.push("orange");
    }

    console.log(foods); // ["apple", "banana", "orange"]
    ```

- Array.prototype.push

  - 인수를 배열의 마지막 요소로 추가

    ```jsx
    const arr = [1, 2];

    // 인수로 전달받은 모든 값을 원본 배열 arr의 마지막 요소로 추가하고 변경된 length 값을 반환
    let result = arr.push(3, 4);
    console.log(result); // 4

    // push 메서드는 원본 배열을 직접 변경
    console.log(arr); // [1, 2, 3, 4]
    ```

  - push 메서드보다 length 프로퍼티로 마지막에 직접 추가하는 것이 성능적으로는 더 좋음

    ```jsx
    const arr = [1, 2];

    // arr.push(3)과 동일한 처리, 이 방법이 push 메서드보다 빠름
    arr[arr.length] = 3;
    console.log(arr); // [1, 2, 3]
    ```

  - push 메서드는 원본 배열을 직접 변경하므로 ES6 스프레드 문법을 사용하는 것이 좋음

    ```jsx
    const arr = [1, 2];

    // ES6 스프레드 문법
    const newArr = [...arr, 3];
    console.log(newArr); // [1, 2, 3]
    ```

- Array.prototype.pop

  - 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환
  - 빈 배열이면 undefined를 반환, 원본 배열을 직접 변경

    ```jsx
    const arr = [1, 2];

    // 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환
    let result = arr.pop();
    console.log(result); // 2

    // pop 메서드는 원본 배열을 직접 변경
    console.log(arr); // [1]
    ```

  - push와 pop을 통해 스택 자료구조 구현 가능

    ```jsx
    const Stack = (function () {
      function Stack(array = []) {
        if (!Array.isArray(array)) {
          // "47. 에러 처리" 참고
          throw new TypeError(`${array} is not an array.`);
        }
        this.array = array;
      }

      Stack.prototype = {
        // "19.10.1. 생성자 함수에 의한 프로토타입의 교체" 참고
        constructor: Stack,
        // 스택의 가장 마지막에 데이터를 밀어넣음
        push(value) {
          return this.array.push(value);
        },
        // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼냄
        pop() {
          return this.array.pop();
        },
        // 스택의 복사본 배열을 반환
        entries() {
          return [...this.array];
        },
      };

      return Stack;
    })();

    const stack = new Stack([1, 2]);
    console.log(stack.entries()); // [1, 2]

    stack.push(3);
    console.log(stack.entries()); // [1, 2, 3]

    stack.pop();
    console.log(stack.entries()); // [1, 2]
    ```

    ```jsx
    class Stack {
      #array; // private class member

      constructor(array = []) {
        if (!Array.isArray(array)) {
          throw new TypeError(`${array} is not an array.`);
        }
        this.#array = array;
      }

      // 스택의 가장 마지막에 데이터를 밀어넣음
      push(value) {
        return this.#array.push(value);
      }

      // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼냄
      pop() {
        return this.#array.pop();
      }

      // 스택의 복사본 배열을 반환
      entries() {
        return [...this.#array];
      }
    }

    const stack = new Stack([1, 2]);
    console.log(stack.entries()); // [1, 2]

    stack.push(3);
    console.log(stack.entries()); // [1, 2, 3]

    stack.pop();
    console.log(stack.entries()); // [1, 2]
    ```

- Array.prototype.unshift

  - 인수를 원본 배열의 선두에 추가, 원본 배열을 직접 변경

    ```jsx
    const arr = [1, 2];

    // 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 값을 반환
    let result = arr.unshift(3, 4);
    console.log(result); // 4

    // unshift 메서드는 원본 배열을 직접 변경
    console.log(arr); // [3, 4, 1, 2]
    ```

  - ES6의 스프레드 문법을 사용하는 것이 좋음

    ```jsx
    const arr = [1, 2];

    // ES6 스프레드 문법
    const newArr = [3, ...arr];
    console.log(newArr); // [3, 1, 2]
    ```

- Array.prototype.shift

  - 원본 배열에서 첫 번째 요소를 제거하고 제거된 요소를 반환, 원본 배열을 직접 변경

    ```jsx
    const arr = [1, 2];

    // 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환
    let result = arr.shift();
    console.log(result); // 1

    // shift 메서드는 원본 배열을 직접 변경
    console.log(arr); // [2]
    ```

  - shift와 push를 통해 자료구조 큐 규현 가능

    ```jsx
    const Queue = (function () {
      function Queue(array = []) {
        if (!Array.isArray(array)) {
          // "47. 에러 처리" 참고
          throw new TypeError(`${array} is not an array.`);
        }
        this.array = array;
      }

      Queue.prototype = {
        // "19.10.1. 생성자 함수에 의한 프로토타입의 교체" 참고
        constructor: Queue,
        // 큐의 가장 마지막에 데이터를 밀어넣음
        enqueue(value) {
          return this.array.push(value);
        },
        // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 꺼냄
        dequeue() {
          return this.array.shift();
        },
        // 큐의 복사본 배열을 반환
        entries() {
          return [...this.array];
        },
      };

      return Queue;
    })();

    const queue = new Queue([1, 2]);
    console.log(queue.entries()); // [1, 2]

    queue.enqueue(3);
    console.log(queue.entries()); // [1, 2, 3]

    queue.dequeue();
    console.log(queue.entries()); // [2, 3]
    ```

    ```jsx
    class Queue {
      #array; // private class member

      constructor(array = []) {
        if (!Array.isArray(array)) {
          throw new TypeError(`${array} is not an array.`);
        }
        this.#array = array;
      }

      // 큐의 가장 마지막에 데이터를 밀어넣음
      enqueue(value) {
        return this.#array.push(value);
      }

      // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 꺼냄
      dequeue() {
        return this.#array.shift();
      }

      // 큐의 복사본 배열을 반환
      entries() {
        return [...this.#array];
      }
    }

    const queue = new Queue([1, 2]);
    console.log(queue.entries()); // [1, 2]

    queue.enqueue(3);
    console.log(queue.entries()); // [1, 2, 3]

    queue.dequeue();
    console.log(queue.entries()); // [2, 3]
    ```

- Array.prototype.concat

  - 값 또는 배열을 인수로 전달받아 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환

    ```jsx
    const arr1 = [1, 2];
    const arr2 = [3, 4];

    // 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
    // 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가
    let result = arr1.concat(arr2);
    console.log(result); // [1, 2, 3, 4]

    // 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
    result = arr1.concat(3);
    console.log(result); // [1, 2, 3]

    // 배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
    result = arr1.concat(arr2, 5);
    console.log(result); // [1, 2, 3, 4, 5]

    // 원본 배열은 변경되지 않음
    console.log(arr1); // [1, 2]
    ```

  - 새로운 배열을 반환하며 반환값을 반드시 변수에 할당해야 함

    ```jsx
    const arr1 = [3, 4];

    // unshift 메서드는 원본 배열을 직접 변경함
    // 따라서 원본 배열을 변수에 저장해 두지 않으면 변경된 배열을 사용할 수 없음
    arr1.unshift(1, 2);
    // unshift 메서드를 사용할 경우, 원본 배열을 반드시 변수에 저장해 두어야 결과 확인 가능
    console.log(arr1); // [1, 2, 3, 4]

    // push 메서드는 원본 배열을 직접 변경
    // 따라서 원본 배열을 변수에 저장해 두지 않으면 변경된 배열을 사용할 수 없음
    arr1.push(5, 6);
    // push 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 결과 확인 가능
    console.log(arr1); // [1, 2, 3, 4, 5, 6]

    // unshift와 push 메서드는 concat 메서드로 대체할 수 있음
    const arr2 = [3, 4];

    // concat 메서드는 원본 배열을 변경하지 않고 새로운 배열을 반환
    // arr1.unshift(1, 2)를 다음과 같이 대체할 수 있음
    let result = [1, 2].concat(arr2);
    console.log(result); // [1, 2, 3, 4]

    // arr1.push(5, 6)를 다음과 같이 대체할 수 있음
    result = result.concat(5, 6);
    console.log(result); // [1, 2, 3, 4, 5, 6]
    ```

    ```jsx
    const arr = [3, 4];

    // unshift와 push 메서드는 인수로 전달받은 배열을 그대로 원본 배열의 요소로 추가
    arr.unshift([1, 2]);
    arr.push([5, 6]);
    console.log(arr); // [[1, 2], 3, 4,[5, 6]]

    // concat 메서드는 인수로 전달받은 배열을 해체하여 새로운 배열의 요소로 추가
    let result = [1, 2].concat([3, 4]);
    result = result.concat([5, 6]);

    console.log(result); // [1, 2, 3, 4, 5, 6]
    ```

  - ES6의 스프레드 문법으로 대체 가능

    ```jsx
    let result = [1, 2].concat([3, 4]);
    console.log(result); // [1, 2, 3, 4]

    // concat 메서드는 ES6의 스프레드 문법으로 대체 가능
    result = [...[1, 2], ...[3, 4]];
    console.log(result); // [1, 2, 3, 4]

    // push, pop, unshift 등의 메서드보다는 ES6의 스프레드 문법을 일관성있게 사용하는 것이 더 좋음
    ```

- Array.prototype.splice

  - 원본 배열의 중간에 있는 요소를 추가 또는 제거할 때 사용

    ```jsx
    const arr = [1, 2, 3, 4];

    // 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입
    const result = arr.splice(1, 2, 20, 30);

    // 제거한 요소가 배열로 반환
    console.log(result); // [2, 3]
    // splice 메서드는 원본 배열을 직접 변경
    console.log(arr); // [1, 20, 30, 4]
    ```

    ```jsx
    const arr = [1, 2, 3, 4];

    // 원본 배열의 인덱스 1부터 0개의 요소를 제거하고 그 자리에 새로운 요소 100을 삽입
    const result = arr.splice(1, 0, 100);

    // 원본 배열이 변경
    console.log(arr); // [1, 100, 2, 3, 4]
    // 제거한 요소가 배열로 반환
    console.log(result); // []
    ```

    ```jsx
    const arr = [1, 2, 3, 4];

    // 원본 배열의 인덱스 1부터 2개의 요소를 제거
    const result = arr.splice(1, 2);

    // 원본 배열이 변경
    console.log(arr); // [1, 4]
    // 제거한 요소가 배열로 반환
    console.log(result); // [2, 3]
    ```

    ```jsx
    const arr = [1, 2, 3, 4];

    // 원본 배열의 인덱스 1부터 모든 요소를 제거
    const result = arr.splice(1);

    // 원본 배열이 변경
    console.log(arr); // [1]
    // 제거한 요소가 배열로 반환
    console.log(result); // [2, 3, 4]
    ```

  - indexOf로 특정 요소의 인덱스를 알아내서 제거 가능
  - 중복된 특정 요소를 전부 제거하려면 filter 메서드를 사용할 수 있음

- Array.prototype.slice

  - 인수로 전달된 범위의 요소들을 복사하여 배열로 반환
  - 복사를 시작할 인덱스(start), 복사를 종료할 인덱스(end)를 매개 변수로 받음

    ```jsx
    const arr = [1, 2, 3];

    // arr[0]부터 arr[1] 이전(arr[1] 미포함)까지 복사하여 반환
    arr.slice(0, 1); // [1]

    // arr[1]부터 arr[2] 이전(arr[2] 미포함)까지 복사하여 반환
    arr.slice(1, 2); // [2]

    // 원본은 변경되지 않음
    console.log(arr); // [1, 2, 3]
    ```

    ```jsx
    const arr = [1, 2, 3];

    // 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환
    const copy = arr.slice();
    console.log(copy); // [1, 2, 3]
    console.log(copy === arr); // false
    ```

- Array.prototype.join

  - 원본 배열의 요소를 문자로 변환하고 인수로 전달 받은 문자열을 구분자로 연결하여 반환

    ```jsx
    const arr = [1, 2, 3, 4];

    // 기본 구분자는 ','
    // 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 기본 구분자 ','로 연결한 문자열을 반환
    arr.join(); // '1,2,3,4'

    // 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 빈 문자열로 연결한 문자열을 반환
    arr.join(""); // '1234'

    // 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 구분자 ':'로 연결한 문자열을 반환
    arr.join(":"); // '1:2:3:4'
    ```

- Array.prototype.reverse

  - 원본 배열의 순서를 반대로 뒤집어서 원본 배열이 변경됨

    ```jsx
    const arr = [1, 2, 3];
    const result = arr.reverse();

    // reverse 메서드는 원본 배열을 직접 변경
    console.log(arr); // [3, 2, 1]
    // 반환값은 변경된 배열
    console.log(result); // [3, 2, 1]
    ```

- Array.prototype.fill

  - ES6에서부터 도입, 인수로 전달받은 값으로 배열의 처음부터 끝까지 변경

    ```jsx
    const arr = [1, 2, 3];

    // 인수로 전달 받은 값 0을 배열의 처음부터 끝까지 요소로 채움
    arr.fill(0);

    // fill 메서드는 원본 배열을 직접 변경
    console.log(arr); // [0, 0, 0]
    ```

    ```jsx
    const arr = [1, 2, 3, 4, 5];

    // 인수로 전달받은 값 0을 배열의 인덱스 1부터 3 이전(인덱스 3 미포함)까지 요소로 채움
    arr.fill(0, 1, 3);

    // fill 메서드는 원본 배열을 직접 변경
    console.log(arr); // [1, 0, 0, 4, 5]
    ```

  - Array.from 메서드와 함께 사용할 경우, 새로운 배열을 만들면서 요소를 채울 수 있음

    ```jsx
    // 인수로 전달받은 정수만큼 요소를 생성하고 0부터 1씩 증가하면서 요소를 채움
    const sequences = (length = 0) => Array.from({ length }, (_, i) => i);
    // const sequences = (length = 0) => Array.from(new Array(length), (_, i) => i);

    console.log(sequences(3)); // [0, 1, 2]
    ```

- Array.prototype.includes

  - 배열 내에 특정 요소가 포함되어 있는지 확인하여 true 또는 false를 반환

    ```jsx
    const arr = [1, 2, 3];

    // 배열에 요소 2가 포함되어 있는지 확인
    arr.includes(2); // true

    // 배열에 요소 100이 포함되어 있는지 확인
    arr.includes(100); // false
    ```

- Array.prototype.flat

  - 인수로 전달한 깊이 만큼 재귀적으로 배열을 평탄화

    ```jsx
    [1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]

    // 중첩 배열을 평탄화하기 위한 깊이 값의 기본값은 1
    [1, [2, [3, [4]]]].flat(); // [1, 2, [3, [4]]]
    [1, [2, [3, [4]]]].flat(1); // [1, 2, [3, [4]]]

    // 중첩 배열을 평탄화하기 위한 깊이 값을 2로 지정하여 2단계 깊이까지 평탄화
    [1, [2, [3, [4]]]].flat(2); // [1, 2, 3, [4]]
    // 2번 평탄화한 것과 동일하다.
    [1, [2, [3, [4]]]].flat().flat(); // [1, 2, 3, [4]]

    // 중첩 배열을 평탄화하기 위한 깊이 값을 Infinity로 지정하여 중첩 배열 모두를 평탄화
    [1, [2, [3, [4]]]].flat(Infinity); // [1, 2, 3, 4]
    ```

<br>

## 27.9 배열 고차 함수

- 고차 함수(Higher-Order Function, HOF)
  - 함수를 인수로 전달받거나 함수를 반환하는 함수
  - 외부 상태의 변경이나 가변 데이터를 피하고, 불변성을 지향하는 함수형 프로그래밍에 기반을 둠
- Array.prototype.sort

  - 배열의 요소를 정렬
  - sort의 기본 정렬 순서는 유니코드 코드 포인트의 순서를 따름
  - 배열의 요소를 일시적으로 문자열로 변환한 후 정렬하는데, 숫자의 경우 의도치 않은 결과 나오기도 함

    - 숫자 정렬 시 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 함
    - 양수: 두 번째 인수 우선 / 음수: 첫 번째 인수 우선 / 0: 정렬 X

      ```jsx
      const points = [40, 100, 1, 5, 2, 25, 10];

      // 숫자 배열의 오름차순 정렬
      // 비교 함수의 반환값이 0보다 작으면 a를 우선하여 정렬
      points.sort((a, b) => a - b);
      console.log(points); // [1, 2, 5, 10, 25, 40, 100]

      // 숫자 배열에서 최소/최대값 취득
      console.log(points[0], points[points.length]); // 1

      // 숫자 배열의 내림차순 정렬
      // 비교 함수의 반환값이 0보다 작으면 b를 우선하여 정렬
      points.sort((a, b) => b - a);
      console.log(points); // [100, 40, 25, 10, 5, 2, 1]

      // 숫자 배열에서 최대값 취득
      console.log(points[0]); // 100
      ```

      ```jsx
      const todos = [
        { id: 4, content: "JavaScript" },
        { id: 1, content: "HTML" },
        { id: 2, content: "CSS" },
      ];

      // 비교 함수, 매개변수 key는 프로퍼티 키
      function compare(key) {
        // 프로퍼티 값이 문자열인 경우, 산술 연산으로 비교하면 NaN이 나오므로 비교 연산을 사용
        // 비교 함수는 양수/음수/0을 반환하면 됨, 산술 연산 대신 비교 연산을 사용할 수 있음
        return (a, b) => (a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0);
      }

      // id를 기준으로 오름차순 정렬
      todos.sort(compare("id"));
      console.log(todos);
      /*
      [
        { id: 1, content: 'HTML' },
        { id: 2, content: 'CSS' },
        { id: 4, content: 'JavaScript' }
      ]
      */

      // content를 기준으로 오름차순 정렬
      todos.sort(compare("content"));
      console.log(todos);
      /*
      [
        { id: 2, content: 'CSS' },
        { id: 1, content: 'HTML' },
        { id: 4, content: 'JavaScript' }
      ]
      */
      ```

- Array.prototype.forEach

  - for문을 대체할 수 있는 고차 함수
  - 내부에서 반복문을 통해 자신을 호출한 배열을 순회
  - 수행해야 할 처리를 콜백 함수로 전달받아 반복적으로 호출

    ```jsx
    const numbers = [1, 2, 3];
    let pows = [];

    // forEach 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출
    numbers.forEach((item) => pows.push(item ** 2));
    console.log(pows); // [1, 4, 9]
    ```

  - ES6의 화살표 함수와 같이 사용하면 this의 바인딩 이슈도 깔끔하게 해결하면서 사용 가능

    ```jsx
    class Numbers {
      numberArray = [];

      multiply(arr) {
        // 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조
        arr.forEach((item) => this.numberArray.push(item * item));
      }
    }

    const numbers = new Numbers();
    numbers.multiply([1, 2, 3]);
    console.log(numbers.numberArray); // [1, 4, 9]
    ```

  - forEach문 = 함수 → return으로 탈출 가능

    ```jsx
    [1, 2, 3].forEach(item => {
      console.log(item);
      if (item > 1) break; // SyntaxError: Illegal break statement
    });

    [1, 2, 3].forEach(item => {
      console.log(item);
      if (item > 1) continue;
      // SyntaxError: Illegal continue statement: no surrounding iteration statement
    });
    ```

- Array.prototype.map

  - 자신을 호출한 배열을 순회하면서 콜백 함수의 반환값들로 구성된 새로운 배열을 반환

    ```jsx
    const numbers = [1, 4, 9];

    // map 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출
    // 콜백 함수의 반환값들로 구성된 새로운 배열을 반환
    const roots = numbers.map((item) => Math.sqrt(item));

    // 위 코드는 다음과 같음
    // const roots = numbers.map(Math.sqrt);

    // map 메서드는 새로운 배열을 반환
    console.log(roots); // [ 1, 2, 3 ]
    // map 메서드는 원본 배열을 변경 X
    console.log(numbers); // [ 1, 4, 9 ]
    ```

  - map 메서드가 반환한 새로운 배열은 기존의 배열과 반드시 1대 1 매핑됨
    ```jsx
    // map 메서드는 콜백 함수를 호출하면서 3개(요소값, 인덱스, this)의 인수를 전달
    [1, 2, 3].map((item, index, arr) => {
      console.log(
        `요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`
      );
      return item;
    });
    /*
    요소값: 1, 인덱스: 0, this: [1,2,3]
    요소값: 2, 인덱스: 1, this: [1,2,3]
    요소값: 3, 인덱스: 2, this: [1,2,3]
    */
    ```

- Array.prototype.filter

  - 자신을 호출한 배열을 순회하면서 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환

    ```jsx
    const numbers = [1, 2, 3, 4, 5];

    // filter 메서드는 numbers 배열의 모든 요소를 순회하면서 콜백 함수를 반복 호출
    // 콜백 함수의 반환값이 true인 요소로만 구성된 새로운 배열을 반환
    // 다음의 경우 numbers 배열에서 홀수인 요소만을 필터링(1은 true로 평가)
    const odds = numbers.filter((item) => item % 2);
    console.log(odds); // [1, 3, 5]
    ```

  - map과 달리 filter가 반환한 새로운 배열은 기존의 배열과 길이가 다를 수 있음

- Array.prototype.reduce

  - 자신을 호출한 배열을 순회하면서 콜백 함수를 호출
  - 콜백 함수의 반환값은 다음 순회에서의 콜백 함수의 첫 번째 인수로 전달됨
  - 순회하면서 최종적으로는 하나의 결과값을 반환

    ```jsx
    // [1, 2, 3, 4]의 모든 요소의 누적을 구함
    const sum = [1, 2, 3, 4].reduce(
      (accumulator, currentValue, index, array) => accumulator + currentValue,
      0
    );

    console.log(sum); // 10
    ```

  - 평균 구하기, 최대값 구하기, 요소의 중복 횟수 구하기, 중첩 배열 평탄화, 중복 요소 제거 등의 문제에서 활용 가능

- Array.prototype.some

  - 자신을 호출한 배열을 순회하면서 콜백 함수를 호출
  - 콜백 함수 반환값이 단 한 번이라도 참이면 true, 거짓이면 false를 반환

    ```jsx
    // 배열의 요소 중에 10보다 큰 요소가 1개 이상 존재하는지 확인
    [5, 10, 15].some((item) => item > 10); // true

    // 배열의 요소 중에 0보다 작은 요소가 1개 이상 존재하는지 확인
    [5, 10, 15].some((item) => item < 0); // false

    // 배열의 요소 중에 'banana'가 1개 이상 존재하는지 확인
    ["apple", "banana", "mango"].some((item) => item === "banana"); // true

    // some 메서드를 호출한 배열이 빈 배열인 경우 언제나 false를 반환
    [].some((item) => item > 3); // false
    ```

- Array.prototype.every

  - 자신을 호출한 배열을 순회하면서 콜백 함수를 호출
  - 콜백 함수 반환값이 모두 참이면 true, 거짓이면 false를 반환

    ```jsx
    // 배열의 모든 요소가 3보다 큰지 확인
    [5, 10, 15].every((item) => item > 3); // true

    // 배열의 모든 요소가 10보다 큰지 확인
    [5, 10, 15].every((item) => item > 10); // false

    // every 메서드를 호출한 배열이 빈 배열인 경우 언제나 true를 반환
    [].every((item) => item > 3); // true
    ```

- Array.prototype.find

  - 자신을 호출한 배열을 순회하면서 콜백 함수를 호출하면서 반환값이 true인 첫 번째 요소를 반환

    ```jsx
    const users = [
      { id: 1, name: "Lee" },
      { id: 2, name: "Kim" },
      { id: 2, name: "Choi" },
      { id: 3, name: "Park" },
    ];

    // id가 2인 첫 번째 요소를 반환, find 메서드는 배열이 아니라 요소를 반환
    users.find((user) => user.id === 2); // {id: 2, name: 'Kim'}
    ```

- Array.prototype.findIndex

  - 자신을 호출한 배열을 순회하면서 콜백 함수를 호출
  - 반환값이 true인 첫 번째 요소의 인덱스를 반환

    ```jsx
    const users = [
      { id: 1, name: "Lee" },
      { id: 2, name: "Kim" },
      { id: 2, name: "Choi" },
      { id: 3, name: "Park" },
    ];

    // id가 2인 요소의 인덱스를 구함
    users.findIndex((user) => user.id === 2); // 1

    // name이 'Park'인 요소의 인덱스를 구함
    users.findIndex((user) => user.name === "Park"); // 3

    // 위와 같이 프로퍼티 키와 프로퍼티 값으로 요소의 인덱스를 구하는 경우, 다음과 같이 콜백 함수를 추상화할 수 있음
    function predicate(key, value) {
      // key와 value를 기억하는 클로저를 반환
      return (item) => item[key] === value;
    }

    // id가 2인 요소의 인덱스를 구함
    users.findIndex(predicate("id", 2)); // 1

    // name이 'Park'인 요소의 인덱스를 구함
    users.findIndex(predicate("name", "Park")); // 3
    ```

- Array.prototype.flatMap

  - map 메서드를 통해 생성된 새로운 배열을 평탄화 → map 메서드와 flat 메서드를 순차적으로 실행

    ```jsx
    const arr = ["hello", "world"];

    // map과 flat을 순차적으로 실행
    arr.map((x) => x.split("")).flat();
    // ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']

    // flatMap은 map을 통해 생성된 새로운 배열을 평탄화
    arr.flatMap((x) => x.split(""));
    // ['h', 'e', 'l', 'l', 'o', 'w', 'o', 'r', 'l', 'd']
    ```
