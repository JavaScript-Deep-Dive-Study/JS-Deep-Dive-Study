## 10.1 객체란?

- JS를 구성하는 거의 모든 것이 객체
  - 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식…)은 모두 객체
- 원시 타입은 단 하나의 값을 나타냄, 원시 값은 변경 X
- 객체 타입은 다양한 타입의 값을 하나의 단위로 구성한 복잡한 자료구조, 객체 값은 변경 O
- 객체

  - 0개 이상의 프로퍼티(키와 값)로 구성된 집합, 프로퍼티와 메서드로 구성된 집합체

    ```jsx
    var person = {
      // 프로퍼티는 키와 값으로 구성됨
      name: "Jeong", // name과 age는 프로퍼티 키
      age: 17, // 'Jeong'과 17은 프로퍼티 값
    };

    var counter = {
      // 객체는 프로퍼티와 메서드로 구성된 집합체
      num: 0, // 프로퍼티: 객체의 상태를 나타내는 값
      increase: function () {
        // 함수 = 메서드: 프로퍼티를 참조하고 조작할 수 있는 동작
        this.num++;
      },
    };
    ```

  - JS에서 사용 가능한 모든 값은 프로퍼티 값이 될 수 있음
    - JS의 함수는 일급 객체이므로 값, 따라서 함수도 프로퍼티 값이 될 수 있음
      - 일급 객체 (First-Class Object)
        - 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체
        - 변수에 할당 가능, 다른 함수를 인자로 전달받음, 다른 함수의 결과로 리턴될 수 있음
    - 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메서드라고 부름

- 객체지향 프로그래밍
  - 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
  - 객체지향 프로그래밍에서, 객체는 클래스와 인스턴스를 포함한 개념

<br>

## 10.2 객체 리터럴에 의한 객체 생성

- 클래스 기반 객체지향 언어(ex. C++, Java)의 객체 생성 방법
  - 클래스 사전 정의 → 필요 시점에 new 연산자와 생성자 호출 → 인스턴스 생성 → 객체 생성
  - 인스턴스: 클래스에 의해 생성되어 메모리에 저장된 실체
- JS는 프로토타입 기반 객체지향 언어, 다양한 객체 생성 방법이 있음
  - 객체 리터럴, Object 생성자 함수, 생성자 함수, Object.create 메서드, 클래스(ES6)
- 객체 리터럴

  - JS의 객체 생성 방법 중 가장 일반적이고 간단한 방법
  - 리터럴: 사람이 이해할 수 있는 문자/기호 사용해 값을 생성하는 표기법
  - 중괄호 안에 0개 이상(빈 객체일 수도 있으니까)의 프로퍼티를 정의, 변수 할당 시점에 객체 생성됨

    ```jsx
    var person = {
      name: "Lee",
      seyHello: function () {
        console.log("Hello, my name is ${this.name}.");
      },
    }; // 객체 리터럴의 중괄호 != 코드 블록, 따라서 세미콜론 붙여야 함

    console.log(typeof person); // object
    console.log(person); // {name: 'Lee', sayHello: f}
    ```

<br>

## 10.3 프로퍼티

- 객체는 프로퍼티의 집합, 프로퍼티는 키와 값으로 구성

  - 프로퍼티를 나열할 때는 쉼표로 구분, 마지막 프로퍼티 뒤에는 쉼표 쓰든 안 쓰든 상관 X
  - 키: 빈 문자열을 포함하는 모든 문자열 or 심벌 값

    - 프로퍼티 값에 접근할 수 있는 이름, 식별자 역할을 함
    - 식별자 네이밍 규칙을 반드시 따를 필요 X, 규칙 안 따르는 이름에는 반드시 따옴표 사용해야 함
      ```jsx
      var person = {
        firstName: "abcde",
        "last-name": "Lee", // 따옴표 사용, '-'를 JS 엔진이 연산자로 해석하기 때문
      };
      ```
    - 문자열 or 문자열로 평가 가능한 표현식으로, 프로퍼티 키를 동적으로 생성할 수 있음 (대괄호 사용)
      ```jsx
      var obj = {};
      var key = "hello"; // 빈 문자열을 키로 사용할 수도 있음, 근데 그러면 키로서의 의미가 없음...
      obj[key] = "world";
      console.log(obj); // {hello: 'world'}
      ```
    - 프로퍼티 키로 문자열이나 심벌 값 이외의 값을 사용하면, 암묵적 타입 변환으로 문자열이 됨

      ```jsx
      var foo = {
        0: 1,
        1: 2,
      };

      console.log(foo); // {0: 1, 1: 2} -> 키에 따옴표가 붙지 않았지만 문자열 변환이 된 것
      ```

    - 예약어(ex. var, function…)를 키로 사용할 수 있지만 권장사항 X
    - 이미 존재하는 키를 중복 선언하면, 나중에 선언한 것이 먼저 선언한 것을 덮어씀

  - 값: JS에서 사용 가능한 모든 값

<br>

## 10.4 메서드

- 프로퍼티 값이 함수일 때, 일반 함수와 구분하기 위해 메서드라고 부름
- 메서드는 객체에 묶여 있는 함수

<br>

## 10.5 프로퍼티 접근

- 마침표 표기법

  - 마침표 프로퍼티 접근 연산자(.)를 사용해 프로퍼티에 접근하는 방법

    ```jsx
    var person = {
      name: "Lee",
    };

    console.log(person.name); // Lee
    ```

- 대괄호 표기법

  - 대괄호 프로퍼티 접근 연산자([])를 사용해 프로퍼티에 접근하는 방법

    ```jsx
    var person = {
      name: "Lee",
    };

    // 프로퍼티 키를 반드시 따옴표 감싼 문자열로 지정해야 함 (name X, 'name' O)
    // 따옴표를 붙이지 않으면 JS는 식별자로 해석하기 때문 (ReferenceError)
    console.log(person["name"]); // Lee
    ```

  - 프로퍼티 키가 식별자 네이밍 규칙을 따르지 않을 경우, 대괄호 표기법을 사용해야 함
  - 프로퍼티 키가 숫자로 이뤄진 문자열일 경우, 따옴표 생략 가능, 대괄호 표기법 사용해야 함

    ```jsx
    var person = {
    	'last-name': 'Lee', // 식별자 네이밍 규칙 X
    	1: 10 // 숫자 키는 따옴표 생략 가능
    };

    console.log(person.'last-name'); // X
    console.log(person.last-name); // X
    console.log(person['last-name']); // O

    console.log(person.'1'); // X
    console.log(person.1); // X
    console.log(person[1]); // O, 따옴표 생략 가능
    console.log(person['1']); // O
    ```

- 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환 (ReferenceError 안 뜸)

  ```jsx
  var person = {
    name: "Lee",
  };

  console.log(person.age); // undefined
  ```

<br>

## 10.6 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 값이 갱신됨

  ```jsx
  var person = {
    name: "Lee",
  };

  person.name = "Yoon"; // 이렇게 값을 할당할 수 있음
  console.log(person); // {name: 'Yoon'}
  ```

<br>

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면, 프로퍼티가 동적으로 생성되어 추가됨

  ```jsx
  var person = {
    name: "Lee",
  };

  person.age = 10; // 객체에 없는 키와 값을 추가
  console.log(person); // {name: 'Lee', age: 10}
  ```

<br>

## 10.8 프로퍼티 삭제

- delete 연산자로 객체의 프로퍼티 삭제, 존재하지 않는 프로퍼티 삭제하려고 하면 에러 없이 무시됨

  ```jsx
  var person = {
    name: "Lee",
  };

  person.age = 10;

  delete person.age;
  delete person.abc; // 에러 X, 그냥 무시됨

  console.log(person); // {name: 'Lee'}
  ```

<br>

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

- 프로퍼티 축약 표현
  ```jsx
  // 프로퍼티 값으로 변수 사용할 때, 변수 이름 == 프로퍼티 키 -> 프로퍼티 키 생략 가능
  let x = 1,
    y = 2;
  const obj = { x, y }; // 프로퍼티 키 생략됨 (축약 표현)
  console.log(obj); // {x: 1, y: 2}
  ```
- 계산된 프로퍼티 이름

  ```jsx
  // ES5
  var prefix = "prop";
  var i = 0;
  var obj1 = {}; // 일단 객체 만들고

  // 나중에 키를 동적으로 생성했음
  obj[prefix + "-" + ++i] = i;
  obj[prefix + "-" + ++i] = i;
  obj[prefix + "-" + ++i] = i;

  console.log(obj1); // {prop-1: 1, prop-2: 2, prop-3: 3}

  // ES6
  // 객체 리터럴 내부에서 계산된 프로퍼티 이름으로 키를 동적 생성할 수 있음
  const obj2 = {
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
    [`${prefix}-${++i}`]: i,
  };

  console.log(obj2); // {prop-1: 1, prop-2: 2, prop-3: 3}
  ```

- 메서드 축약 표현

  ```jsx
  // ES5
  var obj = {
  	name: 'Lee',
  	sayHi: function() { ... }
  };
  obj.sayHi();

  // ES6
  var obj = {
  	name: 'Lee',
  	sayHi() { ... } // 메서드 축약 표현, function 키워드 생략
  };
  obj.sayHi();
  ```
