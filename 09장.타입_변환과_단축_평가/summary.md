## 암묵적 타입 변환

> javascript 엔진에 의해 자동으로 타입이 변환 (문맥으로 판단)

### 문자열 타입으로 변환

```jsx
`1 + 1` = ${1 + 1} // '1 + 1 = 2'

//숫자 타입
0 + ''; // '0'
-0 + ''; // '0'
1 + ''; //'1'
-1 + ''; // '-1'
NaN + ''; // 'NaN'
Infinity + '' // 'Infinity'

true + '' // 'true'
false + '' // 'false'

null + ' // 'null'
undefined + '' // 'undefined'

(Symbol()) + '' // TypeError

({}) + '' // "[Object Object]"
Math + '' // "[Object Math]"

[] + ''  // ""
[10, 20] + '' // "10, 20"
```

### 숫자 타입으로 변환

```jsx
1 - "1"; //0
1 / "one" + // NaN
  "" + // 0
  "string" + //NaN
  true + // 1
  false + // 0
  null + // 0
  undefined + // Nan
  Symbol() + // TypeError
  []; // 0
```

### boolean 타입으로 변환

```jsx
if ("") return 0; //falsy
if (true) return 1; //truthy
if (0) return 0; //falsy
if ("str") return 1; //truthy
if (null) return 0; //falsy
```

> **🚀 팀원들의 생각 🚀**
>
> - 암묵적 타입 변환은 지양해야 할 것 같다.
>
> - 암묵적 타입 변환을 이용하는 의도가 있을 수 있지만, 의도가 아닌 경우에는 디버깅 하기 힘들 것 같다. (정신건강에는 타입스크립트를 사용하는게 좋을 듯 하긴 하다..)
>
> - Dart 언어를 사용했을 때, 해당 개념을 많이 이용해서 코딩했었다.
>
> - 예제 09-06에서 객체 타입을 암묵적 타입 변환할 때는 그 자체를 문자열로 처리하지 않는 이유는 자바스크립트에서 객체 혹은 배열을 문자열로 변환할 때는 toString를 호출하여 문자열로 변환한다고 한다.

## 명시적 타입 변환

> 의도에 따라 명시적으로 타입을 변환

### 문자열 타입으로 변환

```jsx
// String 생성자 함수를 new 연산자 없이 호출하는 방법
String(1)(
  // '1'

  // Object.prototype.toString 메서드를 사용하는 방법
  1
).toString(); // '1'

// 문자열 연결 연산자를 이용하는 방법
1 + ""; // '1'
```

### 숫자 타입으로 변환

```jsx
// Number 생성자 함수를 new 연산자 없이 호출하는 방법
Number("0"); // 0

// parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
parseInt("0") + // 0
  // + 단항 산술 연산자를 이용하는 방법
  "0"; // 0

// * 산술 연산자를 이용하는 방법
"0" * 1; // 0
```

### boolean 타입으로 변환

```jsx
//Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
Boolean("x"); // true
Boolean(""); // false

Boolean(Infinity); // true, Infinity : 매우 큰 양수

// !부정 논리 연산자를 두 번 사용하는 방법
// 첫 번째 !는 값을 부정하여 불리언으로 변환하고, 두 번째 !는 그 결과를 다시 부정하여 원래의 논리값
!!"x"; //true
!!""; //false
```

## 단축 평가

> 표현식을 평가하는 도중에 결과가 확정되면 종료

### 논리 연산자를 사용한 단축 평가

- `‘고양이’ && ‘강아지’` → `‘강아지’` 좌우를 모두 비교
- `‘고양이’ || ‘강아지’` → `‘고양이’` 좌에서 우로 비교하며 하나라도 `true`면 종료한다

```jsx
let done = true;
let msg = "";

if (done) msg = "완료";

//단축 평가
//if 문을 대체
msg = done && "완료"; // '완료'를 return
console.log(msg); // 완료
```

> **🚀 팀원들의 생각 🚀**
>
> - 함수 매개변수에 기본값을 설정할때 || 연산자를 이용하여 undefined 할당을 막을 수 있다는 관점을 얻었다.(그러나, undefined 만을 막으려면, null 병합 연산자를 쓰는게 더 나은 것 같다.)
>
> - Dart 언어를 사용했을 때, 해당 개념을 많이 이용해서 코딩했었다.
>
> - 함수 매개변수에 기본값을 설정할때 || 연산자를 이용하여 undefined 할당을 막을 수 있다는 관점을 얻었다.(그러나, undefined 만을 막으려면, null 병합 연산자를 쓰는게 더 나은 것 같다.)
>
> - 기본값을 설정할 때 논리 연산자 ||를 사용할 수는 있어도, es6에서는 기본 매개변수를 설정하니까 굳이? 싶다.

## 💡 TypeError 방지

```jsx
let el = null;
let value = el.value; // TypeError: Cannot read property 'value' of null

let preventErrorValue = el && el.value; // null
```

```jsx
//함수 매개변수에 기본값을 설정할 경우
const getStrLength = (str = "") => {
  return str.length;
};

getStrLength(); // 0;
getStrLength("dddd"); // 4;
```

### 옵셔널 체이닝

> ES11에서 도입된 객체 속성에 안전하게 접근하기 위해 사용되는 연산자로, `el` && `el.value` 를 실행하는 로직이다.

```jsx
let el = null;

let value = el?.value; //undefined
```

> **🚀 팀원들의 생각 🚀**
>
> - 타입스크립트를 사용할 때 옵셔널 체이닝 연산자를 자주 애용했는데, 요즘에는 옵셔널 체이닝 연산자도, undefined를 결국 반환하니까 무조건적인 남용은 안좋은 것 같다..

### null 병합 연산자

> ES11에서 도입된 `??` 는 좌항이 `null` 또는 `undefined` 면 우항을 반환하고, 그렇지 않으면 좌항을 반환한다.

```jsx
const exist = "있는 데이터";

let foo = null ?? "없는 데이터";
let foo2 = exist ?? "없는 데이터";

console.log(foo); // '없는 데이터';
console.log(foo2); // '있는 데이터';
```

## ✨ 예상 면접 질문

- 암묵적 타입 변환이란 무엇이며, 자바스크립트에서 언제 발생하나요?
- 명시적 타입 변환과 암묵적 타입 변환의 차이점은 무엇이며, 각각의 예시를 들어 설명해보세요.
- 단축 평가란 무엇이며, 이를 활용한 코드 예시를 설명해보세요.
- TypeError 방지할 수 있는 방법에 대해서 아는대로 설명해주세요.
- 타입 변환의 종류와 그에 대해 설명해주세요.
- 단축 평가가 무엇인지 설명해주세요.
- 옵셔널 체이닝과 단축 평가가 뭐가 다른지 설명해주세요.
