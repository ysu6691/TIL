# Vuex

## 1. Vuex 기본

### State Management
- 웹 애플리케이션에서 상태는 현재 애플리케이션이 가지고 있는 data로 표현할 수 있다.
- 여러 개의 컴포넌트를 조합해서 하나의 app을 만드는 경우, 각 컴포넌트는 독립적이기 때문에 각각의 상태(data)를 갖는다.
- 따라서 **상태 관리**를 통해 각각의 컴포넌트가 같은 상태를 유지하게 해야 한다.
- `pass props`와 `emit event`로도 상태 관리가 가능하지만, 컴포넌트가 많아지면 데이터 전달 구조가 복잡해질 수 있다.
- 따라서 **vuex** 라이브러리를 이용해 중앙 저장소(store)에 데이터를 모아서 상태를 관리할 수 있다.
- 중앙 저장소에서 데이터를 관리하지만, 개발 환경에 따라 적절히 중앙 저장소를 거치지 않고 데이터를 주고 받을 수 있다.

### Vuex 시작하기
```bash
$ vue create 앱 이름 # Vue 프로젝트 생성
$ cd 앱 이름 # 디렉토리 이동
$ vue add vuex # Vue CLI를 통해 vuex plugin 적용
```

### Vuex 인스턴스
```js
// src/store/index.js

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
  },
  getters: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})
```

- state
  - 중앙에서 관리하는 모든 상태 정보(data)
  - state의 데이터가 변화하면 해당 데이터를 사용하는 컴포넌트도 자동으로 다시 렌더링
  - 개별 컴포넌트는 **computed**에서 `$store.state`로 state 데이터에 접근한다.

- Mutations
  - state를 변경하는 유일한 방법
  - mutations에서 선언한 핸들러 함수는 **반드시 동기적**이어야 한다. (mutations, actions에서 호출되는 함수를 핸들러 함수라고 함)
  - 선언한 핸들러 함수는 첫 번째 인자로 state, 두 번째 인자로 데이터를 받는다.(데이터는 생략 가능) 
  - Mutations의 핸들러 함수명은 actions의 핸들러 함수와 구별하기 위해 보통 UPPER_SNAKE_CASE를 사용한다.
  - component 혹은 Actions에서 `commit(핸들러 함수명, 데이터)` 형식으로 호출할 수 있다.

- Actions
  - component에서 데이터를 받아 다양한 로직을 수행하며, **비동기 작업을 포함**할 수 있다.(외부 API와의 소통 등)
  - state를 직접 변경하지 않고 mutations를 호출해서 state를 변경한다.
  - 선언한 핸들러 함수는 첫 번째 인자로 context, 두 번째 인자로 데이터를 받는다.(데이터는 생략 가능)
    - context: store.js의 모든 요소와 메소드에 접근할 수 있다. 즉, state를 직접 변경할 수 있지만 하지 않는다.
  - component에서 `dispatch(핸들러 함수명, 데이터)` 형식으로 호출할 수 있다.

- Getters
  - vue 인스턴스의 computed에 해당
  - state를 활용하여 계산된 값을 얻고자 할 때 사용한다.
  - state의 원본 데이터를 건들지 않고 계산된 값을 얻으며, 종속된 값이 변경된 경우에만 재계산한다.
  - 첫 번째 인자로 state, 두 번째 인자로 getter를 받음

- 데이터의 흐름
  - component에서 데이터를 조작할 때
     component -> (actions) -> mutations -> state
  - component에서 데이터를 사용할 때
     state -> (getters) -> component

- 예시
```vue
<!-- src/app.vue -->

<template>
  <div>
    {{ count }}
    <button @click="increment">+</button>
  </div>
</template>

<script>

export default {
  name: 'IndexView',
  computed: {
    count() {
      return this.$store.state.count
    }
  },
  methods: {
    increment() {
      this.$store.dispatch('increment')
    }
  }
}
</script>
```

```js
// src/store/index.js

const store = createStore({
  state: {
    count: 0
  },
  mutations: {
    INCREMENT (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})

// 자바스크립트 비구조화를 사용하면 인자를 간편하게 사용할 수 있다.
// 예시
//  actions: {
//     increment ({ commit }) {
//       commit('increment')
//     }
//  }
```

## 2. Todo 실습

### Todo 프로젝트 개요
- Todo CRUD
- Todo 진행 상황 별 템플릿 변경 및 개수 계산
- 컴포넌트 구성
  - App: TodoForm, TodoList의 자식 컴포넌트를 가짐
  - TodoList: TodoListItem의 자식 컴포넌트를 가짐
  - TodoListItem: 각 Todo 출력
  - TodoForm: Todo 생성

### 컴포넌트 작성
```vue
<!-- src/App.vue -->

<template>
  <div id="app">
    <h1>Todo List</h1>
    <TodoList/>
    <TodoForm/>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import TodoForm from '@/components/TodoForm'

export default {
  name: 'App',
  components: {
    TodoList,
    TodoForm,
  },
}
</script>
<!-- 생략 -->
```

```vue
<!-- src/components/TodoList.vue -->

<template>
  <div>
    <TodoListItem/>
  </div>
</template>

<script>
import TodoListItem from '@/components/TodoListItem'

export default {
  name: 'TodoList',
  components: {
    TodoListItem,
  },
}
</script>

<style>

</style>
```

```vue
<!-- src/components/TodoListItem.vue -->

<template>
  <div>
    TodoListItem
  </div>
</template>

<script>
export default {
  name: 'TodoListItem',
}
</script>

<style>

</style>
```
```vue
<!-- src/components/TodoForm.vue -->

<template>
  <div>
    TodoForm
  </div>
</template>

<script>
export default {
  name:'TodoForm',
}
</script>

<style>

</style>
```

### Read
```js
// src/store/index.js

import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    todos: [],
  },
  // 생략
})
```

```vue
<!-- src/components/todoList.vue -->

<template>
  <div>
    <!-- 편의상 key를 index로 설정(원래는 유일한 식별자로 해야 함) -->
    <TodoListItem
      v-for="(todo, index) in todos"
      :key="index"
      :todo="todo"
    />
  </div>
</template>

<script>
import TodoListItem from '@/components/TodoListItem'

export default {
  name: 'TodoList',
  components: {
    TodoListItem,
  },
  // computed에서 state 데이터 가져오기
  computed: {
    todos() {
      return this.$store.state.todos
    }
  },
}
</script>

<style>

</style>
```

```vue
<!-- src/components/todoListItem.vue -->

<template>
  <div>
    {{ todo.title }}
  </div>
</template>

<script>
export default {
  name: 'TodoListItem',
  props: {
    todo: Object,
  },
}
</script>

<style>

</style>
```

### Create
- TodoForm 컴포넌트에 todo 생성 기능 구현
```vue
<!-- src/components/TodoForm.vue -->

<template>
  <div>
    <!-- title의 좌우 공백을 제거함으로써, 빈 값이 입력되었는지 확인 -->
    <input 
      type="text"
      v-model.trim="todoTitle"
      @keyup.enter="createTodo"
    >
  </div>
</template>

<script>
export default {
  name:'TodoForm',
  data() {
    return {
      todoTitle: null,
    }
  },
  methods: {
    createTodo() {
      // 빈 값이 입력되지 않았다면 함수 호출
      if (this.todoTitle) {
        // 비동기 관련 작업이 아니기 때문에 바로 mutations로 보내도 되지만,
        // 흐름을 이해하기 위해 actions로 보내기
        // actions의 createdTodo 함수 호출
        // todoTitle을 데이터로 함께 보내기
        this.$store.dispatch('createTodo', this.todoTitle)
      }
      this.todoTitle = null // 보낸 후 input 비우기
    }
  }
}
</script>

<style>

</style>
```

```js
// src/store/index.js

// 생략
export default new Vuex.Store({
  state: {
    todos: [],
  },
  getters: {
  },
  mutations: {
    // todos 배열에 생성한 todo 저장
    CREATE_TODO(state, todoItem) {
      state.todos.push(todoItem)
    },
  },
  actions: {
    createTodo(context, todoTitle) {
      // Todo 객체 만들기
      const todoItem = {
        title: todoTitle,
        isCompleted: false,
      }
      // mutations의 CREATE_TODO 함수 호출
      // 생성한 todo 객체 함께 전달
      context.commit('CREATE_TODO', todoItem)
    },
  }
  // 생략
})
```

### UPDATE
- todo를 클릭하면 완료의 의미로 취소선 스타일 적용
```vue
<!-- src/components/TodoListItem.vue -->

<template>
  <div>
    <!-- todo의 완료 상태에 따라 동적으로 클래스 적용 -->
    <span 
      @click="updateTodoStatus"
      :class="{ 'is-completed': todo.isCompleted }"
    >
      {{ todo.title }}
    <span>
  </div>
</template>

<script>
export default {
  name: 'TodoListItem',
  props: {
    todo: Object,
  },
  methods: {
    updateTodoStatus() {
      this.$store.dispatch('updateTodoStatus', this.todo)
    },
  }
}
</script>

<style>
  .is-completed {
    text-decoration: line-through;
  }
</style>
```

```js
// src/store/index.js

// 생략
export default new Vuex.Store({
  state: {
    todos: [],
  },
  getters: {
  },
  mutations: {
    // 생략
    UPDATE_TODO_STATUS(state, todoItem) {
      // 방법 1.
      // todos 배열에서 선택된 todo의 isCompleted값만 토글한 후 todos 업데이트
      state.todos = state.todos.map((todo) => {
        if (todo === todoItem) {
          todo.isCompleted = !todo.isCompleted
        } 
        return todo
      })

      // 방법 2.
      // 선택된 todo의 인덱스를 이용해 todos에 접근하여 해당 todo의 isCompleted 값만 토글한다.
      // const index = state.todos.indexOf(todoItem)
      // state.todos[index].isCompleted = !state.todos[index].isCompleted
    },
  },
  actions: {
    // 생략
    updateTodoStatus(context, todoItem) {
      context.commit('UPDATE_TODO_STATUS', todoItem)
    },
})
```

### DELETE
- TodoListItem 컴포넌트에 삭제 기능 구현
```vue
<!-- src/components/TodoListItem.vue -->

<template>
  <div>
    <span 
      @click="updateTodoStatus"
      :class="{ 'is-completed': todo.isCompleted }"
    >
      {{ todo.title }}
    </span>
    <!-- 삭제 버튼 생성 -->
    <button @click="deleteTodo">Delete</button>
  </div>
</template>

<script>
export default {
  name: 'TodoListItem',
  props: {
    todo: Object,
  },
  methods: {
    // 삭제
    deleteTodo() {
      this.$store.dispatch('deleteTodo', this.todo)
    },
    updateTodoStatus() {
      this.$store.dispatch('updateTodoStatus', this.todo)
    },
  }
}
</script>
<!-- 생략 -->
```

```js
// src/store/index.js

// 생략
export default new Vuex.Store({
  state: {
    todos: [],
  },
  getters: {
  },
  mutations: {
    // 생략
    DELETE_TODO(state, todoItem) {
      const index = state.todos.indexOf(todoItem)
      state.todos.splice(index, 1)
    },
  },
  actions: {
    // 생략
    deleteTodo(context, todoItem) {
      context.commit('DELETE_TODO', todoItem)
    },
  },
  modules: {
  }
})
```

### 상태별 todo 개수 계산
```js
// src/store/index.js

// 생략
export default new Vuex.Store({
  state: {
    todos: [],
  },
  getters: {
    // 모든 todo 개수 세기
    allTodosCount(state) {
      return state.todos.length
    },

    // 완료된 todo 개수 세기
    completedTodosCount(state) {
      // 완료된 todo만 모은 새로운 배열을 생성
      const completedTodos = state.todos.filter((todo) => {
        return todo.isCompleted === true
      })
      // 그 새로운 배열의 길이를 반환
      return completedTodos.length
    },

    // 미완료된 todo 개수 세기
    unCompletedTodosCount(state, getters) {
      return getters.allTodosCount - getters.completedTodosCount
    },
  },
  // 생략
})
```

```vue
<!-- src/app.vue -->

<template>
  <div id="app">
    <h1>Todo List</h1>
    <h2>모든 Todo 개수: {{ allTodosCount }}</h2>
    <h2>완료된 Todo 개수: {{ completedTodosCount }}</h2>
    <h2>미완료된 Todo 개수: {{ unCompletedTodosCount }}</h2>
    <TodoList/>
    <TodoForm/>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import TodoForm from '@/components/TodoForm'

export default {
  name: 'App',
  components: {
    TodoList,
    TodoForm,
  },
  // computed에서 getters 데이터 가져오기
  computed: {
    allTodosCount() {
      return this.$store.getters.allTodosCount
    },
    completedTodosCount() {
      return this.$store.getters.completedTodosCount
    },
    unCompletedTodosCount() {
      return this.$store.getters.unCompletedTodosCount
    }    
  },
  methods: {
    loadTodos() {
      this.$store.dispatch('loadTodos')
    }
  }
}
</script>

<!-- 생략 -->
```

## 3. Local Storage
### Local Storage
- 브라우저의 Local Storage에 데이터를 저장하면, 브라우저를 종료하고 다시 실행해도 데이터를 보존할 수 있다.
- `window.localStorage`: 브라우저에서 제공하는 저장공간 중 하나로, 데이터가 문자열 형태로 저장되며 만료되지 않음
- `localStorage` 관련 메소드
  - `setItem(key, value)`: key, value 형태로 데이터 저장
  - `getItem(key)`: key에 해당하는 데이터 조회

### Todo에 적용하기

- 데이터 저장하기
  ```js
  // src/store/index.js

  // 생략
  export default new Vuex.Store({
    // 생략
    actions: {
      createTodo(context, todoTitle) {
        const todoItem = {
          title: todoTitle,
          isCompleted: false,
        }
        context.commit('CREATE_TODO', todoItem)
        context.dispatch('saveTodosToLocalStorage') // 생성 반영
      },
      deleteTodo(context, todoItem) {
        context.commit('DELETE_TODO', todoItem)
        context.dispatch('saveTodosToLocalStorage') // 삭제 반영
      },
      updateTodoStatus(context, todoItem) {
        context.commit('UPDATE_TODO_STATUS', todoItem)
        context.dispatch('saveTodosToLocalStorage') // 수정 반영
      },
      // todos 배열을 local storage에 저장
      // state를 변경하는 작업이 아니기 때문에 actions에 작성
      saveTodosToLocalStorage(context) {
        // 객체를 문자열로 변환
        const jsonTodos = JSON.stringify(context.state.todos)
        localStorage.setItem('todos', jsonTodos)
      },
    },
    modules: {
    }
  })
  ```

- 데이터 불러오기
```vue
<!-- src/app.vue -->

<template>
  <div id="app">
    <h1>Todo List</h1>
    <h2>모든 Todo 개수: {{ allTodosCount }}</h2>
    <h2>완료된 Todo 개수: {{ completedTodosCount }}</h2>
    <h2>미완료된 Todo 개수: {{ unCompletedTodosCount }}</h2>
    <TodoList/>
    <TodoForm/>
    <!-- 데이터 불러오는 버튼 생성 -->
    <button @click="loadTodos">Todo 불러오기</button>
  </div>
</template>

<script>
import TodoList from '@/components/TodoList'
import TodoForm from '@/components/TodoForm'

export default {
  name: 'App',
  // 생략
  methods: {
    // 데이터 불러오는 loadTodos 함수 호출
    loadTodos() {
      this.$store.dispatch('loadTodos')
    }
  }
}
</script>
<!-- 생략 -->
```

```js
// src/store/index.js

// 생략
export default new Vuex.Store({
  // 생략
  mutations: {
    // 생략
    LOAD_TODOS(state) { 
      // 데이터 불러오기
      const localStorageTodos = localStorage.getItem('todos')
      // 문자열을 객체 타입으로 변환하여 반영
      const parsedTodos = JSON.parse(localStorageTodos)
      state.todos = parsedTodos
    },
  },
  actions: {
    // 생략
    loadTodos(context) {
      context.commit('LOAD_TODOS')
    }
  },
  modules: {
  }
})
```

### vuex-persistedstate
- Vuex state를 자동으로 브라우저의 Local Storage에 저장하고 불러오는 라이브러리 중 하나
- `localStorage` 관련 메소드를 사용한 별도의 로직 없이도 data를 자동으로 저장하고 불러올 수 있음
- 사용하기
  ```bash
  $ npm i vuex-persistedstate
  ```
  ```js
  // src/store/index.js

  // 생략
  import createPersistedState from 'vuex-persistedstate'

  Vue.use(Vuex)

  export default new Vuex.Store({
    // 생략
    plugins: [
      createPersistedState(),
    ],
  })
  ```