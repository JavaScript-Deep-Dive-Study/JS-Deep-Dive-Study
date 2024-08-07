## 블록문

> `{}` 중괄호에 감싸져 있는 문

```jsx
{
  let foo = 10;
}

if (true) {
  console.log("참");
}

const sum = (a, b) => {
  return a + b;
};
```

## 조건문

> parameter의 결과에 따라 블록문을 실행하는 문

### if - else 문

```jsx
if (조건식a) {
  //조건식a가 참이면 실행
} else if (조건식b) {
  //조건식b가 참이면 실행
} else {
  //조건식a,b가 모두 참이 아니면 실행
}
```

```jsx
//블록을 생략할 수 있다.
if (num > 0) kind = "양수";
else if (num < 0) kind = "음수";
else kind = "0";
```

### 삼항연산자

```jsx
let 일이많다 = true; // 일이 많은 경우
let 결과 = 일이많다 ? "야근" : "칼퇴";
```

※ 주의: 조건이 많아질 수록 피라미드 코드가 되어 가독성을 해칠 수 있다.

[https://opendeveloper.tistory.com/entry/코드이슈-if문에서-else와-else-if를-지양하는-의견에-대한-생각정리](https://opendeveloper.tistory.com/entry/%EC%BD%94%EB%93%9C%EC%9D%B4%EC%8A%88-if%EB%AC%B8%EC%97%90%EC%84%9C-else%EC%99%80-else-if%EB%A5%BC-%EC%A7%80%EC%96%91%ED%95%98%EB%8A%94-%EC%9D%98%EA%B2%AC%EC%97%90-%EB%8C%80%ED%95%9C-%EC%83%9D%EA%B0%81%EC%A0%95%EB%A6%AC)

> **🚀 팀원들의 생각 🚀**
>
> - 현업에서는 api 코드 작성할 때, 삼항연산자를 이용해서 간단하게 호출 주소가 다를 때 자주 사용했었음
>
> - 클린코드를 위해서라면 지향해야 하지만 1개 사용할 때는 현업에서는 문제가 든다고 생각은 안들었음
>
> - 코드 리뷰로 가독성을 해친다는 말을 듣고 나서부터는 사용 지양

### switch 문

```jsx
let numKor = "영";
let num = 1;

switch (num) {
  case 1:
    numKor = "일";
    break;
  case 2:
    numKor = "이";
    break;
  case 3:
    numKor = "삼";
    break;
}

console.log(numKor); //일
```

- switch문에 break를 걸지 않는다면, 모든 case문을 실행하게 된다. 이를 `fall-through` 라고 한다.

> **🚀 팀원들의 생각 🚀**
>
> - switch 문은 자주 사용하지 않았음
>
> - 폴스루라는 개념은 알았는데, 책에 나와있는 개념을 폴스루라고 칭하는 것을 처음 알게됨
>
> - 상황에 따라 조건식이 불리언 값으로 평가되어야 하는 경우 if … else 문을 사용하는 게 좋고, 나머지의 경우에는 switch 문을 사용하는 경우를 고려하면서 앞으로 개발할 수 있을 것 같다. (근데 switch 문은 폴스루도 고려하면서 하다 보면 코드 길이가 늘어나 가독성을 해칠수도 있을 것 같다..?)

```jsx
let numKor = "영";
let num = 1;

switch (num) {
  case 1:
    numKor = "일";
  case 2:
    numKor = "이";
  case 3:
    numKor = "삼";
}

console.log(numKor); //삼
```

## 반복문

> parameter의 결과가 참일 때 까지 실행하는 문

### for 문 실행순서

```jsx
//'i'는 iteration;
for (let i = 0; i < 2; i++) {
  console.log(i);
}
```

1. `let i = 0;` 선언문이 실행
2. `i < 2` 조건문이 실행, `i = 0`이므로 true
3. 코드 블록이 실행, `console.log(i) // 0`
4. 마지막으로 증감식 `i++` 을 실행

> **🚀 팀원들의 생각 🚀**
>
> - for 문의 실행 순서는 잘 알아둬야 할 것 같다.

### while 문

```jsx
let num = 0;

while (num < 3) {
  console.log(num); //0, 1, 2
  num += 1;
}

//while문 안에 탈출 조건을 걸 수 있다.
while (true) {
  console.log(num); //0, 1, 2
  num += 1;

  if (num === 3) break;
}
```

### do while 문

- 코드 블록을 무조건 한 번 이상 실행된다.

```jsx
let num = 0;

do {
  console.log(num); //0
  num += 1;
} while (num === 0);
```

## break 문

> `label` 문, 반복문, switch 문의 코드 블록을 탈출

`label` : [https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/label](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/label)

### label 문

> 식별자가 붙은 문

```jsx
outerLoop: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      break outerLoop;
    }
    console.log(`i = ${i}, j = ${j}`);
  }
}
```

- label문의 단점으로는 코드가 복잡해질 수 있고, 무엇보다 코드의 흐름을 직관적으로 파악하기 힘들다.

```jsx
let shouldBreak = false;

for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) {
      shouldBreak = true;
      break;
    }
    console.log(`i = ${i}, j = ${j}`);
  }

  if (shouldBreak) break;
}
```

> **🚀 팀원들의 생각 🚀**
>
> - 레이블문을 처음 들음. 사용해본적 없음
>
> - 레이블 문을 탈출할 때는 break 문에 레이블 식별자를 안붙이면 SyntaxError(문법 에러) 발생
>
> - 레이블 문을 사용해서 중첩된 반복문에서 한번에 여러 반복문을 탈출할 수 있게 코드를 작성할 때는 유용할 것 같다.

## continue 문

> 코드 실행을 현 지점을 건너뛰고 다음 단계의 코드를 실행

```jsx
for (let i = 0; i < 5; i++) {
  if (i === 2) continue; // i가 2일 때 현재 반복을 건너뛰고 다음 반복으로 넘어간다.
  console.log(i); // 0, 1, 3, 4
}
```

> **🚀 팀원들의 생각 🚀**
>
> - 코테 준비할 때 continue를 자주 이용했었음
>
> - 보통은 개발할 때 if 문에서 논리연산자로만 코드를 처리 했는데, if 문 내에서 실행해야 할 코드가 긴 경우에 continue 를 사용하는게 가독성에 더 좋다는 관점을 얻게 되었음

## ✨ 예상 면접 질문

- for 문과 while 문은 어떤 상황에서 각각 사용하는 것이 좋을지 설명해주세요.
- continue 문과 break 문의 차이점을 설명하고, 각 문을 어떤 상황에서 사용하는 것이 좋은지 예를 들어 설명해보세요.
- for 문의 실행순서를 자세히 설명해주세요.
- if 문을 지양하는 추세인데, 그 이유와 대안을 제시해주세요.
- if else 문과 switch 문은 서로 어떤 상황에서 사용하면 좋을지 설명해주세요.
- 고차 함수에 대해서 설명해주세요.
- 레이블 문을 탈출할 때는 break 문에 레이블 식별자를 지정해야 하는 이유에 대해서 설명해주세요.
