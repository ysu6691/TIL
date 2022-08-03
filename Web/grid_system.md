# Grid System

## 1. CSS Layout

### CSS Layout techniques
- Display
- Position
- Float
- Flexbox
- Grid
- 기타


### float
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

### flex
- CSS Flexible Box Layout
  - 행과 열 형태로 아이템들을 배치하는 1차원 레이아웃 모델
	- Normal flow를 벗어나는 수단 중 불편한 position과 float의 대안
	- 축:
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
		- nowrap
		- wrap
		- wrap-reverse

- flex 속성-공간 나누기 (justify-content & align-content)
  - flex-start(기본 값): 아이템들을 axis 시작점으로
	- flex-end
	- center
	- space-between
	- space-around
	- space-evenly

- flex 속성-flex 정렬 (align-items & align-self)
	- stretch
	- flex-start
	- flex-end
	- center
	- baseline

- flex 속성-기타 속성
  - flex-grow: 남은 영역을 아이템에 분배
	- order: 배치 순서




<head>
	<style>
		.box{
			width: 100px;
			height: 100px;
			border: 1px, solid, red;
			background-color: yellow;
		}
	</style>
</head>
<body>
	<div class="box"></div>
</body>





## 2. bootstrap


## 3. Responsive web








