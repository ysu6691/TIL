# Vue 기초

## 1. Front-end Development

### SPA(Single Page Application)
- 서버로부터 새로운 페이지를 불러오지 않고 현재의 페이지를 동적으로 다시 작성함으로써 사용자와 소통하는 웹 어플리케이션
- 서버에서 최초 1장의 HTML만 전달받아 모든 요청에 대응하는 방식
- CSR(Client Side Rendering) 방식으로 요청을 처리

### SSR vs CSR
- SSR(Server Side Rendering)
  - 사용자의 요청에 따라 서버가 HTML을 렌더링하여 제공하는 방식
  - 전달 받은 새 문서를 보여주기 위해 브라우저는 새로고침을 진행
  - 렌더링이 준비된 HTML파일을 로드하기 때문에 초기 페이지 로드시간은 빠르지만, 이후에는 일부만 수정하더라도 전체를 갱신하기 때문에 비효율적이다.
  - SEO(Search Engine Optimization)에 친화적이다.

- CSR(Client Side Rendering)
  - 서버로부터 최초 한 장의 빈 HTML을 받아온 뒤, JavaScript가 동작하면서 데이터를 주고 받아 클라이언트에서 렌더링을 진행하는 것
  - 클라이언트는 서버에 AJAX로 요청을 보내고, 서버는 데이터를 JSON으로 전달
  - JSON 데이터를 JavaScript로 처리 후 DOM 트리에 반영(렌더링)
  - 초기 로드시간은 느리지만, 이후에는 응답 속도가 빠르며 요청에 대한 응답을 끊김없이 진행할 수 있다.
  - Back-End와 Front-End 간의 작업 영역을 명확히 분리할 수 있다.
  - SEO가 어렵다.

### SEO(Search Engine Optimization)
- 검색엔진 결과 페이지에서 웹사이트의 상위 노출도를 높이는 작업
- 검색자의 의도를 이해하고 이에 맞춰 웹페이지의 태그와 링크 구조를 개선해, 자연 유입 트래픽을 늘림
- SSR 방식은 초기 HTML 파일에 소스코드가 다 들어있으므로 SEO에 친화적이다.
- CSR 방식은 초기 웹페이지의 소스코드가 거의 비어있어 SEO에 불리하다.
- 최근에는 CSR로 구성된 서비스의 비중이 증가하면서, SPA 서비스도 검색 대상으로 넓히기 위해 JavaScript를 지원하는 방식으로 발전하고 있다.


## 2. Vue 시작하기

### VSCode Vetur extention
- 문법 하이라이팅, 자동완성, 디버깅 기능 제공

### Chrome Vue devtools extension
- 크롬 브라우저 개발자 도구에서 vue 디버깅 기능 제공

### Vue CDN
- [Vue2 공식 문서](https://v2.vuejs.org/)
- Vue2 공식 문서 접속 -> GET STARTED -> Installation -> CDN 복사


## 3. Vue 기본

### MVVM Pattern
