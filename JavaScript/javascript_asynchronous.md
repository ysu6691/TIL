# JavaScript Asynchronous

## 1. 비동기 처리

### 동기와 비동기
- 동기(Synchronous)
  - 모든 일을 순서대로 하나씩 처리하는 것
  - 이전 작업이 끝나야 다음 작업을 시작

- 비동기(Asynchronous)
  - 작업을 시작한 후 결과를 기다리지 않고 다음 작업을 처리하는 것(병렬적 수행)
  - 시간이 필요한 작업들은 요청을 보낸 뒤 응답이 빨리 오는 작업부터 처리

- 비동기를 사용하는 이유
  - **사용자 경험**을 고려하기 때문
  - 먼저 처리되는 부분부터 보여주면서, 긍정적인 효과를 볼 수 있음
  - 예시
    ```js
    console.log("첫 번째 작업")
    setTimeout(() => console.log("시간이 오래 걸리는 두 번째 작업"), 2000)
    console.log("세 번째 작업")

    /*
    첫 번째 작업
    세 번째 작업
    시간이 오래 걸리는 두 번째 작업
    */
    ```

### JavaScript의 비동기 처리
- 자바스크립트는 single thread 언어로, 한 번에 하나의 일만 수행할 수 있다.
  - thread: 작업을 처리할 때 실제로 작업을 수행하는 주체
- 요청이 들어올 때마다 **Call Stack**에서 순차적으로 처리한다.
- **따라서 비동기 처리를 위한 환경(런타임)이 필요하다.**
  - 런타임: 특정 언어가 동작할 수 있는 환경(자바스크립트의 경우 웹 브라우저, Node.js 등이 있다.)
- 브라우저 환경에서는 **Web API**, **Task Queue**, **Event Loop**를 제공한다.
  - Web API: 브라우저에서 제공하는 런타임 환경으로, 시간이 소요되는 작업을 처리(setTimeout, DOM event, AJAX 요청 등)
  - Task Queue: Web API에서 비동기 처리된 CallBack 함수가 대기하는 Queue
  - Event Loop: Call Stack이 비어 있는지 지속적으로 확인 후, 비어 있다면 Task Queue에서 대기 중인 가장 오래된 작업을 Call Stack으로 push

- 브라우저 환경에서의 비동기 동작은 다음과 같다.
  1. 모든 작업이 Call Stack(LIFO)으로 들어간 후 처리된다.
  2. 비동기 함수의 경우 Web API로 보내서 처리한다.
  3. Web API에서 처리가 끝난 작업들은 Task Queue(FIFO)에 순서대로 들어간다.
  4. Event Loop가 Call Stack이 비어 있는지 확인한 뒤, Task Queue에서 가장 오래된 작업을 Call Stack으로 보낸다.
  - 예시

    <img src="https://user-images.githubusercontent.com/109272360/198319006-e573f375-b447-434b-ba98-6f5ed776036d.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/198319011-c4ab7038-e87f-4276-a816-cc2cdf1414e9.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/198319020-06f0eab9-fe31-48b0-a8db-a33dccd4a98a.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/198319023-269304bb-4949-46b7-95a6-8584f7f727cc.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/198319031-af423bac-fa42-4dce-8f35-3149b5a21a30.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/198319041-6868164e-f3a4-45bd-ba99-8721d6e55f07.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/198324589-a7166c91-98eb-482c-a8ac-0e06de53eed8.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/200122230-f1a8dd0e-0518-4df3-a0e7-7fd80ee9755b.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/200122235-b7a85dc2-2f41-4ea3-a712-7d403438e97f.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/200122238-90262da4-37fb-4549-ab4f-21444aa130b2.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">
    <img src="https://user-images.githubusercontent.com/109272360/200122240-c3784e9c-ec62-451a-a95b-faeded91571c.png" width="830px" style="margin-bottom:16px; border: 2px solid black;">

## 2. Promise

### Promise란

Promise는 **자바스크립트 비동기 처리에 사용되는 객체**이다

웹 애플리케이션을 구현할 때 서버로부터 데이터를 받아오기 위해 API 요청을 보낸다.

이때 데이터를 받아오기도 전에 해당 데이터를 사용하려고 하면 오류가 발생하게 된다.

따라서 작업 비동기적으로 처리하기위해 Promise를 사용할 수 있다.

### Promise 기초

- Promise에는 3가지 상태가 있다.
  - Pending(대기): 비동기 처리 로직이 아직 완료되지 않은 상태
  - Fulfilled(이행): 비동기 처리가 완료되어 프로미스 결과 값을 반환해준 상태
  - Rejected(실패): 비동기 처리가 실패하거나 오류가 발생한 상태

- Pending
  - `new Promise()` 메소드를 호출함으로써 비동기 로직을 대기 상태로 시작한다.
  - 인자로 콜백 함수를 선언할 수 있고, 콜백 함수의 인자로 `resolve`, `reject`를 받는다.
  ```js
  new Promise(function(resolve, reject) {
    // ...
  })
  ```
  ```js
  // 아래와 같이 Promise 객체를 반환하는 함수를 생성해 사용할 수 있다.
  function asynFunction() {
    return new Promise(function(resolve, reject) {
      // ...
    })
  }
  asynFunction().then().catch()

  // 아래와 같이 Promise 객체를 반환하는 변수를 생성해 사용할 수 있다.
  const myPromise = new Promise(function (resolve, reject) {
    // ...
  })
  myPromise.then().catch()
  ```

- Fulfilled
  - 콜백 함수의 인자 `resolve`를 실행하면 이행(=완료) 상태가 된다.
  ```js
  new Promise(function(resolve, reject) {
    resolve(이행 상태일 때 넘겨줄 값)
  })
  ```

- Rejected
  - 콜백 함수의 인자 `reject`를 실행하면 실패 상태가 된다.
  ```js
  new Promise(function(resolve, reject) {
    reject(실패 상태일 때 넘겨줄 값)
  })
  ```

### Promise Chaining

`resolve()`가 호출되면 Promise가 대기 상태에서 이행 상태로 넘어가고, `.then()`을 이용해 전달된 값을 받을 수 있다.

`reject()`가 호출되면 Promise가 대기 상태에서 실패 상태로 넘어가고, `.catch()`를 이용해 전달된 값을 받을 수 있다.

`then()`과 `catch()`는 인자로 모두 콜백함수를 갖고, 그 콜백함수의 인자로 전달된 값을 받는다.

이때 Promise는 return 값으로 새로운 Promise 객체를 반환하기 때문에 chaining을 이용해 비동기 작업을 연속적으로 처리할 수 있다.

```js
function asynFunction() {
  return new Promise(function (resolve, reject) {
    resolve(1)
  })
}

asynFunction()
.then(function (response) {
  return response + 1
})
.then(function (response) {
  console.log(response)
})
.catch(function (error) {
  console.log(error)
})
```

### Promise 참고할 점

- Promise 객체를 반환하는 함수는 실행 시 Promise 객체가 된다.
  ```js
  function asynFunction() {
    return new Promise(function (resolve, reject) {
      reject()
    })
  }

  asynFunction // 함수
  asynFunction() // Promise 객체
  ```

- `new Promise()` 메소드는 `resolve()`와 `reject()`를 분기처리하지 않으면 먼저 호출한 메소드부터 실행된다.
  ```js
  function asynFunction() {
    return new Promise(function (resolve, reject) {
      resolve('성공')
      reject('실패') // 실행 x
    })
  }
  
  asynFunction()
  .then(response => console.log(response))
  .catch(error => console.log(error))
  // 성공
  ```

- `then()`은 인자로 두 가지 콜백 함수를 받는다.
  - 첫 번째 인자: fulfilled 상태일 때 실행할 콜백 함수
  - 두 번째 인자: rejected 상태일 때 실행할 콜백 함수
  ```js
  function asynFunction() {
    return new Promise(function (resolve, reject) {
      reject()
    })
  }

  function successCallback () {
    console.log('성공')
  }
  function failureCallback () {
    console.log('실패')
  }

  asynFunction()
  .then(successCallback, failureCallback)
  // 실패

  // 이렇게 then 하나로도 두 가지 인자를 모두 받을 수는 있지만, 대부분이 then과 catch로 나눠서 작성
  asynFunction()
  .then(successCallback)
  .catch(failureCallback)
  // catch(failureCallback)는 .then(null, failureCallback)과 같은 결과를 낸다.
  ```

## 3. async & await

async & await을 사용하면 promise 없이도 비동기 로직을 실행할 수 있다.

### 사용 방법

- `async`
  - 비동기 로직을 감싸는 함수의 예약어로 사용

- `await`
  - 비동기 처리 코드 앞에서 사용
  - 비동기 함수가 꼭 promise 객체를 반환해야한다.

```js
// await은 async 함수 내부에서만 사용 가능하다.
async function 함수명() {
  await 비동기함수()
  await 비동기함수()
  ...
}
```

### 예외 처리

`try`와 `catch`를 이용해 에러를 처리할 수 있고, `finally`를 이용해 무조건 실행할 코드를 적을 수 있다.

- `try`: 비동기 처리 코드를 작성
- `catch`: 오류가 발생했을 때 실행할 코드를 작성
- `finally`: `try` 블록과 관계없이 무조건 실행할 코드를 작성

```js
async function 함수명() {
  try {
    await 비동기함수()
  } catch (error) {
    console.log(error)
  } finally {
    무조건 실행할 코드
  }
}
```

### 사용 예시

```js
// promise 객체를 반환하는 함수
function timer (time) {
  return new Promise ((resolve, reject) => {
    setTimeout(() => {
      resolve(time)
    }, time)
  })
}

// async & await 사용
async function run () {
  console.log('start') // start, 처음에 실행
  await timer(1000)
  console.log('1') // 1, 1초 후에 실행
  await timer(2000)
  console.log('2') // 2, 3초 후에 실행
  await timer(3000)
  console.log('3') // 3, 6초 후에 실행
  console.log('end') // end, 마지막에 실행
}

run()
```

`await`은 promise 객체를 반환하는 함수 앞에서 쓰이며 promise 객체를 반환하기도 한다.

따라서 다음과 같이 promise 객체를 저장해서 사용할 수도 있다.

```js
function timer (time) {
  return new Promise ((resolve, reject) => {
    setTimeout(() => {
      resolve(time)
    }, time)
  })
}

// promise 객체를 변수에 저장
async function run () {
  console.log('start') // start, 처음에 실행
  let time = await timer(1000)
  console.log(time) // 1000, 1초 후에 실행
  time = await timer(time + 1000)
  console.log(time) // 2000, 3초 후에 실행
  time = await timer(time + 1000)
  console.log(time) // 3000, 6초 후에 실행
  console.log('end') // end, 마지막에 실행
}

run()
```

`async` 함수 또한 promise 객체를 반환한다.

```js
function timer (time) {
  return new Promise ((resolve, reject) => {
    setTimeout(() => {
      resolve(time)
    }, time)
  })
}

async function run () {
  console.log('start')
  await timer(1000)
  console.log('1')
  await timer(2000)
  console.log('2')
  await timer(3000)
  console.log('3')
  console.log('end')
}

async function parentRun () {
  console.log('parent start')
  await run()
  console.log('parent end')
}

parentRun()
// parent start
// start
// 1
// 2
// 3
// end
// parent end
```

만약 `await`을 promise 객체를 반환하지 않는 함수 앞에서 사용한다면 그 다음 코드를 실행하지 못 한다.

```js
function timer (time) {
  return new Promise ((resolve, reject) => {
    setTimeout(() => {
      console.log('nothing') // promise 반환 x
    }, time)
  })
}

async function run () {
  console.log('start')
  await timer(1000) // promise 객체를 반환받지못함
  // 밑에서부터 실행 x
  console.log('1')
  await timer(2000)
  console.log('2')
  await timer(3000)
  console.log('3')
  console.log('end')
}

run()
// start
// nothing
// 더 이상 출력 x
```

## 4. Axios

### Axios 사용 이유
- 클라이언트(브라우저)에서 서버로 요청을 보낼 때 form tag나 url을 통해 요청을 보내면, html 파일을 응답한다.(새로고침)
- 일부만 수정이 필요한 경우에도 전체 html 파일을 요청해야 하므로 이러한 방식은 비효율적이다.
- 따라서 html의 일부 요소만 수정할 수 있는 언어인 자바스크립트를 이용해, json을 요청하고 응답받음으로써 일부만 수정이 가능하다.
- 자바스크립트는 **XMLHttpRequest(XHR)** 객체를 이용해 서버로 요청을 보내는데, 이는 **비동기 함수**이므로 순서 보장이 불가능하다.
- 따라서 선후관계가 필요한 경우 콜백함수를 사용해야 하는데, 코드가 길어질 경우 가독성을 해치는 '콜백지옥'에 빠지게 된다.
  ```js
  // 선후관계는 보장되지만 가독성을 해침
  const func1 = function () {
    const func2 = function () {
      const func3 = function () {
        ...
      }
    }
  }
  ```
- 따라서 자바스크립트 내장 객체인 **promise 객체**를 이용해 콜백지옥을 탈출할 수 있다.
- **Axios**는 promise기반의 HTTP통신 라이브러리로, 비동기로 HTTP 통신을 가능하게 해주며(XHR 전송) return을 promise 객체로 해주기 때문에 response 데이터를 다루기도 용이하다.
- [Axios Docs](https://axios-http.com/docs/intro)

### Axios 기본 구조
```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script>
  axios({
    method: '요청 방식',
    url: '요청할 url',
    ...
  })
    .then(성공하면 수행할 콜백함수)
    .catch(실패하면 수행할 콜백함수)

// 다음과 같은 방식으로도 사용 가능하지만, 위 방식이 권장 사항이다.
// axios.요청방식('요청할 url')
//  .then()
//  .catch()
</script>
```
- `then(콜백함수)`
  - 요청한 작업이 성공하면 콜백함수 실행
  - 각 콜백함수는 이전 작업의 성공 결과를 promise 객체로 전달 받음

- `catch(콜백함수)`
  - `then()`이 하나라도 실패하면 콜백함수 실행
  - 각 콜백함수는 이전 작업의 실패 객체를 promise 객체로 전달 받음

- 예시1: 기본 양식
  ```html
  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  <script>
    axios({
      method: 'get',
      url: '#',
    })
      .then((response) => {
        return result1
      })
      .then((result1) => {
        return result2
      })
      .catch((error) => {
        console.log(error)
      })
  </script>
  ```

- 예시2: 고양이 사진 불러오기
  ```html
  <body>
    <button>Get Cat Image</button>

  <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
  <script>
    const catImageSearchURL = 'https://api.thecatapi.com/v1/images/search'
    const btn = document.querySelector('button')

    btn.addEventListener('click', function () {
      axios({
        method: 'get',
        url: catImageSearchURL,
      })
        .then((response) => {
          imgElem = document.createElement('img')
          return response
        })
        .then((response) => {
          imgElem.setAttribute('src', response.data[0].url)
          document.body.appendChild(imgElem)
        })
        .catch((error) => { 
          console.log(error)
        })
    })
  </script>
  ```

## 5. Fetch

Axios와 마찬가지로 서버에 요청을 보내고 응답을 받는 작업을 처리할 수 있다.

브라우저에서 지원하는 내장 라이브러리로, 따로 호출하지 않고 사용가능하다.

### Fetch 기본 구조

`fetch(url, [options])`

promise 객체를 반환하며 함수에 사용되는 인자는 다음과 같다.

- `url`: 접근하고자 하는 url
- `options`: 객체 형태로 method나 header 등을 지정할 수 있다. (method의 기본 값은 `GET`)

### Fetch 사용해보기

- chaining 이용
  ```js
  function getData () {
    fetch('https://jsonplaceholder.typicode.com/todos/1')
    .then(res => console.log(res))
    .catch(err => console.log(err))
  }

  getData()
  ```

- async & await 와의 사용

  ```js
  async function getData () {
    try {
      const res = await fetch('https://jsonplaceholder.typicode.com/todos/1')
      console.log(res)
    } catch (err) {
      console.log(err)
    }
  }
  ```

  또는 다음과 같이 작성할 수도 있다.

  ```js
  async function getData () {
    const res = await fetch('https://jsonplaceholder.typicode.com/todos/1')
    if (res.ok) {
      console.log(res)
    } else {
      // error 발생시키기
      throw Error(res)
    }
  }
  ```

## 6. Ajax

### Ajax란?
- Asynchronous JavaScript and XML(비동기 웹 개발 기술)
- 빠르게 동작하는 동적 웹 페이지를 만들기 위한 방법론의 하나이다.
- 웹 페이지 새로고침 없이 웹 페이지의 일부를 수정할 수 있다.
- 백그라운드에서 서버로 요청을 보내고 응답(데이터)를 받거나 데이터를 보낼 수 있다.
- 데이터의 형태는 JSON, XML, HTML 등 다양하다.

### Django에 적용하기
- `data-* attributes`
  - DOM 요소에 데이터를 담는 방법
  - `data-name1-name2-... = data` 형식으로 HTML 요소의 속성명과 데이터를 지정
  - 자바스크립트에서 `element.dataset.name1Name2...` 형식으로 접근 가능
  - 속성명 작성 시 주의사항
    - 대소문자 여부에 관계없이 'xml'로 시작 x
    - 세미콜론 및 대문자 포함 x
  - 예시
    ```html
    <div data-my-data="value"></div>
    <script>
      const divTag = document.querySelector('div')
      const myId = divTag.dataset.myData
      console.log(myId) // value
    </script>
    ```

- 팔로우 구현
  ```django
  <!-- base.html -->
  <body>
    ...
    {% block script %}
    {% endblock script %}
  </body>
  </html>
  ```
  ```python
  # accounts/views.py
  from django.http import JsonResponse

  @require_POST
  def follow(request, user_pk):
      if request.user.is_authenticated:
          User = get_user_model()
          me = request.user
          you = User.objects.get(pk=user_pk)
          if me != you:
              if you.followers.filter(pk=me.pk).exists():
                  you.followers.remove(me)
                  is_followed = False
              else:
                  you.followers.add(me)
                  is_followed = True
              context = {
                  'is_followed': is_followed,
                  'followers_count': you.followers.count(),
                  'followings_count': you.followings.count(),
              }
              return JsonResponse(context)
          return redirect('accounts:profile', you.username)
      return redirect('accounts:login')
  ```
  ```django
  <!-- accounts/profile.html -->
  {% block content %}
    <h1>{{ person.username }}님의 프로필</h1>
    <div>
      팔로워 : <span id="followers-count">{{ person.followers.all|length }}</span> / 
      팔로잉 : <span id="followings-count">{{ person.followings.all|length }}</span>
    </div>

    {% if request.user != person %}
    <div>
      <form id="follow-form" data-user-id="{{ person.pk }}">
        {% csrf_token %}
        {% if request.user in person.followers.all %}
          <input type="submit" value="언팔로우">
        {% else %}
          <input type="submit" value="팔로우">
        {% endif %}
      </form>
    <div>
    {% endif %}
    ...
  {% endblock content %}
    
  {% block script %}
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
      const form = document.querySelector('#follow-form')
      // django에서 hidden 타입으로 숨겨져 있는 csrf 값을 가진 input 태그 찾기
      const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value
      
      form.addEventListener('submit', function (event) {
        event.preventDefault()
        
        const userId = event.target.dataset.userId
        axios({
          method: 'post',
          url: `/accounts/${userId}/follow/`,
          // AJAX로 csrftoken 보내기
          headers: {'X-CSRFToken': csrftoken,}
        })
          .then((response) => {
      
            const isFollowed = response.data.is_followed
            const followBtn = document.querySelector('#follow-form > input[type=submit]')
            
            if (isFollowed === true) {
              followBtn.value = '언팔로우'
            } else {
              followBtn.value = '팔로우'
            }
      
            const followersCountTag = document.querySelector('#followers-count')
            const followingsCountTag = document.querySelector('#followings-count')
            const followersCount = response.data.followers_count
            const followingsCount = response.data.followings_count
            followersCountTag.innerText = followersCount
            followingsCountTag.innerText = followingsCount
          })
          .catch((error) => {
            console.log(error.response)
          })
      })
    </script>
  {% endblock script %}
  ```


- 좋아요 구현
  ```python
  # articles/views.py
  from django.http import JsonResponse

  @require_POST
  def likes(request, article_pk):
      if request.user.is_authenticated:
          article = Article.objects.get(pk=article_pk)

          if article.like_users.filter(pk=request.user.pk).exists():
              article.like_users.remove(request.user)
              is_liked = False
          else:
              article.like_users.add(request.user)
              is_liked = True
          context = {
              'is_liked': is_liked,
          }
          # json 객체로 반환
          return JsonResponse(context)
      return redirect('accounts:login')
  ```
  ```django
  <!-- articles/index.html -->
  {% extends 'base.html' %}

  {% block content %}
    <h1>Articles</h1>
    {% if request.user.is_authenticated %}
      <a href="{% url 'articles:create' %}">CREATE</a>
    {% endif %}
    <hr>
    {% for article in articles %}
      <p>
        <b>작성자 : <a href="{% url 'accounts:profile' article.user %}">{{ article.user }}</a></b>
      </p>
      <p>글 번호 : {{ article.pk }}</p>
      <p>제목 : {{ article.title }}</p>
      <p>내용 : {{ article.content }}</p>
      <div>
        <form class="like-forms" data-article-id="{{ article.pk }}">
          {% csrf_token %}
          {% if request.user in article.like_users.all %}
            <input type="submit" value="좋아요 취소" id="like-{{ article.pk }}">
          {% else %}
            <input type="submit" value="좋아요" id="like-{{ article.pk }}">
          {% endif %}
        </form>
      </div>
      <a href="{% url 'articles:detail' article.pk %}">상세 페이지</a>
      <hr>
    {% endfor %}
  {% endblock content %}

  {% block script %}
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
      const forms = document.querySelectorAll('.like-forms')
      const csrftoken = document.querySelector('[name=csrfmiddlewaretoken]').value

      forms.forEach((form) => {
        form.addEventListener('submit', function (event) {
          event.preventDefault()

          const articleId = event.target.dataset.articleId

          axios({
            method: 'post',
            url: `http://127.0.0.1:8000/articles/${articleId}/likes/`,
            headers: {'X-CSRFToken': csrftoken},
          })
            .then((response) => {

              const isLiked = response.data.is_liked

              const likeBtn = document.querySelector(`#like-${articleId}`)
              if (isLiked === true) {
                likeBtn.value = '좋아요 취소'
              } else {
                likeBtn.value = '좋아요'
              }
              // likeBtn.value = isLiked ? '좋아요 취소' : '좋아요'
            })
            .catch((error) => {
              console.log(error.response)
            })
        })
      })
    </script>
  {% endblock script %}
  ```

