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
  - 'src/router/index.js' 에서 해당 컴포넌트의 path에 `:동적인자` 형태로 사용한다.
  - 이는 컴포넌트 내에서 `this.$route.params.동적인자` 형태로 사용할 수 있다.
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

## 3. Navigation Guard

### Navigation Guard
- Vue router를 통해 특정 URL에 접근할 때 다른 URL로 redirect를 하거나 해당 URL로의 접근을 막는 방법
- 네비게이션 가드 종류
  - 전역 가드: 애플리케이션 전역에서 동작
  - 라우터 가드: 특정 URL에서만 동작
  - 컴포넌트 가드: 라우터 컴포넌트 안에 정의

### 전역 가드
- 다른 URL 주소로 이동할 때 항상 실행
- src/router/index.js에 `router.beforeEach()`를 사용하여 설정
- 콜백 함수의 인자
  - to
    - 이동할 URL 정보가 담긴 Route 객체
  - from
    - 이동 전 URL 정보가 담긴 Route 객체
  - next
    - 지정한 URL로 이동하기 위해 호출하는 함수
    - 기본적으로는 to에 해당하는 URL로 이동하지만, next 함수의 인자를 사용해 다른 URL로 보낼 수 있다.
    - 콜백 함수 내에서 반드시 한 번만 호출되어야 한다.
- URL이 변경되어 화면이 전환되기 전에 함수가 호출되며, 대기 상태가 된다.
- `next()`가 호출되기 전까지는 화면이 전환되지 않는다.
- 예시
  ```js
  // src/router/index.js

  router.beforeEach((to, from, next) => {
    console.log('to', to) // 이동할 URL 정보 출력
    console.log('from', from) // 이동 전 URL 정보 출력
    next() // 화면 전환
  })
  ```
- 로그인 여부에 따른 라우팅 처리 실습
  ```vue
  <!-- src/views/LoginView.vue -->

  <template>
    <div>
      <h1>로그인 페이지</h1>
    </div>
  </template>

  <script>
  export default {
    name: 'LoginView',
  }
  </script>
  ```
  ```vue
  <!-- src/App.vue -->

  <template>
    <div>
      <!-- 로그인으로 향하는 링크 생성 -->
      <router-link :to="{ name: 'login' }">Login</router-link>
    </div>
  </template>
  ```
  ```js
  // src/router/index.js

  import LoginView from '@/views/LoginView'

  Vue.use(VueRouter)

  // 로그인되어있다고 가정
  const isLoggedIn = true

  const routes = [
    {
      path: '/login',
      name: 'login',
      component: LoginView,
    },
  ]

  const router = new VueRouter({
    mode: 'history',
    base: process.env.BASE_URL,
    routes
  })

  // router 선언 이후에 작성
  router.beforeEach((to, from, next) => {

    // 로그인되어있다고 가정
    const isLoggedIn = true

    // 로그인이 필요한 페이지
    const authPages = ['hello', 'home', 'about']

    const isAuthRequired = authPages.includes(to.name)

    // 로그인이 필요없는 페이지를 모아두는 것도 가능
    // const allowAllPages = ['login']
    // const isAuthRequired = !allowAllPages.includes(to.name)

    // 로그인이 필요한 페이지이고 로그인이 되어있지 않다면,
    if (isAuthRequired && !isLoggedIn) {
      // 로그인으로 이동
      next({ name: 'login' })
    // 그렇지 않다면
    } else {
      // to(기존에 가려던 URL)로 이동
      next()
    }
  })
  ```

### 라우터 가드
- 특정 route에 대해서만 가드를 설정할 경우 사용
- src/router/index.js 내 특정 route에 `beforeEnter()`를 사용하여 설정
- route에 진입했을 때 실행되며 콜백 함수의 인자로 to, from, next를 받는다.
- 로그인 여부에 따른 라우팅 처리 실습
  - 이미 로그인 되어있는 경우 HomeView로 이동
  ```js
  // src/router/index.js

  const isLoggedIn = true

  const routes = [
    {
      path: '/login',
      name: 'login',
      component: LoginView,
      beforeEnter(to, from, next) {
        // 로그인 되어있는 경우 home으로 이동
        if (isLoggedIn === true) {
          next({ name: 'home' })
        // 로그인 되어있지 않은 경우 그대로 로그인 페이지로 이동
        } else {
          next()
        }
      }
    },
  ]
  ```

### 컴포넌트 가드
- 특정 컴포넌트 내에서 가드를 지정할 경우 사용
- 컴포넌트 내에서 `beforeRouteUpdate()`를 사용하여 설정
- URL로 동적 인자를 받을 경우 한 컴포넌트에 여러 URL이 매핑될 수 있다.
- 이 때 다른 URL로 이동하였지만 같은 컴포넌트가 재사용될 경우, lifecycle hook이 호출되지 않는다.
- 따라서 `$route.params`에 있는 데이터를 새로 가져오지 않는 문제가 발생한다.
- 이는 컴포넌트 가드를 이용해 해결할 수 있다.
- 예시
  ```vue
  <!-- src/views/HelloView.vue -->

  <template>
    <div>
      <h1>hello, {{ userName }}</h1>
    </div>
  </template>

  <script>
  export default {
    name: 'HelloView',
    data() {
      return {
        userName: this.$route.params.userName
      }
    },
    // 동적 인자로 사용중인 userName 갱신하기
    beforeRouteUpdate(to, from, next) {
      this.userName = to.params.userName
      next()
    }
  }
  </script>
  ```

### 404 Not Found
- 요청한 리소스가 존재하지 않을 때 응답한다.
- 예시
  ```vue
  <!-- src/views/NotFound404.vue -->

  <template>
    <div>
      <h1>404 Not Found</h1>
    </div>
  </template>

  <script>
  export default {
    name: 'NotFound404',
  }
  </script>
  ```
  ```js
  // src/router/index.js

  import NotFound404 from '@/views/NotFound404'

  const routes = [
    {
      path: '/404',
      name: 'NotFound404',
      component: NotFound404,
    },
    {
      // 기존에 명시한 경로가 아닌 모든 경로가 404 page로 redirect 된다.
      // 이때, routes의 최하단부에 작성해야 한다.
      path: '*',
      redirect: '/404',
      // redirect: { name: 'NotFound404' } 형태도 가능
    }
  ]
  ```

- 형식은 유효하지만 특정 리소스를 찾을 수 없는 경우, 404 page가 렌더링되지 않는 문제가 발생한다.
- 예를 들어, `articles/1/`로 요청을 보냈지만 1번 게시글이 없는 경우, `articles/:id/`에 대한 component가 렌더링 될 것이다.
- 하지만 데이터가 존재하지 않기 때문에 정상적으로 렌더링되지 않는다.
- 따라서 이러한 경우에는 해당 컴포넌트에 404 page로 redirect하는 기능을 추가할 수 있다. (`this.$router.push('/404')` 사용)

## 4. DogView with Vue
- 동적 인자로 강아지 품종을 전달해 품종에 대한 랜덤 이미지를 출력하는 페이지 실습
```bash
# axios 설치
$ npm i axios
```
```vue
<!-- src/views/DogView.vue -->

<template>
  <div>
    <p v-if="!imgSrc">{{ message }}</p>
    <input v-if="imgSrc" type="text" v-model="inputData" @keyup.enter="goToDog" placeholder="품종을 입력해주세요.">
    <div><button v-if="imgSrc" @click="getDogImage">다시 가져오기</button></div>
    <img v-if="imgSrc" :src="imgSrc" alt="">
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: 'DogView',
  data() {
    return {
      imgSrc: null,
      message: "로딩중..."
    }
  },
  methods: {
    getDogImage() {
      const breed = this.$route.params.breed
      const dogImageSearchURL = `https://dog.ceo/api/breed/${breed}/images/random`
      axios({
        method: 'get',
        url: dogImageSearchURL
      })
        .then((response) => {
          const imgSrc = response.data.message
          this.imgSrc = imgSrc
        })
        .catch((error) => {
          // this.message = `${this.$route.params.breed}는 없는 품종입니다.`
          this.$router.push('/404')
          console.log(error)
        })
    },
    goToDog() {
      this.$router.push({ name: 'dog', params: { breed: this.inputData } })
    }
  },
  created() {
    this.getDogImage()
  }
}
</script>

<style>

</style>
```

## 5. Articles with Vue
- vuex와 router를 활용한 article 앱의 index, create, detail, delete 구현

### 사전 준비
```bash
$ vue create articles
$ cd articles
$ vue add vuex
$ vue add router
```

### Create
```js
// src/store/index.js

export default new Vuex.Store({
  state: {
    // DB의 AUTO INCREMENT를 표현하기 위해 article_id를 수동으로 조정
    article_id: 1,
    articles: []
  }
  // 생략
})
```

```js
// src/router/index.js

const routes = [
  {
    path: '/',
    name: 'index',
    components: IndexView
  }
]
```

```vue
<!-- src/views/IndexView.vue -->

<template>
  <div>
    <h1>Articles</h1>
    <ArticleItem 
      v-for="article in articles"
      :key="article.id"
      :article=article
    />
  </div>
</template>

<script>
import ArticleItem from '@/components/ArticleItem'

export default {
  name: 'IndexView',
  components: {
    ArticleItem,
  },
  computed: {
    articles() {
      return this.$store.state.articles
    }
  }
}
</script>
```
```vue
<!-- src/components/ArticleItem.vue -->

<template>
    <p>글 번호 : {{ article.id }}</p>
    <p>글 제목 : {{ article.title }}</p>
    <hr>
</template>

<script>
export default {
  name: 'ArticleItem',
  props: {
    article: Object,
  },
}
</script>
```

### create
```js
// src/router/index.js

const routes = [
  // 생략
  {
    path: '/create',
    name: 'create',
    component: CreateView
  },
]
```

```vue
<!-- src/views/IndexView.vue -->

<template>
  <div>
    <h1>Articles</h1>
    <!-- 게시글 작성 페이지로 이동하는 링크 추가 -->
    <router-link :to="{ name: 'create' }">게시글 작성</router-link>
    <hr>
    <ArticleItem 
      v-for="article in articles"
      :key="article.id"
      :article=article
    />
  </div>
</template>
```

```vue
<!-- src/views/CreateView.vue -->

<template>
  <div>
    <h1>게시글 작성</h1>
    <!-- prevent를 이용해 submit 이벤트 동작 취소 -->
    <form @submit.prevent="createArticle">
      <!-- v-model.trim을 사용하여 공백 제거 -->
      <input type="text" v-model.trim="title"><br>
      <textarea v-model.trim="content"></textarea>
      <input type="submit">
    </form>
    <router-link :to="{ name: 'index' }">뒤로가기</router-link>
  </div>
</template>

<script>
export default {
  name: 'CreateView',
  data() {
    return {
      title: null,
      content: null,
    }
  },
  methods: {
    createArticle() {
      const title = this.title
      const content = this.content
      if (!title) {
        alert('제목을 입력해주세요')
      } else if (!content) {
        alert('내용을 입력해주세요')
      } else {
        const payload = {
          title, content
        }
        // actions에 데이터를 넘길 때 하나의 object로 만들어서 전달
        this.$store.dispatch('createArticle', payload)
        // index 페이지로 이동
        this.$router.push({ name: 'index' }) 
      }
    }
  }
}
</script>
```
```js
// src/store/index.js
export default new Vuex.Store({
  // 생략
  mutations: {
    CREATE_ARTICLE(state, article) {
      state.articles.push(article) // articles 배열에 추가
      state.article_id = state.article_id + 1 // article id 값 1 증가
    }
  },
  actions: {
    createArticle(context, payload) {
      const article = {
        id: context.state.article_id,
        title: payload.title,
        content: payload.content,
        createdAt: new Date().getTime() // 생성 시각 저장
      }
      context.commit('CREATE_ARTICLE', article)
    }
  },
})
```

### Detail
```js
// src/router/index.js

const routes = [
  // 생략
  {
    path: '/:id',
    name: 'detail',
    component: DetailView
  }
]
```

```vue
<!-- src/components/ArticleItem.vue -->

<template>
  <!-- 다음과 같이 인자를 직접 넣어 전달할 수도 있다. -->
  <div @click="goDetail(article.id)">
    <p>글 번호 : {{ article.id }}</p>
    <p>글 제목 : {{ article.title }}</p>
    <hr>
  </div>
</template>

<script>
export default {
  name: 'ArticleItem',
  props: {
    article: Object,
  },
  methods: {
    goDetail(id) {
      this.$router.push({ name: 'detail', params: {id}})
    }
  }
}
</script>
```

```vue
<!-- src/views/DetailView.vue -->

<template>
  <div>
    <h1>Detail</h1>
    <!-- 만약 서버로부터 데이터를 가져오는 경우 시간이 걸린다. -->
    <!-- 따라서 article이 null이기 때문에 id 등의 속성을 가져올 수 없다. -->
    <!-- optional chaining(?.)을 통해 article 객체가 있을 때만 출력되게 할 수 있다.-->
    <p>글 번호 : {{ article?.id }}</p>
    <p>글 제목 : {{ article?.title }}</p>
    <p>글 내용 : {{ article?.content }}</p>
    <p>작성시간 : {{ articleCreatedAt }}</p>
    <!-- 아래와 같이 작성하면 제대로 된 시각을 확인할 수 없음 -->
    <!-- <p>글 작성시간 : {{ article?.createdAt }}</p> -->
    <router-link :to="{ name: 'index' }">뒤로가기</router-link>
  </div>
</template>

<script>
export default {
  name: 'DetailView',
  data() {
    return {
      article: null,
    }
  },
  computed: {
    articles() {
      return this.$store.state.articles
    },
    // 로컬시간으로 변환해주는 computed 작성
    // Date객체는 1970년 1월 1일 UTC 자정과의 시간 차이를 밀리초로 나타내는 정수 값을 담음
    // Date().toLocalString(): 로컬 시간으로 변환해줌
    articleCreatedAt() {
      const article = this.article
      const createdAt = new Date(article?.createdAt).toLocaleString()
      return createdAt
    },
  },
  methods: {
    getArticleById(id) {
      // const id = this.$route.params.id
      for (const article of this.articles) {
        // 동적 인자를 통해 받은 id는 str이므로, 형변환 후 비교
        if (article.id === Number(id)) {
          this.article = article
          break
        }
      }
    },
  },
  // 인스턴스가 생성되었을 때 article을 불러오는 함수를 호출
  // 해당 article의 id를 가져온다.
  created() {
    this.getArticleById(this.$route.params.id)
  }
}
</script>
```

### Delete
```vue
<!-- src/views/DetailView.vue -->

<template>
  <div>
    <h1>Detail</h1>
    <!-- 생략 -->
    <button @click="deleteArticle">삭제</button><br>
    <router-link :to="{ name: 'index' }">뒤로가기</router-link>
  </div>
</template>

<script>
export default {
  methods: {
    deleteArticle() {
      // mutations의 DELETE_ARTICLE 함수 호출
      this.$store.commit('DELETE_ARTICLE', this.article.id)
      // 삭제 후 index 페이지로 이동
      this.$router.push({ name: 'index' })
    }
  }
}
</script>
```

```js
// src/store/index.js

export default new Vuex.Store({
  mutations: {
    // id에 해당하는 게시글 지우기
    DELETE_ARTICLE(state, article_id) {
      state.articles = state.articles.filter((article) => {
        return !(article.id === article_id)
      })
    }
  }
})
```

### 404 Not Found
```js
// src/router/index.js

const routes = [
  // Detail에 대한 route 보다 404 url을 먼저 등록해줘야 함
  {
    // path를 /404로 하면 404번째 게시글과 혼동할 수 있으므로 주의
    path: '/404-not-found',
    name: 'NotFound404',
    component: NotFound404
  },
  {
    path: '/:id',
    name: 'detail',
    component: DetailView
  },
  // article id 관련 요청 외에도 리소스가 존재하지 않는 경우
  // 최하단에 작성
  {
    path: '*',
    redirect: { name: 'NotFound404' }
  },
]
```
```vue
<!-- src/views/NotFound404.vue -->

<template>
  <div>
    <h1>404 Not Found</h1>
  </div>
</template>

<script>
export default {
  name: 'NotFound404',
}
</script>
```

```vue
<!-- src/views/DetailView.vue -->

<script>
export default {
  methods: {
    getArticleById(id) {
      for (const article of this.articles) {
        if (article.id === Number(id)) {
          this.article = article
          break
        }
      }
      // 해당 article이 없으면 404로 이동
      if (this.article === null) {
        this.$router.push({ name: 'NotFound404' })
      }
    },
  },
}
</script>
``