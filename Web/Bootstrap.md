# Bootstrap

## 1. Bootstrap 기본 원리

### Bootstrap 불러오기
- link 태그 (head 태그 내부 위치)
```html
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
```

- scripts 태그 (닫는 body 태그 바로 위에 위치)
```html
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-A3rJD856KowSb7dwlZdYEkO39Gagi7vIsF0jrRAoQmDKKtQBHUuLZ9AsSv4jD4Xa" crossorigin="anonymous"></script>
```

### Spacing (Margin and Padding)
- {property}{sides}-{size}

- property
  - m: margin
  - p: padding

- sides
  - t: top
  - b: bottom
  - s: left(start)
  - e: right(end)
  - x: left and right
  - y: top and bottom
  - black: 4 방향 모두

- size
  - 0: $spacer * 0 = 0rem = 0px
  - 1: $spacer * .25 = 0.25rem = 4px
  - 2: $spacer * .5 = 0.5rem = 8px
  - 3: $spacer * 1 = 1rem = 16px
  - 4: $spacer * 1.5 = 1.5rem = 24px
  - 5: $spacer * 3 = 3rem = 48px
  - auto: auto

```html
<!-- box class는 따로 설정 -->
<div class="mt-3 my-5 box">bootstrap-spacing</div>
<!-- mt-3: margin-top: 0.25rem; -->
<!-- ms-5: margin-top: 48px; margin-bottom: 48px; -->

{property}{sides}-{size}

'm': property
't': sides
'3': size
```

### text & color
- text
  - text-start: 글자 왼쪽 정렬
  - text-end: 글자 오른쪽 정렬
  - text-center: 글자 중앙 정렬
  - text-decoration-none: 링크 밑줄 없애기
  - fw-bold: bold text
  - fw-normal: normal weight text
  - fw-light: light weight text
  - fst-italic: italic text
  - <a href="https://getbootstrap.com/docs/5.2/utilities/text/" target="_blank">bootstrap text 코드</a>

- 배경색
  - bg-dark: 검은색 배경
  - bg-white: 흰색 배경
  - bg-danger: 빨간색 배경
  - bg-primary: 파란색 배경
  - <a href="https://getbootstrap.com/docs/5.2/utilities/background/" target="_blank">bootstrap background 코드</a>

- 글자색
  - text-dark: 검은색 글씨
  - text-white: 흰색 글씨
  - text-danger: 빨간색 글씨
  - text-primary: 파란색 글씨
  - <a href="https://getbootstrap.com/docs/5.2/utilities/colors/" target="_blank">bootstrap color 코드</a>

### layout
- position
  - top, bottom, start, end로 위치 조절 (단위는 %)
  - position-absolute (부모 클래스가 static이 아니여야 함)
  - position-relative
  - position-fixed
  - position-sticky
  - position-static
  - <a href="https://getbootstrap.com/docs/5.2/utilities/position/" target="_blank">bootstrap position 코드</a>

```html
<div class="position-relative">
  <div class="position-absolute top-0 start-0">
    상단, 왼쪽으로 붙여서 배치
  </div>
  <div class="position-absolute bottom-0 end-0">
    하단, 오른쪽으로 붙여서 배치
  </div>
  <div class="position-absolute top-50">
    상단에서 50%만큼 마진
  </div>
</div>
```

- display
  - d-inline: inline으로 설정
  - d-block: block으로 설정
  - d-inline-block: inline-block으로 설정
  - d-none: none으로 설정
  - d-breakpoint-block: breakpoint부터 block으로 설정
  - d-breakpoint-none: breakpoint부터 none으로 설정
  - <a href="https://getbootstrap.com/docs/5.2/utilities/display/" target="_blank">display 코드</a>

- flex
  - d-flex: flex 설정
  - d-inline-flex: inline flex 설정
  - flex 속성도 지정 가능
    - ex) justify, align 등
  - <a href="https://getbootstrap.com/docs/5.2/utilities/flex/" target="_blank">flex 코드</a>

### component
  - Navbar
    - <a href="https://getbootstrap.com/docs/5.2/components/navbar/" target="_blank">Navbar 코드</a>

  - button
    - <a href="https://getbootstrap.com/docs/5.2/components/buttons/" target="_blank">button 코드</a>

  - dropdown
    - <a href="https://getbootstrap.com/docs/5.2/components/dropdowns/" target="_blank">dropdown 코드</a>

  - carousel
    - <a href="https://getbootstrap.com/docs/5.2/components/carousel/" target="_blank">carousel 코드</a>

  - modal
    - modal은 중첩해서 들어가면 안됨
    - <a href="https://getbootstrap.com/docs/5.2/components/modal/" target="_blank">modal 코드</a>

  - form
    - <a href="https://getbootstrap.com/docs/5.2/forms/form-control/" target="_blank">form 코드</a>

  - card
    - <a href="https://getbootstrap.com/docs/5.2/components/card/#grid-cards" target="_blank">grid card 코드</a>

## 2. Grid system

### Bootstrap grid system
- <a href="https://getbootstrap.com/docs/5.2/layout/grid/" target="_blank">grid system</a>

- 기본 요소
  - column: 실제 컨텐츠를 포함하는 부분
  - gutter: column과 column 사이의 공간 (사이 간격)
  - container: column들을 담고 있는 공간
    - container-fluid로, width 100%의 container 생성 가능
  - 12개의 column과 6개의 grid breakpoints가 존재

  ```html
  <div class="container">
    <div class="row">
      <div class="col">column 1</div>
      <div class="col">column 2</div>
      <div class="col">column 3</div>
    </div>
  </div>
  ```

- Breakpoints
  - <a href="https://getbootstrap.com/docs/5.2/layout/breakpoints/" target="_blank">breakpoints</a>

  |Breakpoint|Class infix|Dimensions|
  |---|---|---|
  |Extra small|None|<576px|
  |Small|sm|≥576px|
  |Medium|md|≥768px|
  |Large|lg|≥992px|
  |Extra large|xl|≥1200px|
  |Extra extra large|xxl|≥1400px|

```html
<!-- Grid system 연습 -->
<html>
<head>
  <style>
    .box {
      width: 100px;
      height: 100px;
      border: 1px solid black;
      background-color: red;
    }
  </style>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-gH2yIJqKdNHPEq0n4Mqa/HGKIhSkIHeL5AyhkYV8i59U5AR6csBvApHHNl/vI1Bx" crossorigin="anonymous">
</head>
<body>
  <div class="container">
    <h2 class="text-center">column</h2>
    <div class="row">
      <div class="box col">1</div>
      <div class="box col">2</div>
      <div class="box col">3</div>
    </div>

    <hr>

    <div class="row">
      <div class="box col">1</div>
      <div class="box col">2</div>
    </div>

    <hr>

    <!-- 12칸의 column 중 해당 숫자 만큼의 비율을 차지 -->
    <div class="row">
      <div class="box col-3">3칸 만큼의 column 차지</div> 
      <div class="box col-6">6칸 만큼의 column 차지</div>
    </div>

    <hr>

    <!-- 12칸을 넘어가면 다음 줄로 -->
    <div class="row">
      <div class="box col-9">9칸 만큼의 column 차지</div> 
      <div class="box col-4">4칸 만큼의 column 차지</div>
    </div>

    <hr>

    <!-- 크기에 따라 비율 변경 -->
    <div class="row">
      <div class="box col-2 col-sm-8 col-md-4 col-lg-5">1</div>
      <div class="box col-8 col-sm-2 col-md-4 col-lg-2">2</div>
      <div class="box col-2 col-sm-2 col-md-4 col-lg-5">3</div>
    </div>

    <hr>

    <!-- row 중첩 -->
		<div class="row">
      <div class="box col-8">아래에 12개의 column 생성
        <div class="row">
          <div class="box col-4">2</div>
          <div class="box col-4">3</div>
          <div class="box col-4">1</div>
        </div>
      </div>
      <div class="box col-6">1</div>
      <div class="box col-6">2</div>
    </div>

    <hr>

    <!-- offset으로 여백 주기 -->
    <div class="row">
      <div class="box offset-2 col-4">2칸의 공백주고 4칸의 column 차지</div> 
    </div>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.0/dist/js/bootstrap.bundle.min.js" integrity="sha384-A3rJD856KowSb7dwlZdYEkO39Gagi7vIsF0jrRAoQmDKKtQBHUuLZ9AsSv4jD4Xa" crossorigin="anonymous"></script>
</body>
</html>
```






