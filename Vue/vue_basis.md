# Vue 기초

## 1. Front-end Development

### SPA(Single Page Application)
- 서버로부터 새로운 페이지를 불러오지 않고 현재의 페이지를 동적으로 다시 작성함으로써 사용자와 소통하는 웹 어플리케이션
- 서버에서 최초 1장의 HTML만 전달받아 모든 요청에 대응하는 방식
- CSR(Client Side Rendering) 방식으로 요청을 처리

### SSR vs CSR
- SSR(Server Side Rendering)
  - 사용자의 요청에 따라 서버가 HTML을 렌더링하여 제공하는 방식
  - 전달 받은 새 문서를 보여주기 위해 브라우저는 새로고침을 진행
  - 렌더링이 준비된 HTML파일을 로드하기 때문에 초기 페이지 로드시간은 빠르지만, 이후에는 일부만 수정하더라도 전체를 갱신하기 때문에 비효율적이다.
  - SEO(Search Engine Optimization)에 친화적이다.

- CSR(Client Side Rendering)
  - 서버로부터 최초 한 장의 빈 HTML을 받아온 뒤, JavaScript가 동작하면서 데이터를 주고 받아 클라이언트에서 렌더링을 진행하는 것
  - 클라이언트는 서버에 AJAX로 요청을 보내고, 서버는 데이터를 JSON으로 전달
  - JSON 데이터를 JavaScript로 처리 후 DOM 트리에 반영(렌더링)
  - 초기 로드시간은 느리지만, 이후에는 응답 속도가 빠르며 요청에 대한 응답을 끊김없이 진행할 수 있다.
  - Back-End와 Front-End 간의 작업 영역을 명확히 분리할 수 있다.
  - SEO가 어렵다.

### SEO(Search Engine Optimization)
- 검색엔진 결과 페이지에서 웹사이트의 상위 노출도를 높이는 작업
- 검색자의 의도를 이해하고 이에 맞춰 웹페이지의 태그와 링크 구조를 개선해, 자연 유입 트래픽을 늘림
- SSR 방식은 초기 HTML 파일에 소스코드가 다 들어있으므로 SEO에 친화적이다.
- CSR 방식은 초기 웹페이지의 소스코드가 거의 비어있어 SEO에 불리하다.
- 최근에는 CSR로 구성된 서비스의 비중이 증가하면서, SPA 서비스도 검색 대상으로 넓히기 위해 JavaScript를 지원하는 방식으로 발전하고 있다.


## 2. Vue 시작하기

### VSCode Vetur extention
- 문법 하이라이팅, 자동완성, 디버깅 기능 제공

### Chrome Vue devtools extension
- 크롬 브라우저 개발자 도구에서 vue 디버깅 기능 제공

### Vue CDN
- [Vue2 공식 문서](https://v2.vuejs.org/)
- Vue2 공식 문서 접속 -> GET STARTED -> Installation -> CDN 복사


## 3. Vue 기본

### MVVM Pattern
<img src="https://user-images.githubusercontent.com/109272360/199487211-89e110af-8d09-481a-892b-76cf31abb40c.png" width="650px">

- 사용자 인터페이스의 개발을 Back-End로부터 분리시킨 소프트웨어 아키텍처 패턴의 일종이다.
- View와 Model 사이의 의존성이 없어 독립성을 유지할 수 있다.
- Model, View, ViewModel로 구성
  - Model: 어플리케이션에서 사용되는 데이터를 처리하는 부분(Plain JavaScript Objects)
  - View: 사용자에게 보여지는 UI 부분(DOM)
  - View Model: View를 나타내기 위한 모델로, view와 binding 된다.(Vue)

### Vue instance
- Vue 인스턴스 생성
  ```html
  <body>
    <div id="app">
      {{ message }}
    </div>

    <!-- vue CDN 작성 -->
    <script>
      const app = new Vue({
        el: '#app',
        data: {
          message: 'Hello!'
        },
        methods: {
          bye: function () {
            this.message = 'Bye!'
          },
        }
      })
    </script>
  </body>
  ```

- el(element)
  - Vue 인스턴스와 DOM을 mount(연결)하는 옵션(View와 Model을 연결)
  - HTML id 혹은 class와 마운트 가능(HTML tag와 마운트할 경우, 첫 번째로 나오는 tag와 마운트됨)

- data
  - Vue 인스턴스의 데이터 객체 혹은 인스턴스 속성
  - 데이터 객체는 반드시 기본 객체({ }, object)여야 함
  - 정의된 속성은 **interpolation {{ }}**을 통해 view에 렌더링 가능
  - 객체의 각 속성들은 `this.속성명` 형태로 접근 가능

- methods
  - Vue 인스턴스의 메소드를 정의
  - 메소드를 이용해 Vue 인스턴스의 데이터 접근 및 조작 가능
  - 주의사항) 메소드를 정의할 때 arrow function 사용 시, this가 window를 가리킴
  - 따라서 콜백함수로 쓰지 않는 이상, arrow function 사용 지양


## 4. Basic of Syntax
- Vue는 렌더링 된 DOM을 Vue 인스턴스의 data에 바인딩할 수 있는 HTML 기반 문법을 제공

### Template Interpolation
  - 가장 기본적인 바인딩 방법
  - `{{ }}`로 표기
  - 자바스크립트 표현식 형태로도 작성 가능
  - 예시
    ```html
    <body>
      <div id="app">
        {{ message }}
        {{ message.split('').reverse().join('') }}
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2.7.13/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            message: 'Hello!'
          },
        })
      </script>
    </body>
    ```

### Directives
- `v-접두사` 형태로 사용
- 표현식의 값이 변경될 때 반응적으로 DOM에 적용

- `v-html`
  - RAW HTML을 표현할 수 있는 방법
  - 단, 사용자가 입력하거나 제공하는 컨텐츠에는 **절대 사용 금지(XXS 공격)**
  - 예시
    ```html
    <body>
      <div id="app">
        <span v-html="rawHTML"></span>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2.7.13/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            rawHTML: '<p style="color:red">빨간 글씨</p>'
          },
        })
      </script>
    </body>
    ```

- `v-text`
  - Template Interpolation과 비슷한 역할
  - 예시
    ```html
    <body>
      <div id="app">
        <!-- <span>{{ message }}</span>와 같은 결과 -->
        <span v-text="message"></span>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2.7.13/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            message: 'Hello'
          },
        })
      </script>
    </body>
    ```

- `v-show`
  - 표현식에 작성된 값(boolean)에 따라 element를 보여줄 지 결정
  - 해당 element의 display 속성을 기본 속성과 none으로 toggle
  - 예시
    ```html
    <body>
      <div id="app">
        <span v-show="isActive">Vue</span>
        <!-- 같은 결과 -->
        <span style="display: none;">Vue</span>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2.7.13/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            // true로 하면 나타남
            isActive: false
          },
        })
      </script>
    </body>
    ```

- `v-if`, `v-else-if`, `v-else`
  - 표현식에 작성된 값(boolean)에 따라 element를 DOM에 렌더링할 지 결정
  - 값이 false인 경우, DOM에서 사라짐
  - 예시
    ```html
    <body>
      <div id="app">
        <!-- 아예 DOM에서 삭제 -->
        <span v-if="isActive">Vue</span>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2.7.13/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            // true로 하면 DOM에 추가
            isActive: false
          },
        })
      </script>
    </body>
    ```

- `v-show` vs `v-if`
  - `v-show`: 표현식 결과와 관계 없이 렌더링
  - `v-if`: 표현식 결과가 false인 경우 렌더링조차 x
  - 따라서 초기 렌더링에 필요한 비용은 `v-show`가 더 높을 수 있으나, 표현식 값이 자주 변경되는 경우 잦은 재 렌더링으로 `v-if`의 비용이 증가할 수 있다.

- `v-for`
  - `v-for="(value, key) in iterable" :key="keyname"`형태로 사용
  - 반복 가능한 데이터 타입(string, array, object)에 모두 사용 가능
  - `v-for="value in iterable"` 형태로도 사용 가능하지만, key값의 사용을 위해 key(or index)도 함께 적을 것을 권장
  - key 값을 사용해야 하는 이유
    - Vue 인스턴스가 바인딩하고 있는 요소 내에서 key가 겹치는 경우(특히 array의 경우), 에러 발생(코드가 동작하지 않는 것은 아님)
    - `v-for`가 서로에게 영향을 받지 않고 내부적으로 올바르게 동작하기 위함
  - 예시
    ```html
    <body>
      <!-- 3. v-for -->
      <div id="app">
        <h2>Array</h2>
        <div v-for="item in myArr">
          <p>{{ item }}</p>
        </div>
        <!--
          python
          django
          vue.js
        -->

        <!-- key값의 중복을 피하기 위해 중복되지 않는 식별자 생성 -->
        <div v-for="(item, index) in myArr2" :key="`arry-${index}`">
          <p>{{ index }}번째 아이템 : {{ item.name }}</p>
        </div>
        <!--
          0번째 아이템 : python
          1번째 아이템 : django
          2번째 아이템 : vue.js
        -->

        <div v-for="(value, key) in myObj"  :key="key">
          <p>{{ key }} : {{ value }}</p>
        </div>
        <!--
          name : harry
          age : 27
        -->
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            myArr: ['python', 'django', 'vue.js'],

            myArr2: [
              { id: 1, name: 'python'},
              { id: 2, name: 'django'},
              { id: 3, name: 'vue.js'},
            ],
            
            myObj: {
              name: 'harry',
              age: 27
            },
          }
        })
      </script>
    </body>
    ```

- `v-on`
  - 이벤트 발생 시 지정한 표현식 실행(methods 내 함수 및 자바스크립트 표현식)
  - `v-on` 이벤트 리스너가 DOM 이벤트가 발생하는지 듣고 있다가 핸들러 함수를 호출한다.
  - `:`를 통해 이벤트 및 표현식 지정
  - `@` shortcut 제공
  - 예시
    ```html
    <body>
      <div id="app">
        <button v-on:click="number++">increase Number</button>
        <p>{{ number }}</p>

        <button v-on:click="toggleActive">toggle isActive</button>
        <p>{{ isActive }}</p>

        <button @click="checkActive(isActive)">check isActive</button>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            number: 0,
            isActive: false,
          },
          methods: {
            toggleActive: function () {
              this.isActive = !this.isActive
            },
            checkActive: function (check) {
              console.log(check)
            }
          }
        })
      </script>
    </body>
    ```

- `v-bind`
  - HTML 요소의 속성에 Vue data를 연결
  - class의 경우 다양한 방식으로 연결 가능
    - `{'class명': '조건 표현식'}`
    - `['표현식1', '표현식2', ...]`
  - `:` shortcut 제공
  - 예시
    ```html
      <!-- 생략 -->
      <style>
        .red-text {
          color: red;
        }
        .border-black {
          border: solid 1px black;
        }
        .dark-mode {
          color: white;
          background-color: black
        }
        .white-mode {
          color: black;
          background-color: white;
        }
      </style>
    </head>
    <body>
      <div id="app">
        <!-- bind 예시 -->
        <a v-bind:href="url">Go To GOOGLE</a>

        <!-- 클래스 bind하는 경우 -->
        <p v-bind:class="redTextClass">빨간 글씨</p>
        <p v-bind:class="{ 'red-text': true }">빨간 글씨</p>
        <p v-bind:class="[redTextClass, borderBlack]">빨간 글씨, 검은 테두리</p>

        <!-- bind와 on 합치기 -->
        <!-- 클릭할 때마다 vue 인스턴스 속성 변경 -->
        <p :class="theme">상황에 따른 활성화</p>
        <button @click="darkModeToggle">dark Mode {{ isActive }}</button>

        <!-- bind, on, for 합치기 -->
        <!-- 버튼 누를 때마다 해당 버튼에 맞는 내용 출력 -->
        <input type="submit" @click="printName(name)" v-for="(name, index) in names" :key="`names-${index}`" :value="name">
        <p>{{ message }}</p>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            url: 'https://www.google.com/',
            redTextClass: 'red-text',
            borderBlack: 'border-black',
            isActive: true,
            theme: 'dark-mode',
            names: ['철수', '훈이', '영희'],
            message: '',
          },
          methods: {
            darkModeToggle() {
              this.isActive = !this.isActive
              if (this.isActive) {
                this.theme = 'dark-mode'
              } else {
                this.theme = 'white-mode'
              }
            },
            printName(name) {
              this.message = name
            }
          }
        })
      </script>
    </body>
    ```

- `v-model`
  - Vue 인스턴스와 DOM의 양방향 바인딩
  - `v-vind`와 `v-on` 기능의 조합으로 간편하게 동작
  - 단, IME(한글, 일본어, 중국어) 입력 시 한 글자의 입력이 끝나야 동기화되는 한계가 있음
  - 또한 event 발생 시 추가적인 custom이 필요한 경우에도 `v-vind`와 `v-on` 사용이 필요
  - 예시
    ```html
    <body>
      <div id="app">
        <h2>1. Input -> Data</h2>
        <h3>{{ myMessage }}</h3>
        <input @input="onInputChange" type="text">
        <hr>

        <!-- 이벤트 인식하는 메소드 없이도 소통 가능 -->
        <!-- 실시간 한글 입력이 필요한 경우 위 방식을 이용 -->
        <h2>2. Input <-> Data</h2>
        <h3>{{ myMessage2 }}</h3>
        <input v-model="myMessage2" type="text">
        <hr>
      </div>

      <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
      <script>
        const app = new Vue({
          el: '#app',
          data: {
            myMessage: '',
            myMessage2: '',
          },
          methods: {
            onInputChange: function (event) {
              this.myMessage = event.target.value
            },
          }
        })
      </script>
    ```

### computed
- computed 객체에 정의한 함수를 페이지가 최초로 렌더링 될 때 호출하여 계산
- 계산 결과가 변하기 전까지 함수를 재호출하는 것이 아닌, 저장(캐싱)된 값을 반환
- `method` vs `computed`
  - method: 호출 될 때마다 함수를 실행(같은 결과여도 매번 계산)
  - computed: 함수의 종속 대상의 변화에 따라 계산 여부가 결정
- 예시1
  ```html
  <body>
    <div id="app">
      <h1>add_method : {{ add_method() }}</h1>
      <h1>add_method : {{ add_method() }}</h1>
      <h1>add_method : {{ add_method() }}</h1>
      <hr>
      <h1>add_computed : {{ add_computed }}</h1>
      <h1>add_computed : {{ add_computed }}</h1>
      <h1>add_computed : {{ add_computed }}</h1>
    </div>
    <!--
      method 실행됨!
      method 실행됨!
      method 실행됨!
      computed 실행됨!
    -->

    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script>
      const app = new Vue({
        el: '#app',
        data: {
          number1: 100,
          number2: 100
        },
        computed: {
          add_computed: function () {
            console.log('computed 실행됨!')
            return this.number1 + this.number2
          }
        },
        methods: {
          add_method: function () {
            console.log('method 실행됨!')
            return this.number1 + this.number2
          },
        }
      })
    </script>
  </body>
  ```
- 예시2
  ```html
  <body>
    <div id="app">
      <p v-for="name in fullName" :key="name">{{ name }}</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script>
    const app = new Vue({
      el: '#app',
      data: {
        students: [
            {
              firstName: "길동",
              lastName: "홍",
            },
            {
              firstName: "민수",
              lastName: "김",
            },
            {
              firstName: "민지",
              lastName: "박",
            },
          ],
      },
      computed: {
        fullName() {
          return this.students.map((item) => {
            return item.lastName + item.firstName
          })
        },
      },
    })
    </script>
  </body>
  ```

### watch
- watch 객체를 정의해 특정 데이터의 변화를 감지
- key: 감시할 대상 data / value: 해당 data가 변할 시 실행할 함수
- 함수의 첫 번째 인자는 변동 전 data, 두 번째 인자는 변동 후 data
- watch 내 각 데이터 객체의 key값으로 `handler`, `deep`, `immediate`를 사용할 수 있다.
  - `handler`: 실행할 함수를 정의할 때 사용한다. (`deep`, `immediate` 사용 안 하는 경우 생략 가능)
  - `deep`: array나 object 내 데이터의 변화를 감지할 때 사용한다.
  - `immediate`: data의 변화와 관계없이 처음에 한 번 실행한다.
- 예시
  ```html
  <body>
    <div id="app">
      <!-- input에 변화가 있을 때마다 watch 감지 -->
      <div>
        <input type="text" v-model="message">
        <p>{{ message }}</p>
      </div>
      <hr>

      <!-- array에 숫자가 push될 때마다 watch 감지 -->
      <p>{{ myArray }}</p>
      <button @click="pushNumber">Push</button>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script>
      const app = new Vue({
        el: '#app',
        data: {
          message: '',
          myArray: [],
        },
        methods: {
          pushNumber() {
            this.myArray.push(1)
          },
        },
        watch: {
          // handler 생략
          message: function (val, oldVal) {
            console.log(val, oldVal)
          },

          // handler, deep 사용
          myArray: {
            handler: function (val, oldVal) {
              console.log(val, oldVal)
            },
            deep: true
          },
        }
      })
    </script>
  </body>
  ```

### filters
- 텍스트의 형식화를 적용
- `|`와 함께 사용
- interpolation 혹은 v-bind를 이용할 때 사용 가능
- filter 중첩해서 사용 가능
- 예시
  ```html
  <body>
    <div id="app">
      <p>{{ numbers | getOddNums }}</p>
      <!-- filter 중첩해서도 사용 가능 -->
      <p>{{ numbers | getOddNums | getUnderTenNums }}</p>
    </div>

    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script>
      const app = new Vue({
        el: '#app',
        data: {
          numbers: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
        },
        filters: {
          getOddNums: function (nums) {
            const oddNums = nums.filter((num) => {
              return num % 2
            })
            return oddNums
          },
          
          getUnderTenNums: function (nums) {
            const underTen = nums.filter((num) => {
              return num < 6
            })
            return underTen
          }
        }
      })
    </script>
  </body>
  ```