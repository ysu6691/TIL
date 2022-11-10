# Vue Router

## 1. Routing
- 유저가 방문한 URL에 대해 적절한 결과를 응답하는 것
- SPA(Single Page Rendering)/CSR(Client Side Rendering)은 하나의 HTML만을 제공받아 하나의 URL만 가진다.
- 동작에 따라 URL이 바뀌지 않는다면, 즉 routing이 없다면 유저가 URL을 통한 페이지의 변화를 감지할 수 없다.
- 또한 페이지가 무엇을 렌더링 중인지에 대한 상태를 알 수 없다.
- **Vue Router**는 SPA 상에서 라우팅을 쉽게 개발할 수 있는 기능을 제공한다.

## 2. Vue Router

### Vue Router
- Vue의 공식 라우터
- 라우트에 컴포넌트를 매핑한 후, 어떤 URL에서 렌더링 할 지 알려준다.
- 즉 SPA를 MPA처럼 URL을 이동하면서 사용할 수 있다.

### Vue Router 시작하기
```bash
# router plugin 적용
vue add router

# history mode 사용 (Y 입력)
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n)
```
- History mode
  - 브라우저의 History API를 활용한 방식으로, 새로고침 없이 URL 이동 기록을 남길 수 있다.

### Vue Router 기본
```js
// src/router/index.js

import Vue from 'vue'
import VueRouter from 'vue-router'
import HomeView from '../views/HomeView.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
]
// 생략
```
```vue
<!-- src/App.vue -->

<template>
  <div id="app">
    <nav>
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </nav>
    <router-view/>
  </div>
</template>
```

- `router-link`
  - URL을 이동시키는 태그
  - `to`에 해당하는 문자열은 routes에 등록된 컴포넌트의 `name`과 매핑된다.
  - 히스토리 모드에서는 a 태그와 달리 브라우저가 페이지를 새로고침하지 않는다.

- `router-view`
  - 주어진 URL에 대해 일치하는 컴포넌트를 렌더링하는 컴포넌트이다.
  - 실제 컴포넌트가 DOM에 부착되어 보이는 자리를 의미한다.

- `src/router/index.js`
  - 라우터에 관련된 정보 및 설정이 작성된다.
  - `path`: 해당 컴포넌트의 URL을 작성
  - `name`: router-link 태그의 to에 들어갈 이름을 작성
  - `component`: 해당하는 컴포넌트를 작성

- `src/views`
  - router-view에 들어갈 컴포넌트를 작성
  - 컴포넌트는 views와 component 두 폴더에 나뉘어져 들어간다.
    - views: routes에 매핑되는 컴포넌트로, 컴포넌트의 이름은 `View`로 끝나도록 만드는 것을 권장
    - components: routes에 매핑된 컴포넌트의 하위 컴포넌트들을 모아두는 폴더

### URL 이동하기
- 선언적 방식 네비게이션
  - router-link의 to 속성으로 주소를 전달한다.
  - routes에 등록된 주소와 매핑된 컴포넌트로 이동한다.
  - v-bind를 사용하면 동적인 값을 사용할 수도 있다. 
  - 예시
    ```vue
    <template>
      <div>
        <!-- 두 링크는 같은 URL을 가리킨다. -->
        <router-link to="/">Home</router-link> |
        <router-link :to="{ name: 'home' }">Home</router-link>
      </div>
    </template>
    ```
- 프로그래밍 방식 네비게이션
  - Vue 인스턴스 내부에서 라우터 인스턴스에 `this.$router`로 접근한다.
  - 다른 URL로 이동하려면 `this.$router.push`를 사용한다.
  - history stack에 이동할 URL을 넣는 방식이다.
  - 예시
    ```vue
    <template>
      <div>
        <button @click="toHome">홈으로</button>
      </div>
    </template>

    <script>
    export default {
      name: 'App',
      methods: {
        toHome() {
          this.$router.push({ name: 'home' })
        }
      }
    }
    </script>
    ```

- Dynamic Route Matching
  - URL의 특정 값을 변수처럼 사용
  - 'src/router/index.js' 에서 해당 컴포넌트의 path에 `:동적 인자` 형태로 사용한다.
  - 예시
    ```js
    // src/router/index.js

    import HelloView from '../views/HelloView.vue'

    const routes = [
      {
        path: '/hello/:userName',
        name: 'hello',
        component: HelloView
      },
    ]
    // 생략
    ```
    ```vue
    // views/HelloView.vue

    <template>
      <div>
        <!-- {{ $route.params.userName }}도 가능하지만, -->
        <!-- data에 넣어서 사용하는 것을 권장 -->
        <h1>Hello, {{ userName }}</h1>
      </div>
    </template>

    <script>
    export default {
      name: 'HelloView',
      data() {
        return {
          userName: this.$route.params.userName
        }
      }
    }
    </script>
    ```

    ```vue
    <!-- 선언적 방식으로 동적 인자 전달하기  -->

    <template>
      <div>
        <!-- 데이터를 입력받아 해당 URL로 이동하기 -->
        <input type="text" v-model="inputData">
        <button><router-link :to="{ name: 'hello', params: objData }">Go to Hello</router-link></button>
        <!-- 아래와 같은 결과 -->
        <!-- <button><router-link :to="{ name: 'hello', params: {userName: this.inputData} }">Go to Hello</router-link></button> -->
      </div>
    </template>

    <script>
    export default{
      name: 'App',
      data() {
        return {
          inputData: '',
        }
      },
      computed: {
        objData() {
          return {userName: this.inputData}
        }
      },
    }
    </script>
    ```

    ```vue
    <!-- 프로그래밍 방식으로 동적 인자 전달하기 -->

    <template>
      <div>
        <input type="text" @keyup.enter="goToHello" v-model="inputData">
      </div>
    </template>

    <script>
    export default{
      name: 'App',
      data() {
        return {
          inputData: '',
        }
      },
      methods: {
        goToHello() {
          this.$router.push({ name: 'hello', params: { userName: this.inputData } })
        }
      }
    }
    </script>
    ```

### lazy-loading
- 모든 파일을 한 번에 로드하면 다 읽는 시간이 매우 오래 걸린다.
- 모든 컴포넌트를 미리 로드하지 않고 특정 라우트에 방문할 때 매핑된 컴포넌트를 로드하는 것이 lazy-loading이다.
- 모든 파일을 한 번에 로드하지 않기 때문에 최초 로드 시간이 빨라진다.
- 'src/router/index.js'에서 해당 컴포넌트를 바로 import 하지 않고, `component`에서 함수를 작성해 사용한다.
```js
// src/router/index.js

// AboutView 컴포넌트는 lazy-loading 처리
const routes = [
  {
    path: '/about',
    name: 'about',
    component: () => import('../views.AboutView.vue')
  }
]
```