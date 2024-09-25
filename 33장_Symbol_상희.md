# 심벌이란?

ES6에서 도입된 7번째 데이터 타입으로 **변경 불가능한 원시 타입의 값**이다.

다른 값과 **중복되지 않는 유일무이한 값**이므로, **주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용**한다.

<br/>

# 심벌 값의 생성

## Symbol 함수

심벌 값은 Symbol 함수를 호출하여 생성한다.
이때 생성된 Symbol은 객체가 아니라 변경 불가능한 원시 타입의 값이다.

```jsx
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()
```

Symbol 함수에는 선택적으로 문자열을 인수로 전달할 수 있는데, 이는 설명으로 디버깅 용도, 이름표 용도로만 사용된다.  
​심볼은 유일성이 보장되는 자료형이기 때문에, 설명이 동일한 심볼을 여러 개 만들어도 각 심볼값은 다르다.

```jsx
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1); // Symbol(mySymbol)
console.log(mySymbol2); // Symbol(mySymbol)
console.log(mySymbol1 === mySymbol2); // false
```

심벌 값도 문자열, 숫자, 불리언과 같이 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.

```jsx
const mySymbol = Symbol("mySymbol");

// 심벌도 레퍼 객체를 생성한다
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
단, 불리언 타입으로는 암묵적으로 타입 변환된다.

```jsx
const mySymbol = Symbol();

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인을 위해 사용할 수 있다.
if (mySymbol) console.log("mySymbol is not empty.");
```

<br/>

## Symbol.for 메서드

Symbol() 은 고유한 심볼을 반환한다.

하지만 Symbol.for() 는 전역 심벌 레지스트리의 목록을 참조한다.  
Symbol.for 메서드는 인수로 전달받은 문자열을 키로 사용하여 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

```jsx
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
console.log(s1); // Symbol(mySymbol)
console.log(s2); // Symbol(mySymbol)
```

<br/>

## Symbol.keyFor 메서드

Symbol.keyFor 메서드는 심볼 값을 인수로 받아서 전역 심볼 레지스트리에 저장된 심볼 값 키를 가져올 수 있다.

```jsx
const s1 = Symbol.for("mySymbol");
Symbol.keyFor(s1); // -> mySymbol
```

<br/>

# 심벌과 상수

변경/중복될 가능성이 있는 무의미한 상수 대신 중복될 가능성이 없는 유일무이한 심벌 값을 사용할 수 있다.

```jsx
// 위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의한다.
// 이때 값 1, 2, 3, 4에는 특별한 의미가 없고 상수 이름에 의미가 있다.
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};

// 변수에 상수를 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going UP.");
}
```

```jsx
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going UP.");
}
```

<br/>

# 심벌과 프로퍼티 키

심벌 값을 프로퍼티 키로 사용하려면 **프로퍼티 키로 사용할 심벌 값에 대괄호를 사용**해야 한다. 접근할때도 마찬가지다.

심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는 장점이 있다.

```jsx
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for("mySymbol")]: 1,
};

obj[Symbol.for("mySymbol")]; // -> 1
```

<br/>

# 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for…in 문이나 Object.keys, Object.getOwnProPertyNames 메서드로 찾을 수 없다.

심벌로 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.

```jsx
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol("mySymbol")]: 1,
};

for (const key in obj) {
  console.log(key); // 아무것도 출력되지 않는다.
}

console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []
```

하지만 ES6에서 도입된 getOwnPropertySymbols 메서드로 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```jsx
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol("mySymbol")]: 1,
};

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

<br/>

# 심벌과 표준 빌트인 객체 확장

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.

표준 빌트인 객체는 읽기 전용으로 사용하는 것이 좋다. 개발자가 직접 추가한 메서드와 미래에 표준 사양으로 추가될 메서드의 이름이 중복될 수 있기 때문이다.

하지만 **심벌 값으로 프로퍼티 키를 생성하면 안전하게 표준 빌트인 객체를 확장할 수 있다.**

```jsx
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for("sum")](); // -> 3
```

<br/>

# Well-known Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값이 있는데, 이는 Symbol 함수의 프로퍼티에 할당되어 있다. 이 값을 Well-known Symbol이라 부른다.

만약 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면,  
 Symbol.iterator를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 된다.

```jsx
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      },
    };
  },
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```

Symbol.iterator는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않는다.

이처럼 심벌을 활용하면 중복되지 않는 상수 값을 생성하고 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해(하위 호환성 보장) 도입되었다.
