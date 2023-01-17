# SCSS

## 1. SCSS란?

먼저, SASS(Syntactically Awesome StyleSheets)는 CSS의 단점을 보완하기 위한 CSS 확장 언어이다.

프로젝트 규모가 커지면서 스타일시트의 유지보수 또한 힘들어지는데, SASS는 변수, 중첩 등의 개념을 이용해 CSS의 단점을 보완한다.

여기서 SASS는 `{}`가 아닌 들여쓰기를 사용하고 세미콜론을 사용하지 않는데, 조금 더 CSS에 친화적으로 나온 것이 SCSS(Sassy CSS)이다.

SCSS는 SASS와 동일한 기능을 가지고 있으며 `{}`와 세미콜론을 그대로 사용한다.

## 2. SCSS의 사용

### 부모 선택자(&)

`&`을 이용해 바로 위의 부모 요소를 그대로 가져올 수 있다.

 ```scss
.font {
   /* .font-large */ 
  &-large {
     /* .font-large:hover */ 
    &:hover {
    }
    /* .font-large::after */ 
    &::after {
    }
    /* .font-large::before */ 
    &::before {
    }
  }
  /* .font-medium */ 
  &-medium {
  }
  /* .font-small */ 
  &-small {
  }
}
 ```

### 변수 할당(Variable)

`$`을 이용해 변수를 생성한다.

```scss
$my-font: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $my-font;
  color: $primary-color
}
```

### 중첩 구문(Nesting)

중괄호 내부에 중괄호를 사용해 자식 요소의 스타일을 지정할 수 있다.

```scss
nav {
  ul {
    margin: 0;
    padding: 0;
  }

  li {
    display: inline-block;
  }
}
```

### 모듈화(Modularity)

`@use`를 사용하여 파일을 분할하고 모듈화 할 수 있다.

```scss
/* base.scss */
$primary-color: #333;

body {
  color: $primary-color
}
```
```scss
@use 'base';

.inverse {
  background-color: base.$primary-color;
  color: white;
}
```

### 믹스인(Mixins)

함수처럼 default parameter를 지정할 수 있고 parameter를 받아서 속성을 부여할 수 있다.

```scss
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, .25);
  color: #fff;
}

.info {
  @include theme;
}
/*
.info {
  background: DarkGray;
  box-shadow: 0 0 1px rgba(169, 169, 169, 0.25);
  color: #fff;
}
*/

.alert {
  @include theme($theme: DarkRed);
}
/*
.alert {
  background: DarkRed;
  box-shadow: 0 0 1px rgba(139, 0, 0, 0.25);
  color: #fff;
}
*/
```

### 확장 & 상속

`@extend`를 사용해 `%`로 묶인 css 속성 집합을 상속받을 수 있다.

```scss
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
}

.message {
  @extend %message-shared;
  border-color: green;
}

.success {
  @extend %message-shared;
  border-color: red;
}
```
