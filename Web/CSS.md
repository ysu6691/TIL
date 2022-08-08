# CSS

## 1. CSS란?

### CSS
- CSS(Cascading Style Sheets)
  - 스타일을 지정하기 위한 언어

- CSS 구문
  - 선택자를 통해 지정할 HTML 요소를 선택
  - 중괄호 안에 속성과 값으로 이루어진 선언을 진행
  ```html
    h1 {
      color: blue;
      font_size: 15px;
    }

    * h1: 선택자(Selector)
    * color: blue; : 선언
    * color, font-size: 속성(property)
    * blue, 15px: 값(Value)
  ```

### CSS 정의 방법
- 인라인(inline)
- 내부 참조(embedding) - `<style>`
- 외부 참조(link file) - 분리된 CSS 파일

```html
인라인:
<body>
<h1 style = "color: blue; font-size: 100px;">Hello</h1>
</body>


내부 참조:
<head>
  <style>
    h1 {
      color: blue;
      font-size: 100px;
    }
  </style>
</head>
<body>
  <h1>Hello</h1>
</body>


외부 참조:
<!--
mystyle.css
h1 {
    color: blue;
    font-size: 100px;
}
-->

<head>
  <link rel="stylesheet" href="mystyle.css">
</head>
<body>
  <h1>Hello</h1>
</body>
```

## 2. CSS 기본

### 선택자(Selector) 유형
- 기본 선택자
  - 전체 선택자(*), 요소 선택자(태그)
  - 클래스 선택자(.), 아이디 선택자(#), 속성 선택자

- 결합자(Combinators)
  - 자손 결합자, 자식 결합자
  - 일반 형제 결합자, 인접 형제 결합자

- 의사 클래스/요소 (Pseudo Class)
  - 링크, 동적 의사 클래스
  - 구조적 의사 클래스, 기타 의사 클래스, 의사 엘리먼트, 속성 선택자

```css
/* 전체 선택자 */
* {
  color: red;
}

/* 요소 선택자 */
h2 {
  color: orange;
}

h3,
h4 {
    color: blue;
}

/* 클래스 선택자 */
.green {
  color: green;
}

/* id 선택자 */
#purple {
  color: purple;
}

/* 자손 결합자 */
/* .클래스 요소 형태로 사용*/
.box p{
  font-size: 30px;
}
```

### CSS 적용 우선순위(cascading order)
1. 중요도(Importance)
    - !important
2. 우선 순위(Specificity)
    - 인라인 > id > class, 속성, pusedo-class > 요소, pseudo-element
3. CSS 파일 로딩 순서

```html
예제
1~8번까지 색은?
<head>
  <style>
    h2 {
      color: darkviolet !important;
    }

    p {
      color: orange;
    }

    .blue {
     color: blue;
    }

    .green {
     color: green;
    }

    #red {
     color: red;
    }
  </style>
</head>
<body>
  <p>1</p>
  <p class="blue">2</p>
  <p class="blue green">3</p>
  <p class="green blue">4</p>
  <p id="red" class="blue">5</p>
  <h2 id="red" class="blue">6</h2>
  <p id="red" class="blue" style="color: yellow">7</p>
  <h2 id="red" class="blue" style="color: yellow">8</h2>

<!--
1: orange (요소)
2: blue (클래스)
3: green (클래스 여러개면 css에서 뒤에 나온 클래스 우선)
4: green (클래스 여러개면 css에서 뒤에 나온 클래스 우선)
5: red (id)
6: darkviolet (important)
7: yellow (inline)
8: darkviolet (important)
-->
```

### CSS 상속
- CSS는 상속을 통해 부모 요소의 속성을 자식에게 상속
- 속성(프로퍼티) 중에는 상속이 되는 것과 되지 않는 것이 있음
  - 상속 되는 것들: text 관련 요소(font, color, text-align), opacity, visibility 등
  - 상속 되지 않는 것들: Box model 관련 요소(width, height, margin), Position 관련 요소(position, top/right/bottom/left 등) 등

```html
<head>
  <style>
    p {
      color: red;
      border: 3px solid black;
    }
  </style>
</head>
<body>
  <p>안녕하세요! <span>테스트</span> 입니다.</p>
</body>

<!--
span에는 border 적용 x
-->
```

### CSS 기본 스타일
- 크기 단위
  - px(픽셀): 고정적인 단위
- 백분율 단위
  - %: 가변적인 레이아웃에서 자주 사용
- em
  - (바로 위, 부모 요소에 대한) 상속의 영향을 받아 상대적인 크기를 가짐
- rem
  - (바로 위, 부모 요소에 대한) 상속의 영향을 받지 않음
  - 최상위 요소(html)의 사이즈를 기준으로 배수 단위를 가짐 (1rem = 16px)

```html
<head>
  <style>
    .font-big {
        font-size: 36px;
    }
    .em {
      font-size: 2em;
    }
    .rem {
      font-size: 2rem;
    }
  </style>
</head>
<body>
  <ul class="font-big">
    <li class="em">2em</li> <!-- 72px -->
    <li class="rem">2rem</li> <!-- 32px -->
    <li>no class</li> <!-- 36px -->
  </ul>
</body>
```

- 크기 단위(viewport)
  - 웹페이지를 방문한 유저에게 바로 보이게 되는 웹 컨텐츠의 영역 (디바이스 화면)
  - 디바이스의 viewport를 기준으로 상대적인 사이즈가 결정됨
  - vw, vh, vmin, vmax

```html
<head>
  <style>
    h1 {
      color: black;
      background-color: pink;
    }
    .px {
      width: 200px;
    }
    .vw {
      width: 50vw;
    }
  </style>
</head>
<body>
  <h1 class="px">px사용</h1> <!-- 브라우저를 변경해도 그대로-->
  <h1 class="vw">vw사용></h1> <!-- 브라우저에 따라 크기가 변함-->
</body>
```

- 색상 단위
  - 색상 키워드
    - 대소문자 구분 x
    - red, blue와 같은 특정 색을 직접 글자로 나타냄
  - RGB 색상
    - 16진수 표기법 혹은 함수형 표기법을 사용해서 색 표현
  - HSL 색상
    - 색상, 채도, 명도를 통해 색 표현

```css
p {color: black;}

p {color: #000;}
p {color: #000000;}

p {color: rgb(0, 0, 0);}
p {color: rgba(0, 0, 0, 0.5);} /* a는 투명도 */
p {color: hsl(120, 100%, 0);}
```

### Selectors 심화
- 결합자 (Combinators)
  - 자손 결합자(A B)
    - selector A 하위의 모든 selector B 요소
  - 자식 결합자(A > B)
    - selector A 바로 아래의 selector B 요소
  - 일반 형제 결합자(A ~ B)
    - selector A의 형제 요소 중 뒤에 위치하는 selector B 요소를 모두 선택
  - 인접 형제 결합자(A + B)
    - selector A의 형제 요소 중 바로 뒤에 위치하는 selector B 요소를 선택 (바로 뒤에 다른 요소가 있으면 적용x)

```HTML
<!-- 자손 결합자 -->
<head>
  <style>
    div span {
      color: red;
    }
  </style>
</head>
<body>
<div>
  <span>이건 빨강입니다.</span>
  <p>이건 빨강이 아닙니다.</p>
  <p>
    <span>이건 빨강입니다.</span>
  </p>
</div>
</body>

<!-- 자식 결합자 -->
<head>
  <style>
    div > span {
      color: red;
    }
  </style>
</head>
<body>
<div>
  <span>이건 빨강입니다.</span>
  <p>이건 빨강이 아닙니다.</p>
  <p>
    <span>이건 빨강이 아닙니다.</span>
  </p>
</div>
</body>

<!-- 일반 형제 결합자 -->
<head>
  <style>
    p ~ span {
      color: red;
    }
  </style>
</head>
<body>
<span>이건 빨강이 아닙니다.</span>
<p>이건 빨강이 아닙니다.</p>
<b>이건 빨강이 아닙니다.</b>
<span>이건 빨강입니다.</span>
<b>이건 빨강이 아닙니다.</b>
<span>이건 빨강입니다.</span>
</body>

<!-- 인접 형제 결합자 -->
<head>
  <style>
    p + span {
      color: red;
    }
  </style>
</head>
<body>
<span>이건 빨강이 아닙니다.</span>
<p>이건 빨강이 아닙니다.</p>
<span>이건 빨강입니다.</span>
<b>이건 빨강이 아닙니다.</b>
<span>이건 빨강이 아닙니다.</span>
</body>
```

## CSS Layout Techniques

### Box model
- CSS 원칙 1
  - 모든 요소는 네모(박스모델)이다.
  - 위에서부터 아래로, 왼쪽에서 오른쪽으로 쌓인다. (Normal Flow)

- Box model의 구성
  - 하나의 박스는 네 부분(영역)으로 이루어짐
    - margin: 테두리 바깥의 외부 여백 (배경색 지정 불가)
    - border: 테두리 영역
    - padding: 테두리 안쪽의 내부 여백 (배경색 지정 가능, 이미지 적용)
    - content: 글이나 이미지 등 요소의 실제 내용

```css
.margin {
  margin-top: 10px;
  margin-right: 20px;
  margin-bottom: 30px;
  margin-left: 40px;
}

.padding {
  padding-top: 10px;
  padding-right: 20px;
  padding-bottom: 30px;
  padding-left: 40px;
}

.border {
  border-width: 2px;
  border-style: dashed;
  border-color: black;
}

/* shorthand */
.margin-1 {
  margin: 10px;
}

.margin-2 {
  margin: 10px 20px;
}

.margin-3 {
  margin: 10px 20px 30px;
}

.margin-4 {
  margin: 10px 20px 30px 40px;
}

.border{
  border: 2px dashed black;
}
```
```html
<!-- 박스 만들기 -->
<html>
  <head>
    <style>
      .box {
        width: 100px;
        margin: 10px auto;
        padding: 20px;
        border: 1px solid black;
        color: white;
        text-align: center;
        background-color: blueviolet;
      }
  
      .box-sizing {
        box-sizing: border-box;
        margin-top: 50px;
      }
    </style>
  </head>
  <body>
    <!-- content의 width가 100px인 박스 생성 -->
    <div class="box">content-box</div>

    <!-- border의 width가 100px인 박스 생성 -->
    <div class="box box-sizing">border-box</div>
  </body>
</html>
```
```html
<!-- 위아래 margin이 겹칠 경우, 더 큰 margin만 적용 -->
<!-- 양 옆 margin은 겹쳐도 둘 다 적용 -->
<head>
  <style>
    .box1 {
      width: 100px;
      height: 100px;
      background-color: red;
      margin-bottom: 50px;
    }

    .box2 {
      width: 100px;
      height: 100px;
      background-color: blue;
      margin-top: 30px;
    }
  </style>
</head>
<body>
  <!-- 두 박스 사이 간격: 50px -->
  <div class="box1"></div>
  <div class="box2"></div>
</body>
```
### CSS Display
- CSS 원칙 2
  - display에 따라 크기와 배치가 달라짐

- 대표적으로 활용되는 display
  - display: block
    - 줄 바꿈이 일어나는 요소
    - 화면 크기 전체의 가로 폭을 차지
    - 블록 레벨 요소 안에 인라인 레벨 요소가 들어갈 수 있음
    - 대표적인 블록 레벨 요소: h, div, ul, ol, li, p, hr, form

  - display: inline
    - 줄 바꿈이 일어나지 않는 행의 일부 요소
    - content 너비만큼 가로 폭을 차지
    - width, height, margin-top, margin-bottom 지정 불가
    - 상하 여백은 line-height로 지정
    - 대표적인 인라인 레벨 요소: span, a, img, input, label, b, em, i, strong

  - display: inline-block
    - block과 inline 레벨 요소의 특징을 모두 가짐
    - inline처럼 한 줄에 표시 가능
    - block처럼 width, height, margin 속성을 모두 지정 가능
    - margin: auto 지정 불가

  - display: none
    - 해당 요소를 화면에 표시하지 않고, 공간도 부여 x
    - visibility: hidden은 화면에는 표시하지 않지만, 공간은 차지

```html
  <head>
    <style>
      div {
        width: 100px;
        height: 100px;
        border: 2px solid black;
        background-color: crimson;
      }

      .none {
        display: none;
      }

      .hidden {
        visibility: hidden;
      }
    </style>
  </head>
  <body>
    <h1>나는 block입니다</h1>
    <div>block</div>
    <p>나는 <span>인라인</span>속성입니다.</p>
    <hr>
    <h2>display none vs visibility hidden</h2>
    <div>1</div>
    <div class="none">2</div>
    <div class="hidden">3</div>
    <div>4</div>
  </body>
```
  
### CSS Position
- CSS 원칙 3
  - Position으로 위치의 기준을 변경
  - static: 모든 태그의 기본 값(기준 위치)
    - 일반적인 요소의 배치 순서에 따름(좌측 상단)
    - 부모 요소 내에서 배치될 때는 부모 요소의 위치를 기준으로 배치 됨
  - 아래는 좌표 프로퍼티(top, bottom, left, right)를 사용하여 이동 가능
    - relative
    - absolute
    - fixed
    - sticky

- CSS position
  - normal flow: CSS를 적용하지 않은 경우, 요소가 스스로 배치되는 것
  - relative: 상대 위치
    - 자기 자신의 static 위치를 기준으로 이동(normal flow 유지)
  - absolute: 절대 위치
    - static이 아닌 가장 가까이 있는 부모/조상 요소를 기준으로 이동(없는 경우 브라우저 화면 기준으로 이동) (normal flow에서 벗어남)
  - fixed: 고정 위치
    - 부모 요소와 관계없이 viewport를 기준으로 이동
    - 스크롤 시에도 항상 같은 곳에 위치
  - sticky: 스크롤에 따라 static -> fixed로 변경
    - 평소에는 static 상태와 같이 일반적인 흐름에 따르지만, 스크롤 위치가 임계점에 이르면 fixed와 같이 화면에 고정됨

```html
  <head>
    <style>
      div {
        box-sizing: border-box;
        height: 100px;
        width: 100px;
        border: 1px solid black;
      }

      .parent {
        position: relative;
        width: 300px;
        height: 300px;
      }
   
      .absolute {
        position: absolute;
        top: 100px;
        left: 100px;
        background-color: crimson;
      }

      .relative {
        position: relative;
        top: 100px;
        left: 100px;
        background-color: crimson;
      }

      .sibling {
        background-color: deepskyblue;
      }

      .fixed {
        position: fixed;
        bottom: 0;
        right: 0;
      }
    </style>
  </head>
  <body>
    <!-- absolute는 normal flow에서 벗어남 -->
    <div class="parent">
      <div class="absolute">형</div>
      <div class="sibling">동생</div>
    </div>

    <!-- relative는 normal flow 유지 -->
    <div class="parent">
      <div class="relative">형</div>
      <div class="sibling">동생</div>
    </div>
    <div class="fixed">고정</div>
  </body>
```

### CSS Float
- float
  - 박스를 왼쪽 혹은 오른쪽으로 이동시켜 텍스트를 포함 인라인 요소들이 주변을 wrapping하도록 함
  - 요소가 Normal flow를 벗어나도록 함

- float 속성
  - none: 기본값
  - left: 요소를 왼쪽으로 띄움
  - right: 요소를 오른쪽으로 띄움

```html
<head>
  <style>
    .box {
      width: 150px;
      height: 150px;
      border: 1px solid black;
      background-color: crimson;
      margin: 20px;
    }

    .left {
      float: left;
    }

    .right {
      float: right;
    }
  </style>
</head>
<body>
  <div class="box left">float left</div>
  <div class="box right">float right</div>
  <p>글자는 left와 right 사이에서 채워짐</p>
</body>
```

### CSS Flex
- CSS Flexible Box Layout
  - 행과 열 형태로 아이템들을 배치하는 1차원 레이아웃 모델
  - Normal flow를 벗어나는 수단 중 불편한 position과 float의 대안
  - Display
    - flex: block 특성의 flex container를 정의
    - inline-flex: inline 특성의 flex container를 정의

	- 축
	  - main axit(메인 축)
	  - cross axis(교차 축)

	- 구성 요소
	  - Flex Container(부모 요소)
	  - Flex Item(자식 요소)

- flex 속성
  - 배치 설정
	  - flex-direction (main axis 기준 방향 설정)
		- flex-wrap (아이템이 컨테이너를 벗어나는 경우, 해당 영역 내에 배치되도록 설정)
		- flex-flow: flex-direction과 flex-wrap의 shorthand

	- 공간 나누기
	  - justify-content (main axis)
	  - align-content (cross axis)

	- 정렬
	  - align-items (모든 아이템을 cross axis 기준으로)
	  - align-self (개별 아이템)

- flex 속성-배치 설정
  - flex-direction
	  - row
    - row-reverse
    - column
    - column-reverse

  - flex-wrap
	  - nowrap(기본 값): 크기를 조정해 한 줄에 배치
	  - wrap: 넘치면 그 다음 줄로 배치
	  - wrap-reverse: 교차 축 방향으로 역전시켜 배치

- flex 속성-공간 나누기 (justify-content & align-content)
  - flex-start(기본 값): 아이템들을 axis 시작점으로
  - flex-end: 아이템들을 axis 끝 쪽으로
  - center: 아이템들을 axis 중앙으로
  - space-between: 아이템 사이의 간격을 균일하게 분배
  - space-around: 아이템을 둘러싼 영역을 반으로 나눠서 양쪽에 균일하게 분배
  - space-evenly(justify): 전체 영역에서 아이템 간 간격을 균일하게 분배
  - stretch(align): 남은 공간만큼 늘리기

- flex 속성-flex 정렬 (align-items & align-self)
	- flex-start: 아이템을 컨테이너의 교차축의 시작점에 배치
	- flex-end: 아이템을 컨테이너의 교차축의 종점에 배치
	- center: 아이템을 컨테이너 중앙에 배치
	- stretch: 아이템을 컨테이너 높이까지 꽉 채워 배치
	- baseline: 아이템을 베이스 라인에 맞춰 배치

- flex 속성-기타 속성
  - flex-grow: 남은 영역을 아이템에 분배
  - order: 배치 순서

```html
<!-- 예시 -->
<head>
  <style>
    .flex-container {
      display: flex;
      flex-direction: row;
      flex-wrap: nowrap;
      justify-content: flex-start;
      align-content: flex-start;
      align-items: stretch;
    }

    .item1 {
      align-self: stretch;
      order: 0;
      flex-grow: 1;
    }

    .item2 {
      align-self: center;
      order: -1;
      flex-grow: 2;
    }

    .item3 {
      align-self: flex-end;
      order: 1;
      flex-grow: 3;
    }

    .large-box {
      width: 300px;
      height: 300px;
      border: 2px solid black;
    }

    div div {
      width: 100px;
      height: 100px;
      border: 1px solid black;
      background-color: red;
    }
  </style>
</head>
<body>
  <div class="large-box flex-container">
    <div class="item1">1</div>
    <div class="item2">2</div>
    <div class="item3">3</div>
  </div>
</body>
```

```html
<!-- 연습해보기 -->
<html>
<head>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;500&display=swap');

    * {
        box-sizing: border-box;
        font-family: 'Roboto', sans-serif;
      }
    body {
      margin: 1rem;
    }
    .example_container {
      display: grid;
      place-items: center;
    }
    .flex_container {
      display: flex;
      width: 400px;
      height: 400px;
      border: 1px solid black;
      background-color: rgb(248, 237, 227);
      margin: 1rem;
    }
    .flex_item {
      background-color: rgb(189, 210, 182);
      border: 1px dotted rgb(121, 135, 119);
      text-align: center;
      margin: 3px;
      width: 100px;
      height: 100px;
    }
    #layout_01 {
      display: flex;
      flex-direction: row;
      flex-wrap: wrap;
      justify-content: space-around;
      align-content: space-around;
    }
  </style>   
</head>
<body>
  <div class="example_container">
    <div id="layout_01" class="flex_container">
      <div class="flex_item"><p>1</p></div>
      <div class="flex_item"><p>2</p></div>
      <div class="flex_item"><p>3</p></div>
      <div class="flex_item"><p>4</p></div>
      <div class="flex_item"><p>5</p></div>
      <div class="flex_item"><p>6</p></div>
    </div>
  </div>
</body>
</html>
```




