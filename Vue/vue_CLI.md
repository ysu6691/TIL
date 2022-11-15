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
  $ vue create 프로젝트명
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
    ```bash
    # git push할 때 node_modules는 git ignore
    # 다시 pull 받은 후 package를 이용해 재설치 해야함
    $ npm install
    ```
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
  - 비어 있어도 안되며(template은 렌더링에 포함x), 하나의 요소 안에 추가 요소를 작성해야 함
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
- 한 페이지는 여러 컴포넌트로 구성되어 있다.
- 만약 여러 컴포넌트가 같은 데이터를 공유한다면, 컴포넌트 간의 데이터가 일치해야 한다.
- 하지만 컴포넌트는 서로 독립적이므로 서로 데이터를 주고받아야 한다.
- 따라서 데이터 흐름 파악 및 유지보수를 위해 **부모-자식 관계의 컴포넌트끼리만 데이터를 주고받는다.**
- 부모 -> 자식으로의 데이터 흐름을 **pass props** 방식이라고 하고, 자식 -> 부모로의 데이터 흐름을 **emit event** 방식이라고 한다.
- 데이터의 전달은 한 단계씩만 가능하므로 데이터를 부모-자식 간에 순차적으로 전달해야 한다.

### Data function
- 하나의 vue 인스턴스를 사용할 때는 데이터를 객체 형태로 선언했지만, 컴포넌트를 사용하게 되면 데이터를 함수형으로 선언해야 한다.
- 각각의 컴포넌트마다 data를 객체로 생성하더라도, 자바스크립트가 작동하면서 서로의 데이터를 침범할 수 있다.
- 따라서 각 컴포넌트마다 함수 내부에서 객체를 return함으로써, 데이터를 분리하여 관리할 수 있다.

### Pass props
- 요소의 속성(property)을 사용하여 하위 컴포넌트로 데이터를 전달
  - `prop-data-name="value"` 형태로 사용(kebab-case, HTML 속성명은 대소문자를 구분하지 않기 때문)
  - v-bind를 통해 동적인 데이터를 전달할 수 있고, 부모 컴포넌트의 데이터가 업데이트 되면 자식 컴포넌트로 전달되는 데이터 또한 자동으로 업데이트 된다.
  - 정적인 데이터라도 숫자나 boolean 등은 문자열로 인식되기 때문에, v-bind를 이용해 전달한다.
- 하위 컴포넌트는 props 옵션을 사용하여 수신하는 props를 선언해야 한다.
  - 전달받은 props를 type과 함께 명시
  - `propDataName: type` 형태로 데이터를 받아서 사용(camelCase)
  - 부모 템플릿(html)에서 kebab-case로 넘긴 변수를 자식 스크립트(vue)에서 자동으로 camelCase로 변환하여 인식한다.
- 예시
  ```vue
  <!-- src/App.vue -->
  <template>
    <div id="app">
      <!-- 하위 컴포넌트 태그의 요소로 전달할 props 지정 -->
      <MyComponent 
        static-props="정적 데이터"
        :dynamic-props="dynamicData"
        :number-props="10"/>
    </div>
  </template>

  <script>
  import MyComponent from '@/components/MyComponent'

  export default {
    name: 'App',
    components: {
      MyComponent,
    },
    // 데이터는 함수 형태로 사용
    data: function () {
      return {
        // 전달할 동적 데이터
        dynamicData: "동적 데이터"
      }
    }
  }
  </script>

  <!-- 생략 -->
  ```
  ```vue
  <!-- src/components/MyComponent.vue -->
  <template>
    <div id="app">
      <!-- 전달받은 props 나타내기 -->
      <p>{{ staticProps }}</p>
      <p>{{ dynamicProps }}</p>
      <p>{{ numberProps }}</p>
    </div>
  </template>

  <script>
  export default {
    name: 'MyComponent',
    // 전달받은 props 선언
    props: {
      staticProps: String,
      dynamicProps: String,
      numberProps: Number,
    }
  }
  </script>

  <style>

  </style>
  ```

### Emit event
- 모든 props는 부모에서 자식으로 단방향 바인딩을 형성한다.
- 즉 부모 컴포넌트가 업데이트되면 자식 컴포넌트의 props는 자동으로 업데이트되지만, 반대 방향은 그렇지 않다.
- 오직 데이터의 업데이트를 부모 컴포넌트에서만 함으로써, 데이터 추적 및 관리를 용이하게 할 수 있다는 장점이 있다.
- 따라서 자식 컴포넌트에서 부모 컴포넌트로 데이터를 전달할 때는 **이벤트를 발생시킨다.**
  - v-on을 통해 실행하고자 하는 핸들러 함수를 html 요소에 등록한다.
  - 메소드 내부에서 `this.$emit('event-name', 'data')` 형태로 부모 컴포넌트에 'event-name'이라는 이벤트가 발생했다는 것을 알리면서 데이터를 함께 전달한다.(데이터 인자는 생략 가능)
  - 부모 컴포넌트는 template에서 'event-name' 이벤트를 듣고 script에서 지정한 핸들러 함수를 실행한다.(핸들러 함수의 인자는 전달된 데이터이다.)
- 예시
  ```vue
  <!-- src/App.vue -->
  <template>
    <div id="app">
      <!-- 하위 컴포넌트 태그의 요소로 전달받을 이벤트 리스너 지정 -->
      <MyComponent 
        :dynamic-props="dynamicData"
        @child-input="getDynamicData"/>
    </div>
  </template>

  <script>
  import MyComponent from '@/components/MyComponent'

  export default {
    name: 'App',
    components: {
      MyComponent,
    },
    data: function () {
      return {
        dynamicData: "동적 데이터"
      }
    },
    methods: {
      // 이벤트가 발생하면 실행할 핸들러 함수
      // 인자로 전달 받은 데이터를 사용할 수 있음
      getDynamicData(inputData) {
        this.dynamicProps = inputData
      }
    }
  }
  </script>

  <!-- 생략 -->
  ```
  ```vue
  <!-- src/components/MyComponent.vue -->
  <template>
    <div id="app">
      <p>
        <!-- 이벤트 리스터 등록 -->
        <input
          type="text"
          v-model="childInputData"
          @keyup.enter="childInput"
        >
      </p>
      <p>{{ dynamicProps }}</p>
    </div>
  </template>

  <script>
  export default {
    name: 'MyComponent',
    props: {
      dynamicProps: String,
    },
    data: function () {
      return {
        childInputData: '',
      }
    }
    methods: {
      // 이벤트 발생 시 실행할 핸들러 함수
      childInput() {
        // $emit을 통해 부모 컴포넌트의 어떤 이벤트 리스너에 전달할지와 어떤 데이터를 전달할지 지정
        // `child-input` 이벤트는 부모 컴포넌트에서 HTML 요소로 사용되므로 kebab-case로 명시
        this.$emit('child-input', this.childInputData)
        // 데이터 전달한 뒤 input 내부 비우기
        this.childInputData = ""
      }
    }
  }
  </script>

  <style>

  </style>
  ```

### Component style guide
- 싱글 파일 컴포넌트 이름
  - PascalCase 또는 kebab-case 중 하나로 통일해서 사용한다.
  - 예시(둘 중 하나로 통일)
    - components/MyComponent.vue
    - components/my-component.vue

- 베이스 컴포넌트 이름
  - 베이스 역할을 하는 컴포넌트는 Base, App 또는 V로 된 접두사를 갖는다.
  - 예시
    - components/BaseButton.vue
    - components/BaseTable.vue
    - components/BaseIcon.vue

- 싱글 인스턴스 컴포넌트 이름
  - 루트를 제외하고 어느 컴포넌트의 상위나 하위 컴포넌트가 아닌 싱글 컴포넌트는 The로 시작한다.
  - 예시
    - components/TheHeading.vue
    - components/TheSidebar.vue

- 강한 연관성을 가진 컴포넌트 이름
  - 부모 컴포넌트와 같은 접두사를 갖는다.
  - 예시
    - components/TodoList.vue
    - components/TodoListItem.vue
    - components/TodoListItemButton.vue

- pass props / emit event 컨벤션
  - HTML의 요소로 사용할 이름: kebab-case
  - 메소드, 변수명으로 사용할 이름: camelCase