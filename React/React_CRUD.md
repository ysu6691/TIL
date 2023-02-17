# React CRUD  

## 1. READ
READ는 `React_basis`에서 구현한 코드를 그대로 사용한다.

나머지 CREATE, UPDATE, DELETE도 이에 기반해서 구현한다.

```js
// src/App.js

import './App.css';
import {useState} from 'react'

function Header(props) {
  return <header>
  <h1><a href="/" onClick={(event) => {
    event.preventDefault()
    props.sampleEvent()
  }}>{props.title}</a></h1>
</header>
}

function Nav(props) {
  const lis = []
  for (let i = 0; i< props.topics.length; i++) {
    const t = props.topics[i];
    lis.push(<li key={t.id}>
    <a id={t.id} href={'/read/'+t.id} onClick={(event) => {
      event.preventDefault()
      props.sampleEvent(Number(event.target.id))
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

function Article(props) {
  return <article>
  <h2>{props.title}</h2>
  {props.body}
</article>
}

function App() {
  const [mode, setMode] = useState('WELCOME')
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
          setId(myId)
          }}></Nav>
      {content}
    </div>
  );
}

export default App;
```

## 2. CREATE

CREATE 링크를 누르면 article이 form으로 바뀌며, 글을 작성하면 Nav 태그에 새로운 topic이 생성된다.


```js
// src/App.js

// Create 컴포넌트 생성
function Create() {
  return <article>
    <h2>Create</h2>
    <form onSubmit={(event) => {
      event.preventDefault()
      const title = event.target.title.value
      const body = event.target.body.value
      props.onCreate(title, body)
    }}>
      <p><input type="text" name="title" placeholder='title'/></p>
      <p><textarea name="body" placeholder='body'></textarea></p>
      <p><input type="submit" value="Create"/></p>
    </form>
  </article>
} 

function App() {
  const [mode, setMode] = useState('WELCOME')
  const [id, setId] = useState(null)
  const [topics, setTopics] = useState([ // topics에 topic을 추가해야 하므로, state로 변경
    {id: 1, title: 'html', body: 'html is ...'},
    {id: 2, title: 'css', body: 'css is ...'},
    {id: 3, title: 'js', body: 'js is ...'}
  ])
  let content = null
  // 다음 topics에 들어갈 id 설정
  // nextId도 변화하는 값이기 때문에 state로 관리할 수 있지만, 이번 예시에서는 let으로 사용했다.(CREATE 마지막에 설명)
  let nextId = topics.length + 1

  if (mode === 'WELCOME') {
    // 생략
  } else if (mode === 'CREATE') {
    content = <Create onCreate={(title, body) => {
      // 유저에게 받은 정보로 새로운 topic 생성
      const newTopic = {id: nextId, title: title, body: body}
      topics.push(newTopic) // topics에 새로운 topic 넣기
      setTopics(topics) // topics 업데이트
      nextId++ // 다음 id 1 추가
    }}></Create>
  }

  return (
    <div>
      <Header title="WEB" sampleEvent={() => {setMode('WELCOME')}}></Header>
      <Nav topics={topics} sampleEvent={(myId) => {
          setMode('READ')
          setId(myId)
          }}></Nav>
      <a href="/create" onClick={(event) => { // create a태그 생성
        event.preventDefault() // 새로고침 방지
        setMode('CREATE') // 클릭하면 mode를 CREATE로 변경
      }}>CREATE</a>
      {content}
    </div>
  )
}
```

하지만 새로운 topic이 생성되지 않는 것을 확인할 수 있다.

실제로는 topics에 새로운 topic이 계속해서 push되고 있기 때문에 topics는 바뀌고 있지만, 그 변화를 인식하지 못하고 App 함수가 재실행되지 않는다.

React 공식문서는 object나 array와 같이 참조 타입의 데이터 타입은 **변경 불가능한 것처럼 취급할 것**이라고 알려준다.

따라서 object와 array의 값을 수정하고 싶을 때는 **spread syntax**를 이용해 복사한 뒤 수정할 것을 권장한다.

array의 경우 권장하는 메소드를 다음과 같이 명시하고 있다.

<img src="https://user-images.githubusercontent.com/109272360/204806494-a138d095-ab8a-4877-bbcd-70c257114a31.PNG" width="830px">

따라서 `topics` 또한 array이므로, 다음과 같이 변경해서 사용해야 한다.

```js
function App() {
  const [mode, setMode] = useState('WELCOME')
  const [id, setId] = useState(null)
  const [nextId, setNextId] = useState(4) // nextId도 state로 변경
  const [topics, setTopics] = useState([
    {id: 1, title: 'html', body: 'html is ...'},
    {id: 2, title: 'css', body: 'css is ...'},
    {id: 3, title: 'js', body: 'js is ...'}
  ])

  let content = null
  if (mode === 'WELCOME') {
    // 생략
  } else if (mode === 'CREATE') {
    content = <Create onCreate={(title, body) => {
      const newTopic = {id: nextId, title: title, body: body}
      const newTopics = [...topics] // spread syntax 사용
      newTopics.push(newTopic) // newTopics에 새로운 topic 넣기
      setTopics(newTopics) // topics 업데이트

      // 글 작성 후 해당 글로 바로 이동하기
      setMode('READ')
      setId(nextId)
      setNextId(nextId + 1) // setNextId를 이용해 다음 topic의 id값 변경
    }}></Create>
  }

  // 생략
}
```

사실 `nextId`를 state를 이용해 갱신했다면, `newTopics`를 생성하지 않아도 라우팅이 되면서 새로운 글이 계속해서 생성되었을 것이다.

(`newTopics`의 변화는 인식하지 못하지만 `nextId`의 변화를 인식해 App 함수가 재실행되기 때문)

object나 array의 변경을 위해서는 새로운 변수를 이용해야 한다는 것을 쉽게 인지하기 위해, 전 예시에서는 일부러 let을 이용해 `nextId`를 변경했다.

또한 다음과 같이 사용해서 변경할 수도 있다.

  ```js
  const newTopic = {id: nextId, title: title, body: body}
  setTopics( (prevState) => {
    return [...prevState, newTopic]
  })
  // console.log(topics)
  // 이 경우는 newTopic이 추가되기 전의 스냅샷을 갖고 있기 때문에
  // topics를 출력해보면 추가되기 이전의 배열을 반환한다.
  ```

> 참고1] **양방향 바인딩 설정하기**
> 
> 우리는 form이 제출되면 title과 body를 다시 빈 칸으로 되돌려놓고 싶다.
>
> ```js
> function Create(props) {
>   const [title, setTitle] = useState('')
>   const [body, setBody] = useState('')
>   // title과 body의 변화를 실시간으로 인식해 state 변화시키기
>   function titleChange (event) {
>     setTitle(event.target.value)
>   }
>   function bodyChange (event) {
>     setBody(event.target.value)
>   }
> 
>   return (
>     <article>
>       <h2>Create</h2>
>       <form onSubmit={(event) => {
>         event.preventDefault()
>         props.onCreate(title, body)
>         // form이 제출되면 빈칸으로 변경
>         setTitle('')
>         setBody('')
>       }}>
>         // 사용자로부터 입력을 받을 때마다 함수 실행
>         // value에 state를 지정함으로써, 양방향 바인딩
>         <p><input type="text" value={title} onChange={titleChange} name="title" placeholder='title' /></p>
>         <p><textarea name="body" value={body} onChange={bodyChange} placeholder='body'></textarea></p>
>         <p><input type="submit" value="Create" /></p>
>       </form>
>     </article>
>   )
> }

> 참고2] **ref 사용하기**
> 
> 위 방식대로 양방향 바인딩을 설정하면 사용자의 실시간 입력을 반영할 수도 있고, 다시 빈 칸으로 돌려놓는 것도 가능하다.
>
> 하지만 만약 실시간 반영은 필요없고 단지 최종 입력을 받는 것, 그리고 빈 칸으로 되돌리는 것만 구현하기에는 state를 사용하는 것이 불필요할 수 있다.
>
> state를 값의 변경 대신 단지 기록용으로 사용하기에는 불필요한 코드와 작업이 늘어나게 된다.
>
> 따라서 이러한 경우에 `ref`를 사용할 수 있다.
>
> ```js
> import { useRef } from 'react'
> 
> function App () {
>   const titleInputRef = useRef()
>   const bodyInputRef = useRef()
>
>   return (
>     <article>
>       <h2>Create</h2>
>       <form onSubmit={(event) => {
>         event.preventDefault()
>         // console.log(titleInputRef) // 실제 DOM 노드와 연결된다.
>         // 사용자가 최종으로 입력한 input을 얻을 수 있다.
>         props.onCreate(titleInputRef.current.value, bodyInputRef.current.value)
>         // form이 제출되면 빈칸으로 변경
>         // DOM을 조작하기 위해 Ref를 쓰는 것은 드물지만 편의상 사용 가능하다. 
>         titleInputRef.current.value = ''
>         bodyInputRef.current.value = ''
>       }}>
>         // value와 onChange를 이용한 양방향 바인딩없이도 최종 상태를 알 수 있고 변경도 가능하다.
>         // ref 속성을 이용해 ref와 연결
>         <p><input type="text" ref={titleInputRef} name="title" placeholder='title' /></p>
>         <p><textarea name="body" ref={bodyInputRef} placeholder='body'></textarea></p>
>         <p><input type="submit" value="Create" /></p>
>       </form>
>     </article>
>   )
> }



## 3. UPDATE

각 topic을 클릭하면 해당 topic을 update 할 수 있는 버튼이 생성되고, 해당 글을 수정할 수 있다.

```js
// 생략

// UPDATE 컴포넌트 생성
function Update(props) {
  // input과 textarea에 기존 값을 넣고 시작하기 위해 props로부터 title과 body 값을 받아와서 value로 넣기
  return <article>
    <h2>Update</h2>
    <form onSubmit={(event) => {
      event.preventDefault()
      const title = event.target.title.value
      const body = event.target.body.value
      props.onUpdate(title, body)
    }}>
      <p><input type="text" name="title" placeholder='title' value={props.title}/></p>
      <p><textarea name="body" placeholder='body' value={props.body}></textarea></p>
      <p><input type="submit" value="Update"/></p>
    </form>
  </article>
}

function App() {
  // 생략
  let contextControl = null // topic을 클릭하면 노출되는 update 버튼

  if (mode === 'WELCOME') {
    // 생략
  } else if (mode === 'READ') {
    // 생략
    // READ 모드일 때만 해당 id에 맞는 update로 향하는 태그 생성
    contextControl = <li><a href={`/update/${id}`} onClick={(event) => {
      event.preventDefault()
      setMode('UPDATE') // 모드를 update로 변경
    }}>Update</a></li>
  } else if (mode === 'CREATE') {
    // 생략
  // UPDATE 모드일 때를 분기
  } else if (mode === 'UPDATE') {
    // UPDATE 컴포넌트 출력
    // 기본적으로 topic의 기존 내용을 담고 있음
    let title, body = null
    for (let i=0; i<topics.length; i++) {
      if (topics[i].id === id) {
        title = topics[i].title
        body = topics[i].body
      }
    }
    content = <Update title={title} body={body} onUpdate={(title, body) => {
      // create와 동일하게 동작
      const newTopics = [...topics]
      // id는 Nav 컴포넌트에서 해당 topic을 클릭할 때 setId를 통해 이미 바뀌어 있음
      const updatedTopic = {id:id, title:title, body:body}
      for (let i=0; i<newTopics.length; i++) {
        if (newTopics[i].id === id) {
          newTopics[i] = updatedTopic
          break
        }
      }
      setTopics(newTopics)
      setMode('READ')
    }}></Update>
  }

  // update 버튼(contextControl) 생성
  // ul태그로 create와 update 감싸기
  return (
    <div>
      <Header title="WEB" sampleEvent={() => {setMode('WELCOME')}}></Header>
      <Nav topics={topics} sampleEvent={(myId) => {
          setMode('READ')
          setId(myId)
          }}></Nav>
      <ul>
        <li>
          <a href="/create" onClick={(event) => {
            event.preventDefault()
            setMode('CREATE')
          }}>CREATE</a>
        </li>
        {contextControl}
      </ul>
      {content}
    </div>
  );
}

export default App;
```

이는 UPDATE 클릭 시 form은 잘 바뀌지만 입력이 되지 않는 것을 확인할 수 있다.

문제는 UPDATE 컴포넌트에서 초기 값을 넣고 시작하기 위해 `input`과 `textarea`의 `value`에 props값을 넣은 것이다.

이는 props는 수정이 불가해서 사용자가 값을 입력해 `value`를 변경하려 하면 에러가 발생하기 때문이다.

따라서 props를 `useState()`를 이용해 state로서 사용해야 한다.

```js
// UPDATE 컴포넌트 내부에서 state를 생성해 에러를 해결하기
function Update(props) {
  const [title, setTitle] = useState(props.title) // title state
  const [body, setBody] = useState(props.body) // body state
  // value에 props 대신 state 넣어주기
  // onChange 속성값을 이용해 event를 감지하고, 그때의 value로 title과 body를 지속해서 갱신
  return <article>
    <h2>Update</h2>
    <form onSubmit={(event) => {
      event.preventDefault()
      const title = event.target.title.value
      const body = event.target.body.value
      props.onUpdate(title, body)
    }}>
      <p><input type="text" name="title" placeholder='title' value={title} onChange={event => {
        setTitle(event.target.value)
      }}/></p>
      <p><textarea name="body" placeholder='body' value={body} onChange={event => {
        setBody(event.target.value)
      }}></textarea></p>
      <p><input type="submit" value="Update"/></p>
    </form>
  </article>
}
```

## 4. DELETE

각 topic을 클릭하면 update와 함께 delete 버튼이 출력되고, 버튼을 클릭하면 해당 topic은 삭제된다.

```js
function App() {
  // 생략
  if (mode === 'WELCOME') {
    content = <Article title="Welcome" body="Hello, WEB"></Article>
  } else if (mode === 'READ') {
    let title, body = null
    for (let i=0; i<topics.length; i++) {
      if (topics[i].id === id) {
        title = topics[i].title
        body = topics[i].body
      }
    }
    content = <Article title={title} body={body}></Article>
    // ul 태그 안에 delete 버튼 추가하기
    // 변수를 이용해 태그를 다룰 때는 하나의 태그 안에 있어야 한다.
    // 따라서 react에서는 빈 태그를 의미하는 <></>(html에서는 나타나지 않음)를 이용해 여러 태그를 하나로 묶을 수 있다. 
    contextControl = <>
      <li><a href={`/update/${id}`} onClick={(event) => {
        event.preventDefault()
        setMode('UPDATE')
      }}>Update</a></li>
      <li><input type="button" value="Delete" onClick={() => {
        // 새로운 빈 배열을 생성해, 해당 topic 외 나머지 topic을 담아 state 변경하기
        const newTopics = []
        for (let i=0; i<topics.length; i++) {
          if (topics[i].id !== id) {
            newTopics.push(topics[i])
          }
        }
        setTopics(newTopics)
        setMode('WELCOME') // mode를 WELCOME으로 변경
      }} /></li>
    </>
  } 
  // 생략
}
```

## 5. CRUD 종합 코드
```js
import './App.css';
import {useState} from 'react'

function Header(props) {
  return <header>
  <h1><a href="/" onClick={(event) => {
    event.preventDefault()
    props.sampleEvent()
  }}>{props.title}</a></h1>
</header>
}

function Nav(props) {
  const lis = []
  for (let i = 0; i< props.topics.length; i++) {
    const t = props.topics[i];
    lis.push(<li key={t.id}>
    <a id={t.id} href={'/read/'+t.id} onClick={(event) => {
      event.preventDefault()
      props.sampleEvent(Number(event.target.id))
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

function Article(props) {
  return <article>
  <h2>{props.title}</h2>
  {props.body}
</article>
}

function Create(props) {
  return <article>
    <h2>Create</h2>
    <form onSubmit={(event) => {
      event.preventDefault()
      const title = event.target.title.value
      const body = event.target.body.value
      props.onCreate(title, body)
    }}>
      <p><input type="text" name="title" placeholder='title'/></p>
      <p><textarea name="body" placeholder='body'></textarea></p>
      <p><input type="submit" value="Create"/></p>
    </form>
  </article>
} 

function Update(props) {
  const [title, setTitle] = useState(props.title)
  const [body, setBody] = useState(props.body)
  return <article>
    <h2>Update</h2>
    <form onSubmit={(event) => {
      event.preventDefault()
      const title = event.target.title.value
      const body = event.target.body.value
      props.onUpdate(title, body)
    }}>
      <p><input type="text" name="title" placeholder='title' value={title} onChange={event => {
        setTitle(event.target.value)
      }}/></p>
      <p><textarea name="body" placeholder='body' value={body} onChange={event => {
        setBody(event.target.value)
      }}></textarea></p>
      <p><input type="submit" value="Update"/></p>
    </form>
  </article>
}

function App() {
  const [mode, setMode] = useState('WELCOME')
  const [id, setId] = useState(null)
  const [nextId, setNextId] = useState(4)
  const [topics, setTopics] = useState([
    {id: 1, title: 'html', body: 'html is ...'},
    {id: 2, title: 'css', body: 'css is ...'},
    {id: 3, title: 'js', body: 'js is ...'}
  ])
  let content = null
  let contextControl = null

  if (mode === 'WELCOME') {
    content = <Article title="Welcome" body="Hello, WEB"></Article>
  } else if (mode === 'READ') {
    let title, body = null
    for (let i=0; i<topics.length; i++) {
      if (topics[i].id === id) {
        title = topics[i].title
        body = topics[i].body
      }
    }
    content = <Article title={title} body={body}></Article>
    contextControl = <>
      <li><a href={`/update/${id}`} onClick={(event) => {
        event.preventDefault()
        setMode('UPDATE')
      }}>Update</a></li>
      <li><input type="button" value="Delete" onClick={() => {
        const newTopics = []
        for (let i=0; i<topics.length; i++) {
          if (topics[i].id !== id) {
            newTopics.push(topics[i])
          }
        }
        setTopics(newTopics)
        setMode('WELCOME')
      }} /></li>
    </>
  } else if (mode === 'CREATE') {
    content = <Create onCreate={(title, body) => {
      const newTopic = {id: nextId, title: title, body: body}
      const newTopics = [...topics]
      newTopics.push(newTopic)
      setTopics(newTopics)
      setMode('READ')
      setId(nextId)
      setNextId(nextId + 1)
    }}></Create>
  } else if (mode === 'UPDATE') {
    let title, body = null
    for (let i=0; i<topics.length; i++) {
      if (topics[i].id === id) {
        title = topics[i].title
        body = topics[i].body
      }
    }
    content = <Update title={title} body={body} onUpdate={(title, body) => {
      const newTopics = [...topics]
      const updatedTopic = {id:id, title:title, body:body}
      for (let i=0; i<newTopics.length; i++) {
        if (newTopics[i].id === id) {
          newTopics[i] = updatedTopic
          break
        }
      }
      setTopics(newTopics)
      setMode('READ')
    }}></Update>
  }

  return (
    <div>
      <Header title="WEB" sampleEvent={() => {setMode('WELCOME')}}></Header>
      <Nav topics={topics} sampleEvent={(myId) => {
          setMode('READ')
          setId(myId)
          }}></Nav>
      <ul>
        <li>
          <a href="/create" onClick={(event) => {
            event.preventDefault()
            setMode('CREATE')
          }}>CREATE</a>
        </li>
        {contextControl}
      </ul>
      {content}
    </div>
  );
}

export default App;
```