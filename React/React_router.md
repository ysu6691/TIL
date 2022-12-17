# React Router

## 1. React Router 시작하기

### react-router 설치

```bash
$ npm install react-router-dom
```

- `react-router` vs `react-router-dom`
  - `react-router`: 라우팅을 구현하기 위한 모든 기능을 포함하는 코어 패키지이다.
  - `react-router-dom`: 웹 브라우저 기반 애플리케이션 개발에 특화된 패키지이다.

### react-router 적용
- `index.js`에서 `BrowserRouter` 태그로 root 컴포넌트(=APP) 감싸주기
  ```js
  // src/index.js

  // 생략
  import { BrowserRouter } from 'react-router-dom'

  const root = ReactDOM.createRoot(document.getElementById('root'));
  root.render(
    <BrowserRouter>
      <App />
    </BrowserRouter>
  );

  // 생략
  ```

- 경로를 지정할 컴포넌트 생성
  - 경로가 있는 컴포넌트의 경우 `pages`와 같은 폴더에서 따로 관리하는 것이 유지보수 측면에서 유리하다.
  ```js
  // src/pages/Welcome.js

  const Welcome = () => {
    return (
      <h1>
        Welcome!
      </h1>
    )
  }

  export default Welcome
  ```

- 컴포넌트 경로 지정 및 사용하기
  - `Routes`
    - `Route`를 감싸는 컴포넌트이며, 내부 `Route`에서 지정한 `path`에 해당하는 url로 이동하면 해당 컴포넌트가 렌더링된다.
    - 만약 `path`가 같은 `Route`가 여러 개이면, 위에서부터 탐색하다가 먼저 발견한 컴포넌트가 렌더링된다.
  - `Route`: 경로와 컴포넌트를 매칭한다.
    - `path`: 지정할 경로
    - `element`: 컴포넌트

  ```js
  // src/App.js

  import { Route, Routes } from 'react-router-dom'

  import Welcome from './pages/Welcome'

  function App() {
    return (
      <Routes>
        <Route path="/welcome" element={<Welcome />} />
      </Routes>
    )
  }

  export default App;
  ```

## 2. React Router 기본

### Link & NavLink

`Link`는 새로고침 없이 원하는 경로로 이동할 수 있게 하는 기능을 제공한다.

`NavLink`는 `Link`의 기본적인 기능에 추가로 특별한 기능을 제공한다. 

`NavLink`가 지정한 경로가 현재 사용자의 url과 같으면 **활성화**되어 있다는 것을 인지해, 따로 클래스를 지정할 수 있다.

- `to`: 이동할 경로 지정
- `isActive`: 현재 `NavLink`가 활성화 되어있는지 여부

```js
// src/components/MainHeader.js

import { NavLink } from 'react-router-dom'

const MainHeader = () => {
  return (
    <nav>
      <ul>
        <li>
          {/* to 뒤에 경로를 지정한다. */}
          {/* active 상태를 인지해 style과 class를 지정할 수 있다. */}
          <NavLink 
            to='/welcome' 
            style={({ isActive }) => ({ color: isActive ? 'black' : 'red' })}
            className={({ isActive }) => "nav-link" + (isActive ? " activated" : "")}>
            Welcome
          </NavLink>
        </li>
      </ul>
    </nav>
  )
}

export default MainHeader
```

### Params: 동적 경로 사용하기

Url 뒤에 동적 인자를 추가해 하나의 컴포넌트에 다양한 내용을 렌더링할 수 있다.

- 동적 경로 지정하기
  ```js
  // src/App.js

  import { Route, Routes } from 'react-router-dom'

  import Products from './pages/Products'
  import ProductDetail from './pages/ProductDetail'

  function App() {
    return (
      <Routes>
        <Route path="/products" element={<Products />} />
        <Route path="/products/:productId" element={<ProductDetail />} />
      </Routes>
    )
  }

  export default App;
  ```

  > **참고**
  > 
  > react router v5에서는 `Routes`가 위에서부터 순서대로 탐색하다가 `Route`의 **경로 시작 부분**과 일치하는 컴포넌트를 바로 렌더링했다.
  >
  > 예를 들어 위 코드에서 `/products/1`로 이동하면 `/products`에 먼저 걸리면서 하위 경로를 보지 않고 Products 컴포넌트가 라우팅 되었을 것이다.
  >
  > 그래서 이러한 문제를 해결하기 위해서는 Route 내 props로 `exact`를 넣어줬어야 했는데, v6부터는 `exact`가 기본적으로 적용되기 때문에 따로 적을 필요는 없다.

  ```js
  // src/pages/Products.js

  import { NavLink } from 'react-router-dom'

  const Products = () => {
    return (
      <section>
        <h1>Products Page</h1>
        <ul>
          <li>
            <NavLink to='/products/1'>Product1</NavLink>
          </li>
          <li>
            <NavLink to='/products/2'>Product2</NavLink>
          </li>
          <li>
            <NavLink to='/products/3'>Product3</NavLink>
          </li>
        </ul>
      </section>
    )
  }

  export default Products
  ```

- 동적 매개변수 사용하기
  ```js
  // src/pages/productDetail.js

  import { useParams } from "react-router-dom" 

  const ProductDetail = () => {
    // useParams Hook을 이용해 현재 url의 params를 객체 형태로 받아올 수 있다.
    const params = useParams()

    return (
      <section>
        <h1>Product Detail Page</h1>
        <p>{ params.productId }</p>
      </section>
    )
  }

  export default ProductDetail
  ```

### 중첩 라우팅 사용하기

전체 제품을 렌더링하는 Products 컴포넌트는 현재 `/products` 경로로 지정되어 있다.

이때 `/products/:productId` 경로로 이동하면 전체 제품의 상단에 해당 제품의 정보를 렌더링하려고 한다.

즉 Products 컴포넌트 내에서 제품을 클릭했을 때 상단에 해당 제품 정보를 함께 렌더링하려면, Products 컴포넌트 내에서 ProductDetail 컴포넌트를 렌더링 해야한다.

이는 **중첩 라우팅**을 이용해 쉽게 구현할 수 있다.

```js
// src/App.js

import { Route, Routes, NavLink } from 'react-router-dom'
import Products from './pages/Products'

function App() {
  return (
    <div>
      <NavLink to='/products'>Products</NavLink>
      <Routes>
        {/* 경로의 맨 뒤에 /*를 붙여 하위 라우팅을 탐색할 수 있게 설정한다. */}
        <Route path="/products/*" element={<Products />} />
      </Routes>
    </div>
  )
}

export default App;
```

```js
// src/pages/Products.js

import { Routes, Route, NavLink } from 'react-router-dom'
import ProductDetail from './ProductDetail'

const Products = () => {
  return (
    <section>
      <h1>Products Page</h1>
      {/* 제품 선택하면 해당 제품 정보 렌더링 */}
      <Routes>
        <Route path="/:productId" element={<ProductDetail />}/>
      </Routes>
      {/* 각 제품의 경로로 이동하는 link */}
      <ul>
        <li>
          <NavLink to='/products/1'>Product1</NavLink>
        </li>
        <li>
          <NavLink to='/products/2'>Product2</NavLink>
        </li>
        <li>
          <NavLink to='/products/3'>Product3</NavLink>
        </li>
      </ul>
    </section>
  )
}

export default Products
```

```js
// src/pages/ProductDetail.js

import { useParams, NavLink } from "react-router-dom" 

const ProductDetail = () => {
  const params = useParams()
  
  return (
    <section>
      <h1>Product Detail Page</h1>
      <p>{ params.productId }</p>
      {/* 참고: v6에서는 상대경로(..)을 이용해 상위 경로로 이동할 수 있다. */}
      <NavLink to="..">Back</NavLink>
    </section>
  )
}

export default ProductDetail
```

> 참고
>
> 다음과 같은 방식으로 `Route`를 모아서 한 컴포넌트에서 관리할 수도 있다.
>
> ```js
> import { Route, Routes, NavLink } from 'react-router-dom'
> import Products from './pages/Products'
> import ProductDetail from './pages/ProductDetail'
> 
> function App() {
>   return (
>     <div>
>       <NavLink to='/products'>Products</NavLink>
>       <Routes>
>         <Route path="/products/*" element={<Products />}>
>           <Route path=":productId" element={<ProductDetail />} />
>         </Route>
>       </Routes>
>     </div>
>   )
> }
> 
> export default App;
> ``` 

### Navigate & useNavigate

- **Navigate**

사용자가 존재하지 않는 경로로 이동했거나 로그인한 상태에서 로그인 페이지로 이동하려고 하면 다른 주소로 이동을 시켜야 한다.

이때 `Navigate` 컴포넌트를 이용해 이동할 경로를 정하여 redirect를 구현할 수 있다.

이때 props로 `replace`를 받는데, `replace`를 사용해 기존에 이동하려던 경로를 history stack에 넣을지 결정할 수 있다.

`replace`가 `true`면 기존에 이동하려던 경로를 대체 경로로 대체하고, `false`면 기존 경로도 history stack에 쌓여 뒤로가기 버튼을 누르면 해당 경로로 이동할 수 있다.

```js
// src/App.js

import { Route, Routes, Navigate } from 'react-router-dom'
import Welcome from './pages/Welcome'
import Products from './pages/Products'

function App() {
  return (
    <div>
      <Routes>
        <Route path="/welcome" element={<Welcome />} />
        <Route path="/products" element={<Products /> } />
        {/* 지정한 어떤 Route의 경로도 아닐 경우, Welcome 컴포넌트 렌더링 */}
        <Route path="/*" element={<Navigate to="/welcome" replace={ true } />} />
      </Routes>
    </div>
  )
}

export default App;
```

- **useNavigation**

`useNavigation` 훅을 사용해서 이벤트 발생 시 특정 경로로 redirect 할 수 있다.

`useNavigation`도 마찬가지로 `replace` 속성을 정의할 수 있다.

```js
// src/App.js

import { Route, Routes, useNavigate } from 'react-router-dom'
import Welcome from './pages/Welcome'

function App() {
  const navigate = useNavigate()
  
  const moveToWelcome = () => {
    navigate("/welcome", {replace: true})
  }

  // 다음과 같이 뒤로 가기와 앞으로 가기도 지원된다.
  const goBack = () => {
    navigate(-1)
  }

  const goForward = () => {
    navigate(1)
  }

  return (
    <div>
      <button onClick={moveToWelcome}>Welcome으로 이동</button>
      <button onClick={goBack}>뒤로 가기</button>
      <button onClick={goForward}>앞으로 가기</button>
      <Routes>
        <Route path="/welcome" element={<Welcome />} />
      </Routes>
    </div>
  )
}

export default App;
```

### 쿼리 파라미터

필수로 지정해야 하는 경로 외에도, `?` 뒤에서 쿼리 파라미터를 이용해 추가적인 정보를 전달할 수 있다.

이는 해당 경로에서 `useLocation` 훅을 이용해 쿼리 파라미터를 받아올 수 있다.

```js
// src/App.js

import { useRef } from 'react'
import { Route, Routes, useNavigate } from 'react-router-dom'
import Welcome from './pages/Welcome'

function App() {
  // input value를 가져오기 위해 useRef 훅 사용
  const inputRef = useRef()

  // welcome 경로로 이동하면서 input value를 쿼리 파라미터로 전달
  const navigate = useNavigate()
  const moveWelcome = (event) => {
    event.preventDefault()
    navigate("/welcome?inputdata=" + inputRef.current.value)
  }

  return (
    <div>
      <form onSubmit={moveWelcome}>
        <input type="text" ref={inputRef} />
      </form>
      <Routes>
        <Route path="/welcome" element={<Welcome />} />
      </Routes>
    </div>
  )
}

export default App;
```

```js
import { useLocation } from "react-router-dom"

const Welcome = () => {
	const location = useLocation()

  // 자바스크립트 내장 함수인 URLSearchParams를 이용해 문자열 형태로 전달된 쿼리 파라미터를 간편하게 사용할 수 있다.
	const queryParams = new URLSearchParams(location.search)
	const inputData = queryParams.get('inputdata')
  // console.log(location.search) // ?inputdata=입력받은 값 (문자열)
	// console.log(queryParams.get('inputdata')) // 입력받은 값

	return (
		<div>
			<h1>
				Welcome!
			</h1>
			<p>{inputData}</p>
		</div>
	)
}

export default Welcome
```
