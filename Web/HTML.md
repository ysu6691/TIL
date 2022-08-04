# HTML

## 1. 웹이란?

### Web
- 웹사이트의 구성 요소
  - 웹사이트란 브라우저를 통해서 접속하는 웹 페이지(문서)들의 모음
  - '링크'를 통해 여러 웹 페이지를 연결한 것이 웹사이트
  - HTML(구조), CSS(표현), Javascript(동작)으로 구성
  - <a href="https://html-css-js.com/" target="_blank">https://html-css-js.com</a>에서 확인 가능

- 웹사이트와 브라우저
  - 웹사이트는 브라우저를 통해 동작
  - 브라우저: chrome, edge, safari, ... 
  - 브라우저마다 동작이 약간씩 달라 문제가 생길 수 있음(파편화)
    - 이를 해결하기 위해 웹 표준이 등장

- 웹 표준
  - 웹에서 표준적으로 사용되는 기술이나 규칙
  - 어떤 브라우저든 웹 페이지가 동일하게 보이도록 함(크로스 브라우징)
  - <a href="https://caniuse.com/" target="_blank">can I use</a>에서 확인 가능

### 개발 환경 설정
- Visual Studio Code 확장 프로그램
  - Open in vrowser
  - Auto rename tag
  - Highlight Matching Tag

- 크롬 개발자 도구

## 2. HTML 기본 구조

### HTML이란
- HTML: Hyper Text Markup Language
  - Hyper Text: 참조(하이퍼링크)를 통해 사용자가 한 문서에서 다른 문서로 즉시 접근할 수 있는 텍스트
  - Markup Language: 태그 등을 이용하여 문서나 데이터의 구조를 명시하는 언어
- 웹 페이지를 작성(구조화)하기 위한 언어

### HTML 기본 구조
- 기본 구조
  - html: 문서의 최상위(root) 요소
  - head: 문서 메타데이터 요소(일반적으로 브라우저에 나타나지 않는 내용)
  - body: 문서 본문 요소(실제 화면 구성과 관련된 내용)

- head 예시
  - title: 브라우저 상단 타이틀
  - meta: 문서 레벨 메타데이터 요소
  - link: 외부 리소스 연결 요소
  - script: 스크립트 요소 (JavaScript 파일/코드)
  - style: CSS 직접 작성

- 요소(element)
  - 태그와 내용으로 구성
    ```html
    <h1>contents</h1>
    ```

- HTML with 개발자 도구
  - 원하는 요소를 선택해 구조를 탐색할 수 있음

- 속성(attribute)
  - 속성명과 속성값을 입력
    ```html
    <a href="https://google.com"></a> -> (공백 x, 쌍따옴표 사용)
      속성명        속성값
    ```
  - 태그에 부가적인 정보 설정
  - HTML Global Attribute: 태그에 관계없이 사용 가능한 속성
    - id, class, data-*, style, title, tabindex

- 시맨틱 태그
  - HTML5에서 의미론적 요소를 담은 태그의 등장
  - 단순히 구역을 나누는 것 뿐만 아니라 의미를 갖기 때문에, 검색엔진 등에 의미 있는 정보를 제공
  - 기존 영역을 의미하는 div 태그를 대체하여 사용
  - header, nav, aside, section, article, footer 등
  ```html
  <div>                    <header>
    <div></div>              <nav></nav>
  </div>                   </header>
  <div>                    <section>
    <div></div>              <article></article>
    <div></div>              <article></article>
  </div>                   </section>
  <div></div>              <footer></footer>
  ```

- 렌더링(Rendering)
  - 웹사이트 코드를 사용자가 보게 되는 웹사이트로 바꾸는 과정
  - DOM(Document Object Model)트리
    - 텍스트 파일인 HTML 문서를 브라우저에서 렌더링 하기 위한 구조

## 3. HTML 문서 구조화

### 인라인/블록 요소
- 인라인/블록 요소
  - HTML 요소는 크게 인라인/블록 요소로 나눔
  - 인라인 요소는 글자처럼 취급
  - 블록 요소는 한 줄 모두 사용

- 텍스트 요소
  ```html
  <a></a> : href 속성을 활용하여 다른 URL로 연결하는 하이퍼링크 생성
  <b></b> : 굵은 글씨 요소
  <strong></strong> : 중요한 강조하고자 하는 요소
  <i></i> : 기울임 글씨 요소
  <em></em> : 중요한 강조하고자 하는 요소
  <br> : 텍스트 내에 줄 바꿈 생성
  <img> : scr 속성을 활용하여 이미지 표현
  <span></span> : 의미 없는 인라인 컨테이너
  ```

- 그룹 컨텐츠
  ```html
  <p></p> : 하나의 문단(paragraph)
  <hr> : 문단 레벨 요소에서 주제의 분리를 의미, 수평선으로 표현
  <ol></ol> : 순서가 있는 리스트(ordered)
  <ul></ul> : 순서가 없는 리스트(unordered)
  <pre></pre> : HTML에 작성한 내용을 그대로 표현
  <blockquote></blockquote> : 텍스트가 긴 인용문, 주로 들여쓰기를 한 것으로 표현됨
  <div></div> : 의미 없는 블록 레벨 컨테이너
  ```

### form과 input
- form
  - 정보(데이터)를 서버에 제출하기 위해 사용하는 태그
  - 기본 속성
    - action: form을 처리할 서버의 URL (데이터를 보낼 곳)
    - method: form을 제출할 때 사용할 HTTP 메소드 (GET 혹은 POST)
    - enctype: 메소드가 post인 경우 데이터의 유형
      - application/x-www-form-urlencoded: 기본값
      - multipart/form-data: 파일 전송시 (input type이 file인 경우)

  ```html
  <form action="/search" method="GET">
  </form>
  ```

- input
  - 다양한 타입을 가지는 입력 데이터 유형과 위젯이 제공
  - input의 대표적인 속성
    - name: form control에 적용되는 이름
    - value: form control에 적용되는 값

  ```html
  <form action="/search" method="GET">
    <input type="text" name="q">
  </form>
  ```

- input label
  - label을 클릭하여 input 자체의 초점을 맞추거나 활성화 시킬 수 있음
  - input에 id 속성을, label에는 for 속성을 활용하여 상호 연관을 시킴

  ```html
  <label for="agreement">개인정보 수집에 동의합니다.</label>
  <input type="checkbox" name="agreement" id="agreement">
  * for와 id를 맞춰서 연관되게 적용
  ```

- input 유형 - 일반
  - text: 일반 텍스트 입력
  - password: 입력 시 특수기호(*)로 표현
  - email: 이메일 형식이 아닌 경우 form 제출 불가
  - number: min, max, step 속성을 활용하여 숫자 범위 설정 가능
  - file: accept 속성을 활용하여 파일 타입 지정 가능

- input 유형 - 항목 중 선택
  - 일반적으로 label 태그와 함께 사용하여 선택 항목을 작성함
  - 동일 항목에 대하여는 name을 지정하고 선택된 항목에 대한 value를 지정해야 함
  - checkbox: 다중 선택
  - radio: 단일 선택

- input 유형 - 기타
  - 다양한 종류의 input을 위한 picker를 제공
    - color: color picker
    - date: date picker
  - hidden input을 활용하여 사용자 입력을 받지 않고 서버에 전송되어야 하는 값을 설정
    - hidden: 사용자에게 보이지 않는 input


### 마크업 해보기
- 구조 설정
  - header
  - section
  - footer

```html
<html>
<head>
</head>

<body>
<header>
  <a href=""https://www.google.com">
    <img src="google.png" alt="main img" width="300">
  </a>
  <h1>Google 회원가입</h1>
</header>

<section>
  <form action="#">
    <div>
      <label for="id">아이디를 기재해주세요.</label><br>
      <input type="text" id="id" name="id" autofocus>
    </div>
    <div>
      <label for="password">비밀번호를 기재해주세요.</label><br>
      <input type="password" name="password" id="password" required></input>
    </div>
    <div>
      <label for="region">지역을 선택해주세요.</label>
      <select name="region" id="region" required>
        <option value="서울">서울</option>
        <option value="경기">경기</option>
        <option value="대전">대전</option>
        <option value="없음" disabled>없음</option>
      </select>
    </div>
    <div>
      <p>성별을 선택해주세요.</p>
      <input type="radio" name="gender" id="male" value="male" checked>
      <label for="male">남자</label>
      <input type="radio" name="gender" id="female" value="female">
      <label for="female">여자</label>
    </div>
    <input type="submit" value="제출">
  </form>
</section>
<footer>
개인정보를 보호해주세요.
</footer>
</body>
</html>
```
