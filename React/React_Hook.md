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
