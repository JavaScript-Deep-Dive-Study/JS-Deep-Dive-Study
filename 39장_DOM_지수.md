## 39.1 노드

- DOM
  - HTML 문서의 계층적 구조와 정보를 표현하고 이를 제어할 수 있는 API
  - 프로퍼티와 메서드를 제공하는 트리 자료구조
- 노드
  - HTML 요소는 HTML 문서를 구성하는 개별적인 요소
  - 이 요소는 렌더링 엔진에 의해 파싱되어 DOM을 구성하는 요소 노드 객체로 변환됨
  - DOM을 구성하는 노드 객체는 ECOMScript에 정의된 거 X
  - 브라우저 환경에서 추가적으로 제공하는 호스트 객체 O
- 트리 자료구조
  - 노드들의 계층 구조로 이뤄짐
  - 부모 노드와 자식 노드로 구성됨, 노드 간의 계층적 구조를 표현하는 비선형 자료구조
  - 최상위 노드를 루트 노드, 자식 노드가 없으면 리프 노드라고 함
  - 노드 객체들로 구성된 트리 자료구조를 DOM 트리라고 함
- 노드 객체의 타입
  - 문서 노드: DOM 트리의 최상위에 존재하는 루트 노드, document 객체
  - 요소 노드: HTML 요소를 가리키는 객체
  - 어트리뷰트 노드: HTML요소의 어트리뷰트를 가리키는 객체
  - 텍스트 노드: HTML 요소의 텍스트를 가리키는 객체
- 노드 객체의 특성
  - 이벤트 관련 기능은 EventTarget 인터페이스가 제공
  - 트리 탐색 기능, 노드 정보 제공 기능 등은 Node 인터페이스가 제공
  - 노드 타입에 따른 고유한 기능 예시
    - input 요소 노드 객체에는 value 프로퍼티가 필요하지만 div에서는 필요하지 않음

<br>

## 39.2 요소 노드 취득

- Document.prototype.getElementById
  - 인수로 전달한 id 어트리뷰트 값 요소 노드를 탐색하여 단 하나의 요소 노드를 반환
  - 일치하는 노드가 없으면 null을 반환
- Document.prototype.getElementsByTagName
  - 인수로 전달한 태그 이름의 모든 요소들을 탐색하여 반환
  - 여러 개의 요소 노드 객체를 갖는 DOMCollenction 객체를 반환
  - DOMCollenction은 배열 객체이면서 이터러블
- Docment.prototype.getElementsByClassName
  - 인수로 전달한 class의 어트리뷰트 값을 갖는 모든 요소 노드들을 탐색
  - 존재하지 않는 경우 빈 객체 DOMCollenction 을 반환
- Docment.prototype.querySelectorAll
  - CSS 선택자를 이용한 요소 노드 취득으로 인수로 전달한 CSS 선택자를 만족시키는 모든 노드를 탐색
  - 존재하지 않으면 빈 NodeList 객체를 반환
  - 문법에 맞지 않은 경우 에러가 발생
- Element.prototype.maches
  - 인수로 전달한 CSS 선택자를 통해 특정 요소 노드를 취득
- HTMLCollenction과 NodeList 과 NodeList
  - DOM API가 여러 개의 결과값을 반환하기 위한 DOM 컬렉션 객체, 모두 유사 배열이면서 이터러블
  - HTMLCollenction: 언제나 live 객체
  - NodeCollection: 노드 객체의 상태 변화를 실시간으로 반영
  - NodeList: 실시간으로 노드 객체의 상태 변경을 반영하지 않는 객체

<br>

## 39.3 노드 탐색

- Document.prototype.getElementById
  - 인수로 전달한 id 어트리뷰트 값 요소 노드를 탐색하여 단 하나의 요소 노드를 반환
  - 일치하는 노드가 없으면 null을 반환
- Node.porotype.childNodes, Element.prototype.children
  - 자식 노드를 탐색
- Node.porotype.hasChildNodes
  - 자식 노드가 존재하는지 확인
  - 불리언 값을 반환, 리프 노드라면 텍스트 노드를 포함
- Node.prototype.parentNode
  - 부모 노드를 탐색
- Node.prototype.previousSibling,Node.prototype.nextSibling
  - 형제 노드를 탐색

<br>

## 39.4 노드 정보 취득

- Node.prototype.nodeType
  - 노드 객체의 종류(타입)을 나타내는 상수를 반환
- Node.prototype.nodeName
  - 노드 이름을 문자열로 반환

<br>

## 39.5 요소 노드의 텍스트 조작

- Node.prototype.nodeValue
  - 노드 객체의 값(텍스트)를 가져올 수 있고(getter), 설정할 수 있음(setter)
- Node.prototype.textContent
  - 요소 노드의 텍스트와 모든 자손 노드의 텍스트를 취득하거나 변경
  - HTML 마크업은 무시됨, 이와 유사한 동작을 하는 innerText 프로퍼티가 있는데, 지양하는 게 좋음
    - innerText 프로퍼티는 css에 순종적이라 visibility:hidden으로 지정된 요소 노드의 텍스트를 반환하지 않기 때문에
    - textContent보다 느리기 때문에

<br>

## 39.6 DOM 조작

- 새로운 노드를 생성하여 DOM에 추가하거나 기존 노드를 삭제 또는 교체할 수 있음
- Element.prototype.innerHTML
  - 요소 노드의 콘텐츠 영역 내에 포함된 모든 HTML 마크업을 문자열로 반환
  - 사용자로부터 입력받은 데이터를 그대로 innerHTML에 할당하는 것은 크로스 사이트 스크립팅 공격에 취약하므로 위험
  - 구현이 간단하고 직관적이지만 삽입될 위치를 지정할 수 없다는 단점
- Element.prototype.insertAdjancentHTML
  - 기존 요소를 제거하지 않으면서 위치를 지정해 새로운 요소를 반환
- 노드 생성과 추가
  - element.prototype.createElemenmt(tagName): 요소 노드를 생성
  - element.prototype.createTextNode(text): 텍스트 노드를 생성
  - Node.prototype.appendChild(childNode): 매개변수의 노드를 요소 노드의 마지막 자식 노드로 추가
- 복수의 노드 생성과 추가
  - 생성한 노드를 번번히 appendChild로 추가할 경우 DOM을 여러 번 변경하게 됨
  - 이를 회피하기 위해 DocumentFragment 노드를 사용할 수 있음
  - 노드 객체의 일종으로, 부모 노드가 없어서 기존 DOM과는 별도로 존재
  - 서브 DOM을 구성하여 기존 DOM에 추가하기 위한 용도로 사용
- 노드 삽입
  - Node.prototype.appendChild
    - 매개변수의 노드를 요소 노드의 마지막 자식 노드로 추가
    - 요소를 DOM에 추가할 수도 있지만 위치 지정이 불가능
  - Node.prototype.insertBefore
    - 첫 번째 인수로 전달받은 노드를 두 번째 인수로 전달받은 노드 앞에 삽입
- 노드 복사
  - Node.prototype.cloneNode
    - 노드의 사본을 생성하여 반환
    - 매개변수 deep에 true를 전달하면 깊은 복사로 자식 노드까지 복사되고, false를 전달하거나 생략하면 얕은 복사로 자식 노드를 제외한 노드 자신만의 사본을 생성
- 노드 교체
  - Node.prototype.replaceChild
    - 자신을 호출한 노드의 자식 노드를 다른 노드로 교체
- 노드 삭제
  - Node.prototype.removeChild(child)
    - child 매개변수에 인수로 전달한 노드를 DOM에서 삭제

<br>

## 39.7 어트리뷰트

- HTML요소는 여러 개의 어트리뷰트(속성)을 가질 수 있음
- HTML 요소의 시작 태그에 어트리뷰트 이름="어트리뷰트 값" 형식으로 정의
- 노드 요소의 모든 어트리뷰트 노드는 Element.prototype.attributes 프로퍼티로 취득
- 어트리뷰트 조작은 Element.prototype.getAttribute/setAttribute 메서드를 사용
- data 어트리뷰트(data- 접두사가 붙은 어트리뷰트)와 dataset 프로퍼티를 이용하면 HTML 요소에 정의한 사용자 정의 어트리뷰트와 자바스크립트 간 데이터를 교환할 수 있음
- HTML 어트리뷰트와 DOM 프로퍼티
  - HTML 어트리뷰트는 DOM에서 중복 관리되는 것처럼 보임, 그러나 요소 노드는 상태를 가지고 있으며, 초기 상태와 최신 상태 2개를 관리
  - HTML 어트리뷰트는 HTML요소의 초기 상태를 의미, 이는 변하지 않음, HTML 요소의 초기 상태는 어트리뷰트 노드에서 관리
  - DOM 프로퍼티는 사용자가 입력한 최신 상태를 관리, DOM 프로퍼티는 사용자의 입력에 의한 상태 변화에 반응하여 언제나 최신 상태를 유지
  - HTML 어트리뷰트와 DOM 프로퍼티는 반드시 1:1로 대응하는 것은 아님
    - ex) checkbox 요소의 checked 어트리뷰트 값은 문자열이지만 프로퍼티 값은 불리언 타입

<br>

## 39.8 스타일

- 인라인 스타일
  - HTMLElement.prototype.style 프로퍼티를 이용하여 케밥케이스 스타일로 인라인 스타일을 추가/변경할 수 있음
- CSS 클래스
  - 클래스 선택자를 이용하여 Element.prototype.className을 변경하여 미리 작성한 CSS class에 연결
- Element.prototype.classList 프로퍼티는 class 어트리뷰트의 정보를 잡은 DOMTokenList 객체를 반환
- DOMTokenList 객체는 add(...className), remove(...className), item(index), contains(className), replace(old,new), toggle(className[,force])의 유용한 메서드를 지원

<br>

## 39.9 DOM 표준

- HTML과 DOM 표준은 W3C와 WHATWG 두 단체가 협력하면서 공통 표준을 만들어 왔음
- 최근에 두 단체 서로 다른 표준 결과물 내놓음
- WHATWG이 단일 표준 내놓기로 두 단체가 합의함
