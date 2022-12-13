# Redux

## 1. React에서 Redux를 사용하는 이유

Redux는 컴포넌트 간의 props로 연결되는 state나 앱 전체적으로 쓰인 state를 관리하는 **상태 관리 라이브러리**이다.

하지만 React에는 이미 상태 관리를 도와주는 **Context Hook**이 존재한다.

그렇다면 왜 Redux를 사용할까?

- Context Hook의 잠재적인 단점
  - 애플리케이션의 규모에 따라 상태 관리가 복잡해질 수 있다.
    ```js
    return (
      <AuthContextProvider>
        <ThemeContextProvider>
          <UIInteractionContextProvider>
            ...
          </UIInteractionContextProvider>
        </ThemeContextProvider>
      </AuthContextProvider>
    )
    ```
  - 데이터의 변경이 자주 일어나는 경우 성능이 다소 떨어질 수 있다.

## 2. Redux 작동 방식

Redux는 중앙 데이터 저장소 역할을 수행한다.

각 컴포넌트는 중앙 저장소를 **구독**하고 데이터를 사용하며, 중앙 저장소는 데이터의 변화를 컴포넌트에 알린다.

데이터는 단방향으로만 전달되므로, 데이터를 변경할 땐 컴포넌트에서 직접 조작하지 않는다.

대신 컴포넌트는 **Reducer 함수**를 실행하고 저장소 데이터를 변경할 수 있다. (useReducer Hook과는 다른 것임)


<img src="https://user-images.githubusercontent.com/109272360/206216366-2936e8e8-5f8e-49cc-aded-24b6e57a53b3.png" width="600px">

## 3. Redux 개념 살펴보기
### redux 설치
```bash
# 새로운 프로젝트 생성
# -y는 모든 설정에 대해 y를 입력하는 것과 같음
$ npm init -y

# Redux 설치
$ npm install redux
```

### redux 불러오기
- JavaScript 파일 생성
  ```js
  // redux-demo.js

  const redux = require('redux')
  ```

### 중앙 저장소 만들기
```js
const redux = require('redux')

// 중앙 저장소 생성
const store = redux.createStore()
```

### Reducer 함수 생성
- Reducer 함수는 자바스크립트 내장 함수지만 Redux 라이브러리에 의해 호출된다.
- 항상 2개의 파라미터를 갖는다. -> **기존 상태**와 **전달받은 action**
- 새로운 **state 객체를 반환**한다.
- 함수 내에서 부가적인 일을 수행할 수 없다. ex) HTTP 요청, 저장소 변경 등
```js
const redux = require('redux')

// state, action을 받는 reducer 함수 생성
// state에는 데이터의 초기값을 객체 형태로 넣어줘야 함
const counterReducer = (state = {counter: 0}, action) => {
  // state 객체를 반환
  // key: state, value: 변경된 값
  return {
    counter: state.counter + 1
  }
}

// 저장소가 어떤 reducer에 의해 state를 변경하는지 저장
// 초기화되면서 reducer가 실행되었으므로, 현재 counter의 값은 1임
const store = redux.createStore(counterReducer)
```

### Subscribe
- `subscribe(listener)`를 이용해 상태가 변경될 때마다 listener를 실행한다.
```js
const redux = require('redux')

const counterReducer = (state = {counter: 0}, action) => {
  return {
    counter: state.counter + 1
  }
}

const store = redux.createStore(counterReducer)

// 구독 함수 생성
// 상태가 변경될 때마다 실행됨
const counterSubscriber = () => {
  // 업데이트 후 최신의 상태를 반환
  const latestState = store.getState()
}

// 구독하기
store.subscribe(counterSubscriber)
```

### Dispatch
- `dispatch(action)`을 사용해, action(객체 형태)을 전달한다.
```js
const redux = require('redux')

const counterReducer = (state = {counter: 0}, action) => {
  // 전달받은 action에 해당하는 로직을 수행하도록 변경
  if (action.type === 'increment') {
    return {
      counter: state.counter + 1
    }
  } else if (action.type === 'decrement') {
    return {
      counter: state.counter - 1
    }
  }
}
// 이제 조건에 따라 분기가 되었으므로, 처음에 초기화되었을 때 counter는 0으로 유지된다.

const store = redux.createStore(counterReducer)

const counterSubscriber = () => {
  const latestState = store.getState()
  // action에 따라 state 변경 출력
  console.log(latestState)
}
store.subscribe(counterSubscriber)

// 어떤 액션을 전달할지 설정
store.dispatch({ type: 'increment' }) // { counter: 1 }
store.dispatch({ type: 'decrement' }) // { counter: 0 }
```

### Payload
- action 객체 내부에 값을 함께 전달할 수 있다.
```js
const redux = require('redux')

const counterReducer = (state = {counter: 0}, action) => {
  if (action.type === 'increment') {
    // action 객체의 amount만큼 더하기
    return {
      counter: state.counter + action.amount
    }
  } else if (action.type === 'decrement') {
    return {
      counter: state.counter - 1
    }
  }
}

const store = redux.createStore(counterReducer)

const counterSubscriber = () => {
  const latestState = store.getState()
  console.log(latestState)
}
store.subscribe(counterSubscriber)

// action 객체 내부에 추가 데이터를 함께 전달
store.dispatch({ type: 'increment', amount: 10 })
store.dispatch({ type: 'decrement' })
```

## 4. React & Redux

### react-redux 연동하기
- react-redux 설치
  ```bash
  $ npm install redux react-redux
  ```

- react용 redux store 생성
  - `src` 폴더에 `store` 폴더 생성(폴더 이름은 컨벤션임)
  - 원하는 파일명의 js파일 생성
  ```js
  // src/store/index.js

  import { createStore } from 'redux'

  const counterReducer = (state = {counter: 0}, action) => {
    if (action.type === 'increment') {
      return {
        counter: state.counter + action.amount
      }
    }

    // dispatch가 아닌 경우에도 state를 반환해야 함
    return state
  }

  const store = createStore(counterReducer)

  export default store
  ```

- store 제공하기
  ```js
  // src/index.js

  import {Provider} from 'react-redux'
  import store from './store/index'

  const root = ReactDOM.createRoot(document.getElementById('root'))
  // Provider 컴포넌트로 App 감싸기
  // Provider props로 store 제공
  root.render(<Provider store={store}><App /></Provider>)
  ```

### 컴포넌트에서 redux 데이터 사용하기
- `useSelector` 사용해 state 받아오기
  - `useSelector`를 사용하면 현재 컴포넌트가 자동으로 store를 구독한다. (store 변경될 때마다 컴포넌트 재실행)
  - 현재 컴포넌트가 DOM에서 사라지면 자동으로 구독을 취소한다.
  ```js
  // src/components/Counter.js

  import { useSelector } from 'react-redux'

  const Counter = () => {
    // store가 관리하는 counter state 받아오기
    const counter = useSelector(state => state.counter)

    return (
      <div>
        { counter }
      </div>
    )
  }

  export default Counter
  ```

- Action을 Dispatch하기
  ```js
  // src/components/Counter.js

  import { useSelector, useDispatch } from 'react-redux'

  const Counter = () => {
    const dispatch = useDispatch()
    const counter = useSelector(state => state.counter)

    const incrementHandler = () => {
      dispatch({ type: 'increment', amount: 5 })
    }

    return (
      <div>
        { counter }
        <button onClick={incrementHandler}>Increment</button>
      </div>
    )
  }

  export default Counter
  ```

- 여러 state 사용하기
```js
// src/store/index.js

import { createStore } from 'redux'

// 가독성을 위해 새로운 변수 생성
const initialState = { counter: 0, showCounter: true }

const counterReducer = (state = initialState, action) => {
  // 각 분기마다 모든 state의 현재 상태 반환
  if (action.type === 'increment') {
    return {
      counter: state.counter + action.amount,
      showCounter: state.showCounter
    }
  } 

  // 버튼 클릭 시 카운터 수 출력하는 토글 실행
  if (action.type === 'toggle') {
    return {
      counter: state.counter,
      showCounter: !state.showCounter
    }
  }

  return state
}

const store = createStore(counterReducer)

export default store
```

```js
// src/components/Counter.js

import { useSelector, useDispatch } from 'react-redux'

const Counter = () => {
  const dispatch = useDispatch()
  const counter = useSelector(state => state.counter)
  const show = useSelector(state => state.showCounter)

  const incrementHandler = () => {
    dispatch({ type: 'increment', amount: 5 })
  }

  const toggleCounterHandler = () => {
    dispatch({ type: 'toggle' })
  }

  // show가 true일 때만 counter 출력
  return (
    <div>
      { show && counter }
      <button onClick={incrementHandler}>Increment</button>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </div>
  )
}

export default Counter
```

> **주의사항**
> reducer 내부에서 새로운 state를 반환할 때, **절대 기존 state 값을 변경하지 않아야 한다.**
> ```js
> // src/store/index.js
>
> const redux = require('redux')
> 
> const initialState = { counter: 0, showCounter: true }
>
> const counterReducer = (state = initialState, action) => {
>   // 다음과 같이 사용 x (예기치 못한 부작용 발생 가능)
>   if (action.type === 'increment') {
>     state.counter = state.counter + action.amount
>     return state
>   }
>   
>   return state
> }
> 
> const store = redux.createStore(counterReducer)
> ```

### Redux toolkit
지금같은 방식으로 redux를 사용하면 대형 프로젝트의 경우 다음과 같은 문제가 빈번하게 발생할 수 있다.

  - action의 payload를 문자열로 전달하므로 오타 발생 가능

  - reducer는 각 분기마다 모든 state(상태가 변하지 않는 state마저도)의 현재 상태를 반환해야 함

이러한 문제를 해결하기 위해 **Redux toolkit  패키지**를 사용할 수 있다.

- Redux toolkit 설치
  - redux toolkit에 redux가 포함되어 있으므로, package.json에서 기존의 redux는 삭제한다.
  ```bash
  $ npm install @reduxjs/toolkit
  ```

- Redux toolkit 적용하기
  - `createSlice`
    - slice를 생성하는 함수
    - 기존의 reducer를 생성하는 방식을 대체해서 사용할 수 있다.
  - `configureStore`
    - 앱의 규모가 커져서 reducer가 여러 개인 경우, 여러 reducer를 하나로 합칠 수 있다.
    - `createStore`를 대체해서 사용할 수 있다.
    
  ```js
  // src/store/index.js
  // slice가 여러 개인 경우 index.js에서 모든 reducer를 통합해서 관리하는 것이 간편하다.

  import { configureStore } from '@reduxjs/toolkit'
  import counterReducer from './counterSlice'

  // slice에서 설정한 reducer에 접근가능하도록 설정
  // 인자로 객체를 받으며, reducer key에 해당하는 value로 reducer들을 넣는다.
  // 기존에 사용했던 것과 동일하게 
  const store = configureStore({
    reducer: { counter: counterReducer }
  })

  export default store
  ```

  ```js
  // src/store/counterSlice.js
  // store에 slice 별로 각각 다른 파일로 관리하는 것이 유지보수 측면에서 좋음

  import { createSlice } from '@reduxjs/toolkit'

  // slice 생성
  // name: slice의 식별자, initialState: 초기값, reducers: 실행할 함수들
  const counterSlice = createSlice({
    name: 'counterSlice',
    initialState: { counter: 0, showCounter: true },
    // reducers 내부의 함수는 인자로 state와 action을 받을 수 있다.
    // 중괄호 내부에는 state를 변경시키는 로직을 적는다.
    // 이때 실제로 기존 state가 변경되는 것은 아님
    // toolkit 내부에서 immer라는 패키지를 이용해 복제한 새로운 객체에 바뀐 state를 저장
    // 따라서 변경할 state에 관한 로직만 간편하게 작성 가능
    reducers: {
      increment(state, action) {
        // action 내 데이터 접근은 payload 객체를 이용
        state.counter = state.counter + action.payload.amount
      },
      toggleCounter(state) {
        state.showCounter = !state.showCounter
      }
    }
  })

  // counterSlice.actions를 이용해 slice의 모든 action에 접근할 수 있다.
  // 따라서 컴포넌트에서 action 식별자를 직접 문자열로 작성하지 않고 호출해서 사용 가능하다.
  export const counterActions = counterSlice.actions

  // counterSlice.reducer를 이용해 slice의 모든 reducer에 접근할 수 있다.
  // 기존에 사용했던 것과 동일하게 counterReducer와 같은 형태로 호출가능하다.
  export default counterSlice.reducer
  ```

  ```js
  // src/components/Counter.js

  import { useSelector, useDispatch } from 'react-redux'
  import { counterActions } from '../store/counterSlice'

  const Counter = () => {
    const dispatch = useDispatch()
    // configureStore에서 reducer가 객체 형태이므로 다음과 같이 접근
    const counter = useSelector(state => state.counter.counter)
    const show = useSelector(state => state.counter.showCounter)

    const incrementHandler = () => {
      // 추가 데이터가 필요한 함수는 객체 형태로 인자에 넣어줘야 함
      dispatch(counterActions.increment({amount: 5}))
      // dispatch({ type: 'counterSlice/increment', payload: {amount: 5} }) 와 같은 형태임
    }

    const toggleCounterHandler = () => {
      dispatch(counterActions.toggleCounter())
    }

    return (
      <div>
        { show && counter }
        <button onClick={incrementHandler}>Increment</button>
        <button onClick={toggleCounterHandler}>Toggle Counter</button>
      </div>
    )
  }

  export default Counter
  ```

## 5. Redux 고급

### Thunk

Redux를 사용하는 React 앱에서도 HTTP 요청과 같은 side effect를 사용해야 한다.

하지만 **reducer는 동기적이며 순수한 함수**여야 하기 때문에 **비동기식 로직**과 같은 side effect를 reducer 내부에서 사용하는 것은 적절하지 않다.

따라서 우리는 **컴포넌트 내부** 혹은 해당 데이터와 관련된 **slice 파일 내부**에서 비동기 처리를 수행할 수 있다.

가장 간편하게 사용하는 방법은 redux toolkit이 제공하는 `createAsyncThunk`를 사용하는 것이다.

```js
// src/store/counterSlice.js

import { createSlice, createAsyncThunk } from '@reduxjs/toolkit'

// createAsyncThunk: 비동기 작업을 처리하는 action을 만들어준다.
const asyncIncrementFetch = createAsyncThunk(
  // 첫 번째 인자로 세 가지 action type을 생성하는 문자열을 받는다. (slice의 name 역할)
  // pending / fulfilled / rejected
  'counterSlice/asyncIncrementFetch',
  // 두 번째 인자로 비동기 로직을 작성한다.
  async () => {
    // 1씩 커지는 숫자 받아오는 API
    const response = await fetch('https://api.countapi.xyz/hit/opesaljkdfslkjfsadf.com/visits')
    const data = await response.json()
    return data.value
  }
)

const counterSlice = createSlice({
  name: 'counterSlice',
  initialState: { counter: 0, status: 'Welcome' },
  reducers: {
    increment(state, action) {
      state.counter = state.counter + action.payload.amount
    },
  },
  // 비동기 로직의 세 가지 type에 따라 state를 변경할 수 있다.
  extraReducers: (builder) => {
    builder.addCase(asyncIncrementFetch.pending, (state, action)=>{
      state.status = 'Loading'
    })
    builder.addCase(asyncIncrementFetch.fulfilled, (state, action)=>{
      state.counter = action.payload
      state.status = 'complete'
      // console.log(action)
      // payload: 8733
      // type: "counterSlice/asyncIncrementAxios/fulfilled"
    })
    builder.addCase(asyncIncrementFetch.rejected, (state, action)=>{
      state.status = 'fail'
    })
  }
})

export const counterActions = counterSlice.actions
export default counterSlice.reducer
// 비동기 요청을 보내는 함수를 내보낸다.
export { asyncIncrementFetch }
```

```js
// src/components/Counter.js

import { useSelector, useDispatch } from 'react-redux'
import { counterActions, asyncIncrementFetch } from '../store/counterSlice'

const Counter = () => {
  const dispatch = useDispatch()
  const counter = useSelector(state => state.counter.counter)
  const status = useSelector(state => state.counter.status)

  return (
    <div>
      { counter } | { status }
      {/* 동기적 */}
      <button onClick={() => dispatch(counterActions.increment({amount: 5}))}>Increment</button>
      {/* 비동기적 */}
      <button onClick={() => dispatch(asyncIncrementFetch())}>Async Increment</button>
    </div>
  )
}

export default Counter
```