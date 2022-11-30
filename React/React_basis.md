# React 시작하기

## 1. React 개발 환경 세팅

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

## 2. React 기본 구조

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

## 3. 컴포넌트 만들기
**"REACT는 사용자 정의 태그를 만드는 기술이다"**

다음과 같이 컴포넌트를 사용하면 가독성 및 유지보수 측면에서 긍정적인 효과를 얻을 수 있다.

<img src="https://user-images.githubusercontent.com/109272360/204721967-61c64a59-0e78-41ec-8119-ee4decc0630d.PNG" width="440px">
<img src="https://user-images.githubusercontent.com/109272360/204721968-a8985abc-305c-423b-bae1-af8df8f88b8b.PNG" width="270px">

- 컴포넌트 만들기 단계
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


## 4. Props
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

## 5. Event

- 이벤트 설정 단계
  1. App 함수 내 컴포넌트에 props로 설정할 이벤트 명시
    ```js
    // src/App.js

    function App() {
      return (
        <div>
          // props로 넘기지 않고 바로 함수를 설정하는 것도 가능하다.
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
      return <header>
      <h1><a href="/" onClick={(event) => {
        event.preventDefault() // 새로고침 방지
        props.sampleEvent() // 설정한 이벤트 호출
      }}>{props.title}</a></h1>
    </header>
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

## 6. State
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

  => 아직 mode를 state로 인식하지 못 하기 때문
  
  => 따라서 mode를 state로 설정하고, mode가 변경되는 것을 인식하도록 설정

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
    const [mode, setMode] = useState('WELCOME')

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