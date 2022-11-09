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
```vue
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
  - 개별 컴포넌트는 `$store.state`로 state 데이터에 접근한다.

- Mutations
  - state를 변경하는 유일한 방법
  - mutations에서 선언한 핸들러 함수는 **반드시 동기적**이어야 한다. (mutations, actions에서 호출되는 함수를 핸들러 함수라고 함)
  - 선언한 핸들러 함수는 첫 번째 인자로 state, 두 번째 인자로 데이터를 받는다.
  - component 혹은 Actions에서 `commit(핸들러 함수명, 데이터)` 형식으로 호출할 수 있다.

- Actions
  - component에서 데이터를 받아 다양한 로직을 수행하며, **비동기 작업을 포함**할 수 있다.(외부 API와의 소통 등)
  - state를 직접 변경하지 않고 mutations를 호출해서 state를 변경한다.
  - 선언한 핸들러 함수는 첫 번째 인자로 context, 두 번째 인자로 데이터를 받는다.
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