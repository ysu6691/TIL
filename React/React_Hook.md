# React Hook

## 1. React Hook 규칙
- React Hook은 반드시 **컴포넌트 함수** 혹은 **Custom Hook** 내에서만 사용되어야 한다.
  ```js
  // 리액트 컴포넌트 함수가 아님(객체를 반환)
  const sampleFunction = () => {
    return {'greeting': 'hello'}
  }

  // 리액트 컴포넌트 함수(JSX를 반환)
  // Hook은 여기서 사용되어야 함
  const ComponentFunction = () => {
    return (
      <div>
        <h1>Hello</h1>
      </div>
    )
  }
  ```

- React Hook은 반드시 함수 내 최상위 스코프에서만 사용되어야 한다.
  ```js
  function App() {
    // 여기서는 사용 가능

    if (true) {
      // 여기서는 사용 불가능
    }

    function sampleFunction() {
      // 여기서도 사용 불가능
    }
  }
  ```

## 2. useEffect

### Side Effects

state는 `setState`에 의해 변경이 될 때마다 컴포넌트가 재실행된다.

이는 state의 변화로 인해 UI를 조작하는 React의 주요한 작업이다.

하지만 이외에도 다른 많은 작업을 수행해야 하는데, 이를 **Side Effects**라고 한다.

예를 들면, http 요청을 보내거나 Local storage에 데이터를 저장하는 등의 작업을 말한다.

즉, 화면에 무언가를 출력하지 않고 수행되는 작업들이 해당된다.

(http 요청에 대한 응답으로 UI를 재구성할 수는 있지만, http 요청 자체를 보내는 것은 react와 관련이 없다.)

### Side Effects는 어디서 실행되어야 하는가?

Side Effects와 관련된 작업은 일반적으로 컴포넌트 밖에서 일어나는 것이 적합하다.

어떤 컴포넌트에서 쓰이는 모든 함수가 모든 state가 바뀔 때마다 실행될 필요는 없기 때문이다.

또한 state의 변화로 인해 http 요청을 보내고 그 응답으로 state를 또다시 변화시키는 로직이 있다면, 무한루프에 빠질 수도 있다.

따라서 이러한 경우에 사용하는 것이 **useEffect Hook**이다.

### useEffect

`useEffect( function, [ dependencies ] )`

useEffect Hook은 두 개의 인자를 갖는다.
  - 첫 번째 인자: `dependencies`가 변경될 경우, 모든 컴포넌트 평가 후에 실행되는 함수
  - 두 번째 인자: 변경되면 useEffect Hook을 실행시킬 변수

- useEffect를 이용해 초기 한 번만 실행하기
  - 이미 로그인 된 유저는 새로고침을 해도 로그인 상태를 유지해야 한다.
  - useEffect와 Local Storage를 활용해 구현할 수 있다.
  ```js
  function App() {
    const [isLoggedIn, setIsLoggedIn] = useState(false)

    useEffect(() => {
      // local storage에서 가져온 로그인 정보가 1이라면(로그인 되어 있다면)
      // isLoggedIn state 변경
      const storedUserLoggedInInformation = localStorage.getItem('isLoggedIn')
      if (storedUserLoggedInInformation === '1') {
        setIsLoggedIn(true)
      }
    }, [])
    // useEffect는 앱이 시작될 때는 dependencies에 관계없이 한 번 실행된다.
    // 하지만 그 이후에는 dependencies가 빈 칸이므로 실행되지 않는다.

    // 로그인 시 local storage에 로그인 정보를 1로 변경
    const loginHandler = (username, password) => {
      localStorage.setItem('isLoggedIn', '1')
      // 검증 과정 생략
      setIsLoggedIn(true)
    }

    // 로그아웃 시 local storage에 로그인 정보 삭제
    const logoutHandler = () => {
      localStorage.removeItem('isLoggedIn')
      setIsLoggedIn(false)
    }
  }
  ```

- useEffect로 디바운스(Debounce) 구현하기

  로그인 창에서 유저의 입력이 유효한지 검증하는 경우에도 useEffect를 이용하면 편리하다.
  
  하지만 유저의 모든 입력에 대해 검증할 필요는 없다.

  유저가 어느정도 입력하고 잠깐 멈췄을 때 유효성 검사를 하는 것이 더 효율적일 것이다.

  이렇게 이벤트가 일시정지되었다고 판단되는 시점이 지나면 최후의 이벤트에 대해서만 처리하는 방식을 **디바운스**라고 한다.

  디바운스는 자바스크립트 내장 함수인 `setTimeout()`과 `clearTimeout`, 그리고 useEffect를 이용해 간편하게 구현가능하다.

  ```js
  function App() {

    const [formIsValid, setFormIsValid] = useState(false)

    // 입력을 받을 때마다 useEffect가 실행된다.
    useEffect(() => {
      // 0.5초 뒤 유효성 검사를 실행하는 함수 생성
      const identifier = setTimeout(() => {
        setFormIsValid(enteredUsername.trim().length > 4 && enteredPassword.trim().length > 4)
      }, 500)

      // useEffect에서 함수를 return하면 '클린업' 함수가 실행된다.
      // 클린업 함수는 처음 앱이 실행되는 경우를 제외하고 useEffect가 실행되기 전 가장 먼저 실행된다.
      return () => {
        // 유효성 검사 직전에 이전 타이머 지우기
        // 즉, 입력을 받을 때마다 이전 타이머 지우기 -> 새로운 타이머 생성의 반복
        // 따라서 입력을 0.5초간 멈추는 경우에만 유효성 검사가 실행된다.
        clearTimeout(identifier)
      }
    }, [enteredUsername, enteredPassword])
  }
  ```

## 3. useMemo

### useMemo

`useMemo( function, [ dependencies ] )`

useMemo는 **Memoization** 기법, 즉 기존에 수행한 연산의 결괏값을 저장해 두고 값이 변하지 않으면 그 결괏값을 재사용하는 Hook이다.

컴포넌트가 재렌더링될 때 결괏값이 변하지 않았을 경우 캐싱된 값을 사용하기 때문에 컴포넌트의 성능을 최적화시킬 수 있다.

첫 번째 인자로는 콜백 함수를 받고 **함수의 return 값**을 결괏값으로 저장한다.

두 번째 인자로는 의존성 배열을 받고, 배열의 요소가 업데이트 될 때만 함수를 재실행한다.

### useMemo 예시

**많은 작업을 거치는 함수를 갖는 컴포넌트인 경우**

```js
import { useState } from 'react'

function App() {
  const [number1, setNumber1] = useState(0)
  const [number2, setNumber2] = useState(0)

  const heavyFunction = (input) => {
    console.log('많은 작업 진행중...')
    return number2 ** 2
  }
  const squareOfNumber2 = heavyFunction(number2)

  return (
    <div>
      <div>number1: {number1}</div>
      <button onClick={() => {setNumber1(number1+1)}}>+</button>
      <div>number2: {number2}</div>
      <div>number2의 제곱: {squareOfNumber2}</div>
      <button onClick={() => {setNumber2(number2+1)}}>+</button>
    </div>
  )
}

export default App;
```

위 예시는 `number1`이 변화하면 컴포넌트가 재실행되면서 `squareOfNumber2`를 계산하기 위해 `heavyFunction` 또한 재실행된다.

`squareOfNumber2`는 `number2`가 변화할 때만 계산해주면 되는 값이므로, `number1`이 변화할 때마다 불필요하게 함수가 재실행되는 문제가 발생한다.

따라서 이러한 경우 `useMemo`를 사용하면 컴포넌트의 성능을 향상시킬 수 있다.

```js
import { useMemo, useState } from 'react'

function App() {
  const [number1, setNumber1] = useState(0)
  const [number2, setNumber2] = useState(0)

  // number2가 변화할 때만 결괏값을 재계산한다.
  const squareOfNumber2 = useMemo(() => {
    console.log('많은 작업 진행중...')
    return number2 ** 2
  }, [number2])

  // 아래는 동일
  return (
    <div>
      <div>number1: {number1}</div>
      <button onClick={() => {setNumber1(number1+1)}}>+</button>
      <div>number2: {number2}</div>
      <div>number2의 제곱: {squareOfNumber2}</div>
      <button onClick={() => {setNumber2(number2+1)}}>+</button>
    </div>
  )
}

export default App;
```

**참조형 자료형(객체, 배열 등)을 변수로 선언해서 사용하는 경우**

```js
import { useEffect, useState } from 'react'

function App() {
  const [number, setNumber] = useState(0)

  const myObj = {'key': 'value'}

  useEffect(() => {
    console.log('useEffect 실행')
  }, [myObj])

  return (
    <div>
      <button onClick={() => {setNumber(number+1)}}>+</button>
    </div>
  )
}

export default App;
```

위 예시에서 `useEffect`는 `myObj`의 변화에만 실행되어야 한다.

하지만 위 로직에서 `myObj`는 변화하지 않아도 `number`가 변화할 때마다 `useEffect`가 실행되는 것을 확인할 수 있다.

이는 참조형 자료형의 경우 재렌더링될 때마다 메모리 상의 새로운 주소를 참조하기 때문에 값이 변경된 것으로 인식되기 때문이다.

따라서 이러한 경우에도 `useMemo`를 사용해 불필요한 함수의 재실행을 막을 수 있다.

```js
import { useEffect, useMemo, useState } from 'react'

function App() {
  const [number, setNumber] = useState(0)

  // 빈 배열을 의존성 배열로 사용함으로써 초기 한 번만 실행되도록 한다.
  // 컴포넌트가 재실행되어도 myObj는 초기값 그대로 변하지 않는다.
  const myObj = useMemo(() => {
    return {'key': 'value'}
  }, [])

  useEffect(() => {
    console.log('useEffect 실행')
  }, [myObj])

  return (
    <div>
      {number}
      <button onClick={() => {setNumber(number+1)}}>+</button>
    </div>
  )
}

export default App;
```

## 4. useCallback

### useCallback

`useCallback( function, [ dependencies ] )`

`useCallback`은 `useMemo`와 비슷하게, 메모이제이션된 콜백 함수를 반환한다.

`useMemo`와의 핵심적인 차이는 `useMemo`는 함수의 값만 반환하지만 `useCallback`은 함수 자체를 반환한다는 것이다.

따라서 `useCallback(fn, [deps])`는 `useMemo(() => fn, [deps])`와 동일하게 동작한다.

```js
// useMemo를 사용하는 경우

import { useMemo, useState } from 'react'

function App() {
  const [number1, setNumber1] = useState(0)
  const [number2, setNumber2] = useState(0)

  const squareOfNumber2 = useMemo(() => {
    console.log('많은 작업 진행중...')
    return number2 ** 2
  }, [number2])

  return (
    <div>
      <div>number1: {number1}</div>
      <button onClick={() => {setNumber1(number1+1)}}>+</button>
      <div>number2: {number2}</div>
      <div>number2의 제곱: {squareOfNumber2}</div>
      <button onClick={() => {setNumber2(number2+1)}}>+</button>
    </div>
  )
}

export default App;
```

`useMemo`는 결국 함수의 값을 반환하기 때문에 인자를 넣을 수 없지만, `useCallback`은 인자의 활용이 가능하다.

```js
// useCallback을 사용하는 경우

import { useCallback, useState } from 'react'

function App() {
  const [number1, setNumber1] = useState(0)
  const [number2, setNumber2] = useState(0)

  // useCallback 사용 (파라미터 전달)
  const squareOfNumber2 = useCallback((param) => {
    console.log('많은 작업 진행중...')
    return number2 ** param
  }, [number2])

  return (
    <div>
      <div>number1: {number1}</div>
      <button onClick={() => {setNumber1(number1+1)}}>+</button>
      <div>number2: {number2}</div>
      {/* 파라미터 전달 가능 */}
      <div>number2의 제곱: {squareOfNumber2(2)}</div>
      <button onClick={() => {setNumber2(number2+1)}}>+</button>
    </div>
  )
}

export default App;
```

이를 `useMemo`로 바꾸려면 다음과 같이 사용해야 한다.

```js
...
const squareOfNumber2 = useMemo(() => (param) => {
  ...
})
...
return (
  ...
  {/* 이제는 파라미터 전달이 가능하다. */}
  <div>number2의 제곱: {squareOfNumber2(2)}</div>
  ...
)
```

## 5. useRef

### useRef

`useRef(initialValue)`

`useRef`는 특정 DOM을 선택할 수 있는 Hook이다.

사용자의 input 값이나 focus를 구현하는 등의 상황에서 유용하게 사용된다.

### useRef 예시

**사용자의 입력이 끝나면 빈 칸으로 되돌리기**

사용자에게 입력을 받은 뒤 input 내부를 비워놓고자 하는 상황이 자주 발생한다.

이러한 경우 state를 이용한 양방향 바인딩으로 구현할 수 있다.

```js
import { useState } from 'react'

function App() {

  const [inputData, setInputData] = useState("")

  const printData = (event) => {
    event.preventDefault()
    console.log(inputData)
    setInputData("")
  }

  return (
    <form onSubmit={printData}>
      <input 
        type="text"
        value={inputData} 
        onChange={(event) => {setInputData(event.target.value)}} />
    </form>
  )
}

export default App;
```
 
위 방식대로 양방향 바인딩을 설정하면 사용자의 실시간 입력을 반영할 수도 있고, 다시 빈 칸으로 돌려놓는 것도 가능하다.

하지만 만약 실시간 반영은 필요없고 단지 최종 입력을 받는 것, 그리고 빈 칸으로 되돌리는 것만 구현하기에는 state를 사용하는 것이 불필요할 수 있다.

state를 값의 변경 대신 단지 기록용으로 사용하기에는 불필요한 코드와 작업이 늘어나게 된다.

따라서 이러한 경우 `useRef`를 이용해 간편하게 구현이 가능하다.

```js
import { useRef } from 'react'

function App() {

  const inputRef = useRef()

  const printInput = (event) => {
    event.preventDefault()
    // ref.current를 이용해 DOM에 접근가능하다.
    console.log(inputRef.current.value)
    // 빈칸으로 되돌리기
    inputRef.current.value = ""
    // 추가 기능: 전송 버튼을 클릭해도 input에 focus 유지
    inputRef.current.focus()
  }

  return (
    <form onSubmit={printInput}>
      <input type="text" ref={inputRef} />
    </form>
  )
}

export default App;
```

### forwardRef

`useRef`는 설정한 ref를 컴포넌트로 전달하는 것은 불가능하다.

하지만 `forwardRef`를 이용하면 ref를 컴포넌트로 전달해서 사용할 수 있다.

**forwardRef를 사용해 컴포넌트 밖을 클릭했는지 확인하기**

```js
import { useRef, useState } from 'react'
import Modal from './components/Modal'

function App() {

  const [modalMode, setModalMode] = useState('close')
  const modalRef = useRef()

  // 컴포넌트 밖을 클릭했는지 확인
  const checkClickModalOutside = (event) => {
    if (modalMode === 'open') {
      // 컴포넌트가 return하는 최상위 div 태그를 ref로 설정해,
      // 클릭한 태그가 컴포넌트 내에 속하는지 확인
      if (!modalRef.current.contains(event.target)) {
        setModalMode('close')
      }
    }
  }

  return (
    <div onClick={checkClickModalOutside}>
      <button onClick={() => {setModalMode('open')}}>Open Modal</button>
      {/* 모드가 open일 때만 컴포넌트를 렌더링한다. */}
      {/* ref를 전달한다. */}
      {modalMode === 'open' && <Modal ref={modalRef} />}
    </div>
  )
}

export default App;
```

```js
import {forwardRef} from 'react'

// forwardRef 사용
// 반드시 props와 ref를 인자로 받아야 함
const Modal = forwardRef((props, ref) => {

  const style = {
    border: "1px solid black",
    width: "100px",
    height: "100px"
  }

  return (
    {/* 컴포넌트 자체를 ref로 설정 */}
    <div ref={ref} style={style}>
      Modal
    </div>
  )
})

export default Modal
```

## 6. useContext

### useContext 사용해보기

`useContext`는 컴포넌트 간에 props로 전달되던 state를 중앙 저장소에서 한 번에 관리할 수 있게 해주는 Hook이다.

- 중앙 저장소 생성
  ```js
  // src/store/activate-context.js

  import { createContext } from "react";

  const ActivateContext = createContext({
    isActivated: false
  })

  export default ActivateContext
  ```

- Provide
  ```js
  // src/App.js

  import ActivateContext from './store/activate-context'
  import MyComponent from './components/MyComponent'
  import { useState } from 'react'

  function App() {

    const [isActivated, setIsActivated] = useState(false)

    // Provider로 컴포넌트를 감싸 store 내 데이터 공급
    // value 내부에 어떤 데이터를 전달할지 반드시 적어야함
    return (
      <ActivateContext.Provider
        // 축약형으로 다음과 같이 작성 가능
        // value={{ isActivated }}
        // 따라서 Context에서 선언한 key값과 동일한 변수명을 사용하는 것이 좋음
        value={{
          isActivated: isActivated,
        }}>
        <button onClick={() => {setIsActivated(!isActivated)}}>Click!</button>
        <MyComponent />
      </ActivateContext.Provider>
    )
  }

  export default App;
  ```

- Context 얻기
  ```js
  // src/components/MyComponent.js

  import { useContext } from "react"
  import ActivateContext from '../store/activate-context'

  const MyComponent = () => {
    // Context 얻기
    // context는 DOM 트리에서 가장 가까이에 있는 Provider의 value prop에 의해 결정된다.
    const context = useContext(ActivateContext)

    return (
      <div>
        {context.isActivated && <p>활성화되었습니다.</p>}
        {!context.isActivated && <p>비활성화되었습니다.</p>}
      </div>
    )
  }

  export default MyComponent
  ```

- 함수도 전달이 가능하다.
  ```js
  // src/App.js

  import ActivateContext from './store/activate-context'
  import MyComponent from './components/MyComponent'
  import { useState } from 'react'

  function App() {

    const [isActivated, setIsActivated] = useState(false)
    // active toggle 함수
    const activateHandler = () => {
      setIsActivated(!isActivated)
    }

    // 버튼을 컴포넌트 내부로 이동
    return (
      <ActivateContext.Provider
        value={{ isActivated, activateHandler }}>
        <MyComponent />
      </ActivateContext.Provider>
    )
  }

  export default App;
  ```

  ```js
  // src/components/MyComponents.js

  import { useContext } from "react"
  import ActivateContext from '../store/activate-context'

  const MyComponent = () => {
    const context = useContext(ActivateContext)

    return (
      <div>
        {context.isActivated && <p>활성화되었습니다.</p>}
        {!context.isActivated && <p>비활성화되었습니다.</p>}
        {/* 버튼 클릭할 때마다 context 내 함수 실행 */}
        <button onClick={context.activateHandler}>Click!</button>
      </div>
    )
  }

  export default MyComponent
  ```

### context 내부에서 로직 관리하기

지금까지 해온 방식은 context와 관련된 데이터도 App 컴포넌트 내에서 state와 함수를 이용해 관리했다.

하지만 context와 관련된 데이터 및 함수를 context 파일 내부에서 관리한다면 더 명확한 코드의 분리가 가능해진다.

```js
// src/store/activate-context.js

import { createContext, useState } from "react";

const ActivateContext = createContext({
  isActivated: false,
  // Tip
  // context 내에 함수를 정의할 필요는 없지만,
  // 더미 함수를 만듦으로써 컴포넌트에서 IDE 자동완성 기능을 편리하게 사용할 수 있다.
  setIsActivated: () => {}
})

// Provider 컴포넌트 생성
export const ActivateContextProvider = (props) => {

  // App에서 관리했던 state와 함수를 옮기기
  const [isActivated, setIsActivated] = useState(false)
  const activateHandler = () => {
    setIsActivated(!isActivated)
  }

  // 합성 이용
  return (
    <ActivateContext.Provider
      value={{ isActivated, activateHandler }}
    >
      {props.children}  
    </ActivateContext.Provider>
    
  )
}

export default ActivateContext
```

```js
// src/index.js

// 생략
import { ActivateContextProvider } from './store/activate-context';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  // App 전체를 Provider로 감싸기
  <ActivateContextProvider>
    <App />
  </ActivateContextProvider>
);

reportWebVitals();
```

```js
// src/App.js

import MyComponent from './components/MyComponent'

// App 컴포넌트는 훨씬 간결해졌다.
function App() {

  return (
    <div>
      {/* 컴포넌트는 전과 동일하게 사용 가능 */}
      <MyComponent />
    </div>
  )
}

export default App;
```

### Context의 한계

context는 props를 이용한 연결 없이도 중앙 저장소에서 데이터를 관리할 수 있었다.

하지만 context에서 관리하는 데이터가 바뀔 경우 App 내 전체 컴포넌트가 재렌더링되므로, 자주 바뀌는 데이터를 context에서 관리하는 것은 성능 측면에서 좋지 않다.

그렇기 때문에 context는 prop을 완전히 대체해서 사용할 수 없다고 할 수 있다.

따라서 자주 바뀌면서 긴 prop 체인으로 연결해야 하는 데이터가 있다면 고민에 빠지게 되는데, 이러한 경우에 **Redux**를 사용하는 것이 좋다.

## 6. 