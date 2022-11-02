# Vue CLI

## 1. Vue CLI

### Node.js
- 자바스크립트는 브라우저를 조작하는 유일한 언어이다.
- Node.js로 인해 브라우저가 아닌 환경에서도 구동할 수 있게 되었으며, server side programming 또한 가능해졌다.
- NPM(Node Package Manage)
  - Node.js의 기본 패키지 관리자(Node.js 설치 시 함께 설치됨)

### Vue CLI
- Vue 개발을 위한 표준 도구로, 프로젝트의 구성을 도와준다.
- 설치
  ```bash
  $ npm install -g @vue/cli
  ```
- 프로젝트 생성
  ```bash
  # vscode terminal에서 실행
  $ vue create vue-cli
  ```
- 프로젝트 실행
  ```bash
  $ npm run serve
  ```

- Vue CLI 프로젝트 구조
  - node_modules: node.js 환경의 여러 의존성 모듈
    - Bable: 자바스크립트 컴파일러로, ES6+ 코드를 구버전으로 변역/변환 해주는 도구이다.
    - Webpack: 모듈 간의 의존성 문제를 해결하기 위한 도구이다.
  - package.json: 프로젝트의 종속성 목록과 지원되는 브라우저에 대한 구성 옵션을 포함
  - package-lock.json: node_modules에 설치되는 모듈과 관련된 모든 의존성을 설정 및 관리(사용할 패키지의 버전을 고정해, 개발 과정 간의 의존성 패키지 충돌을 방지한다.)
  - publick/index.html: Vue 앱의 뼈대가 되는 html 파일
  - src/
    - assets: 정적 파일을 저장하는 디렉토리
    - components: 하위 컴포넌트들이 위치
    - App.vue: 최상위 컴포넌트로, public/index.html과 연결됨
    - main.js: webpack이 빌드를 시작할 때 가장 먼저 불러오는 entry point (publick/index.html과 src/App.vue를 연결시키고, Vue 전역에서 활용할 모듈을 등록할 수 있는 파일)

## 2. SFC

### Component
- UI를 기능별로 분화해 독립적이고 재사용 가능한 조각들로 나눈 것
- src/App.vue를 root node로 하는 tree의 구조를 가짐
- 유지보수를 쉽게 만들어 줄 뿐만 아니라 재사용성의 측면에서도 강력한 기능을 제공
- Component based architecture 특징: 관리 용이, 재사용성, 확장 가능, 캡슐화, 독립적
- **Vue에서의 component는 Vue 인스턴스를 뜻한다.**

### SFC
- Single File Component
- 하나의 .vue 파일이 하나의 Vue 인스턴스이고, 하나의 컴포넌트이다.
- 따라서 **.vue 파일을 기능 단위로 작성**하는 것이 핵심이다.
- Vue 인스턴스에서는 HTML, CSS, JavaScript 코드를 한 번에 관리한다.
- 즉 Vue CLI가 Vue를 component based하게 사용하도록 도와준다.

### Vue component
- Vue component 구조
  - template: HTML의 body 부분
  - script: JavaScript 코드가 작성되는 곳
  - style: CSS가 작성되는 곳
- index.html 파일 하나만을 렌더링하면, App.vue를 최상단으로 컴포넌트들이 tree 구조를 이뤄 하나의 페이지를 만듦

### Vue component 실습
- component 생성
  - template 안에는 반드시 하나의 요소만 추가 가능
  - 비어 있어도 안되며, 하나의 요소 안에 추가 요소를 작성해야 함
  - 컴포넌트명(name)은 파일명과 달라도 되지만, 굳이 다르게 설정할 필요는 없음
  ```vue
  <!-- src/components/MyComponent.vue -->

  <template>
    <div>
      <h1>This is my component</h1>
    </div>
  </template>

  <script>
  export default {
    name: 'MyComponent',
  }
  </script>

  <style>

  </style>
  ```

- component 등록
  - 불러오기 -> 등록하기 -> 보여주기 단계를 모두 거쳐야함
  - `import {컴포넌트명} from {위치}`로 하위 컴포넌트 등록
  - `@`는 src의 shortcut
  - `.vue`는 생략 가능
  - 템플릿 내 컴포넌트를 넣을 때에는 닫는 태그만 있는 요소처럼 사용
  ```vue
  <!-- src/App.vue -->

  <template>
    <div id="app">
      <!-- 3. 보여주기 -->
      <MyComponent/>
    </div>
  </template>

  <script>
  // 1. 불러오기
  // import MyComponent from '@/components/MyComponent' 도 가능
  import MyComponent from './components/MyComponent.vue'

  export default {
    name: 'App',
    components: {
      // 2. 등록하기
      MyComponent,
    }
  }
  </script>

  <style>

  </style>
  ```

## 3. Pass Props & Emit Events

### Data in components
