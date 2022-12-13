# React 시작하기

## 1. React란
React는 **선언적 방식**으로 작성되며, **재사용가능한 반응형 컴포넌트**로 구성된다.

여기서 선언적 방식이란, 개발자가 직접 HTML요소를 UI에서 배치하도록 명령하지 것이다.

개발자는 단지 **최종 상태와 어떤 상황에서 어떤 상태가 사용되어야 하는지**를 정의하고 React는 그 상태에 따라 DOM을 업데이트 한다.


## 2. React 개발 환경 세팅

```bash
# 'myapp' 리액트 앱 생성
# 앱 명을 'react'로 할 경우 오동작할 수 있으므로 주의
# create-react-app을 줄여서 CRA라고도 함
npx create-react-app myapp
```
```bash
# 리액트 개발 모드 실행(미리보기)
npm start
```
```bash
# 리액트 배포판 만들기(build 폴더 생성)
npm run build

# bulid 폴더 내 index.html을 서비스하는 웹 서버 실행
npx serve -s build
```

참고] 개발환경 세팅 없이 react 사용하기: [stackblitz](https://stackblitz.com/edit/react-cbpfxn?file=src%2FApp.js)

## 3. React 기본 구조

- node_modules
  - 프로젝트의 모든 패키지 소스 코드가 존재하는 폴더
- package.json
  - 프로젝트에서 사용하는 라이브러리/패키지 정보가 기록되는 파일
  - dependencies: 프로젝트에서 사용하는 라이브러리에 대한 정보 (`npm install --save {라이브러리명}`을 통해 라이브러리 설치 가능)
  - scripts: 프로젝트에서 사용하는 명령어 (`npm run {script명}`을 통해 실행 가능)
- package-lock.json
  - 프로젝트에서 설치한 패키지와 관련된 모든 패키지의 버전정보를 포함
- public
  - 실제로 CRA를 배포했을 때 실제 서버에 배포되는 폴더가 public 폴더이다.
  - index.html을 포함하며, `root` div tag가 저장되어 있다.
  - 가상 DOM을 위한 html 파일이다.
- src
  - React를 작업할 폴더이다.
  - index.js: HTML 템플릿 및 JavaScript의 컴포넌트를 조합하여 렌더링하고 실제 표시한다. `ReactDON.render()`안에 보여주고 싶은 컴포넌트를 추가한다.
  - App.js: 컴포넌트를 정의하는 작업 파일로, 실제로 화면에 표시되는 내용을 여기서 정의한다.
  - App.css: App.js에 대한 css 파일이다.

## 4. JSX
JSX(JavaScript XML)는 React에서 사용되는 Javascript에 XML을 추가한 확장된 문법이다.

HTML 코드를 작성하면 브라우저에서 실행하기 전에 일반 자바스크립트 형태의 코드로 변환된다.

즉, JSX를 사용하면 다음과 같이 간편하게 컴포넌트를 구성할 수 있다.

  ```js
  // JSX를 사용하지 않을 경우(명령형 접근 방식)
  function App() {
    const para = document.createElement('p')
    para.innerText = 'Hello'
    const root = document.querySelector('#root')
    root.appendChild(para)
  }
  ```

  ```js
  // JSX를 사용할 경우
  function App() {
    return (
      <div>
        <p>Hello</p>
      </div>
    )
  }
  ```

### JSX 문법

- 반드시 최상위 요소 하나가 감싸는 형태여야 한다.
  ```js
  function App() {
    return (
      // 현재 최상위 요소는 div이다.
      // <> </> 를 사용해 빈 태그로 감쌀 수도 있다.
      <div>
        Hello
      </div>
    )
  }
  ```

- 중괄호 내부에서 자바스크립트 표현식을 사용할 수 있다.
  ```js
  function App() {
    const greeting = 'Hello';
    return (
      <div>
        {greeting}!
      </div>
    );
  }
  ```

- if문(for문) 대신 삼항 연산자(조건부 연산자)를 사용해야 한다.
  ```js
  function App() {
    const login = 'Y'
    return (
      <div>
        {login === 'Y' ? (
          <div>안녕하세요.</div>
        ) : (
          <div>비회원입니다.</div>
        )}
      </div>
    )
  }
  ```
  ```js
  // 다음과 같이 return 외부에서는 사용 가능하다.
  function App() {
    const login = 'Y'
    if (login === 'Y') {
      const msg = <div>안녕하세요.</div>
    } else {
      const msg = <div>비회원 입니다.</div>
    }
    return (
      <div>
        {msg}
      </div>
    );
  }
  ```
  ```js
  // 다음과 같이 && 연산자를 이용하는 방법도 있다.
  function App() {
    const login = 'Y'
    return (
      <div>
        {login === 'Y' && <div>안녕하세요.</div>}
        {login === 'N' && <div>비회원입니다.</div>}
      </div>
    )
  }
  ```

- sytle을 적용할 때는 객체 형태로 넣어주며, 각 속성명은 camelCase로 작성한다.
  ```js
  function App() {
    const myFontSize = '16px'
    const isLogin = false
    const style = {
      backgroundColor: 'red',
      fontSize: myFontSize, // 변수 사용도 가능하다.
      color: isLogin ? 'blue' : 'black', // 이런 방식도 유용하게 사용할 수 있다.
    }
    return (
      <div style={style}>
        Hello!
      </div>
    );
  }
  ```

- class 대신 className을 사용한다. (class는 자바스크립트의 예약어이기 때문)
  ```css
  /* src/App.css */

  .my-class {
    background-color: red;
    font-size: 16px;
  }

  .login {
    color: blue;
  }
  ```
  ```js
  // src/App.js

  function App() {
    const isLogin = false
    return (
      <div>
        <div className="my-class">
          Hello!
        </div>
        // 다음과 같이 동적으로도 클래스를 추가할 수 있다.
        <div className=`${isLogin ? 'login' : ''}`>
          Hi!
        </div>
      </div>
    );
  }
  ```

- JSX 내에서 주석은 `{/* */}`형태로 사용한다.
  ```js
  function App() {
    return (
      <div>
        {/* 주석 사용 */}
        <div>Hello</div>
      </div>
    );
  }
  ```

> 참고] React가 JSX를 변환하는 방법
>
> 이러한 JSX 코드가 있다고 해보자.
>
> ```js
> import MyComponent from './components/MyComponent'
>
> function App() {
>   return (
>     <div>
>       <h1>Hello</h1>
>       <MyComponent title="myTitle" />
>     </div>
>   ) 
> }
> ```
>
> 위 코드는 아래와 같은 방식으로 변환과정을 거친다.
>
> ```js
> import React from 'react'
> import MyComponent from './components/MyComponent'
> 
> function App() {
>   // 첫 번째 인자로 요소, 두 번째 인자로 속성, 세 번째 인자로 태그 사이 컨텐츠를 갖는다.
>   return React.createElement('div', {}, 
>     React.createElement('h1', {}, "Hello"),
>     React.createElement('MyComponent', {title="myTitle"})
>   )
> }
> ```
>
> 그래서 사실 꼭 하나의 요소만을 return 해야 하는 이유도 여기에 있다.
>
> 자바스크립트는 여러가지를 한 번에 return 할 수 없기 때문이다.
>
> (`React.createElement()` 객체로 묶어서 return 해야함)

## 5. 컴포넌트 만들기
**"REACT는 사용자 정의 태그를 만드는 기술이다"**

다음과 같이 컴포넌트를 사용하면 가독성 및 유지보수 측면에서 긍정적인 효과를 얻을 수 있다.

<img src="https://user-images.githubusercontent.com/109272360/204721967-61c64a59-0e78-41ec-8119-ee4decc0630d.PNG" width="440px">

<img src="https://user-images.githubusercontent.com/109272360/204721968-a8985abc-305c-423b-bae1-af8df8f88b8b.PNG" width="270px">

- App.js 내에서 컴포넌트 만들기
  1. 함수를 정의한다.
    ```js
    // src/App.js

    import './App.css';

    // 함수명은 반드시 대문자로 시작
    // html 코드를 리턴한다.
    function Header() {
      return <header>
        <h1><a href="/">WEB</a></h1>
      </header>
    }

    function Nav() {
      return <nav>
        <ol>
          <li><a href="/read/1">html</a></li>
          <li><a href="/read/2">css</a></li>
          <li><a href="/read/3">js</a></li>
        </ol>
      </nav>
    }

    function Article() {
      return <article>
        <h2>Welcome</h2>
        Hello, WEB
      </article>
    }

    // 생략
    ```
  
  2. APP 함수 내에서 정의한 컴포넌트를 사용한다.
    ```js
    // src/App.js

    // 생략
    function App() {
      return (
        <div>
          <Header></Header>
          <Nav></Nav>
          <Article></Article>
        </div>
      );
    }

    export default App;
    ```

- 컴포넌트 파일 만들기(이 방법 사용을 권장)
  1. src 폴더 내 components 폴더를 만들고, 자바스크립트 컴포넌트 파일을 만든다.
    - 파일명은 대문자로 시작하며 CamelCase로 작성한다.
    - 컴포넌트의 기능에 따라 components 폴더 내에서 또다시 폴더로 나누면 협업 측면에서 효과적이다.

  2. JSX 코드를 반환하는 함수를 작성한다. 이때 함수명은 파일명와 동일하게 작성하는 것이 컨벤션이다.
    ```js
    // src/components/MyComponent.js

    // 다음과 같이 축약해서 사용도 가능하다.
    // export default function MyCompontent() {
    //   ...
    // }

    // 다음과 같이 표현식과 화살표 함수를 써서 사용도 가능하다.
    // const MyComponent = () => {
    //  ...  
    // }
    // export default MyComponent

    function MyComponent() {
      return (
        <div>
          This is MyComponent
        </div>
      )
    }

    // 위 함수를 이 파일의 기본 함수로 내보내기
    export default MyComponent
    ```

  3. 부모 컴포넌트에서 작성한 컴포넌트를 불러온 뒤 사용한다.
    ```js
    // src/App.js

    import MyComponent from './components/MyComponent'

    function App() {
      return (
        <div>
          <h1>Hello</h1>
          <MyComponent />
        </div>
      )
    }

    export default App;
    ```


## 6. Props
정의한 컴포넌트에 속성(prop)을 부여할 수 있다.

- props 전달 단계
  1. App 함수 내 컴포넌트에 전달할 props를 명시한다.
    ```js
    // src/App.js

    function App() {
      return (
        <div>
          <Header title="WEB"></Header>
        </div>
      );
    }
    ```

  2. 정의한 컴포넌트에서 전달받은 props를 사용한다.
    ```js
    // 첫 번째 인자로 props를 받음
    // props의 이름은 다른 이름을 사용해도 무방하지만 관습상 props로 사용
    // 중괄호 내부는 문자열이 아닌 표현식으로 취급
    function Header(props) {
      return <header>
      <h1><a href="/">{props.title}</a></h1>
    </header>
    }
    ```

- for문 사용해보기
  ```js
  // src/App.js

  function Nav(props) {
    const lis = []
    for (let i = 0; i< props.topics.length; i++) {
      const t = props.topics[i];
      // 동적으로 만든 태그들은 반복문 내에서 각각 유일한 key를 가져야 한다.
      lis.push(<li key={t.id}><a href={'/read/'+t.id}>{t.title}</a></li>)
    }
    return <nav>
    <ol>
      {lis}
    </ol>
  </nav>
  }

  function App() {
    // 동적으로 전달할 변수 생성
    // 유일한 key를 갖게 하기 위해 id를 넣어서 전달
    const topics = [
      {id: 1, title: 'html', body: 'html is ...'},
      {id: 2, title: 'css', body: 'css is ...'},
      {id: 3, title: 'js', body: 'js is ...'}
    ]
    return (
      <div>
        // 중괄호로 감싸서 변수 자체를 전달할 수도 있다.
        <Nav topics={topics}></Nav>
      </div>
    );
  }
  ```

- map 사용하기
  - JSX 구문에서 for문 대신 map을 사용하면 배열의 각 요소를 컴포넌트로 간편하게 만들 수 있다.
  ```js
  // src/App.js

  function Nav(props) {
    return (
      <nav>
        <li>
          <a id={props.id} href={'/read/' + props.id}>
            {props.title}
          </a>
        </li>
      </nav>
    )
  }

  function App() {
    const topics = [
      {id: 1, title: 'html', body: 'html is ...'},
      {id: 2, title: 'css', body: 'css is ...'},
      {id: 3, title: 'js', body: 'js is ...'}
    ]
    const topicList = topics.map((topic) => (
      // key는 자식 컴포넌트로 전달하지는 못하므로 id를 따로 전달해주어야 함
      // (key는 오직 react 내부에서 다른 컴포넌트와 구별하기 위한 목적)
      <Nav
        key={topic.id}
        id={topic.id}
        title={topic.title}
        body={topic.body}
      />
    ))
    return (
      <div>
        <ol>
          // 여기서 직접 map을 사용해도 무관하다.
          {topicList}
        </ol>
      </div>
    );
  }
  ```

## 7. 합성
React는 **합성**이라는 개념을 이용해 코드의 재사용성을 높인다.

Modal이나 Sidebar와 같은 **박스** 역할을 하는 컴포넌트에서 유용하게 사용가능하다.

`props`의 예약어인 `children`을 사용해, Wrapper 컴포넌트라는 것을 알린다.

```js
// src/App.js

import './components/greeting.css'
import Card from './components/Card'
import MyComponent from './components/MyComponent'

function App() {
  // Wrapper 컴포넌트로 감싼다.
  // 이때 greeting이라는 클래스를 적용하기 위해 className에 적었지만, 이는 곧바로 적용되지 않는다.
  // 왜냐하면 이 작업은 Card 컴포넌트에 props의 하나로 className이라는 변수를 넘겨준 것이기 때문이다.
  // 따라서 컴포넌트 내부에서 전체를 묶는 태그의 클래스로 greeting을 추가해줘야 한다.
  <Card className='greeting'>
    <h1>Hello</h1>
    <MyComponent />
  </Card>
}

export default App;
```

```css
/* src/components/greeting.css */

.greeting {
  display: flex;
  align-items: center;
  padding: 1rem;
  margin: 1rem;
}
```

```js
// src/components/Card.js

import './card.css'

// children은 예약어로, 따로 정의하지 않고도 사용 가능하다.
function Card(props) {
  // card 스타일과 props로 전달받은 스타일을 합치기
  // 참고) card 스타일은 또 다른 컴포넌트에서 쓰일 예정 (합성을 사용한 이유)
  const classes = 'card' + props.className
  return <div className={classes}>{props.children}</div>
}

export default Card;
```

```css
/* src/components/card.css */

/* 재사용할 스타일 */
.card {
  border: 1px solid black;
  border-radius: 12px;
}
```

## 8. Event

- 이벤트 설정 단계
  1. App 함수 내 컴포넌트에 props로 설정할 이벤트 명시(props로 넘길 경우만 해당)
    ```js
    // src/App.js

    function App() {
      return (
        <div>
          // 자식 컴포넌트에서 sampleEvent라는 함수가 실행되면 alert 실행
          <Header title="WEB" sampleEvent={() => {alert('Hello')}}></Header>
        </div>
      );
    }
    ```

  2. 정의한 컴포넌트에서 해당 이벤트 사용하기
    ```js
    // src/App.js

    // onClick을 이용해 클릭했을 때 해당 이벤트가 발생하도록 설정
    // 첫 매개변수로 event 객체를 받음
    function Header(props) {
      return (
        <header>
          <h1>
            <a href="/" onClick={(event) => {
              event.preventDefault() // 새로고침 방지
              props.sampleEvent() // 설정한 이벤트 호출
            }}>{props.title}</a>
          </h1>
        </header>
      )
    }
    ```

    ```js
    // 다음과 같이 정의한 함수를 사용하면 가독성 측면에서 효과적이다.

    function Header(props) {
      function clickHeader (event) {
        event.preventDefault()
        props.sampleEvent()
      }
      /* 어느 형태로 함수를 정의해도 무관하다.
      const clickHeader = (event) => {
        event.preventDefault()
        props.sampleEvent()
      }
      */
      return (
        <header>
          <h1>
            // 주의할 점은 clickHeader()와 같이 괄호를 사용하지 않아야 한다는 것이다.
            // 괄호를 사용하게 되면 react가 JSX 코드를 변환할 때 함수가 실행되어 버린다.
            <a href="/" onClick={clickHeader}>{props.title}</a>
          </h1>
        </header>
      )
    }
    ```

- 이벤트에 인자 넣어주기
  ```js
  // src/App.js

  function Nav(props) {
    const lis = []
    for (let i = 0; i< props.topics.length; i++) {
      const t = props.topics[i];
      // 각 a 태그마다 id 값 설정하기
      lis.push(<li key={t.id}>
      <a id={t.id} href={'/read/'+t.id} onClick={(event) => {
        event.preventDefault()
        props.sampleEvent(event.target.id) // 해당 id에 맞는 경고창 띄우도록 설정
        // props.sampleEvent(t.id) 를 이용해 id값 설정 없이도 가능
      }}
      >{t.title}</a>
      </li>)
    }
    return <nav>
    <ol>
      {lis}
    </ol>
  </nav>
  }

  function App() {
    const topics = [
      {id: 1, title: 'html', body: 'html is ...'},
      {id: 2, title: 'css', body: 'css is ...'},
      {id: 3, title: 'js', body: 'js is ...'}
    ]
    return (
      <div>
        // 인자로 id값을 받아 경고창 띄우기
        <Nav topics={topics} sampleEvent={(id) => {alert(id)}}></Nav>
      </div>
    );
  }
  ```

## 9. State
`state`를 사용하면 `props` 처럼 App 컴포넌트의 렌더링 결과물에 영향을 줄 수 있다.

`props`는 컴포넌트에 전달되는 형태로 사용되지만, `state`는 컴포넌트 내부에서 관리된다는 차이가 있다.

따라서 `props`는 자식 컴포넌트에서 변경할 수 없는 데이터라면, `state`는 변경이 가능하다는 차이도 있다.

즉, 현재 컴포넌트에서 사용만 하는 데이터(`props`)와, 현재 컴포넌트에서 스스로 관리하는 데이터(`state`)를 분리하려는 목적이 있다.

- state 사용해보기
  Header, Nav 컴포넌트를 클릭하면 각 데이터에 맞게 Article 컴포넌트를 변경하기
  ```js
  function App() {
    // state로 사용할 변수 생성
    let mode = 'WELCOME'
    const topics = [
      {id: 1, title: 'html', body: 'html is ...'},
      {id: 2, title: 'css', body: 'css is ...'},
      {id: 3, title: 'js', body: 'js is ...'}
    ]
    // 현재 mode 변수에 따라 Article 컴포넌트 변경해서 출력하기
    let content = null
    if (mode === 'WELCOME') {
      content = <Article title="Welcome" body="Hello, WEB"></Article>
    } else if (mode === 'READ') {
      content = <Article title="READ" body="Hello, READ"></Article>
    }
    return (
      <div>
        // 각 컴포넌트를 클릭하면 모드가 바뀌도록 설정
        <Header title="WEB" sampleEvent={() => {mode='WELCOME'}}></Header>
        <Nav topics={topics} sampleEvent={(id) => {mode='READ'}}></Nav>
        {content}
      </div>
    );
  }
  ```

  하지만 이렇게만 하면 컴포넌트를 클릭해도 출력은 변경되지 않는다.

  => App 함수는 초기 한 번만 실행되기 때문에, return 값은 변경되지 않았음

  => React는 state에 변화가 있을 때 재렌더링을 하는데, 아직 mode를 state로 인식하지 못 하기 때문
  
  => 따라서 mode를 state로 설정하고, mode가 변경되는 것을 인식하도록 설정 -> App 함수가 재실행됨

  => `useState(초기값)` 함수를 사용해 state를 지정할 수 있다.

  ```js
  import {useState} from 'react'

  function App() {
    // useState를 사용해 '상태(배열)'를 리턴하도록 설정
    // 0번째 원소는 상태의 값을 읽을 때 사용하는 데이터
    // 1번째 원소는 상태의 값을 변경할 때 사용하는 함수
    // const _mode = useState('WELCOME')
    // const mode = _mode[0]
    // const setMode = _mode[1]
    // 축약형으로 주로 다음과 같이 사용
    // 주로 [state명, setState명] 형태로 사용한다.
    // 컴포넌트 함수 안에서 사용되어야 하며, 중첩된 함수 안에서는 사용할 수 없다.
    const [mode, setMode] = useState('WELCOME')
    // 바뀌는 값이지만 const로 해도 상관없는 이유는, return 될 때마다 새롭게 정의되기 때문이다.

    const topics = [
      {id: 1, title: 'html', body: 'html is ...'},
      {id: 2, title: 'css', body: 'css is ...'},
      {id: 3, title: 'js', body: 'js is ...'}
    ]
    // 현재 mode 변수에 따라 Article 컴포넌트 변경해서 출력하기
    let content = null
    if (mode === 'WELCOME') {
      content = <Article title="Welcome" body="Hello, WEB"></Article>
    } else if (mode === 'READ') {
      content = <Article title="READ" body="Hello, READ"></Article>
    }
    return (
      <div>
        // 각 컴포넌트를 클릭하면 모드가 바뀌도록 설정
        // setMode 함수를 실행해야 App 함수가 재실행됨
        <Header title="WEB" sampleEvent={() => {setMode('WELCOME')}}></Header>
        <Nav topics={topics} sampleEvent={(id) => {setMode('READ')}}></Nav>
        {content}
      </div>
    );
  }
  ```

  이제 어떤 글을 선택했는지도 알 수 있다.

  ```js
  function App() {
    const [mode, setMode] = useState('WELCOME')
    // 선택된 id 값을 알기위해 새로운 state 생성
    const [id, setId] = useState(null)

    const topics = [
      {id: 1, title: 'html', body: 'html is ...'},
      {id: 2, title: 'css', body: 'css is ...'},
      {id: 3, title: 'js', body: 'js is ...'}
    ]

    let content = null
    if (mode === 'WELCOME') {
      content = <Article title="Welcome" body="Hello, WEB"></Article>
    } else if (mode === 'READ') {
      // 유저가 선택한 id에 맞게 title과 body 변경해서 출력
      let title, body = null
      for (let i=0; i<topics.length; i++) {
        if (topics[i].id === id) {
          title = topics[i].title
          body = topics[i].body
        }
      }
      content = <Article title={title} body={body}></Article>
    }
    return (
      <div>
        <Header title="WEB" sampleEvent={() => {setMode('WELCOME')}}></Header>
        <Nav topics={topics} sampleEvent={(myId) => {
          setMode('READ')
          setId(myId) // 헷갈리지 않도록 인자를 myId로 설정(id로 해도 무관)
          }}></Nav>
        {content}
      </div>
    );
  }
  ```

  하지만 이렇게 설정해도 선택에 맞게 출력되지 않는다.

  => 흐름 이해하기

    <img src="https://user-images.githubusercontent.com/109272360/204749804-2a6b7639-083c-4efc-8e5c-f091aef84458.png" width="550px">

    <img src="https://user-images.githubusercontent.com/109272360/204749817-f05fb40d-41c7-4406-87d2-4f60bd9ca88f.png" width="550px">

    해결방법은 두 가지가 있다.

    ```js
    // 첫 번째 방법
    function Nav(props) {
          // 생략
          event.preventDefault()
          // 숫자형으로 바꿔주기
          props.sampleEvent(Number(event.target.id))
          // 생략
    }
    ```

    ```js
    // 두 번째 방법
    function Nav(props) {
        // 생략
        event.preventDefault()
        props.sampleEvent(t.id) // id값을 바로 이용
        // 생략
    }
    ```

> **state 더 알아보기** <br>
> state는 사용자의 input이나 컴퓨터의 input에 따라 변화한다. <br>
> 위의 예시를 보면, 사용자가 Nav의 topic을 클릭할 때 `mode` 가 'READ'로 바뀌는 것을 확인할 수 있다. <br>
> 또, 컴퓨터는 해당 topic을 불러올 때 `mode`를 'LODING' 등으로 바꿀 수도 있을 것이다. <br>
> 이러한 state에 따른 분기는 명령형이 아닌 **선언형 UI**로서 동작할 수 있게 하고, 다른 컴포넌트들과 상호작용할 때 강력한 효과를 얻을 수 있다. <br>
> **그렇다면 state는 어떻게 관리되는가?** <br>
> state는 컴포넌트 내에서 정의하고 변경되지만, 사실 React의 UI tree 구조 내에서 관리된다. <br>
> 즉, 같은 컴포넌트를 여러 개 사용해도 각 컴포넌트 내의 state나 렌더링에는 서로 영향을 주지 않는다. <br>
> 또한 위에서 사용자에게 받은 input을 통해 `topics`의 원소 개수를 늘리면, 재렌더링될 때 `topics`를 재정의하지만 늘어난 `topics`는 저장된다. <br>
> 이러한 결과는 변화된 state를 React 내부에서 관리하고, 가장 최신의 state를 우리에게 제공하기 때문이다. <br>
> 하지만 어떠한 UI tree의 컴포넌트 요소가 사라졌다가 재렌더링되면, subtree의 모든 state는 초기값으로 초기화된다. <br>
> 자세한 사항은 [공식문서](https://beta.reactjs.org/learn/preserving-and-resetting-state)에서 참고할 수 있다. <br>

## 10. module css

각 컴포넌트에서 각기 다른 css 파일을 import하더라도, 클래스명이 같은 경우는 다른 컴포넌트에 영향을 끼치게 된다.

따라서 컴포넌트 별로 재사용되지 않는 고유한 스타일을 적용하려면 **module css**를 사용해야 한다.

### module css 사용법

1. css 파일명을 `파일명.module.css`로 한다.
  ```css
  .greeting {
    color: red;
  }

  .second-greeting {
    color: blue
  }
  ```

2. 컴포넌트에서 해당 css 파일을 import 한다.
  ```js
  import classes from './파일명.module.css'
  ```

3. 중괄호로 묶은 뒤 객체로서 사용한다.
  ```js
  function App() {
    return (
      <div>
        <div className={classes.greeting}>
          Hello
        </div>
        // '-'이 포함된 클래스는 대괄호로 묶는다.
        <div className={classes.['second-greeting']}> 
          Hi
        </div>
        // 여러 개 사용
        <div className={`${classes.greeting} ${classes.['second-greeting']}`}>
          Nice to meet you
        </div>
      </div>
    )
  }
  ```
