# TypeScript with React

## 1. 타입스크립트 기반의 리액트 프로젝트 생성

```bash
# 새로운 리액트 프로젝트 생성
npx create-react-app 앱이름 --template typescript

# 새로운 리액트-리덕스 프로젝트 생성
npx create-react-app my-app --template redux-typescript
```

```bash
# 기존에 존재하는 리액트 프로젝트에 타입스크립트 적용
npm install --save typescript @types/node @types/react @types/react-dom @types/jest

# react-redux 설치
npm install react-redux @reduxjs/toolkit
```

`App.tsx`와 같이 `.tsx` 파일은 JSX 문법을 사용하는 타입스크립트 파일의 확장명이다.

또한 자바스크립트로의 컴파일은 리액트 밑단에서 자동으로 수행되기 때문에 추가적인 컴파일 과정을 진행하지 않아도 된다.

## 2. 타입스크립트 적용

### 컴포넌트 생성 및 props 전달

- 컴포넌트 적용 및 props 전달
  ```tsx
  // src/App.tsx

  import Todos from './components/Todos'

  function App() {
    return (
      <div>
        <Todos items={['Learn React', 'Learn TypeScript']} />
      </div>
    );
  }

  export default App;
  ```

- 컴포넌트 생성

  기존에 리액트에서 사용하던 방식으로 컴포넌트를 생성하면 props의 타입을 명시하지 않아 에러가 발생한다.

  ```tsx
  // props의 타입을 지정하지 않아 에러 발생
  const Todos = function (props) {
    return (
      <ul>
        {props.items.map(item => (
          <li key={item}>
            {item}
          </li>
        ))}
      </ul>
    )
  }

  export default Todos
  ```

  따라서 다음과 같은 다양한 방법으로 컴포넌트를 생성할 수 있다.

  - `FC` 사용

    `FC`(=FunctionComponent)는 제네릭 타입으로, props로 넘겨 받는 인자의 타입을 지정해주어야 한다.

    ```tsx
    // src/components/Todos.tsx

    import { FC } from 'react'

    // props 내 인자(items)의 타입을 명시
    const Todos: FC<{ items: string[] }> = function (props) {
      return (
        // 생략
      )
    }

    export default Todos
    ```

    하지만 `FC`의 사용을 지양해야 한다는 의견이 있는데, 그 중 하나는 불필요한 코드가 발생한다는 것이다.

    또, 함수 선언식에서 사용이 불가하다는 점이 있다.

    ```tsx
    // 함수 선언식에서 사용 불가
    function Todos (props) {
      // 생략
    }
    ```

    (`children`을 사용하지 않는 컴포넌트에서도 항상 props가 `children`을 가지는 문제도 있었지만, 지금은 `children`이 소스 코드에서 빠진 듯 하다.)

    이를 해결하기 위해 사용하는 방법이 **props interface를 직접 생성하는 것이다.**

  - props interface 이용

    ```tsx
    // src/components/Todos.tsx

    // Props interface 생성
    interface Props {
      items: string[]
    }

    // props의 타입 설정
    const Todos = function (props: Props) {
      return (
        // 생략
      )
    }

    export default Todos
    ```

    이같은 방식은 props 타입을 따로 생성해 관리하기 때문에 가독성 측면에서 효과적이다.

    또한 props의 타입을 직접 지정하기 때문에 함수 선언식에서도 사용 가능하다.

  - `PropsWithChildren` 사용

    `children`을 사용하는 컴포넌트인 경우 `PropsWithChildren`을 이용해 간편하게 타입을 지정할 수 있다.

    ```tsx
    // src/components/Todos.tsx

    import { PropsWithChildren } from 'react'

    // props로 넘어오는 인자의 타입만 명시한다.
    interface Props {
      items: string[]
    }

    // PropsWithChildren 사용
    // props.children 으로 children 속성에 접근 가능
    function Todos(props: PropsWithChildren<Props>) {
      return (
        // 생략
      )
    }

    export default Todos
    ```

    또는 `PropsWithChildren` 대신 props 타입에 `children`의 타입을 `ReactNode`로 직접 지정해도 무방하다.

    ```tsx
    // src/components/Todos.tsx

    import { ReactNode } from 'react'

    interface Props {
      items: string[]
      children: ReactNode
    }

    // props 내에 children 속성이 존재
    const Todos = function (props: Props) {
      return (
        // 생략
      )
    }

    export default Todos
    ```

### 다양한 props 전달하기

- 객체로 이루어진 배열 전달

  ```tsx
  // src/App.tsx

  import Todos from './components/Todos'

  function App() {

    // 단순 문자열에서 객체로 변경
    const todos = [
      {
        id: 1,
        text: 'Learn React'
      },
      {
        id: 2,
        text: 'Learn TypeScript'
      }
    ]

    return (
      <div>
        <Todos items={todos} />
      </div>
    );
  }

  export default App;
  ```

  ```tsx
  // src/components/Todos.tsx

  interface Props {
    items: Todo[] // items의 타입을 Todo로 된 배열로 설정
  }

  // Todo interface 생성
  interface Todo {
    id: number
    text: string
  }

  const Todos = function (props: Props) {
    return (
      <ul>
        {props.items.map(item => (
          <li key={item.id}>
            {item.text}
          </li>
        ))}
      </ul>
    )
  }

  export default Todos
  ```

- 이벤트 전달
```tsx
// src/components/CreateTodo.tsx

interface Props {
  // 문자열의 text를 받아 Todo 생성
  // 이후 아무것도 반환하지 않음
  onAddTodo: (text: string) => void
}

// 새로운 Todo 생성하는 컴포넌트
const CreateTodo = function (props: Props) {

  // 입력받은 Todo를 생성하는 로직 작성
  // props.onAddTodo(enteredText) 이용해 알리기

  return (
    <form>
      {/* ... */}
    </form>
  )
}

export default CreateTodo
```

```tsx
// src/App.tsx

import CreateTodo from './components/CreateTodo';
import Todos from './components/Todos'

function App() {

  const todos = [
    {
      id: 1,
      text: 'Learn React'
    },
    {
      id: 2,
      text: 'Learn TypeScript'
    }
  ]

  // 컴포넌트 내 onAddTodo 함수가 전달한 인자 타입 그대로 받기
  const addTodoHandler = (todoText: string) => {
    // ...
  }

  return (
    <div>
      <CreateTodo onAddTodo={addTodoHandler} />
      <Todos items={todos} />
    </div>
  );
}

export default App;
```

## 3. Events

### 이벤트 적용하기

```tsx
// src/components/CreateTodo.tsx

import { FormEvent } from 'react'

interface Props {
  onAddTodo: (text: string) => void
}

const CreateTodo = function (props: Props) {

  // 기존의 react와 달리 event의 타입을 지정해주어야 한다.
  const submitHandler = (event: FormEvent) => {
    event.preventDefault()
    // ...
  }

  return (
    <form onSubmit={submitHandler}>
      <input type="text" />
      <button>Add Todo</button>
    </form>
  )
}

export default CreateTodo
```

### 이벤트 종류

- `FormEvent`

  Form이 submit될 때 마다 작동한다. (`onSubmit`과 함께 사용)

- `ChangeEvent`

  `input`, `select`, `textarea` 요소가 변할 때마다 작동한다. (`onChange`와 함께 사용)

- `MouseEvent`

  마우스를 클릭할 때마다 작동한다. (`onClick`과 함께 사용)

- `FocusEvent`

  focus를 얻거나 잃었을 때 작동한다. (`onFocus` 또는 `onBlur`와 함께 사용)

- [더 다양한 이벤트 보기](https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/forms_and_events)

## 4. Hooks

### useState

`useState`는 제네릭 함수로, state의 타입이 무엇인지 명시할 수 있다.

```tsx
// src/App.tsx

import { useState } from 'react'

import CreateTodo from './components/CreateTodo';
import Todos from './components/Todos'

function App() {

  // Todo interface 생성
  interface Todo {
    id: number
    text: string
  }

  // todos의 타입이 Todo라는 것을 명시
  const [todos, setTodos] = useState<Todo[]>([])
  const [id, setId] = useState(1)

  // 입력 받은 todo 추가
  const addTodoHandler = (todoText: string) => {
    const newTodo = {
      id: id,
      text: todoText
    }
    const newTodos = [...todos]
    newTodos.push(newTodo)
    setTodos(newTodos)
    setId(id + 1)
  }

  return (
    <div>
      <CreateTodo onAddTodo={addTodoHandler} />
      <Todos items={todos} />
    </div>
  );
}

export default App;
```

### useRef

`useRef`는 제네릭 타입으로 어떤 DOM 요소에 접근할지 명시한다.

```tsx
// src/components/CreateTodo.tsx

import { FormEvent, useRef } from 'react'

interface Props {
  onAddTodo: (text: string) => void
}

const CreateTodo = function (props: Props) {

  // 어떤 DOM 요소에 접근할지 제네릭 타입을 지정
  // DOM 요소에 접근하지 못했을 때 null로 취급
  const todoTextInputRef = useRef<HTMLInputElement>(null)

  // 새로운 todo 생성하는 함수
  const submitHandler = (event: FormEvent) => {
    event.preventDefault()
    // !와 ?의 사용
    // !: 확실하게 DOM 요소에 접근가능할 때 사용 (string)
    // ?: DOM 요소에 접근하지 못 할 수도 있을 때 사용 (string or undefined)
    const enteredText = todoTextInputRef.current!.value
    if (enteredText.trim().length === 0) {
      alert('할 일을 입력해주세요.')
      return
    }
    props.onAddTodo(enteredText)
    todoTextInputRef.current!.value = ''
  }

  return (
    <form onSubmit={submitHandler}>
      {/* input에 ref 부착 */}
      <input type="text" ref={todoTextInputRef} />
      <button>Add Todo</button>
    </form>
  )
}

export default CreateTodo
```

## 5. React & Redux

### index

```tsx
// src/index.tsx

import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
import { Provider } from 'react-redux'
import store from './store/index'

const root = ReactDOM.createRoot(
  document.getElementById('root') as HTMLElement
);
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

### store

```ts
// src/store/index.ts

import { configureStore } from '@reduxjs/toolkit'
import counterReducer from './counterSlice'

const store = configureStore({
  reducer: { counter: counterReducer }
})

export type RootState = ReturnType<typeof store.getState>

export type AppDispatch = typeof store.dispatch

export default store
```

### hooks

```ts
// src/store/hooks.ts

import { useDispatch, useSelector, TypedUseSelectorHook } from 'react-redux'
import { RootState, AppDispatch } from './index'

export const useAppDispatch: () => AppDispatch = useDispatch
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector
```

### slice

```ts
// src/store/counterSlice.ts

import { createSlice, PayloadAction } from '@reduxjs/toolkit'

interface CounterState {
  counter: number
  showCounter: boolean
}

const initialState: CounterState = {
  counter: 0,
  showCounter: true,
}

const counterSlice = createSlice({
  name: 'counterSlice',
  initialState,
  reducers: {
    increment(state, action: PayloadAction<{ amount: number }>) {
      state.counter = state.counter + action.payload.amount
    },
    toggleCounter(state) {
      state.showCounter = !state.showCounter
    },
  }
})

export const counterActions = counterSlice.actions

export default counterSlice.reducer
```

### component

```tsx
// src/components/Counter.tsx

import { counterActions } from '../store/counterSlice'
import { useAppDispatch, useAppSelector } from '../store/hooks'

const Counter = function () {

  const dispatch = useAppDispatch()
  const counter = useAppSelector((state) => state.counter.counter)
  const show = useAppSelector((state) => state.counter.showCounter)

  const incrementHandler = () => {
    dispatch(counterActions.increment({ amount: 5 }))
  }

  const toggleCounterHandler = () => {
    dispatch(counterActions.toggleCounter())
  }

  return (
    <div>
      <>
        {show && counter}
      </>
      <button onClick={incrementHandler}>Increment</button>
      <button onClick={toggleCounterHandler}>Toggle Counter</button>
    </div>
  )
}

export default Counter
```

### thunk

```ts
// src/store/counterSlice.ts

import { createAsyncThunk, createSlice, PayloadAction } from '@reduxjs/toolkit'

const asyncIncrementFetch = createAsyncThunk(
  'counterSlice/asyncIncrementFetch',
  async () => {
    function asyncFunction () {
      return new Promise(function (resolve) {
        setTimeout(() => {
          resolve(1)
        }, 1000)
      })
    }
    const response = await asyncFunction()
    return response
  }
)

interface CounterState {
  status: string
  counter: number
}

const initialState: CounterState = {
  status: 'Welcome',
  counter: 0,
}

const counterSlice = createSlice({
  name: 'counterSlice',
  initialState,
  reducers: {
    increment(state, action: PayloadAction<{ amount: number }>) {
      state.counter = state.counter + action.payload.amount
    },
  },
  extraReducers: (builder) => {
    builder.addCase(asyncIncrementFetch.pending, (state, action) => {
      state.status = 'Loading'
    })
    builder.addCase(asyncIncrementFetch.fulfilled, (state, action)=> {
      if (typeof action.payload === 'number') {
        state.counter = state.counter + action.payload
        state.status = 'complete'
      } else {
        state.status = 'fail'
      }
    })
    builder.addCase(asyncIncrementFetch.rejected, (state, action) => {
      state.status = 'fail'
    })
  }
})

export const counterActions = counterSlice.actions

export default counterSlice.reducer

export { asyncIncrementFetch }
```

```tsx
// src/components/Counter.tsx

import { asyncIncrementFetch, counterActions } from '../store/counterSlice'
import { useAppDispatch, useAppSelector } from '../store/hooks'

const Counter = function () {

  const dispatch = useAppDispatch()
  const counter = useAppSelector((state) => state.counter.counter)
  const status = useAppSelector((state) => state.counter.status)

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