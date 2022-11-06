# JavaScript 고급

## 1. DOM

### Browser APIs
- 웹 브라우저에 내장된 API로, 현재 컴퓨터 환경에 관한 데이터를 제공하거나 여러가지 유용하고 복잡한 일을 수행
- 종류: DOM, Geolocation API, WebGL 등

### DOM
- 문서 객체 모델(Document Object Model)로, **Node**라고 불리는 계층적 단위에 정보를 저장

  (Node는 HTML 요소, 요소의 속성, 텍스트, 주석 등 HTML 문서의 모든 것에 해당됨)
- 문서가 논리 트리로 구조화되어 있으며, **각 요소를 객체로 취급**
- 단순한 속성 접근, 메소드 활용 뿐만 아니라 프로그래밍 언어적 특성을 활용한 조작 가능
- 즉, DOM은 웹 페이지의 객체 지향 표현이며, JavaScript와 같은 스크립트 언어를 이용해 DOM을 수정할 수 있음
- 참고) 파싱: 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정

### DOM의 주요 객체
- window
  - 가장 최상위 객체로, DOM을 표현하는 창이다.
  - 예시
    ```js
    // 새 탭 열기
    window.open()

    // 인쇄 대화 상자 표시
    window.print()

    // 경고 대화 상자 표시
    window.alert()
    ```

- document
  - 브라우저가 불러온 웹 페이지
  - 페이지 컨텐츠의 진입점 역할 (title, body 등의 요소를 포함하고 있음)

### DOM 조작
- Document가 제공하는 기능을 사용해 웹 페이지 문서를 조작할 수 있다.
  - 조작 순서: 선택(Select) -> 조작(Manipulation)

- 선택 관련 메소드
  - `document.querySelector(selector)`
    - CSS selector와 일치하는 첫 번째 element 객체를 반환(없다면 null 반환)
  - `document.querySelectorAll(selector)`
    - CSS selector와 일치하는 여러 element를 포함하는 NodeList를 반환

> **NodeList란?**
> - 유사배열로, 인덱스로만 각 항목에 접근이 가능하다.
>   - 유사배열: 배열처럼 보이지만, key가 숫자이고 length 값을 가지고 있는 객체를 말한다. 따라서 배열에서 사용 가능한 메소드를 사용할 수 없다.    
> - 유사배열이지만 forEach 메소드 사용 가능

> **정적 컬렉션 vs 동적 컬렉션**
> - 정적 컬렉션(Static Collection): DOM의 변경사항이 실시간으로 반영되지 않음
>   - 예시: `querySelectorAll`
> - 동적 컬렉션(Live Collection): DOM의 변경사항이 실시간으로 반영
>   - 예시: NodeList 중 `querySelectorAll`을 제외한 대부분의 메소드, `HTMLCollection`

- 조작 관련 메소드
  - `document.createElement(tagName)`
    - tagName의 HTML 요소를 생성하여 반환
  - `Node.innerText`
    - Node 객체와 그 자손의 텍스트 컨텐츠(DOMString)를 표현
    - 사람이 읽을 수 있는 요소만 남김
  - `Node1.appendChild(Node2)`
    - Node1의 자식 NodeList 중 마지막 자식으로 Node2를 삽입하고, Node2 객체를 반환
    - 한 번에 오직 하나의 Node만 추가 가능하며, 추가된 Node 객체를 반환
    - 만약 Node2가 이미 문서에 존재하는 다른 Node를 참조한다면, 현재 위치에서 새로운 위치로 이동
  - `Node1.removeChild(Node2)`
    - Node1의 자식 NodeList 중 Node2를 제거하고, Node2 객체를 반환
  - `Element.getAttribute(attributeName)`
    - 요소의 속성 값을 반환(문자열)
  - `Element.setAttribute(name, value)`
    - 요소의 속성 값을 설정
    - 이미 속성이 존재하면 값을 갱신, 존재하지 않으면 새 속성과 값 추가
  - `classList`: 클래스 관련 메소드 실행 가능
    - `Element.classList.toggle(className)`
      - 요소의 해당하는 클래스가 없다면, 해당 클래스를 추가하고 true 반환
      - 요소의 해당하는 클래스가 있다면, 해당 클래스를 제거하고 false를 반환
    - `Element.classList.add(className)`
      - 해당 클래스 추가
    - `Element.classList.remove(className)`
      - 해당 클래스 제거
  - 예시
    ```js
    const bodyTag = document.querySelector('body') // 
    const aTag = document.createElement('a')
    aTag.innerText = '구글'
    aTag.setAttribute('href', 'https://google.com')
    bodyTag.appendChild(aTag)
    ```

## 2. Event

### Event 
- 프로그래밍하고 있는 시스템에서 일어나는 사건 혹은 발생
- 웹에서는 다양한 event가 존재 (클릭, 키보드 키 입력, 텍스트 복사, 데이터 제출 등)
- DOM 요소는 event를 수신받고 받은 event를 처리할 수 있음

### Event handler
- `addEventListener()`라는 event handler를 이용해 다양한 HTML 요소에 부착할 수 있음
- `EventTarget.addEventListener(type, listener(event)[, options])`
  - 지정한 타입의 event가 대상에 전달될 때마다 호출할 콜백 함수(listener)를 설정
  - Listener는 발생한 event 데이터를 가진 event 객체를 유일한 매개변수로 받음
  - 대표 event type: input, click, submit, ...
  - `event.target`을 이용해 이벤트가 발생한 요소를 반환받을 수 있다.

- `preventDefault()`를 이용해 현재 event의 기본 동작을 중단할 수 있음

### Event 예시
```html
<body>
  <button id="btn">버튼</button>
  <p id="counter">0</p>
  
  <script>
    const btn = document.querySelector('#btn')
    let countNum = 0

    // 버튼을 누를 때마다, 카운트 1씩 증가
    btn.addEventListener('click', function (event) {
      const pTag = document.querySelector('#counter')
      countNum += 1
      pTag.innerText = countNum
    })
  </script>
</body>
```
```html
<body>
  <input type="text" id="text-input">
  <p></p>
  <script>
    // input 선택
    const inputTag = document.querySelector('#text-input')

    // 이벤트 핸들러 부착
    inputTag.addEventListener('input', function (event) {
      const pTag = document.querySelector('p')
      // 입력할 때마다 p 태그 내 텍스트 바뀌도록 설정
      pTag.innerText = event.target.value
    })
  </script>
</body>
```
```html
<body>
  <div>
    <h1>본문</h1>
  </div>
  
  <script>
    const h1Tag = document.querySelector('h1')
    h1Tag.addEventListener('copy', function (event) {
      // 글 복사하지 못하도록 설정
      event.preventDefault()
      alert('복사 할 수 없습니다.')
    })
  </script>
</body>
```

## 3. lodash
- 모듈성, 성능 및 추가 기능을 제공하는 JavaScript 유틸리티 라이브러리
- array, object 등 자료구조를 다룰 때 사용하는 유용하고 간편한 유틸리티 함수를 제공
- ex) reverse, sortBy, range, random, ...
- [https://lodash.com](https://lodash.com)

## 4. this

### this
- 자신이 속한 객체 또는 
- 함수를 호출하면 this가 암묵적으로 함수 내부에 전달된다.
- 단, this가 가리키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
  > **바인딩이란?**
  >
  > 식별자와 값을 연결하는 과정이다.
  >
  > 'this 바인딩'은 식별자 this와 this가 가리킬 객체를 바인딩하는 것이다.

### this의 참조
- 전역에서 사용할 경우 this는 항상 전역 객체를 참조한다.
  ```js
  console.log(this) // window(브라우저의 경우)
  ```
- 함수 내부에서 사용할 경우에는, 함수 호출 방식에 따라 나뉜다.
  - function 키워드로 생성한 일반 함수
    - this는 해당 함수를 실행한 객체에 바인딩된다.
      ```js
      const myObj = {
        data: 1,
        myFunc() {
          console.log(this) // myObj
          console.log(this.data) // 1
        }
      }
      ```
    - 고차 함수의 콜백함수 안에서 this는 전역 객체에 바인딩된다.
      ```js
      const myObj = {
        numbers: [1],
        myFunc() {
          console.log(this) // myObj
          this.numbers.forEach(function (number) {
            console.log(number) // 1
            console.log(this) // window
          })
        }
      }
      ```
    - 이를 방지하기 위해, 콜백함수 다음 인자로 참조할 객체를 함께 전달할 수 있다.
      ```js
      const myObj = {
        numbers: [1],
        myFunc() {
          console.log(this) // myObj
          this.numbers.forEach(function (number) {
            console.log(number) // 1
            console.log(this) // myObj
          }, this)
        }
      }
      ```
  - 화살표 함수
    - lexical scope: 함수를 어디서 호출하는지가 아니라 어디에 선언하였는지에 따라 결정
    - 화살표 함수에는 this가 존재하지 않기 때문에, 상위 스코프로 올라가면서 this를 찾는다.
    - 따라서 객체 안의 함수 내 콜백함수 상황에서 화살표 함수를 사용하면, this는 해당 객체에 바인딩된다.
    ```js
    const myObj = {
      numbers: [1],
      myFunc() {
        console.log(this) // myObj
        this.numbers.forEach((number) => {
          console.log(number) // 1
          console.log(this) // myObj
        })
      }
    }
    ```
    
> **참고**
>
> 이렇게 this가 호출되는 순간에 결정되는 것은 장단점이 있다.
>
> 장점: 함수(메소드)를 하나만 만들어서 여러 객체에서 사용할 수 있다.
>
> 단점: 이런 유연함이 실수로 이어질 수 있다.

### this와 addEventListener
- `addEventListener`에서의 콜백 함수는 특별하게 function 키워드의 경우 this가 `event.target`을 가리킨다.
- 반면 화살표 함수의 경우 상위 스코프를 지칭하기 때문에 this가 전역 객체를 가리킨다.
- 따라서 `addEventListener`의 콜백 함수는 function 키워드를 사용하는 것이 권장된다.
```html
<body>
  <button id="function">function</button>
  <button id="arrow">arrow function</button>

  <script>
    const functionButton = document.querySelector('#function')
    const arrowButton = document.querySelector('#arrow')

    functionButton.addEventListener('click', function(event) {
      console.log(this) // <button id="function">function</button>
    })

    arrowButton.addEventListener('click', event => {
      console.log(this) // window
    })
  </script>
</body>
```