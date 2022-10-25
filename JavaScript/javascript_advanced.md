# JavaScript 고급

## 1. DOM

### Browser APIs
- 웹 브라우저에 내장된 API로, 현재 컴퓨터 환경에 관한 데이터를 제공하거나 여러가지 유용하고 복잡한 일을 수행
- 종류: DOM, Geolocation API, WebGL 등

### DOM
- 문서 객체 모델(Document Object Model)
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

- NodeList
  - 인덱스로만 각 항목에 접근이 가능하며, forEach 메소드 등 다양한 배열 메소드 사용 가능
  - 정적 컬렉션(Static Collection): DOM의 변경사항이 실시간으로 반영되지 않음
    - 예시: `querySelectorAll`
  - 동적 컬렉션(Live Collection): DOM의 변경사항이 실시간으로 반영
    - 예시: NodeList 중 `querySelectorAll`을 제외한 대부분의 메소드, `HTMLCollection`

- 조작 관련 메소드
  - 